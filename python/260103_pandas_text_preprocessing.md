# 🧹 Pandas Text Preprocessing (실전 레시피 모음)
> 파일 경로 추천: `TIL/python/pandas_text_preprocessing.md`  
> 목표: **텍스트/범주형 컬럼을 “일관된 규칙”으로 정리해서 분석/모델링에 바로 쓰기**

---

## 0) 이 문서의 전제
- 여기서 말하는 “텍스트 전처리”는 주로 **범주형/라벨성 문자열**(예: `Occupation`, `Type_of_Loan`)을 **정규화(normalization)** 하는 쪽에 초점.
- 긴 문장(리뷰/자유서술) 같은 **NLP 전처리**는 최소 수준만 다룸(과한 정제는 의미를 깨먹을 수 있음).

---

## 1) 텍스트 전처리에서 흔한 문제들
- 🔀 표기 흔들림: `Media_Manager` vs `Media Manager` vs `media manager`
- 🧻 불필요 공백: `"  Auto Loan "` / `",  "` / `"A ,B"`
- 🧩 멀티값 문자열: `"Auto Loan, Mortgage Loan, Student Loan"`
- ❓ 결측치/미정 표기: `"Not Specified"`, `"None"`, `""`(빈 문자열), `NaN`
- 🧨 숫자가 문자열로 들어옴: `"1,200"` `"₹5,000"` `"12 months"`

---

## 2) 기본 원칙 (이거 지키면 반은 성공)
### ✅ 원칙 A: 먼저 dtype을 통일
- object 컬럼은 섞여있을 수 있어서, 문자열 전처리 전엔 `string` dtype 권장
~~~python
col = "Type_of_Loan"
credit_df[col] = credit_df[col].astype("string")
~~~

### ✅ 원칙 B: “빈 문자열”도 결측치로 취급
~~~python
col = "Occupation"
credit_df[col] = credit_df[col].str.strip()
credit_df.loc[credit_df[col].eq(""), col] = pd.NA
~~~

### ✅ 원칙 C: 단순 치환은 `regex=False` 선호
- 의도 명확 + 불필요한 정규식 부작용 방지
~~~python
s = credit_df["Occupation"].astype("string")
s = s.str.replace("and ", "", regex=False)
~~~

---

## 3) (핵심) 범주형 문자열 정규화 템플릿
> “같은 의미는 같은 표기로 만들기”가 목표

### 3-1) 가장 많이 쓰는 정규화: strip + lower + 공백/구분자 통일
~~~python
def normalize_category(s: pd.Series) -> pd.Series:
    s = s.astype("string")
    s = s.str.strip()
    s = s.str.lower()
    s = s.str.replace(r"\s+", " ", regex=True)      # 다중 공백 -> 1칸
    s = s.str.replace("_", " ", regex=False)        # underscore -> space (필요 시)
    return s

credit_df["Occupation_norm"] = normalize_category(credit_df["Occupation"])
~~~

### 3-2) “미정/결측” 토큰을 NA로 통일
~~~python
MISSING_TOKENS = {"not specified", "none", "na", "n/a", "unknown", "null", "-"}

col = "Occupation_norm"
credit_df[col] = credit_df[col].where(~credit_df[col].isin(MISSING_TOKENS), pd.NA)
~~~

---

## 4) 멀티값 문자열 컬럼 처리 (예: `Type_of_Loan`)
> 분석/모델링 전에 가장 많이 막히는 지점

### 4-1) 구분자 통일 → split
~~~python
col = "Type_of_Loan"

credit_df[col] = (
    credit_df[col]
    .astype("string")
    .str.replace("and ", "", regex=False)
    .str.replace(r"\s*,\s*", ", ", regex=True)  # 쉼표 주변 공백 통일
    .str.strip()
)

loan_list = credit_df[col].str.split(",\s*", regex=True)  # 각 행이 리스트가 됨
~~~

### 4-2) 빈도 분석: split + explode
~~~python
loan_tokens = loan_list.explode().astype("string").str.strip()
loan_tokens = loan_tokens.where(loan_tokens.ne(""), pd.NA).dropna()

loan_tokens.value_counts().head(20)
~~~

### 4-3) 모델링: 멀티라벨 → multi-hot 인코딩(간단 버전)
- `get_dummies`를 explode 기반으로 쓰면 빠르게 multi-hot 만들 수 있음
~~~python
tmp = loan_list.explode().astype("string").str.strip()
tmp = tmp.where(tmp.ne(""), pd.NA).dropna()

dummies = pd.get_dummies(tmp)                    # 토큰별 더미
multi_hot = dummies.groupby(level=0).max()       # 원래 index로 합치기 (0/1)

# 기존 df에 붙이기
credit_df = credit_df.join(multi_hot.add_prefix("loan__"))
~~~

