# 04. SHAP (Shapley Additive exPlanations) ⭐

이 파일은 **SHAP의 이론적 기반과 해석 논리**를 다룬다.  
핵심은 “왜 SHAP의 설명은 신뢰할 수 있는가?”이다.

---

## 1️⃣ SHAP은 왜 등장했는가?

LIME는 Local explanation의 출발점이었지만,
- 실행할 때마다 결과가 달라질 수 있고
- 설명의 “공정성”을 보장하지 못했다

그래서 나온 질문:
> “각 변수가 예측에 기여한 몫을  
> **공정하게 나눌 수는 없을까?**”

이 질문에 대한 답이 **SHAP**이다.

---

## 2️⃣ SHAP의 정체

- **SHAP = Shapley Additive exPlanations**
- 게임이론(Game Theory)의 **Shapley Value** 기반
- 예측값을 **기여도의 합**으로 분해

핵심 아이디어:
> “모든 feature를  
> 공정한 플레이어(player)로 보고  
> 예측값을 분배하자”

---

## 3️⃣ Shapley Value 직관 이해

### ▪ 게임이론에서의 문제
여러 명이 협력해서 얻은 보상이 있을 때,
> “누가 얼마나 기여했는가?”

를 공정하게 나누는 방법이 필요했다.

---

### ▪ Shapley Value의 정의(직관)
- 어떤 플레이어의 기여도는
- **모든 가능한 참여 순서**에서의
- **평균적인 한계 기여도**

즉,
~~~text
“이 feature가
없을 때 → 있을 때
예측이 얼마나 바뀌는가?”
~~~

를 모든 경우에 대해 평균낸 값이다.

---

## 4️⃣ 머신러닝에서 SHAP 해석

### ▪ 대응 관계
~~~text
플레이어        → Feature
보상             → 예측값
한계 기여도      → SHAP value
~~~

---

### ▪ 예측값 분해 구조

~~~text
f(x) = E[f(x)] + Σ SHAP_i
~~~

- E[f(x)] : 전체 데이터 기준 평균 예측값 (Base Value)
- SHAP_i : 각 feature의 기여도
- 모든 SHAP 값의 합 = (예측값 − 평균값)

👉 **완벽한 가법 구조**

---

## 5️⃣ SHAP이 보장하는 핵심 성질 (중요)

### ✅ 1. Efficiency (합 보존)
- 모든 SHAP 값의 합은
- 정확히 예측값 차이를 설명

---

### ✅ 2. Symmetry (대칭성)
- 동일하게 기여한 feature는
- 동일한 SHAP 값을 가짐

---

### ✅ 3. Dummy (무기여 특성)
- 예측에 영향을 주지 않는 feature는
- SHAP 값 = 0

---

### ✅ 4. Additivity (일관성)
- 모델이 변해도
- 기여도가 커진 feature의 SHAP 값은 감소하지 않음

👉 이 성질 때문에  
**SHAP은 “설명에 대한 수학적 보장”**을 가진다.

---

## 6️⃣ SHAP vs LIME (본질적 차이)

| 구분 | LIME | SHAP |
|---|---|---|
| 이론 기반 | 약함 | 게임이론 |
| 안정성 | 낮음 | 높음 |
| 공정성 보장 | ❌ | ⭕ |
| Global 확장 | ❌ | ⭕ |
| 계산 비용 | 낮음 | 높음 |

👉 **신뢰성 vs 비용**의 차이

---

## 7️⃣ Local + Global을 동시에 설명하는 SHAP

### ▪ Local SHAP
[O- 특정 샘플 하나의 예측 설명
- force plot, waterfall plot

### ▪ Global SHAP
- 여러 샘플의 SHAP 평균
- summary plot, dependence plot

👉 **Local을 쌓으면 Global이 된다**  
(LIME과 결정적 차이)

---

## 8️⃣ SHAP 해석 시 반드시 주의할 점

### ⚠️ 1. SHAP ≠ 인과관계
- 기여도일 뿐
- “원인”은 아님

---

### ⚠️ 2. 상관관계 변수
- 상관된 feature가 있으면
- 기여도가 분산됨

---

### ⚠️ 3. Base Value 해석
- 기준선은 “0”이 아님
- **평균 예측값**임

---

## 🎯 이 파일의 핵심 메시지

- SHAP은 **공정한 기여도 분배**다
- 예측값을 수학적으로 분해한다
- Local과 Global을 모두 설명할 수 있다
- 그래서 실무와 면접에서 **표준처럼 쓰인다**

---

## 🔗 다음 단계
👉 `practices/01_shap_code_review.md`

- Explainer 선택 이유
- SHAP value 구조
- summary / force / dependence plot 해석
- **“이 그림을 말로 설명하는 법”**

