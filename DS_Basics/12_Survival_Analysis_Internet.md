# ⏳ 12_Survival_Analysis_Internet.md
> 수업 노트(`8-Survival Analysis - Internet.ipynb`) 기준으로 **전체 흐름 복습 + 코드 위주 리뷰**  
> 목표: “Survival Analysis(생존분석)”을 **오래 기억**할 수 있게, 개념을 코드에 연결해서 정리

---

## 0) 이 수업의 한 줄 목표
- **Churn(이탈)이 ‘발생했냐’**(분류)가 아니라  
- **Churn이 ‘언제 발생하냐’**(시간/생존)을 다룬다.  
- 특히 **아직 안 떠난 사람(검열, censored)** 도 “정보”로 인정하고 분석하는 게 핵심.

---

## 1) 데이터 로딩 & 구조 확인 (수업 흐름 그대로)
### ✅ 데이터 로딩
~~~python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

net_df = pd.read_csv(
    "https://raw.githubusercontent.com/DSNote/fastcampus/refs/heads/main/internet.csv"
)
net_df.head()
~~~

### ✅ 컬럼 구조(수업 데이터)
- `id`: 고객 ID
- `is_tv_subscriber`: TV 구독 여부(0/1)
- `is_movie_package_subscriber`: 영화 패키지 구독 여부(0/1)
- `subscription_age`: 이용 기간(연차처럼 사용, 0~13 근처로 보임)
- `bill_avg`: 평균 청구 금액(월평균/평균금액 느낌)
- `service_failure_count`: 장애/서비스 실패 횟수
- `churn`: 이탈 이벤트(0/1)

### ✅ dtype / 결측 확인(수업 데이터는 전부 int, 결측 거의 없음)
~~~python
net_df.info()
net_df.isna().mean()
~~~

---

## 2) EDA(분포 확인): 왜 먼저 보나?
생존분석 자체도 중요하지만, 수업은 먼저
- `bill_avg` 분포(치우침/0 많음 여부)
- `subscription_age` 분포(연차별 표본 수)
를 확인하고 들어감.

### 2-1) bill_avg 분포
~~~python
sns.displot(net_df["bill_avg"])
sns.displot(net_df["bill_avg"], bins=20)  # bins로 막대 개수 조절
sns.boxplot(y=net_df["bill_avg"])
~~~

#### ✅ 시사점(수업 포인트로 연결)
- `bill_avg`가 0이 많은지, 오른쪽 꼬리가 긴지에 따라
  - (현업) 특정 요금제/프로모션/무료기간 고객이 섞였을 수 있음
- boxplot은 이상치(outlier) 감 잡는 용도

> 참고: “박스플롯이 빈 그래프만 나오면?”  
> 보통 `NaN`이 100%이거나 dtype이 `object(문자열)`일 때가 많음.  
> 이 데이터는 int라서 정상이어야 함(환경/실행 순서 문제면 커널 리셋 후 재실행 추천).

### 2-2) subscription_age 분포
~~~python
sns.displot(net_df["subscription_age"])
~~~

#### ✅ 시사점
- 생존분석은 “시간축에 표본이 얼마나 있냐”가 중요  
- 연차별 표본 수가 너무 적으면 특정 연차의 생존율이 급격히 떨어져 보일 수 있음(해석 주의)

---

## 3) ## 데이터 수집방법 설명 (수업에서 강조한 현실 데이터 형태)
수업에서는 “회사에서는 이런 형태(엑셀/스프레드시트)”로 오는 경우가 많다고 설명함.

예시 형태(패널/롱 포맷):
- `ID`, `Year`, `Churn(0/1)`이 **연도별로 쌓이는 형태**

이걸 생존분석에 바로 넣기 어렵고, 보통은 고객별로 아래 두 개로 요약해서 씀:
- `duration`: (시작~이탈/관측종료)까지 걸린 기간
- `event`: 이탈 발생 여부(1=발생, 0=검열)

### (복습용) 패널 → duration/event 변환 템플릿
> 수업 노트엔 “개념 설명”이 중심이라, 오래 기억하려고 코드 템플릿을 같이 둠
~~~python
df = df.sort_values(["ID", "Year"])
g = df.groupby("ID")

start = g["Year"].min()
end   = g["Year"].max()

