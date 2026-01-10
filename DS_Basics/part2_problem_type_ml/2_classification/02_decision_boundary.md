# 02 · Decision Boundary & Tree Split

이 문서는 **Classification Problem에서의 Decision Boundary** 개념을 중심으로, Decision Tree가 어떻게 데이터를 분할(split)하며 학습하는지를 **강의 PDF의 그림·수식·용어** 기준으로 정리한다.

---

## 1️⃣ Decision Boundary란 무엇인가

### 정의

* **Decision Boundary**란

  > 입력 공간(feature space)에서 서로 다른 클래스가 나뉘는 **경계(surface)** 를 의미한다.

* Binary Classification에서는 보통:

  * 2차원: 선(line)
  * 3차원: 면(plane)
  * 고차원: 초평면(hyperplane)

---

### Regression과의 차이

* Regression: 연속적인 값을 예측 → 경계 개념 ❌
* Classification: 범주를 나눔 → **경계가 핵심**

👉 분류 모델은 결국

> **"입력 공간을 어떻게 나눌 것인가"** 의 문제다.

---

## 2️⃣ Decision Tree는 어떻게 공간을 나누는가

### PDF 그림 해석 (의사결정나무 구조)

* Decision Tree는

  * 질문(조건)을 반복하며
  * 입력 공간을 **직사각형 영역**으로 분할함

* 각 분기(node)는:

  * 하나의 feature
  * 하나의 threshold

를 기준으로 데이터를 둘로 나눈다.

👉 결과적으로 Tree의 Decision Boundary는

> **축에 평행한(axis-aligned) 경계들의 조합**

---

## 3️⃣ Split의 기준: 순도(Homogeneity) vs 불순도(Impurity)

Decision Tree의 핵심 목표:

> **같은 클래스끼리 최대한 모으는 것**

이를 수치화한 개념이 바로 **불순도(Impurity)** 이다.

---

## 4️⃣ Gini Index

### 한 줄 정의

* **Gini Index**는

  > 하나의 노드 안에서 **서로 다른 클래스가 섞여 있을 확률**을 수치화한 지표

즉,

> "이 노드가 얼마나 불순한가"를 나타낸다.

---

### 값의 범위와 해석 (중요)

* **최솟값: 0**

  * 한 클래스만 존재 (완전 순수)
* **최댓값: 0.5 (Binary Classification 기준)**

  * 두 클래스가 50:50으로 섞인 상태

👉 일반적으로

* 0에 가까울수록 **좋은 노드**
* 0.5에 가까울수록 **아무 정보가 없는 노드**

---

### 정의 (PDF 수식)

```
I(A) = 1 - Σₖ pₖ²
```

* pₖ: 노드 A 안에서 클래스 k의 비율

---

### 그림 해석 (PDF 예제)

* 파랑 6개, 주황 10개 (총 16개)

```
I(A) = 1 - (6/16)² - (10/16)² ≈ 0.47
```

### 의미

* I(A) = 0 → 완전 순수 (한 클래스만 존재)
* Binary 기준 최대값 ≈ 0.5 → 가장 섞인 상태

👉 **값이 작을수록 좋은 split**

---

## 5️⃣ Entropy

### 한 줄 정의

* **Entropy**는

  > 노드 안의 클래스 분포가 얼마나 **불확실한지**를 나타내는 지표

정보 이론에서 가져온 개념으로,

> "이 노드의 클래스를 예측하는 데 얼마나 헷갈리는가"를 의미한다.

---

### 값의 범위와 해석

* **최솟값: 0**

  * 한 클래스만 존재 (불확실성 없음)
* Binary Classification 기준 최대값:

  * 클래스가 50:50일 때 최대

👉 Entropy 역시

* 값이 작을수록 **좋은 노드**

---

### 정의 (PDF 수식)

```
Entropy = - Σₖ pₖ log(pₖ)
```

---

### 그림 해석

* 클래스가 섞일수록 Entropy ↑
* 한 클래스만 남으면 Entropy = 0

👉 Gini와 Entropy는

> **형태는 다르지만 목적은 동일**

---

## 6️⃣ Information Gain (IG)

### 한 줄 정의

* **Information Gain**은

  > split을 수행함으로써 **불확실성이 얼마나 줄었는지**를 나타내는 값

즉,

> "이 분기가 얼마나 의미 있는 질문인가"를 수치로 표현한 것

---

### 값의 해석

