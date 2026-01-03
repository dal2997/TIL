# 📌 Pandas `Series.str` 치트시트 (문자열 전처리 모음)
> 경로 추천: `TIL/python/pandas_str_cheatsheet.md`  
> 목적: **문자열 컬럼(Series)에 “벡터화된 문자열 함수”를 일괄 적용**할 때 쓰는 `.str` 예제 모음

---

## 0) `.str` 기본 개념 한 줄
- `Series.str` = **문자열 컬럼(Series)의 각 셀(원소)에 문자열 연산을 벡터화해서 적용**하는 accessor
- 결측치(`NaN`)가 있어도 비교적 안전하게 동작(대부분 `NaN` 유지)

---

## 1) 전처리에서 제일 많이 쓰는 기본 10종
### ✅ 공백/대소문자 정리
~~~python
s = credit_df["Type_of_Loan"]

s = s.str.strip()            # 양끝 공백 제거
s = s.str.lower()            # 소문자
s = s.str.upper()            # 대문자
s = s.str.title()            # Title Case
s = s.str.replace("  ", " ", regex=False)  # 이중 공백 -> 단일 공백
~~~

### ✅ 부분 문자열 치환
~~~python
# 문자열 내부의 "and " 제거
credit_df["Type_of_Loan"] = credit_df["Type_of_Loan"].str.replace("and ", "", regex=False)

# 쉼표 뒤 공백 통일
credit_df["Type_of_Loan"] = credit_df["Type_of_Loan"].str.replace(r"\s*,\s*", ", ", regex=True)
~~~

### ✅ 포함 여부/시작/끝 판단 (Boolean 마스크)
~~~python
s = credit_df["Type_of_Loan"]

mask1 = s.str.contains("Mortgage", na=False)       # 포함 여부
mask2 = s.str.startswith("Auto", na=False)         # 시작
mask3 = s.str.endswith("Loan", na=False)           # 끝

filtered = credit_df[mask1]
~~~

### ✅ 길이/문자 개수
~~~python
s = credit_df["Type_of_Loan"]
lens = s.str.len()               # 문자열 길이
~~~

---

## 2) 분리/추출/파싱 (실전에서 진짜 자주 씀)
### ✅ split + explode (리스트형으로 풀어서 행 늘리기)
`"A, B, C"` 같은 멀티값 문자열을 분석할 때 강력함.
~~~python
tmp = (credit_df["Type_of_Loan"]
       .str.split(",\s*", regex=True)   # ["A","B","C"]
       .explode()                        # 행으로 펼침
       .str.strip())

tmp.value_counts().head(20)
~~~

### ✅ 특정 토큰만 가져오기 (n번째)
~~~python
s = credit_df["Type_of_Loan"].str.split(",\s*", regex=True)

first_item = s.str[0]   # 첫 번째 타입
second_item = s.str[1]  # 두 번째 타입(없으면 NaN)
~~~

### ✅ 정규식으로 “추출” (extract / extractall)
예: 숫자만 뽑거나, 패턴 기반으로 정보만 가져오기
~~~python
# 예: "Score: 721" -> 721 추출
credit_df["score_num"] = credit_df["some_col"].str.extract(r"(\d+)", expand=False).astype("float")

# 여러 개 매칭을 다 가져오고 싶으면 extractall
all_nums = credit_df["some_col"].str.extractall(r"(\d+)")
~~~

### ✅ replace에서 regex 주의
- `regex=True`면 정규식 패턴으로 처리
- 단순 문자열 치환이면 `regex=False` 권장(의도 명확, 성능도 유리한 편)
~~~python
s.str.replace(".", "", regex=False)  # '.'을 진짜 점으로
s.str.replace(r"\.", "", regex=True) # 정규식으로 점 처리
~~~

---

## 3) 검색/검증/클리닝 (정제 파이프라인에 넣기 좋음)
### ✅ 숫자인지/알파벳인지 같은 검증
~~~python
s = credit_df["some_col"].astype("string")  # 안전하게 string dtype로

is_digit = s.str.isdigit()   # 전부 숫자면 True
is_alpha = s.str.isalpha()   # 전부 알파벳이면 True
~~~

### ✅ 공백으로만 된 값 제거(빈값 처리)
~~~python
col = "Type_of_Loan"
credit_df[col] = credit_df[col].str.strip()
credit_df.loc[credit_df[col].eq(""), col] = pd.NA
~~~

### ✅ “Not Specified” 같은 값 통일
(이건 `.replace`로 값 매핑이 더 명확함)
~~~python
credit_df["Type_of_Loan"] = credit_df["Type_of_Loan"].replace({"Not Specified": pd.NA})
~~~

---

## 4) 자주 쓰는 기타 유틸
### ✅ 슬라이싱 (부분 문자열)
~~~python
s = credit_df["some_col"].astype("string")
prefix2 = s.str[:2]     # 앞 2글자
tail3   = s.str[-3:]    # 뒤 3글자
~~~

### ✅ pad / zfill (고정 길이 맞추기)
~~~python
s = credit_df["id"].astype("string")
s = s.str.zfill(6)      # 6자리 0-padding
~~~

### ✅ 반복 / 결합
~~~python
s = credit_df["some_col"].astype("string")
s2 = s.str.cat(credit_df["other_col"].astype("string"), sep=" | ", na_rep="")
~~~

---

## 5) “이거 알면 삽질 줄어드는” 포인트 추천 ⭐
### 5-1) `.str`는 **Series 전용**
- `credit_df["col"].str...` ✅  
- `credit_df.str...` ❌ (DataFrame에는 직접 `.str` 없음)

### 5-2) 결측치가 있으면 `na=` 옵션을 습관화
~~~python
mask = credit_df["Type_of_Loan"].str.contains("Loan", na=False)
~~~

### 5-3) dtype이 `object`면 “문자/숫자 섞임” 가능
- 문자열 처리 전에 **명시적으로 string dtype**으로 바꾸면 안정적
~~~python
credit_df["Type_of_Loan"] = credit_df["Type_of_Loan"].astype("string")
~~~

### 5-4) `.str` vs `.replace` 감각
- **문자열 내부 조작**: `.str.replace`, `.str.split`, `.str.contains`  
- **값 자체 치환/매핑**: `Series.replace`, `map`, `where`

### 5-5) 멀티라벨(쉼표로 여러 값) 컬럼은 `split + explode`가 정답
- 분석(빈도/상관/모델링) 전 단계에서 특히 유용

---

## 6) (너 케이스용) Type_of_Loan 정리 템플릿 예시
~~~python
col = "Type_of_Loan"

credit_df[col] = (
    credit_df[col]
    .astype("string")
    .str.replace("and ", "", regex=False)
    .str.replace(r"\s*,\s*", ", ", regex=True)
    .str.strip()
)

# 멀티값 분석용
loan_tokens = credit_df[col].str.split(",\s*", regex=True).explode().str.strip()
loan_counts = loan_tokens.value_counts()
~~~

---

## 7) 각주 문구 추천 (TIL에 붙이기)
- **문자열 컬럼 전처리: `Series.str`(벡터화 문자열 메서드)로 치환/분리 수행**
- **`Series.str.replace`로 문자열 내부 패턴 정리(na 안전)**
- **`split + explode`로 멀티값 문자열을 토큰 단위로 펼쳐 빈도 분석**

---

