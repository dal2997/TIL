# Chapter02 · Classification — 면접 / 시험 질문 정리

이 문서는 **Chapter02_Classification 전체 내용을 기반으로 한 면접·시험 대비 질문 모음**이다.
단순 암기 문제가 아니라, **개념 이해 여부를 판별할 수 있는 질문 + 핵심 답변 포인트** 중심으로 구성했다.

---

## 1️⃣ Problem Definition / Loss

### Q1. Classification 문제와 Regression 문제의 본질적인 차이는 무엇인가?

**핵심 포인트**

* Regression: 연속값 예측 → 오차 크기 정의 가능
* Classification: 범주 예측 → 맞음/틀림만 관측
* 이 차이로 인해 손실 함수 구조가 달라짐

---

### Q2. 왜 Classification에서 MSE를 그대로 사용하면 안 되는가?

**핵심 포인트**

* 분류 문제에서는 거리 개념이 의미 없음
* 맞춘 경우에도 확률 차이에 따라 손실이 달라지는 문제
* 확신 있는 오답을 강하게 벌점 줄 수 없음

---

### Q3. Binary Cross Entropy는 무엇을 측정하는 손실 함수인가?

**핵심 포인트**

* 정답 클래스에 대한 예측 확률의 적합도
* 확신을 가지고 틀릴수록 큰 패널티
* 확률 분포를 학습하는 손실 함수

---

## 2️⃣ Decision Boundary / Tree Split

### Q4. Decision Boundary란 무엇이며, 분류 문제에서 왜 중요한가?

**핵심 포인트**

* 입력 공간에서 클래스가 나뉘는 경계
* 분류 문제는 공간 분할 문제
* 모델 차이는 경계 형태 차이로 해석 가능

---

### Q5. Decision Tree의 decision boundary는 어떤 특징을 가지는가?

**핵심 포인트**

* axis-aligned (축에 평행)
* 직사각형 영역의 조합
* 비선형 경계 표현 가능하지만 복잡해지기 쉬움

---

### Q6. Gini Index는 무엇을 의미하며 값의 범위는 어떻게 되는가?

**핵심 포인트**

* 노드 내 클래스 혼합 정도
* Binary 기준 0 ~ 0.5
* 0에 가까울수록 순수

---

### Q7. Entropy와 Gini의 공통점과 차이점은 무엇인가?

**핵심 포인트**

* 공통점: 불순도 측정
* 차이점: Entropy는 극단값에 더 민감
* 실무에서는 계산 효율 때문에 Gini 선호

---

### Q8. Information Gain이란 무엇이며 왜 가중 평균을 사용하는가?

**핵심 포인트**

* split 전후 불확실성 감소량
* 노드 크기 차이를 반영하기 위해 가중 평균 사용
* IG는 split의 점수

---

## 3️⃣ Modeling / Ensemble

### Q9. Single Decision Tree의 가장 큰 한계는 무엇인가?

**핵심 포인트**

* 높은 Variance
* 데이터 변화에 민감
* Greedy split의 구조적 한계

---

### Q10. Random Forest는 어떤 문제를 해결하기 위해 등장했는가?

**핵심 포인트**

* Tree의 높은 Variance 감소
* bootstrap + feature randomness
* 안정적인 예측

---

### Q11. Bootstrap과 Subsampling의 차이를 설명하라.

**핵심 포인트**

* bootstrap: 복원 추출, Tree 간 비상관화
* subsampling: 비복원 추출, 과적합 완화
* 사용 모델이 다름

---

### Q12. OOB Error란 무엇이며 왜 가능한가?

**핵심 포인트**

* bootstrap으로 인해 학습에 쓰이지 않은 데이터 존재
* 별도 validation 없이 성능 추정 가능

---

### Q13. AdaBoost는 언제 Random Forest보다 적합한가?

**핵심 포인트**

* 노이즈가 적을 때
* 단순하지만 약간 비선형 경계
* Bias를 빠르게 줄여야 할 때

---

### Q14. AdaBoost가 불리한 상황은 언제인가?

**핵심 포인트**

* 노이즈/이상치 많을 때
* 라벨 오류 많은 데이터
* 의료 데이터 등

---

## 4️⃣ Evaluation Metrics

### Q15. Accuracy가 위험한 지표가 되는 상황은 언제인가?

**핵심 포인트**

* Class imbalance
* 다수 클래스로만 예측해도 높은 정확도

---

### Q16. Precision과 Recall의 차이를 비용 관점에서 설명하라.

**핵심 포인트**

* Precision: FP 비용 중요
* Recall: FN 비용 중요

---

### Q17. 의료 진단 문제에서 Accuracy보다 Recall을 더 중요하게 보는 이유는?

**핵심 포인트**

* FN(놓침)의 비용이 매우 큼
* 환자를 놓치는 리스크

---

## 5️⃣ Regularization / Overfitting

### Q18. Decision Tree에서 Overfitting이 쉽게 발생하는 이유는?

**핵심 포인트**

* split 증가 → 복잡한 경계
* 노이즈 학습

---

### Q19. Pruning의 목적은 무엇인가?

**핵심 포인트**

* 불필요한 분기 제거
* 일반화 성능 향상

---

### Q20. Boosting 계열에서 Shrinkage와 Early Stopping이 필요한 이유는?

**핵심 포인트**

* 과도한 보정 방지
* 점진적 학습
* 성능 붕괴 방지

---

## ✅ 활용 가이드

* **면접 대비**: 질문 → 2~3문장으로 핵심만 답변 연습
* **시험 대비**: 질문을 키워드 형태로 분해해 서술형 대비
* **복습**: 각 질문을 해당 md 파일과 연결해서 다시 확인

---

📌 이 문서는 Chapter02 전체 이해도를 점검하기 위한 **자기 점검용 질문 세트**로 설계되었다.

