# 02. Global vs Local Feature Importance 🎯

이 파일은 **“변수 중요도(feature importance)”를 해석할 때 가장 많이 발생하는 오해**를 정리한다.  
특히 **Global 중요도와 Local 중요도는 전혀 다른 질문에 대한 답**이라는 점이 핵심이다.

---

## 1️⃣ Feature Importance란 무엇인가?

Feature Importance란,
> “모델의 예측에 각 변수가 얼마나 영향을 미쳤는가”

를 수치로 표현한 것이다.

⚠️ 주의할 점:
- **중요도 = 인과관계** ❌
- **중요도 = 기여도(영향력)** ⭕  

---

## 2️⃣ Global Feature Importance

### ▪ Global Importance의 관점
- **전체 데이터셋 기준**
- 모델이 평균적으로 어떤 변수를 많이 사용했는가?

즉,
> “이 모델은 전반적으로 무엇을 중요하게 보는가?”

에 대한 답이다.

---

### ▪ 대표적인 Global Importance 방법

#### 1. Tree 기반 importance
- Random Forest / GBDT
- 분기 시 impurity 감소량(Gini, MSE 등)을 누적
- 장점: 빠르고 직관적
- 단점:
  - 연속형 변수에 유리
  - 변수 스케일/카디널리티 영향

---

#### 2. Permutation Importance
- 변수 하나를 **무작위로 섞음(permutation)**
- 성능이 얼마나 떨어지는지 측정

~~~text
원본 데이터 → 성능 측정
변수 X_i 섞기 → 성능 재측정
성능 감소량 = X_i의 중요도
~~~

- 장점:
  - 모델 독립적
  - 비교적 공정
- 단점:
  - 변수 간 상관관계가 있으면 왜곡

---

### ▪ Global Importance의 한계

- 전체 평균 기준
- **특정 케이스 하나를 설명할 수 없음**

예:
- “이 고객은 왜 대출이 거절됐나요?”
→ Global 중요도로는 답 불가

---

## 3️⃣ Local Feature Importance

### ▪ Local Importance의 관점
- **특정 샘플 하나**
- 그 샘플의 예측값을 기준으로 해석

즉,
> “이 예측은 왜 이렇게 나왔는가?”

에 대한 답이다.

---

### ▪ Local Importance의 특징
- 샘플마다 중요도가 다름
- 같은 변수라도:
  - 어떤 샘플에서는 긍정
  - 어떤 샘플에서는 부정적 영향

---

### ▪ 예시로 이해하기

Global 관점:
- “공부시간은 성적에 중요하다”

Local 관점:
- “이 학생은 공부시간보다 컨디션이 더 결정적이었다”

👉 둘 다 맞을 수 있다.

---

## 4️⃣ Global vs Local 차이가 발생하는 이유

### ▪ 이유 1. 데이터 분포의 평균화
- Global은 전체 데이터를 평균냄
- 극단값 / 소수 케이스가 희석됨

---

### ▪ 이유 2. 변수 간 상호작용
- 특정 조합에서만 중요한 변수 존재
- Global에서는 드러나지 않음

---

### ▪ 이유 3. 모델의 비선형성
- 트리, 부스팅, 딥러닝
- 조건부로 다른 규칙 작동

---

## 5️⃣ 왜 Local Explanation이 필요한가?

현실 문제는 대부분 **Local 질문**이다.

- 왜 이 고객만 거절?
- 왜 이 샘플만 이상치?
- 왜 이 환자만 고위험?

👉 이 질문들에 답하려면  
**Black Box 모델을 “열어봐야” 한다.**

---

## 6️⃣ Local Explanation으로 가는 길

여기서 등장하는 것이:
- **LIME**
- **SHAP**

공통점:
- Model-Agnostic
- Local explanation 제공

차이점:
- 안정성
- 이론적 근거
- Global 확장 가능 여부

👉 다음 파일에서 각각 분해한다.

---

## 🎯 이 파일의 핵심 메시지

- Global 중요도 ≠ Local 중요도
- 서로 다른 질문에 대한 답
- “왜 이 예측이 나왔나?”는  
  **Global importance로 절대 설명 불가**
- 그래서 LIME / SHAP이 필요하다

---

## 🔗 다음 파일 예고
👉 `03_lime.md`  
- Local explanation의 출발점  
- Perturbation 기반 설명의 장단점

