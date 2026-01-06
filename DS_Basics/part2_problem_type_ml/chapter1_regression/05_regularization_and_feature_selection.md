# 05_regularization_and_feature_selection.md
> Regression Problem — Feature Selection & Regularization (Ridge / Lasso / ElasticNet)

---

## 0️⃣ 이 파일의 목표

> **“왜 Regularization이 등장했는지”를  
> Feature Selection의 한계(특히 시간 폭발)에서 출발해  
> Ridge/Lasso/ElasticNet을 선택할 수 있게 만든다.**

이 파일을 이해하면:
- “변수가 많을수록 왜 망하는지” 설명 가능
- “왜 벌점(Penalty)을 주는지” 철학이 잡힘
- Ridge/Lasso/ElasticNet을 상황별로 고를 수 있음

---

## 1️⃣ 출발: Feature가 많아지면 왜 문제인가?

### 1-1. Overfitting은 ‘모델’만의 문제가 아니다

> ✅ Feature 수(p)가 커지면, 모델은 더 많은 “자유도”를 얻는다.  
> 그 자유도가 **정답의 패턴**이 아니라 **노이즈까지 외우는 능력**이 되기도 한다.

즉,

- p ↑ → 모델 복잡도 ↑
- Bias ↓ (좋아 보임)
- Variance ↑ (테스트 성능 망함)

---

## 2️⃣ Feature Selection이란?

> 많은 변수 중에서 “쓸 변수만 남겨서”  
> 복잡도를 줄이는 방법

---

## 3️⃣ 전통적 Feature Selection 방법들 (그리고 33쪽 포인트)

### 3-1. Exhaustive Search (완전탐색) — “33쪽의 핵심”

> 가능한 모든 변수 조합을 다 해본다.

변수가 p개면 가능한 조합 수:

$$
2^p - 1
$$

#### 왜 폭발하나? (감각으로 보기)

| p | 조합 수 $2^p-1$ | 느낌 |
|---:|---:|---|
| 10 | 1,023 | 해볼 만 |
| 20 | 1,048,575 | 이미 빡셈 |
| 30 | 1,073,741,823 | 사실상 불가능 |
| 50 | 1,125,899,906,842,623 | 끝 |

> ✅ p가 조금만 커져도 **지수적으로** 늘어난다.  
> 이것이 “시간 폭발(Exponential)”이다.

---

### 3-2. Forward Selection (전진 선택)

- 변수 없이 시작 → 하나씩 추가
- 성능 개선 없으면 STOP
- 한번 들어온 변수는 다시 제거하지 않음

장점: 완전탐색보다 빠름  
단점: “나중에 보니 빼는 게 맞는 변수”를 못 뺌

---

### 3-3. Backward Elimination (후진 제거)

- 모든 변수로 시작 → 하나씩 제거
- 한번 제거한 변수는 다시 못 넣음

장점: 시작은 풍부  
단점: 조합 최적을 보장 못 함

---

### 3-4. Stepwise Selection (혼합)

- Forward와 Backward를 번갈아

장점: 더 좋은 조합 가능  
단점: 여전히 계산/불안정 문제

---

## 4️⃣ 결론: Feature Selection의 현실적 한계

> ✅ 변수 수가 커질수록  
> “조합을 찾아보는 방식”은 가성비가 급격히 떨어진다.

그래서 등장한 발상:

> **모델이 학습하면서 스스로 중요하지 않은 변수를 약하게 만들면 어떨까?**

이게 바로 **Regularization (벌점 주기)**

---

## 5️⃣ Penalty Term: “벌점”이라는 철학

### 5-1. 핵심 아이디어

> 오차를 줄이되,  
> **계수(β)가 너무 커지거나 복잡해지는 것을 벌점으로 막자**

즉, 목표 함수가 바뀐다:

$$
\text{Loss}(\beta) + \lambda \cdot \text{Penalty}(\beta)
$$

- Loss: 데이터에 잘 맞추기
- Penalty: “쓸데없는 복잡함”에 벌점
- $\lambda$: 벌점의 세기(튜닝 대상)

---

## 6️⃣ Ridge Regression (L2 Regularization)

### 6-1. 정의

$$
\min_{\beta}\;\sum_{i=1}^{N}(y_i-\hat{y}_i)^2 \;+\; \lambda \sum_{j=1}^{p}\beta_j^2
$$

- 벌점: $\sum \beta_j^2$ (제곱합)
- 큰 계수를 강하게 눌러서 “완만한 모델”로 만든다

### 6-2. 특징 요약

