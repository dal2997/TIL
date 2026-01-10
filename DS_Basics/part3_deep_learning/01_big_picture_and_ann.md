# 01. Big Picture & ANN (Artificial Neural Network)

> 이 파일은 **딥러닝의 가장 큰 그림**과  
> **“인공신경망이 도대체 뭐냐?”** 를 직관적으로 정리한다.

---

## 🎯 이 장의 목표

- 딥러닝을 **모델 = 함수** 관점에서 이해한다
- ANN / MLP / DNN 용어를 정확히 구분한다
- “학습이란 무엇인가?”의 실체를 감 잡는다
- **Universal Approximation Theorem**의 의미를 이해한다

---

## 1️⃣ AI / ML / DL 관계

- AI (Artificial Intelligence): 인간의 지능을 흉내내는 모든 시도
- ML (Machine Learning): **데이터로부터 규칙을 학습**
- DL (Deep Learning): **인공신경망을 깊게 쌓아서 학습**

관계:

~~~text
AI ⊃ ML ⊃ DL
~~~

- 선형회귀, 결정트리, SVM → ML
- CNN, RNN, Transformer → DL

---

## 2️⃣ 인공신경망(ANN)의 본질

### 핵심:

> 인공신경망은 그냥 **함수다**.

~~~text
입력 x → (행렬곱 + bias + activation) → 출력 y
~~~

이 연산을 여러 번 반복하는 구조 = 신경망

각 노드에서 하는 일:

~~~text
y = activation( w1*x1 + w2*x2 + ... + b )
~~~

- w: weight (중요도)
- b: bias (민감도)
- activation: 비선형성 추가

---

## 3️⃣ Perceptron vs MLP vs DNN

### 🔹 Perceptron
- activation = **unit step function**
- 이진 분류 가능
- 단점: **미분 불가 → gradient descent 불가**

### 🔹 MLP (Multi-Layer Perceptron)
- 여러 layer를 가진 신경망
- activation: sigmoid, relu, tanh 등 **아무거나 가능**
- 오늘날 우리가 말하는 **기본적인 FC 신경망**

### 🔹 DNN (Deep Neural Network)
- **hidden layer가 많은 MLP**
- 깊기 때문에 표현력이 좋아짐

정리:

~~~text
Perceptron ⊂ MLP ⊂ DNN
~~~

---

## 4️⃣ 인공신경망은 무엇을 학습하는가?

> 신경망은 **구조를 학습하는 게 아니라, weight를 학습한다.**

- 내가 정하는 것:
  - layer 구조
  - 노드 수
  - activation 종류

- 신경망이 스스로 찾는 것:
  - weight
  - bias

---

## 5️⃣ 학습이란 무엇인가?

> 학습 = **원하는 출력이 나오도록 weight를 조정하는 과정**

1. 입력 넣는다
2. 출력 나온다
3. 정답이랑 비교한다 → loss 계산
4. loss 줄어드는 방향으로 weight 조금 움직인다
5. 반복

---

## 6️⃣ 신경망은 결국 “함수”다

> 신경망이 하는 일:

~~~text
f(x) ≈ y
~~~

- f(x)는 매우 복잡한 함수
- weight, bias는 **이 함수의 모양을 바꾸는 손잡이**

---

## 7️⃣ Universal Approximation Theorem

> ❗ Hidden layer **1개만 있어도**  
> 노드 수가 충분하면 **어떤 연속함수든 근사 가능**

의미:

- 이론적으로:
  - “모델 표현력은 충분하다”
- 현실적으로:
  - ❌ 그렇게 학습된다는 보장은 없음
  - ❌ 효율도 안 좋음
  - 그래서 **깊게 쌓는다 (Deep)**

---

## 8️⃣ 그러면 왜 깊게 쌓나?

- 같은 함수를:
  - 얕고 넓게 만들 수도 있고
  - 깊고 좁게 만들 수도 있다

보통:

> **깊게 쌓는 쪽이 파라미터 효율 + 표현력 분해 측면에서 유리**

---

## 9️⃣ 중요한 관점 정리

> 딥러닝에서 제일 중요한 관점:

- 신경망 = 함수
- 학습 = 함수 모양을 loss 기준으로 조금씩 바꾸는 과정
- weight = 함수의 손잡이
- 구조 = “어떤 함수 꼴을 쓸지”에 대한 **사전 가정**

---

## 🧠 한 줄 요약

> 인공신경망은  
> **“아무 함수나 흉내낼 수 있는 초거대 함수”이고,  
> 학습이란 그 함수 모양을 loss 기준으로 다듬는 과정이다.**

---

## ✅ 다음 파일 예고

👉 `02_training_and_optimization.md` 에서 다룰 내용:

- Loss
- Gradient Descent / SGD / Adam
- Backpropagation
- Hyperparameter 개념

