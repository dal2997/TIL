# ✅ Train/Test Split + random_state + stratify (오늘 헷갈린 핵심)

> 목표 🎯: "학습(train)"과 "평가(test)"를 분리해서 **실제 성능을 객관적으로 측정**하기

---

## 1) X, y 분리 (회귀/분류 공통)
- `y` = 내가 맞추고 싶은 값(타겟)
- `X` = y를 맞추기 위한 입력 피처(특징들)

~~~python
X = bike_df.drop('count', axis=1)
y = bike_df['count']
~~~

---

## 2) train_test_split은 왜 하나?
- ✅ train: 모델이 **학습**하는 데이터
- ✅ test: 모델 성능을 **평가**하는 데이터(처음 보는 데이터)

~~~python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=100
)
~~~

---

## 3) test_size 크기 의미 (언제 작게/크게?)
- `test_size=0.3` 👉 전체의 30%를 테스트로 남김 (70%로 학습)

### test_size 작게(0.1~0.2) 쓰는 경우
- 데이터가 작아서 **학습 데이터를 최대한 확보**해야 할 때
- 모델 학습이 잘 안 되는 느낌(underfitting)일 때

### test_size 크게(0.3~0.4) 쓰는 경우
- 데이터가 충분해서 train을 조금 줄여도 되고
- **평가 신뢰도**(테스트 샘플 수)를 올리고 싶을 때

---

## 4) random_state 의미 (숫자 “크기”는 의미 없음)
- random_state는 **난수 시드(seed)** = “섞는 방식 고정”
- 1이든 100이든 999든 **좋고 나쁨 없음**
- ✅ 같은 split이 반복돼서 **비교/디버깅**이 쉬워짐

---

## 5) stratify=y는 뭔데?
### stratify의 의미 🎯
- **분류(classification)**에서 클래스 비율이 train/test에 비슷하게 나눠지도록 강제
- 예) y가 0:95%, 1:5%면 → train/test도 비슷하게 유지

~~~python
train_test_split(X, y, test_size=0.3, random_state=100, stratify=y)
~~~

### 근데 왜 “회귀엔 stratify=y 하면 안 된다”가 많냐?
- 회귀 y는 연속값(거의 전부 다른 값)이라 **클래스 비율 개념이 없음**
- 그래서 stratify=y는 **에러** 나거나 **의미가 없음**

### 회귀에서도 “분포를 비슷하게” 나누고 싶다면(우회 방법)
- y를 구간(bin)으로 잘라서 stratify에 넣기 ✅

~~~python
import pandas as pd
from sklearn.model_selection import train_test_split

y_bins = pd.qcut(y, q=10, duplicates='drop')  # 분위수 기준 10구간
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=100, stratify=y_bins
)
~~~

---

## 6) 🔥 오늘 제일 중요한 실수 포인트 (필수 암기)
✅ **전처리(fillna, drop, encoding)를 X에 했으면**
✅ **반드시 그 X로 다시 train_test_split 해야 한다**

왜?
- 전처리 전 `X_train`을 계속 쓰면 → 전처리 효과가 학습에 반영 안 됨
- “X는 바꿨는데, X_train은 옛날 거” 상태가 되면:
  - NaN/문자열/Datetime 에러가 다시 터짐 💥
  - 성능도 엉망이 될 수 있음 💥

✅ 안전한 순서
1) X, y 분리  
2) (가능하면 Pipeline으로) 전처리  
3) train_test_split  
4) model.fit  

