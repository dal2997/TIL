# 05 · Regularization & Feature Selection (Classification)

이 문서는 **Classification 모델에서의 과적합(Overfitting)을 어떻게 제어할 것인가**를 다룬다.
특히 Decision Tree 및 Boosting 계열 모델에서 **왜 과적합이 쉽게 발생하는지**,
그리고 이를 막기 위해 **어떤 제어 장치들이 설계되었는지**를 강의 PDF 흐름 기준으로 정리한다.

> 목표
>
> * "모델 성능 향상 = 복잡도 증가"가 아님을 명확히 이해
> * 각 규제 기법이 **어떤 실패를 막기 위해 존재하는지** 설명 가능

---

## 1️⃣ Overfitting은 왜 발생하는가 (Classification 관점)

### 핵심 원인

* Classification은 **공간 분할 문제**
* Tree 계열 모델은

  * split을 계속 추가할수록
  * decision boundary가 점점 복잡해짐

👉 결과:

* Training data에는 완벽히 맞음
* Test data에서는 성능 붕괴

---

### PDF 그림 해석 (Training vs Test Error)

* Split 수 ↑ → Training Error ↓
* 일정 지점 이후 → Test Error ↑

👉 **복잡도와 일반화 성능 사이의 trade-off**

---

## 2️⃣ Decision Tree Pruning

### Full Tree란?

* 모든 terminal node의 impurity = 0
* Training error = 0

하지만:

* Noise까지 학습
* 일반화 성능 악화

---

### Pruning의 목적

> "필요 이상으로 복잡해진 분기를 잘라낸다"

즉,

* 약간의 training error 증가를 허용하고
* test error를 줄이는 전략

---

### 주요 Pruning 기준 (개념)

* 최대 깊이 제한 (max_depth)
* 최소 샘플 수 (min_samples_split, min_samples_leaf)
* 불순도 감소 최소 기준

👉 모두 **split을 멈추기 위한 조건**

---

## 3️⃣ Boosting 계열에서의 과적합 문제

### 왜 Boosting은 더 위험한가

* 모델들이 **순차적으로 연결**됨
* 이전 모델의 오차에 집중

👉 잘못된 패턴까지 강화될 위험

---

## 4️⃣ Shrinkage (Learning Rate)

### 개념

* 각 Tree의 기여도를 **조금씩만 반영**

```
F_m(x) = F_{m-1}(x) + η · h_m(x)
```

* η (learning rate): 0 < η ≤ 1

---

### 왜 효과적인가

* 한 번에 큰 수정 ❌
* 여러 번 작은 수정 ⭕

👉 **과적합 억제 + 안정적 수렴**

---

## 5️⃣ Subsampling (≠ Bootstrap)

### 정의

* 매 iteration마다
* 전체 데이터 중 일부만 **비복원 추출**

---

### Bootstrap과의 차이 (중요)

| 항목    | Bootstrap     | Subsampling |
| ----- | ------------- | ----------- |
| 추출 방식 | 복원            | 비복원         |
| 목적    | Tree 간 비상관화   | 과적합 완화      |
| 사용 모델 | Random Forest | GBM         |

👉 Subsampling은

> **Boosting의 공격성을 낮추는 장치**

---

## 6️⃣ Early Stopping

### 개념

* Validation error를 모니터링
* 성능이 악화되기 시작하면 학습 중단

---

### 의미

* "끝까지 학습 = 최적"이라는 가정 제거
* 데이터가 알려주는 시점에서 멈춤

---

## 7️⃣ Feature Selection과의 연결

### 왜 Feature Selection이 필요한가

* 불필요한 feature ↑
* 불필요한 split ↑

👉 모델 복잡도 증가

---

### Tree 기반 모델의 특징

* 내부적으로 feature 선택 수행
* 하지만:

  * 노이즈 feature도 split 후보가 될 수 있음

👉 사전 Feature Selection / 규제는 여전히 의미 있음

---

## 🔑 핵심 정리

* Overfitting은 Tree 계열 모델의 구조적 문제
* Pruning은 Tree의 복잡도를 직접 제어
* Boosting에서는

  * Shrinkage
  * Subsampling
  * Early Stopping

이 **세 가지가 핵심 안전장치**

---

## 📌 Chapter02 전체 요약

* Classification = 공간 분할 문제
* Tree는 강력하지만 불안정
* Ensemble은 이 불안정을 줄이기 위한 설계
* 규제는 성능을 낮추는 게 아니라

  > **성능 붕괴를 막는 장치**

이로써 **Classification 챕터의 개념 흐름은 완결**된다.

