# 04 · Evaluation Metrics (Classification)

이 문서는 **Classification 모델을 어떻게 평가해야 하는가**에 대한 기준을 정리한다.
특히 강의 PDF에서 강조한 것처럼, **데이터의 클래스 분포(Class Balance)** 에 따라
"좋은 모델"의 기준이 달라진다는 점을 중심으로 설명한다.

---

## 1️⃣ 왜 Classification 평가는 까다로운가

Regression에서는:

* 예측값과 실제값의 차이를 바로 계산 가능
* MSE, RMSE 같은 단일 지표로 비교 가능

하지만 Classification에서는:

* 결과가 맞음 / 틀림으로만 관측됨
* "틀림"에도 여러 종류가 존재

👉 따라서 단일 지표로 성능을 말하기 어렵다.

---

## 2️⃣ Confusion Matrix

### 정의

Confusion Matrix는

> 실제 클래스와 예측 클래스의 조합을 표로 정리한 것

Binary Classification 기준:

| 실제 \ 예측      | Positive | Negative |
| ------------ | -------- | -------- |
| **Positive** | TP       | FN       |
| **Negative** | FP       | TN       |

---

### 용어 정리 (강의 기준)

* **TP (True Positive)**

  * 실제 Positive를 Positive로 예측
* **FN (False Negative)**

  * 실제 Positive를 Negative로 예측
* **FP (False Positive)**

  * 실제 Negative를 Positive로 예측
* **TN (True Negative)**

  * 실제 Negative를 Negative로 예측

👉 이후 모든 지표는 이 4개의 조합으로 계산된다.

---

## 3️⃣ Accuracy (정확도)

### 정의

```
Accuracy = (TP + TN) / (TP + FP + FN + TN)
```

---

### 장점

* 직관적
* 클래스 균형이 맞을 때 해석이 쉬움

---

### 치명적인 한계 (강의 핵심)

#### Class Imbalance 상황

* Positive: 1%
* Negative: 99%

모든 데이터를 Negative로 예측해도:

```
Accuracy ≈ 99%
```

👉 하지만 모델은 **아무것도 학습하지 않은 상태**

---

## 4️⃣ Precision (정밀도)

### 정의

```
Precision = TP / (TP + FP)
```

### 의미

> Positive라고 예측한 것 중
> **얼마나 진짜 Positive인가**

---

### 언제 중요한가

* FP가 치명적인 경우

  * 스팸 메일 필터
  * 광고 타겟팅

👉 "괜히 Positive라고 판단하면 안 되는" 문제

---

## 5️⃣ Recall (재현율, Sensitivity)

### 정의

```
Recall = TP / (TP + FN)
```

### 의미

> 실제 Positive 중
> **얼마나 놓치지 않고 잡아냈는가**

---

### 언제 중요한가

* FN이 치명적인 경우

  * 암 진단
  * 불량품 검출

👉 "놓치면 안 되는" 문제

---

## 6️⃣ Precision vs Recall Trade-off

* Threshold를 높이면:

  * Precision ↑
  * Recall ↓

* Threshold를 낮추면:

  * Precision ↓
  * Recall ↑

👉 두 지표는 동시에 극대화 불가

---

## 7️⃣ Specificity (특이도)

### 정의

```
Specificity = TN / (TN + FP)
```

### 의미

> 실제 Negative 중
> **얼마나 잘 Negative로 판별했는가**

---

### 강의에서의 맥락

* 의료 진단에서 자주 함께 사용
* Recall(민감도)과 짝을 이룸

---

## 8️⃣ 지표 선택의 기준 (강의 요약)

| 상황      | 우선 지표                |
| ------- | -------------------- |
| 클래스 균형  | Accuracy             |
| FP 비용 큼 | Precision            |
| FN 비용 큼 | Recall               |
| 의료/검사   | Recall + Specificity |

👉 지표는 **모델이 아니라 문제에 맞춰 선택**한다.

---

## 🔑 핵심 정리

* Confusion Matrix는 모든 분류 지표의 출발점
* Accuracy는 imbalance 상황에서 위험
* Precision / Recall은 trade-off 관계
* "좋은 모델"은

  > **문제의 비용 구조를 반영한 모델**

다음 문서에서는,
👉 분류 모델에서의 **과적합 제어와 정규화 개념**을 다룬다.

➡️ `05_regularization_and_feature_selection.md`

