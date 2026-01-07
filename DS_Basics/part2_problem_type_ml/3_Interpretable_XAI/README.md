# Chapter03 – Interpretable Machine Learning & XAI 🔍

이 챕터는 **Black Box 모델을 어떻게 “설명 가능한 형태”로 이해할 것인가**에 대한 내용이다.  
모델 성능이 아닌 **“왜 이런 예측이 나왔는가?”**에 초점을 둔다.

---

## 📌 이 챕터에서 다루는 핵심 질문

- 성능이 좋은 모델은 **왜 믿으면 안 되는가?**
- Global Feature Importance와 Local Explanation의 차이는?
- LIME과 SHAP은 **언제 / 왜 / 어떻게** 사용하는가?
- 특정 샘플 하나의 예측을 **논리적으로 분해**할 수 있는가?

---

## 📂 폴더 구조

~~~text
Chapter03_Interpretable_XAI/
├── README.md
├── 01_blackbox_and_need_for_xai.md
├── 02_global_vs_local_importance.md
├── 03_lime.md
├── 04_shap.md
└── practices/
    ├── README.md
    └── 01_shap_code_review.md
~~~

---

## 🧠 개념 파일 설명

### 01_blackbox_and_need_for_xai.md
- Black Box 모델의 정의
- Explainability vs Performance 트레이드오프
- 왜 XAI가 필요한가
- Interpretable ML의 등장 배경

---

### 02_global_vs_local_importance.md
- Global Feature Importance
  - 전체 데이터 기준 중요도
  - RF importance, permutation importance 개념
- Local Feature Importance
  - 특정 샘플 하나의 예측 설명
- Global / Local 차이가 발생하는 이유

---

### 03_lime.md
- LIME 개념 정리
- Local / Model-Agnostic의 의미
- Perturbation 기반 근사 설명
- 장점과 한계 (불안정성, 샘플 의존성)

---

### 04_shap.md ⭐
- SHAP (Shapley Additive exPlanations)
- 게임이론 기반 Shapley Value
- 왜 SHAP은 “공정한 설명”인가
- Global / Local 설명을 동시에 제공하는 이유
- LIME과의 본질적 차이

---

## 🧪 실습(practices)

### practices/01_shap_code_review.md
- SHAP 실습 코드 리뷰
- Explainer 선택 이유
- SHAP value 구조 해석
[O- summary / force / dependence plot 해석
- **“이 그림을 말로 설명하면 어떻게 되는가”**에 집중

