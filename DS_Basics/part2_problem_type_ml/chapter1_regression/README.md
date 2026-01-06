# Chapter 01. Regression Problem

이 챕터는 **회귀 문제를 “모델 관점”이 아니라  
👉 문제 해결 관점(현업 관점)**에서 이해하는 것을 목표로 한다.

---

## 이 챕터의 핵심 질문

> ❓ 회귀 문제를 만났을 때,  
> 우리는 **무엇을 먼저 생각해야 하는가?**

- 어떤 모델을 쓸지?
- 선형이냐 비선형이냐?
- 트리냐 부스팅이냐?

❌ 아니다.  
이 챕터의 관점은 아래 하나로 시작한다.

> ✅ **이 문제에서 “오차(Loss)”를 어떻게 정의하고,  
> 그 오차를 어떻게 줄일 것인가?**

---

## 이 챕터의 전체 흐름

| 파일 | 핵심 내용 | 왜 중요한가 |
|---|---|---|
| 01_problem_definition.md | 회귀 문제의 본질 | “값 맞추기 ≠ 회귀” |
| 02_loss_bias_variance.md | Loss / Bias / Variance | 과적합을 설명할 수 있음 |
| 03_modeling_and_interpretation.md | β, p-value 해석 | 숫자를 말로 바꾸는 능력 |
| 04_evaluation_metrics.md | R², MAE, RMSE | 성능을 잘못 해석하지 않기 |
| 05_regularization_and_feature_selection.md | Ridge/Lasso | 현업에서 망하지 않기 |

---

## 이 챕터를 다 보고 나면

- ✅ “왜 이 모델이 과적합인가?” 설명 가능
- ✅ “왜 RMSE를 썼는가?”에 답 가능
- ✅ β 계수를 **의미 단위**로 해석 가능
- ✅ Regularization을 **감각이 아니라 이유로** 선택 가능

> 이 챕터는 **DS_Basics의 회귀 모델들**을  
> 👉 “왜 그렇게 동작하는지” 설명해 주는 이론적 지도다.

