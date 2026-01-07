# 01 · Problem Definition (Classification)

이 문서는 **Classification Problem(분류 문제)** 를 Regression과 대비하여 정의하고,
왜 분류에서는 **확률 기반 예측 + 전용 손실 함수**가 필요한지를 논리적으로 정리한다.

---

## 1️⃣ Classification vs Regression — 문제 정의의 차이

### Regression

* Target(Y): **연속값 (continuous)**
* 관측 가능한 정보:

  * 예측값과 실제값의 **차이(Error)**
* 핵심 질문:

  > "얼마나 틀렸는가?"

대표 손실 함수:

* MSE (Mean Squared Error)

---

### Classification

* Target(Y): **범주형 (categorical)**
* 관측 가능한 정보:

  * **맞음 / 틀림** (0 or 1)
* 핵심 질문:

  > "맞았는가, 틀렸는가?"

⚠️ 여기서 중요한 차이:

* 분류에서는 **오차의 크기 자체가 정의되지 않는다**
* 즉, Regression처럼 단순한 거리 기반 손실을 쓰기 어렵다

---

## 2️⃣ 왜 Regression Loss(MSE)를 그대로 쓰면 안 되는가

가정:

* Binary Classification (Y ∈ {0, 1})
* 예측값을 실수로 출력한다고 가정

문제점:

1. 0.9와 0.6의 예측은 **모두 맞춘 것**이지만
   → MSE는 서로 다른 손실을 부여함
2. 0.49와 0.01은 **둘 다 틀린 예측**이지만
   → 손실 구조가 분류의 목적과 어긋남

👉 분류 문제의 본질은:

> **"정답 클래스에 얼마나 확신을 가지고 있는가"**

즉, **확률적 해석**이 필요하다.

---

## 3️⃣ Classification은 왜 확률 예측이 필요한가

분류 모델은 최종적으로

```
X → f(X) → P(Y = 1 | X)
```

를 학습한다.

* 모델의 출력은 **확률 (0 ~ 1)**
* 실제 클래스 결정은 threshold를 통해 수행

```
P(Y=1|X) ≥ 0.5 → class 1
P(Y=1|X) < 0.5 → class 0
```

이렇게 해야:

* 예측의 **불확실성**을 표현할 수 있고
* 다양한 손실 함수 / 평가 지표와 연결 가능

---

## 4️⃣ Binary Cross Entropy (BCE)

### 정의

Binary Classification에서 가장 기본적으로 사용하는 손실 함수

```
BCE = - [ y · log(p) + (1 - y) · log(1 - p) ]
```

* y ∈ {0, 1} : 실제 레이블
* p ∈ (0, 1) : 모델이 예측한 확률

---

### 직관적 해석

#### Case 1: 정답이 1일 때 (y = 1)

```
Loss = -log(p)
```

* p → 1  → loss ↓ (잘한 예측)
* p → 0  → loss → ∞ (치명적 오답)

#### Case 2: 정답이 0일 때 (y = 0)

```
Loss = -log(1 - p)
```

* p → 0  → loss ↓
* p → 1  → loss → ∞

👉 **확신을 가지고 틀릴수록 더 크게 패널티**

---

## 5️⃣ BCE가 분류에 적합한 이유

1. 확률 기반 해석 가능
2. 미분 가능 → Gradient Descent 적용 가능
3. 확신 있는 오답을 강하게 벌점
4. Logistic Regression, Neural Network와 자연스럽게 연결

즉,

> **분류 문제의 목적과 손실 함수의 구조가 일치**

---

## 6️⃣ 핵심 요약

* 분류 문제는 "얼마나 틀렸는가"가 아닌
  **"얼마나 확신 있게 맞췄는가"** 의 문제
* 따라서 거리 기반 손실(MSE)은 부적합
* 확률 예측 + Cross Entropy가 표준

다음 문서에서는,
👉 이 확률 예측이 **결정 경계(Decision Boundary)** 로
어떻게 이어지는지를 다룬다.

---

## 📎 PDF 기준 보강 포인트 (강의 내용 반영)

본 문서는 강의 PDF *ClassificationProblem*의 **Loss Function 설명 파트(초반부)** 를 기준으로 재구성되었다.

### 📌 강의 핵심 메시지 요약

* Regression Loss는 **Error의 크기**를 직접 측정할 수 있음
* Classification은 결과가 **옳음 / 그름**으로만 관측됨
* 따라서 분류에서는:

  * 거리 기반 Loss ❌
  * 확률 기반 Loss ⭕

---

### 🖼️ PDF 그림이 의미하는 바

* **로그 곡선 그래프**

  * x축: 예측 확률 p
  * y축: loss
  * 정답 클래스에 대해 확률이 0 또는 1에 가까워질수록
    → loss가 급격히 증가

👉 이는 단순 오답보다,

> **"확신을 가지고 틀리는 예측"을 강하게 벌점**하기 위함

---

### 🧠 강의 관점에서의 핵심 용어 정리

* **Binary Cross Entropy**

  * 분류 문제의 표준 손실 함수
  * Logistic Regression, Neural Network의 기본 Loss

* **확률 예측**

  * 단순 class 출력이 아니라
  * 모델의 불확실성을 수치로 표현

* **Threshold**

  * 확률 → class로 변환하는 기준
  * 이후 Precision / Recall과 직접적으로 연결됨

---

### 🎯 강의에서 강조한 분류 문제의 본질

> "Classification은 수치를 맞추는 문제가 아니라
> **확률 분포를 얼마나 잘 모델링했는가**의 문제다"

이 관점이 이후

* Decision Tree의 Impurity
* Ensemble의 필요성
* Boosting의 방향성

으로 자연스럽게 이어진다.

---

다음 문서에서는,
👉 이 확률 기반 예측이 **어떻게 공간을 나누는가**,
즉 **결정 경계(Decision Boundary)** 개념으로 확장된다.

➡️ `02_decision_boundary.md`

