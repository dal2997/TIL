# 01. Distance & Similarity in Clustering 📐

이 파일은 **Clustering에서 가장 중요한 개념인 “거리(distance)”**를 다룬다.  
알고리즘보다 먼저 이해해야 할 것은 **“데이터를 얼마나 가깝다고 볼 것인가”**다.

---

## 1️⃣ Clustering에서 거리란 무엇인가?

Clustering은 본질적으로 다음 질문에 답하는 문제다.

> “어떤 데이터들이 서로 비슷한가?”

이때 **비슷함(similarity)** 또는 **다름(dissimilarity)**을  
수치로 표현한 것이 **거리(distance)**다.

⚠️ 핵심:
- Clustering 결과는 **거리 정의에 의해 거의 결정된다**
- 알고리즘은 그 다음 문제다

---

## 2️⃣ Euclidean Distance (유클리디안 거리)

### ▪ 정의
~~~text
두 점 사이의 직선 거리
~~~

~~~text
d(x, y) = √ Σ (xᵢ − yᵢ)²
~~~

### ▪ 특징
- 가장 직관적인 거리
- K-means의 기본 가정
- 좌표 공간에서 “원형(cluster)”을 잘 찾음

### ▪ 한계
- 스케일에 매우 민감
- 이상치(outlier)에 취약
- 고차원에서 의미 약화 (curse of dimensionality)

---

## 3️⃣ Manhattan Distance (맨해튼 거리)

### ▪ 정의
~~~text
좌표축을 따라 이동한 거리의 합
~~~

~~~text
d(x, y) = Σ |xᵢ − yᵢ|
~~~

### ▪ 특징
- Euclidean보다 이상치에 덜 민감
- 축 방향 이동이 의미 있는 경우 적합
- 다이아몬드 형태의 군집

---

## 4️⃣ Minkowski Distance (일반화 거리)

### ▪ 정의
~~~text
p-차수 거리의 일반형
~~~

~~~text
d(x, y) = ( Σ |xᵢ − yᵢ|^p )^(1/p)
~~~

- p = 2 → Euclidean
- p = 1 → Manhattan

👉 거리 선택은 **p 값 선택 문제**로 일반화 가능

---

## 5️⃣ Cosine Distance (각도 기반 거리)

### ▪ 정의
- 두 벡터 사이의 **각도**
- 크기(magnitude)보다 **방향(direction)**에 집중

~~~text
cosine similarity = (x · y) / (||x|| ||y||)
cosine distance = 1 − cosine similarity
~~~

### ▪ 특징
- 고차원 데이터에 강함
- 텍스트 / 임베딩 / 희소 벡터에 적합
- 길이가 아닌 “패턴” 비교

### ▪ 주의점
- 절대 크기 정보는 무시됨
- 수치 데이터에서는 해석 주의

---

## 6️⃣ Mahalanobis Distance (분포 고려 거리)

### ▪ 핵심 아이디어
> “각 축이 독립이고 동일한 스케일이라는  
> Euclidean 가정을 버리자”

~~~text
d(x, y) = √ (x − y)ᵀ Σ⁻¹ (x − y)
~~~

- Σ : 공분산 행렬

### ▪ 특징
- 변수 간 상관관계 반영
- 분산이 큰 방향은 덜 중요하게
- 타원형 분포에 적합

### ▪ 한계
- 공분산 추정이 필요
- 소표본 / 고차원에서 불안정

---

## 7️⃣ Scaling이 왜 중요한가?

거리 기반 알고리즘에서,
> “스케일이 큰 변수가  
> 거리 계산을 지배한다”

### ▪ 예시
- 키(cm): 150 ~ 190
- 연봉(만원): 3000 ~ 10000

👉 연봉이 거리 계산을 독점  
👉 키 정보는 무시됨

---

### ▪ 결론
- Euclidean / Manhattan / Minkowski
- **Scaling 거의 필수**
- StandardScaler / MinMaxScaler 등 사용

---

## 8️⃣ 거리 선택 = 데이터에 대한 가정

| 거리 | 암묵적 가정 |
|---|---|
| Euclidean | 구형 군집, 동일 스케일 |
| Manhattan | 축 이동 중요 |
| Cosine | 방향이 중요 |
| Mahalanobis | 분포·상관 구조 중요 |

👉 **거리 선택은 알고리즘 선택보다 앞선다**

---

## 🎯 이 파일의 핵심 메시지

- Clustering은 거리 정의 문제다
- 거리 = 데이터 해석 방식
- 잘못된 거리 → 아무리 좋은 알고리즘도 실패
- Scaling은 선택이 아니라 전제

---

## 🔗 다음 파일 예고
👉 `02_kmeans.md`  
- 거리 개념이 실제 알고리즘에서  
  어떻게 사용되는지 확인