| 특징 | Ridge |
|---|---|
| Feature Selection | ❌ (계수가 0이 되기 어렵다) |
| 계수 축소 | ⭕ (0에 가깝게 줄임) |
| 다중공선성 | ⭕ 강하게 도움 |
| 최적화 | 미분 가능 → 해 찾기 상대적으로 쉬움 |

### 6-3. 언제 쓰나?
- 변수들이 서로 강하게 상관될 때(다중공선성)
- “변수 제거”보다는 “안정적인 예측”이 목표일 때

---

## 7️⃣ Lasso Regression (L1 Regularization)

### 7-1. 정의

$$
\min_{\beta}\;\sum_{i=1}^{N}(y_i-\hat{y}_i)^2 \;+\; \lambda \sum_{j=1}^{p}|\beta_j|
$$

- 벌점: $\sum |\beta_j|$ (절댓값 합)

### 7-2. 핵심 차이: Feature Selection이 된다

> Lasso는 일부 계수를 **정확히 0**으로 만들 수 있다.  
> → 변수 선택 효과

| 특징 | Lasso |
|---|---|
| Feature Selection | ⭕ (0으로 떨어짐) |
| 계수 축소 | ⭕ |
| 다중공선성 | ⚠️ 상관 변수 중 일부만 선택(불안정할 수 있음) |
| 최적화 | 절댓값 → 미분 불가 → 수치 최적화 필요 |

### 7-3. 언제 쓰나?
- 변수 수가 많고, **변수 선택이 필요**할 때
- 설명 가능한 모델(적은 변수)로 가고 싶을 때

---

## 8️⃣ ElasticNet (L1 + L2)

### 8-1. 정의

$$
\min_{\beta}\;\sum_{i=1}^{N}(y_i-\hat{y}_i)^2
\;+\; \lambda_1 \sum_{j=1}^{p}|\beta_j|
\;+\; \lambda_2 \sum_{j=1}^{p}\beta_j^2
$$

- L1: 변수 선택
- L2: 안정성(다중공선성 완화)

### 8-2. 특징

> 상관이 강한 변수들이 있을 때  
> “한 놈만 뽑고 버리는” Lasso의 불안정성을 줄여준다.

| 특징 | ElasticNet |
|---|---|
| Feature Selection | ⭕ |
| 안정성 | ⭕ (L2로 보완) |
| 튜닝 | λ가 2개라 실험이 더 필요 |

---

## 9️⃣ Ridge vs Lasso vs ElasticNet: 선택 가이드 (현업용)

| 상황 | 추천 | 이유 |
|---|---|---|
| 다중공선성 강함 | Ridge / ElasticNet | 계수 안정화 |
| 변수 선택이 중요 | Lasso / ElasticNet | 0으로 떨어뜨림 |
| 상관 변수 묶음 존재 | ElasticNet | 그룹 성질 반영 |
| 예측 안정성이 최우선 | Ridge | 흔들림 적음 |

---

## 10️⃣ Regularization의 “부작용”도 알아야 한다

> ✅ 벌점을 주면 **Training 성능은 일부러 나빠질 수 있다.**  
하지만 목적은:

> **Test 성능(일반화 성능)을 올리는 것**

즉, “덜 외우게 만들어서” 실전에서 더 강해지게 한다.

---

## 11️⃣ 반드시 기억할 실전 원칙 1개

> ✅ Regularization을 쓸 때는  
> **Scaling(표준화)이 거의 필수**인 경우가 많다.

왜냐하면:
- 벌점은 β에 걸리는데
- X의 스케일이 다르면 β 스케일도 흔들린다

그래서 보통:

- StandardScaler + Ridge/Lasso/EN

---

## 12️⃣ 이 챕터(Regression) 전체 흐름 다시 연결

~~~text
문제 정의(01)
  -> Loss(02)
    -> 해석(03)
      -> 평가(04)
        -> Feature Selection 한계(Exponential)
          -> Penalty(Regularization)
            -> Ridge / Lasso / ElasticNet(05)
~~~

---

## 🔁 10초 복습 체크리스트

- [ ] 완전탐색 조합 수는 2^p - 1 로 폭발한다
- [ ] Forward/Backward/Stepwise는 최적 보장 어렵다
- [ ] Regularization = Loss + λ*Penalty
- [ ] Ridge(L2): 안정화 / 다중공선성 / 0은 잘 안 됨
- [ ] Lasso(L1): 변수 선택 / 0 가능 / 상관 변수에 불안정
- [ ] ElasticNet: L1+L2로 둘 다 노림
- [ ] Scaling은 거의 필수다

