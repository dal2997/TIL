

# 07. Deep Learning Summary & Interview Guide 🧩

이 파일은 `part3_deep_learning` 전체를 **한 장으로 압축**한다.  
복습·면접·다음 파트 연결을 위한 **정리용 종착지**다.

---

## 7.1 이 파트의 큰 흐름 한 줄 요약

> 딥러닝은  
> **표현력을 키우는 싸움이 아니라,  
> 학습 가능성과 일반화를 설계하는 문제**다.

---

## 7.2 전체 흐름 다시 보기

### ① Big Picture (01)
- NN은 함수 근사기
- Universal Approximation ≠ 잘 학습됨
- 구조와 가정이 성능을 좌우

---

### ② Training & Optimization (02)
- Loss를 줄이는 문제
- Gradient 기반 학습
- Initialization / LR / Normalization 중요

---

### ③ Classification & Loss (03)
- 문제에 맞는 loss 선택
- Softmax + Cross-Entropy의 의미
- 출력 해석이 모델 설계의 일부

---

### ④ Regularization & Generalization (04)
- Overfitting은 자연 현상
- Bias–Variance tradeoff
- Dropout / L2 / Early stopping
- “모델을 덜 믿자”

---

### ⑤ CNN & Feature Learning (05)
- FC의 한계 → CNN 등장
- Convolution = 구조적 가정
- CNN은 feature engineering을 자동화
- 일반화에 강한 이유가 구조에 내재

---

### ⑥ RNN → Transformer (06)
- 순서 데이터를 다루는 방식의 진화
- RNN: 기억하려다 실패
- Transformer: 관계를 직접 본다
- Attention + 병렬화 = 표준 구조

---

## 7.3 자주 나오는 면접 질문 & 핵심 답변

### Q1. Universal Approximation Theorem이면 왜 딥러닝이 어려운가?
- 표현 가능성과 학습 가능성은 다름
- Optimization과 generalization이 핵심 문제

---

### Q2. Overfitting을 어떻게 막나?
- 모델 단순화
- Regularization (L2, Dropout)
- Early stopping
- 데이터 관점 접근

---

### Q3. CNN이 FC보다 이미지에 적합한 이유는?
- 공간 구조 보존
- 파라미터 공유
- 위치 불변성 유도

---

### Q4. RNN의 근본적 한계는?
- 장기 의존성
- 순차 처리로 인한 병렬화 불가

---

### Q5. Transformer가 성공한 이유는?
- Attention으로 장기 의존성 해결
- 병렬 처리 가능
- 구조적 병목 제거

---

## 7.4 이 파트를 끝냈다는 기준

아래를 **말로 설명 가능하면 통과**다.

- “딥러닝은 왜 구조 설계의 문제인가”
- “왜 CNN이 이미지에서 기본인가”
- “RNN에서 Transformer로 왜 넘어갔는가”
- “Regularization은 왜 필수인가”

---

## 7.5 다음 파트로의 연결

- 다음 주제:
  - Vision Transformer
  - Representation Learning
  - Self-supervised Learning
- 혹은
  - Evaluation / XAI와의 연결

👉 딥러닝은 **모델이 아니라 사고방식의 변화**다.

