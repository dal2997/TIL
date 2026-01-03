# Random Forest (Hotel Cancellation Example)

> 노트북: `5-Random Forest - Hotel.ipynb`  
> 목표: 호텔 예약 데이터로 `is_canceled`(취소 여부) 분류 + 성능평가/튜닝 흐름 익히기

---

## 1) Random Forest 한 줄 요약
- **여러 개의 Decision Tree를 랜덤하게 학습**시키고(부트스트랩 + 특성 랜덤 샘플링),
- 그 결과를 **다수결/평균**으로 합쳐서 **과적합(분산)을 줄이는 앙상블 모델**.

---

## 2) 노트북 흐름 (핵심 셀만 뽑기)

### A. 데이터 로드
~~~python
import pandas as pd

hotel_df = pd.read_csv(
  "https://raw.githubusercontent.com/DSNote/fastcampus/refs/heads/main/hotel.csv"
)
hotel_df.head()
~~~

### B. 불필요/민감 컬럼 제거 (PII 포함)
노트북에서 제거한 예시:
- 개인식별 정보: `name`, `email`, `phone-number`, `credit_card`
- 분석에 불필요/중복되는 날짜 컬럼 일부 등

~~~python
hotel_df = hotel_df.drop(
  [
    "arrival_date_day_of_month",
    "arrival_...s_date",
    "name", "email", "phone-number", "credit_card"
  ],
  axis=1
)
~~~

### C. 간단 EDA에서 얻은 포인트
- `distribution_channel`에서 `undefined`는 **표본이 5개 수준**이라
  barplot 에러바가 과하게 커질 수 있음 → **value_counts로 반드시 확인**

~~~python
import seaborn as sns

sns.barplot(x=hotel_df["distribution_channel"], y=hotel_df["is_canceled"])
hotel_df["distribution_channel"].value_counts()
~~~

### D. 범주형 컬럼 리스트 만들기 + 고카디널리티 drop 전략
- `country`는 값 종류가 177개로 큼 → “중요하면 유지, 아니면 drop” 판단 필요
- 노트북에서는 drop으로 진행

~~~python
obj_list = []
for c in hotel_df.columns:
    if hotel_df[c].dtype == "O":
        obj_list.append(c)

for c in obj_list:
    print(c, hotel_df[c].nunique())

hotel_df.drop("country", axis=1, inplace=True)
obj_list.remove("country")
~~~

### E. 범주형 → 원-핫 인코딩 (`pd.get_dummies`)
RandomForest도 숫자만 처리 가능 → object는 인코딩 필요

~~~python
hotel_df_encoded = pd.get_dummies(hotel_df, columns=obj_list)

X = hotel_df_encoded.drop("is_canceled", axis=1)
y = hotel_df_encoded["is_canceled"]
~~~

### F. Train/Test Split (재현성: random_state)
~~~python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.4, random_state=100
)
~~~

### G. 기본 RandomForest 학습
~~~python
from sklearn.ensemble import RandomForestClassifier

rf = RandomForestClassifier()
rf.fit(X_train, y_train)

pred = rf.predict(X_test)
proba = rf.predict_proba(X_test)[:, 1]
~~~

### H. 평가 (Accuracy / Confusion Matrix / Report / ROC-AUC)
- `classification_report`는 보통 **class=1(취소)** 쪽 precision/recall/f1에 집중
- ROC-AUC는 **0/1 예측값이 아니라 proba** 로 계산해야 의미 있음

~~~python
from sklearn.metrics import (
    accuracy_score, confusion_matrix, classification_report, roc_auc_score
)

print("acc:", accuracy_score(y_test, pred))
print("cm:\n", confusion_matrix(y_test, pred))
print(classification_report(y_test, pred))

print("roc_auc:", roc_auc_score(y_test, proba))
~~~

---

## 3) 하이퍼파라미터 튜닝에서 배운 것 (노트북 핵심 메시지)
### A. `max_depth`
~~~python
rf2 = RandomForestClassifier(max_depth=10, random_state=100)
rf2.fit(X_train, y_train)
proba2 = rf2.predict_proba(X_test)[:, 1]
roc_auc_score(y_test, proba2)
~~~

### B. `min_samples_split` (노트북 코멘트 요약)
- `min_samples_split`: **분할을 시도하기 위한 최소 샘플 수**  
  (해당 노드 샘플이 이 값보다 작으면 더 이상 쪼개지 않음)

~~~python
rf2 = RandomForestClassifier(min_samples_split=3, random_state=100)
rf2.fit(X_train, y_train)
proba2 = rf2.predict_proba(X_test)[:, 1]
roc_auc_score(y_test, proba2)
~~~

### C. 중요한 결론
- “각 파라미터에서 제일 좋아 보이는 값”을 **그냥 합친 조합이 최적이 아닐 수 있음**
- 그래서 조합을 체계적으로 탐색하는 도구가 필요 → `GridSearchCV`

---

## 4) GridSearchCV (cv 의미 포함)
- `cv=5`: **X_train 내부를 5-fold로 교차검증**해서 평균 성능 기준으로 최적 조합 탐색

~~~python
from sklearn.model_selection import GridSearchCV

params = {
    "max_depth": [None, 10, 30, 50],
    "min_samples_split": [2, 3, 5, 7],
}

rf3 = RandomForestClassifier(random_state=100)
grid = GridSearchCV(rf3, param_grid=params, cv=5)

grid.fit(X_train, y_train)
grid.best_params_

proba3 = grid.predict_proba(X_test)[:, 1]
roc_auc_score(y_test, proba3)
~~~

> 노트북 메모: GridSearch가 **꽤 오래 걸릴 수 있음**(환경에 따라 분 단위~)

---

## 5) Feature Importances (상위 10개 시각화)
~~~python
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd

rf4 = RandomForestClassifier(max_depth=50, min_samples_split=3, random_state=100)
rf4.fit(X_train, y_train)

feat_imp = pd.DataFrame({
    "features": X_train.columns,
    "importances": rf4.feature_importances_
})

top10 = feat_imp.sort_values("importances", ascending=False).head(10)

plt.figure(figsize=(15, 10))
sns.barplot(x="importances", y="features", data=top10)
~~~

---

## 6) 실전 팁 (짧게)
- 클래스 불균형이면 `accuracy`만 믿지 말고,
  `class=1`의 **recall/f1**, ROC-AUC(또는 PR-AUC) 같이 보자.
- 다음 개선 포인트 후보
  - `n_estimators`, `max_features`, `min_samples_leaf`, `class_weight="balanced"`
  - `train_test_split(..., stratify=y)` 적용 여부 검토

