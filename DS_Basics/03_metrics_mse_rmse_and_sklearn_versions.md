# ✅ MSE / RMSE + sklearn 버전 이슈 + Pipeline(정석)

---

## 1) MSE / RMSE는 “오차 크기”를 재는 지표 📏

### MSE (Mean Squared Error)
- 오차를 제곱해서 평균낸 값
- 큰 오차를 더 강하게 벌점 줌(제곱)
- 단위가 원래 단위의 “제곱”이라 직관은 떨어짐

공식:
- MSE = 평균( (y - y_pred)^2 )

~~~python
from sklearn.metrics import mean_squared_error

mse = mean_squared_error(y_test, pred)
mse
~~~

### RMSE (Root Mean Squared Error)
- RMSE = sqrt(MSE)
- 원래 단위로 돌아와서 직관적
- “평균적으로 몇 정도 틀림?” 느낌으로 해석 가능

~~~python
import numpy as np
from sklearn.metrics import mean_squared_error

rmse = np.sqrt(mean_squared_error(y_test, pred))
rmse
~~~

---

## 2) sklearn 버전 이슈: squared=False가 왜 터졌나 🧨
예전엔 이런 코드가 됐던 시절이 있었음:

~~~python
mean_squared_error(y_test, pred, squared=False)  # RMSE
~~~

근데 최신 sklearn(1.6+)에선 `squared` 파라미터가 제거되어서:
- TypeError: got an unexpected keyword argument 'squared' 발생

✅ 결론(버전 상관없이 안전):
- RMSE는 무조건 `np.sqrt(mean_squared_error(...))` 쓰면 된다

---

## 3) Pipeline이 왜 정석인가? (오늘 너가 고생한 이유 해결용) 🧩
오늘 고생한 핵심:
- NaN 처리
- 문자열(object) 인코딩
- train/test에 동일한 전처리 적용
- “X는 바꿨는데 X_train은 옛날 거” 실수 방지

Pipeline을 쓰면:
- fit 한 번에 전처리 + 학습이 묶이고 ✅
- predict 할 때도 같은 전처리가 자동 적용됨 ✅

---

## 4) (추천) 너 데이터에 맞춘 “정석 형태” 예시
⚠️ 포인트:
- datetime은 그대로 넣으면 Timestamp 에러 나니까
- 먼저 hour/dayofweek/month/year 같은 숫자피처로 만들고 drop

~~~python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import OneHotEncoder
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error
import numpy as np

# 1) X, y
X = bike_df.drop('count', axis=1).copy()
y = bike_df['count']

# 2) datetime -> 숫자 피처로 변환
X['datetime'] = pd.to_datetime(X['datetime'])
X['hour'] = X['datetime'].dt.hour
X['dayofweek'] = X['datetime'].dt.dayofweek
X['month'] = X['datetime'].dt.month
X['year'] = X['datetime'].dt.year
X = X.drop(columns=['datetime'])

# 3) num/cat 컬럼 자동 분리
num_cols = X.select_dtypes(include=['number', 'bool']).columns
cat_cols = X.select_dtypes(exclude=['number', 'bool']).columns  # object 포함

# 4) 전처리 + 모델 파이프라인
preprocess = ColumnTransformer([
    ('num', SimpleImputer(strategy='median'), num_cols),
    ('cat', Pipeline([
        ('imputer', SimpleImputer(strategy='most_frequent')),
        ('onehot', OneHotEncoder(handle_unknown='ignore'))
    ]), cat_cols),
])

model = Pipeline([
    ('prep', preprocess),
    ('lr', LinearRegression())
])

# 5) split -> fit -> predict -> 평가
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=100
)

model.fit(X_train, y_train)
pred = model.predict(X_test)

mse = mean_squared_error(y_test, pred)
rmse = np.sqrt(mse)

mse, rmse
~~~

---

## 5) 오늘 기준 “암기용 한 문장” ✅
- 회귀에서 모델이 원하는 입력은 결국 **숫자 행렬**이다.
- 그래서 **NaN / Timestamp / object(문자열)** 은 반드시 처리해야 한다.
- 가장 안전한 방법은 **Pipeline으로 묶는 것**이다.