* IG > 0 : split이 의미 있음
[O* IG ≈ 0 : split 전과 후가 거의 동일 (쓸모없는 분기)

👉 Decision Tree는

> **IG가 가장 큰 split만 선택**한다.

---

### 개념적 위치 정리

* Gini / Entropy : 노드의 **상태**를 평가
* Information Gain : split의 **효과**를 평가

---

### PDF Split 그림 해석

PDF에서는 먼저 전체 노드 A의 불순도를 계산하고(예: Gini = 0.47), 어떤 split을 했을 때 좌/우 자식 노드의 불순도를 **가중 평균**으로 합쳐(예: 0.34) 그 차이를 **Information Gain**으로 정의한다.

* Split 전: 0.47
* Split 후(가중 평균): 0.34
* IG = 0.47 − 0.34 = 0.13

---

## 🔎 Deep Dive · Information Gain을 “진짜로” 이해하기

> 한 문장으로: **IG는 “질문 하나를 던졌을 때, 클래스 불확실성이 얼마나 줄었는가”**다.

### 1) 왜 ‘가중 평균’인가?

Split을 하면 데이터가 **왼쪽 노드와 오른쪽 노드로 나뉘는데**, 두 노드의 크기가 보통 다르다.

* 90%가 왼쪽, 10%가 오른쪽인데
* 오른쪽만 엄청 순수해졌다고 해서

그 split이 좋은 질문이라고 말하긴 어렵다.

그래서 split 후 불순도는:

```
Impurity(after)
= (n_left / n_total) * Impurity(left)
+ (n_right / n_total) * Impurity(right)
```

처럼 **노드 크기 비율로 가중 평균**을 낸다.

---

### 2) IG는 무엇을 “좋다”고 판단하나?

```
IG = Impurity(before) - Impurity(after)
```

* before가 크고 after가 작아질수록 IG는 커짐
* 즉, **한 번의 질문으로 ‘섞임’을 크게 줄이는 split**이 높은 IG를 갖는다.

👉 Decision Tree는 매 노드마다

> **IG가 가장 큰 질문(분기)을 골라** 트리를 키운다.

---

### 3) IG는 Gini/Entropy 중 무엇을 쓰냐에 따라 달라지나?

IG의 형태는 같고, 안에 들어가는 Impurity만 바뀐다.

* Gini를 쓰면: *Gini 기반 IG*
* Entropy를 쓰면: *Entropy 기반 IG (흔히 “정보이득”이라고 부르는 형태)*

중요한 건:

> **"IG는 split의 점수", Gini/Entropy는 node의 점수**

라는 역할 분담이다.

---

### 4) IG가 0이면 무슨 뜻인가?

* Split 전과 후의 불순도가 거의 동일
* 즉, 그 질문은

  * 클래스를 나누는 데 도움이 안 되거나
  * 데이터가 해당 feature 기준으로 섞여 있는 것

→ 그래서 IG≈0인 split은 트리에서 거의 선택되지 않는다.

---

### 5) IG가 큰 split이 ‘항상’ 좋은가?

여기서부터가 Decision Tree의 한계로 연결된다.

* 트리는 **각 노드에서 그 순간 가장 좋아 보이는 질문(IG max)을 선택**한다.
* 하지만 이 선택이

  * 전체 트리 구조(미래의 분기)까지 고려한 전역 최적은 아니다.

그래서:

* Greedy split은 현실적으로 필수지만
* 과도하게 진행하면 overfitting으로 이어질 수 있어 → pruning/제어가 필요하다.

(이 내용은 뒤의 overfitting/pruning 섹션과 연결된다.)

---

### 6) (실전 감각) IG 숫자 자체에 ‘절대 기준’이 있을까?

IG는 **데이터 분포, 클래스 비율, 현재 노드의 섞임 정도**에 따라 스케일이 달라져서 “IG는 몇 이상이면 좋다” 같은 절대 기준은 만들기 어렵다.

대신 트리 학습에서는 항상:

> **"같은 노드에서 경쟁하는 후보 split들 중 IG가 가장 큰 것"**

을 고르는 **상대 비교**로 사용한다.

따라서 실전에서는 IG 자체를 외워서 판단하기보다,

* (1) 불순도(before)가 얼마나 컸는지
* (2) split 후 불순도(after)가 얼마나 떨어졌는지
* (3) 그 감소가 ‘큰 노드(데이터 많이 포함)’에서 일어난 것인지

이 3가지를 같이 보는 게 맞다.

### 개념

Split 전후의 불순도 감소량

```
IG = Impurity(before) - Impurity(after)
```

---

### PDF Split 그림 해석

* 전체 불순도: 0.47
* Split 후 가중 평균 불순도: 0.34

```
IG = 0.47 - 0.34 = 0.13
```

👉 **Information Gain이 가장 큰 split을 선택**

---

## 7️⃣ Greedy Split의 의미

### 왜 전역 최적이 아닌가

* 모든 가능한 Tree 구조 탐색 ❌ (계산 불가능)
* 따라서:

  * 현재 노드에서
  * 가장 불순도를 줄이는 split을 선택

👉 이를 **Greedy Algorithm** 이라 한다.

---

## 8️⃣ 100% Purity의 함정 — Overfitting

### PDF 그림 해석

* 무한히 split 하면:

  * Training Error → 0
  * Test Error ↑

👉 **결정 경계가 데이터 노이즈까지 학습**

---

## 9️⃣ Pruning의 필요성

* Full Tree:

  * 모든 terminal node가 순도 100%
* 문제:

  * 일반화 성능 저하

👉 해결:

* Tree 깊이 제한
* Pruning

---

## 🔑 핵심 정리

* Classification = 공간 분할 문제
* Decision Tree = 축에 평행한 분할의 조합
* Gini / Entropy = 불순도 측정 도구
* Information Gain = split 선택 기준
* Greedy split + 과도한 분할 → Overfitting

다음 문서에서는, 👉 단일 Tree의 한계를 극복하기 위한 **Ensemble 모델(Random Forest, Boosting)** 을 다룬다.

➡️ `03_modeling_and_interpretation.md`