> ✅ 팁: 멀티값 컬럼을 “한 줄 문자열”로 두면 모델이 못 먹는 경우가 많아서, **multi-hot**으로 바꾸는 게 실전에서 자주 정답.

---

## 5) 숫자/기간/통화가 섞인 문자열 파싱
### 5-1) 통화/콤마 제거 후 숫자 변환
~~~python
col = "Monthly_Balance"  # 예시
s = credit_df[col].astype("string")

# 예: "₹1,234.50" -> "1234.50"
s_num = (s
         .str.replace(",", "", regex=False)
         .str.replace(r"[^\d\.]", "", regex=True))

credit_df[col + "_num"] = pd.to_numeric(s_num, errors="coerce")
~~~

### 5-2) “개월/년” 같은 기간에서 숫자만 추출
~~~python
col = "Credit_History_Age"  # 예: "22 Years and 4 Months"
s = credit_df[col].astype("string")

years  = pd.to_numeric(s.str.extract(r"(\d+)\s*year", expand=False), errors="coerce")
months = pd.to_numeric(s.str.extract(r"(\d+)\s*month", expand=False), errors="coerce")

credit_df["credit_history_months"] = years.fillna(0)*12 + months.fillna(0)
~~~

---

## 6) “자유서술 텍스트” 최소 전처리 (필요할 때만)
> 리뷰/문장형 데이터는 너무 정제하면 의미가 사라질 수 있음

~~~python
col = "Review_Text"
s = credit_df[col].astype("string")

s = s.str.replace(r"<[^>]+>", " ", regex=True)   # HTML 제거(있다면)
s = s.str.replace(r"\s+", " ", regex=True)       # 공백 정리
s = s.str.strip()

credit_df[col + "_clean"] = s
~~~

---

## 7) 전처리 품질 체크(강추)
### 7-1) 정규화 전/후 고유값 변화 확인
~~~python
col_raw = "Occupation"
col_norm = "Occupation_norm"

print("raw nunique:",  credit_df[col_raw].nunique(dropna=False))
print("norm nunique:", credit_df[col_norm].nunique(dropna=False))

# 상위 값 비교
print(credit_df[col_raw].value_counts(dropna=False).head(10))
print(credit_df[col_norm].value_counts(dropna=False).head(10))
~~~

### 7-2) 의도치 않은 폭발(explode) 검증
~~~python
avg_tokens = loan_list.dropna().apply(len).mean()
max_tokens = loan_list.dropna().apply(len).max()
print("avg tokens:", avg_tokens, "max tokens:", max_tokens)
~~~

---

## 8) 재사용 가능한 “텍스트 전처리 미니 파이프라인”
> 실습/프로젝트에서 똑같은 코드 반복하기 싫으면 이 패턴 추천

~~~python
def preprocess_text_cols(df: pd.DataFrame,
                         cat_cols: list[str],
                         missing_tokens: set[str] | None = None,
                         lowercase: bool = True) -> pd.DataFrame:
    out = df.copy()
    missing_tokens = missing_tokens or set()

    for c in cat_cols:
        s = out[c].astype("string").str.strip()

        # 빈 문자열 -> NA
        s = s.where(s.ne(""), pd.NA)

        # 소문자 통일
        if lowercase:
            s = s.str.lower()

        # 다중 공백 정리
        s = s.str.replace(r"\s+", " ", regex=True)

        # 미정 토큰 -> NA
        if missing_tokens:
            s = s.where(~s.isin(missing_tokens), pd.NA)

        out[c] = s

    return out

MISSING_TOKENS = {"not specified", "none", "na", "n/a", "unknown", "null", "-"}

credit_df = preprocess_text_cols(
    credit_df,
    cat_cols=["Occupation", "Payment_Behaviour"],
    missing_tokens=MISSING_TOKENS,
)
~~~

---

## 9) 실전 추천: “언제 무엇을 쓰나” 요약
- 🧼 표기 통일: `str.strip`, `str.lower`, `str.replace(r"\s+", " ")`
- 🧲 포함/필터링: `str.contains(..., na=False)`, `startswith`, `endswith`
- 🪓 파싱/추출: `str.split`, `explode`, `str.extract`
- 🧩 멀티값 → 모델 입력: `split + explode + get_dummies + groupby(max)`
- 🧯 결측치/미정 토큰: `where/isin`으로 `pd.NA` 통일

---

## 10) TIL에 적기 좋은 한 줄(요약 문장 예시)
- **pandas `Series.str` 기반으로 범주형/멀티값 텍스트 전처리(치환·분리·explode·multi-hot) 패턴 정리**
- **`Type_of_Loan` 멀티라벨 컬럼을 split/explode로 토큰화하고 더미화 방식까지 정리**

---

