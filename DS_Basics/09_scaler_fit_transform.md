# 📏 Scaling에서 fit / transform 정리 (Supervised vs Unsupervised)
> 파일명 추천(DS_Basics 규칙 맞춤): `09_scaler_fit_transform.md`  
> (네가 DS_Basics에서 확장자 없이 쓰는 스타일이면 `.md`만 빼서 `09_scaler_fit_transform`로 저장해도 됨)

---

## 1) 결론 한 줄 (암기용)
- **fit = 스케일링 “규칙(평균/표준편차)”을 학습**
- **transform = 그 규칙으로 “변환만” 수행**
- **test(미래 데이터)는 무조건 transform만!** (fit 금지)

---

## 2) 왜 test에는 fit을 하면 안 되나? (데이터 누수)
### ✅ test의 의미
- test set은 “**미래에 들어올 데이터**”라고 가정하고 미리 떼어둔 것
- 모델이 “처음 보는 데이터”에서도 잘 동작하는지 확인하려는 목적

### ❌ test에 fit을 해버리면 생기는 문제
- test 데이터의 분포(평균/표준편차)를 전처리에 섞어버림  
  → 현실에선 모르는 정보를 미리 쓴 셈  
  → 평가가 실제보다 좋아질 수 있음 (**data leakage**)

---

## 3) Supervised Learning에서 정석 순서 (중요)
### ✅ 항상 이 순서
1) train/test split
2) scaler는 **train에 대해서만 fit**
3) train은 `fit_transform`, test는 `transform`만

~~~python
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=100, stratify=y
)

scaler = StandardScaler()

X_train_scaled = scaler.fit_transform(X_train)  # 규칙 학습 + 변환
X_test_scaled  = scaler.transform(X_test)       # 같은 규칙으로 변환만
~~~

---

## 4) “test에도 fit_transform 하면 안 좋은 이유” (현실/운영 관점)
실서비스에서는 데이터가:
- 한 건, 두 건씩 **실시간으로 들어올 수도 있음**
- 매번 들어올 때마다 평균/표준편차를 다시 계산(fit)하는 건
  - 수학적으로도 안정적이지 않고
  - 시스템적으로도 일관성이 깨짐

즉, **전처리도 모델의 일부**로 보고,
- train으로 한 번 학습된 전처리 규칙을
- 이후 들어오는 데이터에 계속 동일하게 적용해야 함.

---

## 5) Unsupervised(클러스터링)에서는 왜 보통 split을 안 하나?
### ✅ 흔한 케이스
- 클러스터링은 정답(y)이 없고,
- 전체 데이터를 기준으로 “군집 구조”를 찾는 게 목적이라
- 보통 **전체 데이터를 통으로 스케일링하고 통으로 클러스터링**하는 흐름이 많음

~~~python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)   # 전체 데이터로 규칙 학습 + 변환 (unsupervised에서 흔함)
~~~

### ✅ 하지만 예외도 있음 (운영/배포 관점)
- “새 고객이 들어오면 기존 군집에 배정” 같은 시나리오라면:
  - train 데이터로 scaler + clustering을 만들고
  - 새 데이터는 **transform만** 해서 군집 할당해야 함  
  (supervised와 같은 철학)

---

## 6) 자주 하는 실수 TOP3
### 6-1) split 전에 전체 데이터에 fit 해버림
- train/test 섞여서 누수 위험

### 6-2) train/test 각각 fit_transform 해버림
- train 기준과 test 기준이 달라져서 스케일이 “서로 다른 세계”가 됨  
- 운영에서도 매번 fit하는 셈이라 비현실적

### 6-3) fit한 컬럼과 transform한 컬럼이 다름 (sklearn 에러 원인)
Confirm:
- fit할 때 사용한 feature 수/순서/이름과
- transform에 넣는 feature가 반드시 동일해야 함

---

## 7) 실전 팁: Pipeline으로 “누수 방지 자동화”
스케일링과 모델을 묶어두면 실수 방지가 쉬움.

~~~python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression

pipe = Pipeline([
    ("scaler", StandardScaler()),
    ("model", LogisticRegression())
])

pipe.fit(X_train, y_train)       # scaler는 train 기준으로 fit됨
pred = pipe.predict(X_test)      # test에는 transform만 적용됨
~~~

---

## 8) 내 요약(딱 이 문장만 기억해도 됨)
- **fit은 “미래 정보를 학습”하는 행동이라 test에 하면 누수**
- **train에서 배운 전처리 규칙(평균/표준편차)을 고정하고**
- **test/새 데이터는 transform만 적용한다**
---

