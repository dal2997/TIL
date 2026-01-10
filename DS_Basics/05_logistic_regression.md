# Logistic Regression (로지스틱 회귀) 🧠
> 0/1(또는 클래스) 분류의 기본 모델  
> 핵심: `predict_proba`, 임계값(threshold), precision/recall, 불균형 대응

---

## 1) 언제 쓰나?
- y가 클래스(예: 이탈/유지, 스팸/정상, 합격/불합격)
- “확률”이 필요한 업무(리스크 점수/경고 시스템)

---

## 2) 개념 한 줄
- `P(y=1|x)`를 예측 → threshold로 0/1 결정
- `coef`는 “로그 오즈(log-odds)” 기반이라 선형회귀처럼 단순 해석하면 망함

---

## 3) 핵심 셀(최소 재현 세트)
> 전처리/결측치/더미는 02번 문서 기반

### (1) split (분류면 stratify 추천)
~~~python
from sklearn.model_selection import train_test_split

X = df.drop(columns=["target"])
y = df["target"]

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)
~~~

### (2) 파이프라인(스케일링 + 로지스틱)
~~~python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression

pipe = Pipeline([
    ("scaler", StandardScaler(with_mean=False)),
    ("model", LogisticRegression(max_iter=2000))
])

pipe.fit(X_train, y_train)
~~~

### (3) 기본 평가
~~~python
from sklearn.metrics import accuracy_score, classification_report

pred = pipe.predict(X_test)
accuracy_score(y_test, pred)
print(classification_report(y_test, pred))
~~~

---

## 4) 확률 + 임계값(threshold) 다루기 (중요)
> 정확도만 보면 착시가 심함 → threshold 조정이 실전 포인트

~~~python
import numpy as np
from sklearn.metrics import confusion_matrix, precision_score, recall_score, f1_score

proba = pipe.predict_proba(X_test)[:, 1]  # y=1 확률

threshold = 0.5
pred_th = (proba >= threshold).astype(int)

confusion_matrix(y_test, pred_th)
precision_score(y_test, pred_th), recall_score(y_test, pred_th), f1_score(y_test, pred_th)
~~~

---

## 5) ROC-AUC (대표 성능 지표)
~~~python
from sklearn.metrics import roc_auc_score, roc_curve
import matplotlib.pyplot as plt

auc = roc_auc_score(y_test, proba)
fpr, tpr, thr = roc_curve(y_test, proba)

plt.figure()
plt.plot(fpr, tpr)
plt.plot([0, 1], [0, 1], linestyle="--")
plt.xlabel("FPR")
plt.ylabel("TPR")
plt.title(f"ROC Curve (AUC={auc:.4f})")
plt.show()
~~~

---

## 6) 불균형(class imbalance) 대응(자주 필요)
~~~python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression

pipe_bal = Pipeline([
    ("scaler", StandardScaler(with_mean=False)),
    ("model", LogisticRegression(max_iter=2000, class_weight="balanced"))
])

pipe_bal.fit(X_train, y_train)
~~~

---

## 7) 체크리스트 ✅
- [ ] y가 진짜 0/1인가? (아니면 인코딩 필요)
- [ ] stratify=y로 split 했나?
- [ ] accuracy만 보고 결론내리지 않았나? (precision/recall/f1/auc 같이 봐라)
- [ ] 운영 목적이면 threshold를 조정했나?

