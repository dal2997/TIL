---

## 🧠 Interview Check – Chapter03 (XAI)

이 챕터는 **면접에서 질문 빈도가 매우 높은 영역**이다.  
아래 질문들은 “이해했는지 / 말로 설명 가능한지”를 바로 걸러낸다.

---

### Q1. Black Box 모델이란 무엇인가요?
**답변 포인트**
- 입력과 출력은 알 수 있지만 내부 동작은 해석 불가
- Random Forest, Boosting, Deep Learning
- 성능은 높지만 신뢰·설명·책임 문제가 있음

---

### Q2. 성능이 좋으면 왜 설명이 필요하죠?
**답변 포인트**
- Accuracy 하나로는 부족
- 금융/의료/채용 등에서는
  - 왜 이런 판단이 나왔는지가 중요
- 디버깅, 편향 탐지, 책임 추적을 위해 필요

---

### Q3. Global Feature Importance와 Local Explanation의 차이는?
**답변 포인트**
- Global: 전체 데이터 기준 평균적인 중요도
- Local: 특정 샘플 하나의 예측 이유
- 서로 다른 질문에 대한 답
- Global로 Local을 설명할 수 없음

---

### Q4. Permutation Importance의 아이디어는?
**답변 포인트**
- 변수를 무작위로 섞어서
- 성능이 얼마나 떨어지는지 측정
- 모델 독립적
- 상관관계 변수에서는 왜곡 가능

---

### Q5. LIME은 어떤 방식인가요?
**답변 포인트**
- 특정 샘플 주변에서 perturbation
- Black Box 예측을
- 단순 모델로 locally 근사
- 직관적이지만 불안정

---

### Q6. LIME의 한계는 무엇인가요?
**답변 포인트**
- 샘플링에 따라 결과가 달라짐
- Local 정의가 애매
- Global 확장 불가
- 이론적 공정성 보장 없음

---

### Q7. SHAP은 무엇을 해결했나요?
**답변 포인트**
- 게임이론 기반 Shapley Value
- 예측값을 feature 기여도의 합으로 분해
- 공정성·일관성 보장
- Local + Global 모두 가능

---

### Q8. SHAP의 base value는 무엇인가요?
**답변 포인트**
- 전체 데이터 기준 평균 예측값
[O- 기준선 역할
- SHAP 값의 합 + base value = 예측값

---

### Q9. SHAP과 LIME의 핵심 차이는?
**답변 포인트**
- LIME: 근사, 불안정
- SHAP: 수학적 보장, 안정적
- SHAP은 Global 확장 가능

---

### Q10. SHAP 값이 크면 인과관계인가요?
**정답**
- ❌ 아니다
- SHAP은 기여도
- 인과관계는 별도의 분석 필요

---

## ✅ 이 챕터를 끝냈다는 기준

아래를 **말로 설명할 수 있으면 통과**다.

- “왜 Global 중요도로 Local을 설명할 수 없는가”
- “SHAP이 예측값을 어떻게 분해하는가”
- “summary plot / force plot을 말로 설명하기”
- “TreeExplainer를 쓰는 이유”

---

