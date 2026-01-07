# 03 · Modeling & Interpretation (Tree → Ensemble)

이 문서는 **Decision Tree 단일 모델의 한계**를 출발점으로,
왜 Ensemble이 필요한지, 그리고 **Random Forest와 Boosting 계열(AdaBoost / GBM)** 이
어떤 문제를 각각 해결하려고 등장했는지를 **개념·구조·강의 PDF 기준**으로 정리한다.

> 목표
>
> * 모델 이름이 아니라 **해결하려는 문제(Bias / Variance)** 로 알고리즘을 구분
> * Random Forest vs AdaBoost를 “언제 무엇이 더 적합한가” 기준으로 설명 가능

---

## 1️⃣ Single Decision Tree의 구조적 한계

### (1) 장점

* 규칙 기반 → 해석력 매우 높음
* 비선형 결정 경계 표현 가능
* Feature scaling 불필요

### (2) 한계 (강의 핵심)

* **Variance가 큼**

  * 데이터가 조금만 바뀌어도
  * split 구조가 크게 달라짐
* Greedy split 특성

  * 각 단계에서의 최선이
  * 전체 구조의 최선은 아님

👉 이 한계를 해결하려는 방향이 **Ensemble**이다.

---

## 2️⃣ Ensemble의 기본 아이디어 (PDF 흐름)

> 여러 개의 약한 모델을 조합하면
> 하나의 불안정한 모델보다 더 안정적인 예측이 가능하다.

핵심 질문:

* **무엇을 줄이려는가?**

  * Variance?
  * Bias?

이 질문에 따라 Ensemble 방식이 갈린다.

---

## 3️⃣ Bagging → Random Forest

### 3-1. Bagging (Bootstrap Aggregating)

#### 개념

* 원본 데이터에서
* **복원 추출(bootstrap)** 로 여러 데이터셋 생성
* 각 데이터셋으로 모델을 따로 학습
* 결과를 평균/다수결로 결합

👉 목적: **Variance 감소**

---

### 3-2. 왜 Bootstrap인가?

* Tree는 Noise에 매우 민감
* 서로 다른 데이터로 학습시켜야
  → Tree 간 상관성 ↓

복원 추출을 쓰는 이유:

* 같은 데이터가

  * 어떤 Tree에는 들어가고
  * 어떤 Tree에는 빠지게 함

→ **의도적으로 차이를 만든다**

---

### 3-3. Random Forest의 추가 랜덤성

Random Forest는 Bagging + α

* 데이터 샘플링: bootstrap
* Feature 샘플링: split마다 랜덤 feature subset

👉 Tree 간 상관성을 더 줄임

---

## 4️⃣ OOB (Out-of-Bag) Error

### 개념

* Bootstrap 시

  * 평균적으로 약 **36.8%** 데이터는
  * 특정 Tree의 학습에 사용되지 않음

이 데이터를 해당 Tree의

* **검증 데이터처럼 사용** → OOB Error

---

### 의미

* 별도의 validation set 불필요
* Random Forest의 내부 성능 추정 지표

---

## 5️⃣ Random Forest Feature Importance

### 원리 (강의 기준)

1. 원래 데이터로 OOB error 계산
2. 특정 feature 값을 무작위로 섞음(permutation)
3. OOB error 증가량 측정

👉 error를 많이 증가시키는 feature일수록 중요

---

## 6️⃣ Boosting의 철학적 출발

### 핵심 아이디어

> **이전 모델이 틀린 데이터에 집중하자**

* Bagging: 모델들을 **독립적으로** 학습
* Boosting: 모델들을 **순차적으로** 학습

👉 목적: **Bias 감소**

---

## 7️⃣ AdaBoost (Adaptive Boosting)

### 구조적 특징

* Weak learner (보통 depth=1 tree, stump)
* 각 데이터 포인트에 **가중치** 부여
* 이전 단계에서 틀린 데이터의 가중치 ↑

---

### 언제 AdaBoost가 유리한가 (중요)

AdaBoost는 다음 조건에서 강함:

* 데이터 노이즈가 **적을 때**
* 결정 경계가

  * 단순하지만
  * 약간 비선형일 때
* **약한 규칙들의 조합**이 효과적일 때

👉 작은 Bias를 빠르게 줄이는 데 강점

---

### 언제 AdaBoost가 불리한가

* 노이즈 / 이상치가 많을 때

  * 틀린 샘플에 집착 → 과적합
* 라벨 오류가 많은 데이터

  * 의료 데이터 등

---

## 8️⃣ Random Forest vs AdaBoost (정리)

| 관점         | Random Forest | AdaBoost   |
| ---------- | ------------- | ---------- |
| 주된 목적      | Variance 감소   | Bias 감소    |
| 학습 방식      | 병렬            | 순차         |
| 노이즈 민감도    | 낮음            | 높음         |
| 기본 learner | 깊은 Tree       | 매우 얕은 Tree |
| 안정성        | 매우 안정적        | 데이터 의존적    |

---

## 9️⃣ Gradient Boosting Machine (GBM)

### AdaBoost의 확장

* Error 자체가 아니라
* **Loss의 Gradient(Residual)** 를 학습

👉 회귀/분류 모두 자연스럽게 확장

---

### Overfitting 제어 기법 (강의 기준)

* Shrinkage (Learning Rate)
* Subsampling
* Early Stopping

※ Subsampling은 **복원 추출이 아님**
→ Bootstrap과의 차이는 다음 챕터에서 지표와 함께 정리

---

## 🔑 핵심 요약

* Single Tree는 해석력은 좋지만 불안정
* Random Forest: Variance 문제 해결
* AdaBoost / GBM: Bias 문제 해결
* 모델 선택 기준은

  > **데이터 특성과 무엇을 줄이고 싶은가**

다음 문서에서는,
👉 이 모델들을 **어떻게 평가할 것인가**,
즉 **Confusion Matrix와 평가 지표**를 다룬다.

➡️ `04_evaluation_metrics.md`

