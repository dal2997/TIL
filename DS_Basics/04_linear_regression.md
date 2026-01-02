# Linear Regression (선형회귀) 📈
> 연속형 타깃(숫자)을 예측하는 기본 회귀 모델  
> 핵심: `coef_` 해석(스케일 함정), RMSE/R²로 평가, 잔차로 문제 감지

---

## 1) 언제 쓰나?
- y가 숫자(예: 가격, 점수, 시간, 수요량)
- “설명 가능한 베이스라인”이 필요할 때

---

## 2) 개념 한 줄
- `y ≈ w1*x1 + w2*x2 + ... + b`
- `lr.coef_` = w들(피처 가중치), `lr.intercept_` = b

### coef_ 관련 함정(너가 자주 물었던 포인트)
- **피처 단위/스케일이 다르면 coef 크기 비교는 위험**
- 해석이 목적이면 표준화 후 비교가 안전

---

## 3) 핵심 셀(최소 재현 세트)
> 전처리/결측치/더미는 02번 문서 기반으로 처리한다고 가정

### (1) split
~~~python
from sklearn.model_selection import train_test_split

X = df.drop(columns=["target"])
y = df["target"]

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)
~~~

### (2) 학습
~~~python
from sklearn.linear_model import LinearRegression

lr = LinearRegression()
lr.fit(X_train, y_train)
~~~

### (3) coef 확인
~~~python
lr.coef_
lr.intercept_
~~~

### (4) 평가 (RMSE / R²)
~~~python
import numpy as np
from sklearn.metrics import mean_squared_error, r2_score

pred = lr.predict(X_test)
rmse = np.sqrt(mean_squared_error(y_test, pred))
r2 = r2_score(y_test, pred)

rmse, r2
~~~

---

## 4) coef를 “보기 좋게” 정렬 (실무형)
~~~python
import pandas as pd

coef_df = pd.DataFrame({
    "feature": X_train.columns,
    "coef": lr.coef_
}).sort_values("coef", ascending=False)

coef_df.head(20)
coef_df.tail(20)
~~~

---

## 5) 표준화 포함 파이프라인(coef 비교/안정성 ↑)
~~~python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LinearRegression

pipe = Pipeline([
    ("scaler", StandardScaler(with_mean=False)),  # 더미 포함이면 with_mean=False가 무난
    ("model", LinearRegression())
])

pipe.fit(X_train, y_train)
~~~

---

## 6) 잔차 플롯(문제 감지: 비선형/이분산)
~~~python
import matplotlib.pyplot as plt

pred = lr.predict(X_test)
residual = y_test - pred

plt.figure()
plt.scatter(pred, residual)
plt.axhline(0)
plt.xlabel("Predicted")
plt.ylabel("Residual (y - pred)")
plt.title("Residual Plot")
plt.show()
~~~

---

## 7) 체크리스트 ✅
- [ ] 결측치 처리했나? (02번 문서)
- [ ] 범주형 더미화 했나? (02번 문서)
- [ ] coef를 중요도처럼 비교하려면 스케일링 했나?
- [ ] RMSE/R² 확인했나? (03번 문서)
- [ ] 잔차에 패턴이 강하면 모델 가정이 깨진 신호다

