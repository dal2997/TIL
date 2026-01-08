# 03. Classification & Loss

> 이 파일은 **분류 문제에서 왜 loss를 그렇게 정의하는지**를 정리한다.  
> 핵심은: **“모델의 출력이 무엇을 의미하느냐에 따라 loss가 결정된다.”**

---

## 🎯 이 장의 목표

- Perceptron → Sigmoid → Softmax 흐름 이해
- Logistic Regression의 정체 이해
- Cross Entropy의 의미 이해
- MSE vs Log-likelihood 차이 이해

---

## 1️⃣ Perceptron: 가장 원시적인 분류기

- activation: **unit step function**
- 출력:
  - 0 또는 1
- 장점:
  - 단순한 이진 분류 가능
- 치명적 단점:
  - ❌ **미분 불가능 → Gradient Descent 사용 불가**
  - ❌ 너무 딱딱한 분류

---

## 2️⃣ Sigmoid: 부드러운 이진 분류

> Unit step을 **부드럽게 만든 함수**

~~~text
σ(x) = 1 / (1 + e^(-x))
~~~

- 출력:
  - 0 ~ 1
- 해석:
  - **확률**

---

## 3️⃣ Logistic Regression의 정체

> Logistic Regression =  
> **“Linear model + Sigmoid”**

~~~text
q = σ(w^T x + b)
~~~

- q = "양성일 확률"
- 그런데 문제:

> ❓ Loss를 MSE로 쓰면 안 되나?

---

## 4️⃣ 왜 MSE가 아니라 Log-likelihood인가?

### 🔹 MSE의 문제

- 정답이 1인데:
  - q = 0 이면
  - MSE = 1
- 근데:
  - 사실 이건 **거의 틀린 정도가 무한대에 가까움**

### 🔹 Log-likelihood (Binary Cross Entropy)

~~~text
L = - [ p log(q) + (1 - p) log(1 - q) ]
~~~

- p = 정답 (0 또는 1)
- q = 예측 확률

특징:

- 틀리면:
  - loss → ∞
- 확률 모델에 **훨씬 더 자연스러운 loss**

---

## 5️⃣ 확률 모델 관점

> Logistic Regression은:

- 출력을:
  - **베르누이 분포의 확률**로 해석
- 학습:
  - **Maximum Likelihood Estimation (MLE)**

---

## 6️⃣ 다중 분류: Softmax

> 클래스가 3개 이상이면?

- 출력 노드:
  - 클래스 개수만큼
- 정답:
  - **one-hot vector**

~~~text
p = [1, 0, 0]  # class 0
p = [0, 1, 0]  # class 1
p = [0, 0, 1]  # class 2
~~~

### Softmax:

~~~text
q_i = exp(z_i) / Σ_j exp(z_j)
~~~

- 출력:
  - 합이 1인 확률 분포

---

## 7️⃣ Cross Entropy Loss

~~~text
L = - Σ_i p_i log(q_i)
~~~

- 정답이 one-hot이면:

~~~text
L = - log(q_true_class)
~~~

해석:

> "정답 클래스 확률을 최대화하자"

---

## 8️⃣ Softmax Regression의 정체

> Softmax Regression =  
> **“Linear model + Softmax + Cross Entropy”**

즉:

- Logistic Regression의:
  - 다중 클래스 버전

---

## 9️⃣ MSE vs Log-likelihood 정리

| 문제 | 출력 해석 | Loss |
|------|-----------|-------|
| 회귀 | 실수값 | MSE |
| 이진 분류 | 확률 | Binary Cross Entropy |
| 다중 분류 | 확률 분포 | Categorical Cross Entropy |

---

## 🔟 중요한 관점 정리

> ❗ Loss는 **"수학적으로 예쁜 것"이 아니라**  
> **"모델 출력의 의미와 맞아야" 한다.**

---

## 🧠 이 장의 핵심 요약

- 분류 문제의 출력 = **확률**
- 확률 모델의 학습 = **Likelihood 최대화**
- Cross Entropy = **-log(likelihood)**

---

## 🏁 한 줄 요약

> 분류 문제에서 loss는  
> **“정답 클래스의 확률을 최대화하도록 설계된 함수”** 여야 한다.

