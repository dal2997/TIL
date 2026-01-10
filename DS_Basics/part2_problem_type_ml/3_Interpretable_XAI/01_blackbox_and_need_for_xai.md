# 01. Black Box Model과 XAI의 필요성 🧠

이 파일은 **왜 Explainable AI(XAI)가 필요해졌는지**를 설명한다.  
“모델이 잘 맞춘다”와 “모델을 신뢰할 수 있다”는 **전혀 다른 문제**라는 점이 핵심이다.

---

## 1️⃣ Black Box 모델이란?

### ▪ Black Box의 의미
- **입력(Input)** 과 **출력(Output)** 은 알 수 있지만
- **내부 동작 원리**는 알 수 없는 시스템

머신러닝에서의 Black Box 모델이란,
> “예측은 잘하지만,  
> 왜 그런 예측이 나왔는지는 설명할 수 없는 모델”

을 의미한다.

---

### ▪ 대표적인 Black Box 모델
- Random Forest
- Gradient Boosting (XGBoost, LightGBM)
- Neural Network / Deep Learning

이 모델들은:
- 수많은 분기 / 파라미터 / 비선형 결합을 사용
- **인간이 직관적으로 추적하기 어려움**

---

## 2️⃣ Explainability vs Performance 트레이드오프

### ▪ 해석 가능한 모델
- Linear Regression
- Logistic Regression
- 단일 Decision Tree

**장점**
- 계수, 규칙 기반 설명 가능
- “X가 1 증가하면 Y는 β만큼 증가”

**단점**
- 복잡한 패턴을 잘 못 잡음
- 성능 한계

---

### ▪ 고성능 모델
- Ensemble, Deep Learning

**장점**
- 높은 예측 성능
- 복잡한 상호작용 학습 가능

**단점**
- 왜 이런 예측이 나왔는지 설명 불가
- 신뢰·책임·디버깅 문제 발생

---

### ▪ 핵심 정리
> 성능이 높아질수록  
> → 해석력은 낮아지는 경향

이 딜레마가 **XAI의 출발점**이다.

---

## 3️⃣ “성능이 좋으면 그냥 믿으면 안 되나?”

### ▪ 현실 문제
단순 Accuracy / RMSE 만으로는 부족하다.

예시:
- 금융 대출 승인
- 의료 진단
- 이상 거래 탐지
- 채용/평가 시스템

이런 상황에서 필요한 질문:
- 왜 이 사람은 거절되었는가?
- 어떤 특성이 결정적이었는가?
- 모델이 편향된 판단을 한 것은 아닌가?

---

### ▪ 신뢰의 문제
모델을 신뢰하려면:
- **설명 가능성**
- **디버깅 가능성**
- **책임 추적 가능성**

이 필요하다.

---

## 4️⃣ Interpretable Machine Learning의 등장

### ▪ 접근 방식 2가지

[O#### 1. 처음부터 해석 가능한 모델 사용
- Linear / Logistic / GAM / Tree
- 해석은 쉬움
- 성능은 제한적

#### 2. Black Box는 유지 + 설명은 외부에서
- **Model-Agnostic Explanation**
- 여기서 등장:
  - LIME
  - SHAP

---

## 5️⃣ Global vs Local 관점의 필요성 (Preview)

이 챕터 이후 계속 등장하는 핵심 구분이다.

### ▪ Global Interpretability
- 전체 데이터 기준
- “모델 전체에서 중요한 변수는?”

### ▪ Local Interpretability
- 특정 샘플 하나
- “이 예측이 왜 이렇게 나왔나?”

> 같은 모델이라도  
> Global과 Local의 답은 **완전히 다를 수 있음**

---

## 🎯 이 파일의 핵심 메시지

- 성능이 좋다고 신뢰할 수 있는 건 아니다
- Black Box 모델은 설명 없이는 위험하다
- **설명은 선택이 아니라 필수**
- 그래서 XAI, Interpretable ML이 등장했다

---

## 🔗 다음 파일 예고
👉 `02_global_vs_local_importance.md`  
- Global / Local 차이를 본격적으로 파고듦  
- RF importance, permutation, Local explanation 연결