event = g["Churn"].max()  # 한 번이라도 1이면 이벤트 발생
churn_year = g.apply(lambda x: x.loc[x["Churn"].eq(1), "Year"].min())

duration = (churn_year.fillna(end) - start + 1)

surv_df = pd.DataFrame({
    "ID": start.index,
    "duration": duration.values,
    "event": event.values
})
~~~

---

## 4) ## 결측치 확인 + 인덱스 세팅(수업 코드 그대로)
수업은 확인 후 `id`를 인덱스로 잡고 진행.
~~~python
net_df.isna().mean()
net_df = net_df.set_index("id")
~~~

✅ 포인트:
- `id`가 모델 특성(feature)로 들어가면 안 되니(의미 없음), 인덱스로 빼두면 안전함  
  (lifelines는 인덱스를 공변량으로 쓰지 않음)

---

## 5) ## Kaplan-Meier로 생존율 분석하기(수업 핵심 1)
Kaplan-Meier(KM)는 “변수 영향 분석”이 아니라,
- **전체 생존곡선(S(t))** 을 그려서
- “시간이 지날수록 고객이 얼마나 남는지(잔존율)”을 보는 도구

### 5-1) lifelines 설치 & KM 학습(수업 그대로)
~~~python
!pip install lifelines

from lifelines import KaplanMeierFitter
kmf = KaplanMeierFitter()

kmf.fit(net_df["subscription_age"], net_df["churn"])
~~~

#### 기억 포인트(진짜 중요)
- `kmf.fit(durations, event_observed)`
- 여기서
  - durations = `subscription_age`
  - event_observed = `churn` (1이면 이벤트 발생)

### 5-2) 결과 테이블 & 시각화(수업 그대로)
~~~python
kmf.survival_function_  # 생존율이 시간에 따라 어떻게 떨어지는지

sns.lineplot(
    x=kmf.survival_function_.index,
    y=kmf.survival_function_["KM_estimate"]
)
~~~

### 5-3) “특정 연차에서 100% 이탈?” (수업에서 했던 필터링)
수업은 `subscription_age==13`에서 특이 케이스를 확인하고,  
그 구간을 제외해 다시 학습함.

~~~python
net_df["subscription_age"].value_counts()

net_df[net_df["subscription_age"] == 13]  # 수업: 여기 연차는 사실상 100% 이탈로 볼 수 있다

new_net_df = net_df[net_df["subscription_age"] < 13]

kmf.fit(new_net_df["subscription_age"], new_net_df["churn"])
kmf.survival_function_

sns.lineplot(
    x=kmf.survival_function_.index,
    y=kmf.survival_function_["KM_estimate"]
)
~~~

✅ 여기서 얻는 실전 감각(수업 의도)
- 특정 연차에서 데이터가 “특이하게” 쏠리면(정책/약정/계약 만료/프로모션 종료)
  - 생존곡선이 급락할 수 있음
- 그래서 “데이터 구조/정책 이벤트”를 의심하고 확인하는 흐름이 중요

---

## 6) ## 생존율 분석에 대한 이해 (수업 핵심 2)
KM을 “그림만 보고 끝”내지 말고, 내부 테이블 의미를 이해하자는 파트.

### 6-1) event_table 읽기 (수업 그대로)
~~~python
kmf.event_table
~~~

수업에서 잡아준 핵심 컬럼 의미:
- `event_at`: 시간축(1년차, 2년차, 3년차 …)
- `at_risk`: 해당 시점에 “아직 남아있는(관측 가능한) 인원”
- `observed`: 해당 시점에 실제 이벤트(이탈) 발생 수
- `censored`: 해당 시점에 관측 종료(검열)된 수

✅ 오래 기억하는 팁
- **at_risk**가 분모(그 시점에 남아있던 사람)
- **observed**가 그 시점에 떠난 사람
- **censored**는 “떠난 건 아니지만, 더 이상 추적이 안 되는 사람”

---

## 7) ## CoxPH로 생존율 분석하기 (수업 핵심 3)
KM은 “전체 곡선”이고, CoxPH는 “변수 영향”을 본다.

### 7-1) CoxPHFitter 학습(수업 그대로)
~~~python
from lifelines import CoxPHFitter

cph = CoxPHFitter()

# 수업 코멘트: id 같은 변수가 영향 주면 안 되는데,
# 지금은 id를 index로 두어서 공변량으로 안 들어간다고 보면 됨.
cph.fit(new_net_df, "subscription_age", "churn")
~~~

