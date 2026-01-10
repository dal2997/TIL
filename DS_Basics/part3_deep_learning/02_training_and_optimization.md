# 02. Training & Optimization

> 이 파일은 **“신경망이 어떻게 학습되는가?”** 를 다룬다.  
> 즉, **loss를 줄이기 위해 weight를 어떻게 업데이트하는지**의 전체 구조를 정리한다.

---

## 🎯 이 장의 목표

- Loss의 의미를 정확히 이해한다
- Gradient Descent의 직관을 잡는다
- SGD / Mini-batch / Adam의 차이를 이해한다
- Backpropagation이 왜 필요한지 이해한다
- Parameter vs Hyperparameter를 구분한다

---

## 1️⃣ Loss란 무엇인가?

> Loss = **“얼마나 틀렸는가”를 수치로 나타낸 것**

~~~text
Loss = L(y_true, y_pred)
~~~

- Loss가 작을수록:
  - 모델이 정답에 가깝다
- 학습의 목표:
  - **Loss를 최소화하는 weight 찾기**

---

## 2️⃣ 학습 문제의 본질

> 결국 이 문제다:

~~~text
min_w L(w)
~~~

- w = 모든 weight, bias
- 우리는:
  - L이 가장 작아지는 w를 찾고 싶다

---

## 3️⃣ Gradient Descent (GD)

> 아이디어:

- 지금 위치에서
- **Loss가 가장 가파르게 내려가는 방향**으로
- **조금 이동**

~~~text
w = w - α * ∇L(w)
~~~

- α (learning rate) = 보폭
- ∇L = 기울기(gradient)

직관:

> "산에서 가장 가파른 내리막 방향으로 조금씩 내려간다"

---

## 4️⃣ Learning Rate의 의미

- 너무 크면:
  - ❌ 튄다, 발산한다
- 너무 작으면:
  - ❌ 너무 느리다

그래서:

- 고정값 쓰기도 하고
- **스케줄링** 하기도 함

---

## 5️⃣ Batch, Epoch, Iteration

- Batch size:
  - 한 번에 몇 개 데이터 볼 거냐
- Iteration:
  - weight 한 번 업데이트
- Epoch:
  - **전체 데이터 1번 다 본 것**

---

## 6️⃣ GD vs SGD vs Mini-batch

### 🔹 GD (Batch GD)
- 전체 데이터로 gradient 계산
- 안정적이지만:
  - ❌ 느림
  - ❌ local minimum 탈출 힘듦

### 🔹 SGD
- 데이터 **1개**로 gradient 계산
- 빠르지만:
  - ❌ 방향이 매우 noisy

### 🔹 Mini-batch SGD
- 적당히 묶어서 계산
- ✅ 현재 딥러닝의 표준

---

## 7️⃣ Momentum

> 아이디어: **관성**

- 이전 방향을 조금 기억한다
- 골짜기에서 좌우로 흔들리는 걸 줄임

---

## 8️⃣ RMSProp

> 아이디어: **좌표별로 보폭 조절**

- 많이 흔들린 축: 보폭 줄임
- 덜 흔들린 축: 보폭 키움

---

## 9️⃣ Adam (Adaptive Moment Estimation)

> Adam = **Momentum + RMSProp**

- 방향:
  - 관성 사용
- 보폭:
  - 좌표별 자동 조절

그래서:

> ✅ 실전에서 거의 default optimizer

---

## 🔟 Weight Initialization

왜 중요?

- 너무 크면:
  - activation 터짐
- 너무 작으면:
  - gradient 사라짐

대표적인 방법:

- Xavier (sigmoid, tanh)
- He (ReLU)

---

## 1️⃣1️⃣ Backpropagation

> Backpropagation = **미분값을 뒤에서부터 전달하는 알고리즘**

- chain rule 사용
- 왜 필요?
  - 앞쪽 layer weight가
  - **출력 loss에 얼마나 기여했는지** 알아야 하니까

직관:

> "이 실수의 책임이 각 단계에 얼마나 있나?" 를 계산하는 과정

---

## 1️⃣2️⃣ Parameter vs Hyperparameter

### 🔹 Parameter (모델이 학습)
- weight
- bias

### 🔹 Hyperparameter (사람이 정함)
- learning rate
- batch size
- epoch
- layer 수
- hidden size
- optimizer 종류

---

#

