# 04. Regularization & Generalization 🧠

이 파일은  
**“왜 모델이 똑똑해 보이다가 실제 데이터에서 망하는가”**에 대한 답이다.

---

## 4.1 Universal Approximation의 함정
- 신경망은 이론적으로 어떤 함수든 표현 가능
- ❗ 하지만
  - 학습이 잘 된다는 뜻 ❌
  - 일반화가 된다는 뜻 ❌

👉 **표현력 ≠ 학습 가능성 ≠ 일반화**

---

## 4.2 Overfitting의 정체
- Train 성능 ↑
- Test 성능 ↓

원인:
- 데이터 패턴이 아니라 **노이즈를 학습**
- 모델이 데이터를 “이해”한 게 아니라 **외움**

---

## 4.3 Bias – Variance 관점
- Bias ↑ → underfitting
- Variance ↑ → overfitting

딥러닝 문제의 대부분은:
> **Variance를 어떻게 제어할 것인가**

---

## 4.4 Regularization의 핵심 철학
> “모델을 덜 믿고, 데이터를 더 믿자”

목표:
- 불필요한 자유도 제한
- 학습을 **안정적인 방향**으로 유도

---

## 4.5 L1 / L2 Regularization
- L1 (Lasso)
  - 가중치 일부를 0으로
  - feature selection 효과
- L2 (Ridge)
  - 큰 가중치 억제
  - 부드러운 모델

👉 딥러닝에서는 **L2가 기본값**

---

## 4.6 Dropout
- 학습 중 일부 뉴런 랜덤 제거
- 앙상블 효과
- 특정 경로 의존 방지

직관:
> “항상 믿던 친구를 가끔 결석시킨다”

---

## 4.7 Early Stopping
- Validation 성능 기준으로 학습 중단
- 가장 강력한 regularization 중 하나

👉 **더 학습한다고 더 좋아지지 않는다**

---

## 4.8 Data Augmentation
- 데이터를 늘리는 게 아니라
- **문제를 쉽게 만드는 것**
- 이미지/텍스트에서 효과 큼

---

## 4.9 Regularization의 핵심 정리
- 수식보다 **의도**가 중요
- 언제:
  - 데이터 적음
  - 모델 큼
  - 성능 흔들림
- 반드시 고려

---

## 한 줄 요약
- 딥러닝의 승부는 **구조 + 제약**
- Overfitting은 실패가 아니라 **자연현상**
- Regularization은 선택이 아니라 필수