### 7-2) baseline survival 확인(수업)
~~~python
cph.baseline_survival_
~~~

✅ baseline survival은
- “특정 공변량(변수)을 0/기준으로 둔” 기준 생존율 곡선 느낌
- KM이랑 완전히 동일하진 않을 수 있음(모델링 방식이 다름)

---

## 8) ## Summary 해석 (수업 핵심 4: 여기서 점수 따는 파트)
### 8-1) Summary 출력(수업 그대로)
~~~python
cph.print_summary()
~~~

수업 노트 결과(핵심만 요약)
- `exp(coef)` = **Hazard Ratio(HR)**
  - HR > 1 : 이탈 위험 ↑ (더 빨리 떠날 가능성)
  - HR < 1 : 이탈 위험 ↓ (더 오래 남을 가능성)

이번 결과(수업 출력 기준 핵심 해석):
- `is_tv_subscriber`: coef 음수, HR≈0.52 → **TV 구독자는 이탈 위험 감소**
- `is_movie_package_subscriber`: HR≈0.47 → **영화 패키지도 이탈 위험 감소**
- `bill_avg`: HR≈0.99(아주 미세) → **요금이 높을수록(혹은 bill_avg 증가) 위험이 조금 감소하는 방향**
- `service_failure_count`: HR≈1.03 → **장애가 늘수록 이탈 위험 증가**

> 외우기 좋은 문장  
> “구독(혜택) 붙어있으면 덜 떠나고, 장애 많으면 더 떠난다.”

### 8-2) coef 시각화(수업)
~~~python
cph.plot()
~~~

### 8-3) 부분효과(수업에서 직접 보여준 ‘곡선 비교’)
~~~python
cph.plot_partial_effects_on_outcome(
    covariates="is_tv_subscriber",
    values=[0, 1]
)

cph.plot_partial_effects_on_outcome(
    covariates="service_failure_count",
    values=[0, 1, 2, 3]
)
~~~

✅ 여기서 얻는 감각
- 단순히 “HR이 0.5다/1.03이다”보다
- “시간에 따라 생존곡선이 어떻게 벌어지는지”가 더 직관적일 때가 많음

### 8-4) 개인별 생존함수 예측(수업 마지막 흐름)
~~~python
cph.predict_survival_function(new_net_df)
~~~

이건:
- “각 고객(각 row)의” 시간별 생존확률 S(t)을 반환  
- 개인화 리텐션/이탈 예측 시나리오로 연결 가능

---

## 9) 수업 흐름을 ‘한 번에’ 실행하는 미니 파이프라인(암기용)
~~~python
# 1) Load
net_df = pd.read_csv("...internet.csv").set_index("id")

# 2) EDA는 생략 가능하지만 subscription_age 분포/특이값은 꼭 확인
# 3) KM
from lifelines import KaplanMeierFitter
kmf = KaplanMeierFitter()
kmf.fit(net_df["subscription_age"], net_df["churn"])

# 4) 특이 연차 필터(수업 예시)
new_net_df = net_df[net_df["subscription_age"] < 13]

# 5) CoxPH
from lifelines import CoxPHFitter
cph = CoxPHFitter()
cph.fit(new_net_df, "subscription_age", "churn")

# 6) 해석
cph.print_summary()
~~~

---

## 10) 헷갈리기 쉬운 포인트(수업 + 너가 겪은 문제 예방)
- ✅ `duration`은 시간(여기서는 `subscription_age`)
- ✅ `event`는 이벤트(여기서는 `churn`)
- ✅ KM은 “전체 생존곡선”
- ✅ Cox는 “변수 영향(HR)”  
- ✅ 특정 연차에서 급락/특이 패턴 → **정책/계약/데이터 편향** 의심하고 확인
- ✅ 그래프가 빈 것처럼 나오면:
  - 컬럼이 전부 NaN인지, dtype이 object인지, 코드 실행 순서가 꼬였는지부터 체크

---

## 11) 다음 단계 추천(수업 이후 확장 아이디어)
- 그룹별 KM 비교(예: TV 가입 vs 미가입)
- CoxPH의 비례위험 가정 체크(`check_assumptions`)로 모델 진단
- “시간에 따라 변하는 변수(요금제 변경 등)”가 있으면 Time-varying Cox로 확장

---

