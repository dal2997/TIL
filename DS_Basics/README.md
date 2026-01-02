# DS_Basics 🧠
> 목표: 데이터 사이언스 기본 흐름 + 기본 모델 3종(LR/Logistic/Tree)을 “md만 읽어도 재현” 가능하게 정리

---

## 📁 폴더 구조
DS_Basics/
├── README.md
├── 01_split_and_random_state.md
├── 02_missing_values_and_dtypes.md
├── 03_metrics_mse_rmse_and_sklearn_versions.md
├── 04_linear_regression.md
├── 05_logistic_regression.md
└── 06_decision_tree.md

---

## ✅ 추천 학습 순서
1) 01_split_and_random_state.md
2) 02_missing_values_and_dtypes.md
3) 03_metrics_mse_rmse_and_sklearn_versions.md
4) 04_linear_regression.md
5) 05_logistic_regression.md
6) 06_decision_tree.md

---

## 🧱 공통 워크플로우(헷갈리지 말고 이것만)
### 0) 라이브러리
~~~python
import pandas as pd
import numpy as np
~~~

### 1) 로드 + 기본 점검
~~~python
df = pd.read_csv("data.csv")  # 경로 수정
df.shape
df.head()
df.info()          # dtype / null
df.isna().sum()    # 결측치
~~~

### 2) 카테고리 확인: value_counts vs nunique (자주 헷갈리는 포인트)
- value_counts(): “각 값이 몇 개냐” 분포표
- nunique(): “종류가 몇 개냐” 개수
~~~python
df["col"].value_counts(dropna=False).head(20)
df["col"].nunique(dropna=False)
~~~

### 3) 범주형 처리: get_dummies 왜 하냐?
- 모델은 문자열을 직접 계산 못함 → 원-핫(더미)로 바꾸기
~~~python
df = pd.get_dummies(df, columns=["cat1","cat2"], drop_first=False)
~~~

### 4) 분리(split)
- 스케일링/결측치대체/인코딩 같은 “기준 계산”은 **train에 맞추는 게 원칙**
~~~python
from sklearn.model_selection import train_test_split

X = df.drop(columns=["target"])
y = df["target"]

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)
~~~

---

## 🔗 모델별 정리
- Linear Regression: 04_linear_regression.md
- Logistic Regression: 05_logistic_regression.md
- Decision Tree: 06_decision_tree.md

---

## ✅ Git 반영(루틴)
~~~bash
git status
git add DS_Basics/
git commit -m "docs: add DS_Basics model notes (04~06)"
git push
~~~

