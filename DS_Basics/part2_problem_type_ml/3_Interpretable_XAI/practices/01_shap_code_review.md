# Practice 01. SHAP Code Review & Interpretation 🧪

이 파일은 SHAP 실습 코드를 **한 줄 한 줄 “왜 필요한지” 기준으로 리뷰**한다.  
목표는 **SHAP 결과를 보고 말로 설명할 수 있는 수준**에 도달하는 것이다.

---

## 1️⃣ 실습의 전체 흐름 요약

SHAP 실습의 큰 흐름은 아래 4단계다.

~~~text
1. 학습된 Black Box 모델 준비
2. SHAP Explainer 생성
3. SHAP value 계산
4. 시각화 + 해석
~~~

중요한 점:
- SHAP은 **모델을 바꾸지 않는다**
- 이미 잘 학습된 모델을 **“설명만” 한다**

---

## 2️⃣ Explainer 선택 (가장 중요)

### ▪ 왜 Explainer가 필요한가?

SHAP value는
- “feature를 하나씩 넣고 빼면서 예측이 얼마나 변하는가”
를 계산한다.

하지만 모델마다 계산 방식이 다르기 때문에
👉 **모델 구조에 맞는 Explainer**를 써야 한다.

---

### ▪ 대표적인 Explainer들

- TreeExplainer  
  - RandomForest, XGBoost, LightGBM
  - 가장 빠르고 정확
- LinearExplainer  
  - Linear / Logistic Regression
- KernelExplainer  
  - Model-agnostic
  - 가장 느림 (LIME과 유사한 샘플링 기반)

👉 **Tree 기반 모델이면 TreeExplainer가 정배**

---

### ▪ 코드 해석

~~~python
explainer = shap.TreeExplainer(model)
~~~

이 한 줄의 의미:
- 이 모델의 내부 구조를 활용해서
- SHAP 값을 **정확하고 빠르게** 계산하겠다는 뜻

---

## 3️⃣ SHAP value 계산

~~~python
shap_values = explainer.shap_values(X)
~~~

### ▪ shap_values의 정체

- shape:
~~~text
(n_samples, n_features)
~~~

[O- 의미:
  - 각 샘플 × 각 feature의 기여도

---

### ▪ Base Value (절대 중요)

~~~python
explainer.expected_value
~~~

- 전체 데이터 기준 평균 예측값
- SHAP의 기준선

👉 SHAP 해석 공식:
~~~text
예측값 = base value + Σ SHAP value
~~~

---

## 4️⃣ Global 해석: summary plot

~~~python
shap.summary_plot(shap_values, X)
~~~

### ▪ 이 그림을 어떻게 해석해야 하나?

- y축: feature 중요도 (평균 |SHAP|)
- x축: SHAP value (기여 방향 + 크기)
- 색:
  - 빨강: feature 값 큼
  - 파랑: feature 값 작음

---

### ▪ 말로 설명하는 예시 (중요)

> “이 모델에서는  
> **feature A와 B가 전체적으로 가장 중요한 변수**이고,  
> A는 값이 클수록 예측을 증가시키는 방향으로,  
> B는 상황에 따라 양/음의 영향을 모두 미칩니다.”

👉 이 한 문장만 말해도  
**Global SHAP 이해했다고 봐도 됨**

---

## 5️⃣ Local 해석: force / waterfall plot

~~~python
shap.force_plot(
    explainer.expected_value,
    shap_values[i],
    X.iloc[i]
)
~~~

### ▪ 이 plot의 구조

- 가운데 기준선: base value
- 오른쪽 밀면: 예측값 증가
- 왼쪽 밀면: 예측값 감소

---

### ▪ 말로 설명하는 예시

> “이 샘플의 기본 예측값은 평균 수준이었지만,  
> feature C와 D가 예측을 크게 증가시켰고,  
> feature E는 이를 일부 상쇄했습니다.  
> 그 결과 최종 예측값이 높아졌습니다.”

👉 **Local explanation의 정석 문장**

---

## 6️⃣ Dependence plot (상호작용 이해)

~~~python
shap.dependence_plot("feature_A", shap_values, X)
~~~

### ▪ 이 plot의 의미

- x축: feature 값
- y축: 해당 feature의 SHAP value
- 색: 다른 feature (상호작용)

---

### ▪ 해석 포인트

- 선형인지 / 비선형인지
- 임계점(threshold)이 있는지
- 특정 구간에서만 영향력이 큰지

---

## 7️⃣ SHAP 해석 시 자주 나오는 오해

### ❌ “SHAP 값이 크니까 중요한 원인이다”
→ ⭕ 기여도이지 인과가 아님

### ❌ “Global SHAP만 보면 된다”
→ ⭕ 실제 질문은 대부분 Local

### ❌ “base value는 0이다”
→ ⭕ 평균 예측값

---

## 8️⃣ 면접에서 이렇게 말하면 된다 (요약 멘트)

- “SHAP은 예측값을 평균 예측값과 각 변수의 기여도로 분해합니다.”
- “Local SHAP은 개별 예측을, Global SHAP은 전체 경향을 설명합니다.”
- “Tree 모델에서는 TreeExplainer를 사용하는 것이 가장 효율적입니다.”

---

## 🎯 이 실습의 핵심 메시지

- SHAP은 **예측을 분해하는 도구**
- 그림보다 **말로 설명하는 능력**이 중요
- Local → Global 연결이 가능
- 실무·면접 모두에서 **신뢰 가능한 설명 방법**

---

## 🔚 Chapter03 정리

- Black Box의 한계를 인식
- Global / Local 질문 구분
- LIME의 한계 이해
- SHAP으로 설명의 표준 도달

