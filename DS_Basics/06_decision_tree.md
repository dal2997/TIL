# Decision Tree (의사결정나무) 🌳
> 규칙 기반 분기로 예측하는 모델  
> 핵심: 과적합이 기본값 → `max_depth`, `min_samples_leaf`로 제어 + 중요도/시각화

---

## 1) 언제 쓰나?
- 비선형/상호작용이 있을 때도 빠르게 베이스라인 가능
- 모델을 “규칙 형태”로 보고 싶을 때
- 단, 성능 목적이면 보통 RandomForest/GBM으로 확장됨

---

## 2) 개념 한 줄
- “질문(분기)”을 반복해서 target을 맞춤
- 깊게 자라면(train만 잘 맞음) → 과적합

---

## 3) 핵심 셀(최소 재현 세트)
> 전처리/결측치/더미는 02번 문서 기반

### (1) split
~~~python
from sklearn.model_selection import train_test_split

X = df.drop(columns=["target"])
y = df["target"]

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)
~~~

### (2) 학습(과적합 방지 파라미터 포함)
~~~python
from sklearn.tree import DecisionTreeClassifier

dt = DecisionTreeClassifier(
    random_state=42,
    max_depth=4,
    min_samples_leaf=20
)

dt.fit(X_train, y_train)
~~~

### (3) 평가
~~~python
from sklearn.metrics import accuracy_score, classification_report

pred = dt.predict(X_test)
accuracy_score(y_test, pred)
print(classification_report(y_test, pred))
~~~

---

## 4) 피처 중요도(feature_importances_)
~~~python
import pandas as pd

imp = pd.DataFrame({
    "feature": X_train.columns,
    "importance": dt.feature_importances_
}).sort_values("importance", ascending=False)

imp.head(30)
~~~

---

## 5) 트리 시각화(구조 빠르게 확인)
~~~python
import matplotlib.pyplot as plt
from sklearn.tree import plot_tree

plt.figure(figsize=(16, 8))
plot_tree(
    dt,
    feature_names=X_train.columns,
    filled=False,
    max_depth=3
)
plt.show()
~~~

---

## 6) 튜닝 템플릿(과적합 제어 = 전부)
~~~python
from sklearn.model_selection import GridSearchCV
from sklearn.tree import DecisionTreeClassifier

param_grid = {
    "max_depth": [2, 3, 4, 5, 8, None],
    "min_samples_leaf": [1, 5, 10, 20],
    "min_samples_split": [2, 10, 20],
    "max_features": [None, "sqrt", "log2"]
}

gs = GridSearchCV(
    DecisionTreeClassifier(random_state=42),
    param_grid=param_grid,
    cv=5,
    scoring="f1",
    n_jobs=-1
)

gs.fit(X_train, y_train)
gs.best_params_
~~~

---

## 7) 체크리스트 ✅
- [ ] 트리는 규제 없으면 과적합이 기본이다(max_depth/min_samples_leaf 필수)
- [ ] 문자열 컬럼은 더미화했나? (sklearn 트리는 문자열 그대로 못 받음)
- [ ] feature_importances_는 편향될 수 있다(범주 많은 피처가 유리해지기도 함)
- [ ] 성능이 더 필요하면 앙상블(RandomForest/GBM)로 확장 고려

