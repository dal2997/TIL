# ✅ NaN(결측치) + dtype(타입) 에러 정리 (오늘 터진 이유들)

오늘 터진 에러는 사실상 2종류였음 👇
1) ❌ NaN 때문에 터짐  
2) ❌ 숫자로 못 바꾸는 타입 때문에 터짐 (Timestamp / 문자열)

---

## 1) NaN 에러: "Input X contains NaN"
### 의미
- X에 결측치(NaN)가 있는데, 많은 sklearn 모델(특히 LinearRegression)은 NaN을 그대로 못 먹음

### 1-1) 어디에 NaN 있는지 먼저 확인 ✅
~~~python
X = bike_df.drop('count', axis=1)

X.isna().sum().sort_values(ascending=False).head(20)
~~~

### 1-2) NaN 처리 방법 ✅ (상황별)
#### A) 의미상 0이 맞는 컬럼이면 0으로 채움 (예: rain_1h, snow_1h)
- 비/눈이 “없음”인데 NaN으로 기록된 경우가 꽤 있음

~~~python
X = bike_df.drop('count', axis=1).copy()
y = bike_df['count']

X['rain_1h'] = X['rain_1h'].fillna(0)
X['snow_1h'] = X['snow_1h'].fillna(0)
~~~

#### B) 일반 정석: Imputer로 채우기 (Pipeline에서 추천)
- 숫자: median/mean
- 문자: most_frequent

---

## 2) Timestamp 에러: "not 'Timestamp'"
### 대표 에러
- TypeError: float() argument must be a string or a real number, not 'Timestamp'

### 원인
- X에 `datetime64[ns]` 컬럼이 남아있음 (`datetime`)

### 해결 2가지 ✅
#### A) 응급처치: datetime 컬럼 제거 (빨리 돌아가게)
~~~python
X = X.drop(columns=['datetime'], errors='ignore')
~~~

#### B) 추천: datetime을 숫자 피처로 뽑고 원본은 drop
- 시간 정보는 예측에 엄청 중요한 경우 많음 (자전거 수요는 특히)

~~~python
import pandas as pd

X['datetime'] = pd.to_datetime(X['datetime'])
X['hour'] = X['datetime'].dt.hour
X['dayofweek'] = X['datetime'].dt.dayofweek
X['month'] = X['datetime'].dt.month
X['year'] = X['datetime'].dt.year
X = X.drop(columns=['datetime'])
~~~

---

## 3) 문자열(object) 에러: "could not convert string to float: 'winter'"
### 대표 에러
- ValueError: could not convert string to float: 'winter'

### 원인
- X에 문자열(범주형) 컬럼이 남아있음
- 오늘 너 데이터 기준으로 이런 애들:
  - weather_main (object)
  - season (object)  ← winter 같은 값
  - day_night (object)
  - covid (object)

### 해결 ✅: 원-핫 인코딩(더미 변수)
~~~python
import pandas as pd

X = pd.get_dummies(
    X,
    columns=['weather_main', 'season', 'day_night', 'covid'],
    drop_first=True
)
~~~

---

## 4) dtype 체크 습관 (에러 터지면 바로 이거부터) ✅
~~~python
X.dtypes
X.select_dtypes(include=['object', 'datetime64[ns]']).columns
~~~

---

## 5) 오늘 제일 많이 꼬인 “순서” 정리 ✅ (진짜 중요)
❌ 실수 패턴:
- X에 fillna/drop 했는데
- 이미 만들어둔 옛날 X_train으로 fit 해서 에러/반영 안 됨

✅ 안전 패턴:
1) X, y 분리  
2) 전처리(또는 Pipeline 준비)  
3) train_test_split  
4) model.fit

---

## 6) ✅ 빠른 체크리스트 (fit 전에 이것만 확인)
- [ ] X에 NaN 남아있나?  
- [ ] X에 datetime64[ns] 남아있나?  
- [ ] X에 object(문자열) 남아있나?  

~~~python
X.isna().sum().sum()                 # 총 NaN 개수
X.select_dtypes('datetime64[ns]').shape[1]  # datetime 컬럼 개수
X.select_dtypes('object').shape[1]          # object 컬럼 개수
~~~

