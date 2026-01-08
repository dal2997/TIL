# 🧠 Part 3. Deep Learning

> 이 파트는 **딥러닝의 전체 큰 그림 + 왜 이런 구조들이 나왔는지에 대한 직관**을 정리한다.  
> 수식 증명보다 **"왜 이런 구조가 필요했는가?"** 에 초점을 맞춘다.

---

## 🎯 이 파트의 목표

딥러닝을 다음 관점에서 이해하는 것:

- 딥러닝 = **함수 근사기**
- 학습 = **loss를 줄이도록 weight를 업데이트하는 과정**
- CNN / RNN = **문제의 구조적 사전정보를 모델 구조에 심은 것**
- 모델 설계 = **문제 구조를 어떻게 신경망에 반영할지에 대한 설계 문제**

---

## 🧱 핵심 관점 요약

### 1️⃣ 인공신경망은 결국 함수다
- 입력 → (행렬곱 + bias + activation) → 출력
- 이 과정을 여러 번 반복한 것이 DNN
- **Universal Approximation Theorem**:  
  → 충분히 큰 신경망은 어떤 연속함수든 근사 가능

---

### 2️⃣ 학습이란 무엇인가?
- Loss = "얼마나 틀렸는지"
- 학습 = "Loss 줄어드는 방향으로 weight 조금씩 움직이기"
- 핵심 도구:
  - Gradient Descent
  - SGD / Mini-batch
  - Momentum / RMSProp / Adam
  - Backpropagation

---

### 3️⃣ 출력이 '확률'이면 Loss도 달라져야 한다
- 이진 분류:
  - Sigmoid + Binary Cross Entropy (Log-likelihood)
- 다중 분류:
  - Softmax + Cross Entropy
- 회귀:
  - MSE

> "출력이 어떤 의미를 가지는가?" 에 따라 loss는 달라진다.

---

### 4️⃣ 딥러닝의 영원한 적: Overfitting
- 문제들:
  - Overfitting
  - Vanishing Gradient
- 해결 전략:
  - ReLU
  - BatchNorm
  - Dropout / Dropconnect
  - L1 / L2 Regularization
  - Data Augmentation
  - 모델 단순화

---

### 5️⃣ CNN: 공간 구조를 신경망에 심다
- 이미지 = **위치 정보가 중요한 데이터**
- CNN의 핵심 아이디어:
  - 가까운 것끼리 먼저 본다
  - 위치별 패턴(특징)을 추출한다
  - Weight 공유로 일반화 성능 증가
- Convolution / Kernel / Channel / Pooling / Receptive field

> CNN = "이미지가 가진 구조적 사전정보를 모델 구조에 반영한 것"

---

### 6️⃣ RNN: 시간 구조를 신경망에 심다
- 순서가 중요한 데이터 (문장, 시계열)
- Hidden state로 과거 정보 요약
- 한계:
  - Long-term dependency 문제
- 그래서:
  - LSTM / GRU
  - 결국 Transformer로 발전

> RNN = "과거 정보를 state로 요약하는 구조"

---

