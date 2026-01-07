# Chapter02 · Classification Problem

이 챕터는 **분류(Classification) 문제**를 다루며, 회귀(Regression)와 근본적으로 다른 문제 정의, 손실 함수, 결정 경계, 모델 구조, 평가 지표, 그리고 과적합 제어까지의 **전체 사고 흐름**을 정리한다.

> 목표:
>
> * 분류 문제를 수식·그림·모델 관점에서 일관되게 이해
> * Decision Tree 계열 모델이 왜 분류에서 강력한지 구조적으로 설명 가능
> * 평가 지표를 상황(불균형 데이터 등)에 맞게 선택할 수 있는 기준 확보

---

## 📌 Classification Problem 이란?

* **목표 변수(Target)** 가 연속값이 아닌 **범주형(label)** 인 문제
* 예:

  * 스팸 / 정상 메일
  * 합격 / 불합격
  * 이탈 / 유지

Regression과 달리 **"얼마나 틀렸는가"** 가 아니라
**"맞았는가 / 틀렸는가"** 가 직접적인 관측값이다.
→ 이 차이 때문에 **손실 함수, 모델 구조, 평가 지표가 전부 달라진다.**

---

## 🗂️ 파일 구성 및 학습 흐름

이 챕터는 아래 순서대로 읽는 것을 전제로 한다.

### 1️⃣ `01_problem_definition.md`

* 분류 문제의 정의
* Regression Loss vs Classification Loss
* Binary Cross Entropy(BCE)의 의미
* 확률 예측이 필요한 이유

👉 *"왜 MSE를 그대로 쓰면 안 되는가"에 대한 근본적인 답*

---

### 2️⃣ `02_decision_boundary.md`

* Decision Boundary 개념
* Decision Tree의 분기 구조
* 불순도(Impurity) 개념

  * Gini Index
  * Entropy
  * Information Gain

👉 *모델이 데이터를 어떻게 "나눈다"고 말할 수 있는가*

---

### 3️⃣ `03_modeling_and_interpretation.md`

* Decision Tree의 장단점
* Ensemble의 필요성

  * Bagging → Random Forest
  * Boosting → AdaBoost / GBM
* XGBoost / LightGBM의 등장 배경
* Bias–Variance 관점에서의 모델 비교

👉 *"왜 단일 트리는 약하고, 숲은 강한가"*

---

### 4️⃣ `04_evaluation_metrics.md`

* Confusion Matrix 구조
* Accuracy의 함정
* Precision / Recall / Specificity
* 데이터 불균형(Class Imbalance) 상황에서의 지표 선택

👉 *"모델이 잘 맞춘다"는 말의 정확한 의미 정의*

---

### 5️⃣ `05_regularization_and_feature_selection.md`

* Overfitting in Classification
* Tree Pruning
* Boosting 계열의 과적합 방지 기법

  * Shrinkage (Learning Rate)
  * Subsampling
  * Early Stopping

👉 *성능 향상 ≠ 복잡도 증가 임을 이해*

---

## 🔗 Regression 챕터와의 연결

* Regression에서 다룬:

  * Loss Function
  * Bias–Variance Tradeoff
  * Regularization

은 **형태만 다를 뿐 Classification에서도 그대로 등장**한다.

이 챕터의 핵심은

> *같은 개념이 어떻게 다른 모습으로 나타나는가* 를 파악하는 것

이다.

---

## ✅ 이 챕터를 끝내고 나면

* 분류 문제를 수식 + 그림 + 말로 동시에 설명할 수 있고
* RandomForest / AdaBoost / GBM 계열 모델의 차이를
  **"언제 무엇을 써야 하는가" 기준으로 말할 수 있으며
* Accuracy 하나만 보고 모델을 평가하지 않게 된다.

---

📎 참고: 본 챕터는 강의 PDF(`ClassificationProblem`)의 흐름을 기반으로 하되,
**단순 요약이 아닌 재구성된 개념 노트**를 목표로 한다.

