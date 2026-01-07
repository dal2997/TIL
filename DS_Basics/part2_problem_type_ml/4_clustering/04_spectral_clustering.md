# 04. Spectral Clustering 🧩

이 파일은 **그래프(Graph) 기반으로 군집을 찾는 방법**을 설명한다.  
Spectral Clustering의 핵심은 “거리”가 아니라 **연결 구조(connectivity)**다.

---

## 1️⃣ Spectral Clustering은 왜 필요한가?

K-means, Hierarchical Clustering은 모두
- 거리 공간(distance space)
- 군집을 “뭉쳐 있는 덩어리”로 가정

하지만 현실 데이터는 종종:
- 비선형 구조
- 꼬인 형태
- 링 / 달 모양

👉 이런 구조는 **거리 기반 방법으로 잘 안 된다**

그래서 나온 질문:
> “점과 점 사이의 **연결 관계**를 보면 어떨까?”

---

## 2️⃣ 기본 아이디어 한 줄 요약

> “데이터를 그래프로 바꾼 뒤,  
> **잘 끊어지는 지점**을 기준으로 나누자”

즉,
- 데이터 → 그래프
- 군집 → 그래프의 subgraph

---

## 3️⃣ Step 1. Similarity Graph 만들기

### ▪ 데이터 → 그래프 변환
- 각 데이터 = 노드(node)
- 유사한 데이터끼리 엣지(edge) 연결
- 엣지에는 가중치(weight) 부여

---

### ▪ Graph 구성 방식

#### 1. Fully Connected Graph
- 모든 노드가 연결
- 계산량 큼

#### 2. ε-neighborhood Graph
- 거리 ≤ ε 인 경우만 연결
- 밀도 차이에 민감

#### 3. k-NN Graph ⭐
- 각 노드에서 가장 가까운 k개만 연결
- 실무에서 가장 많이 사용

---

### ▪ 가중치 계산 (Gaussian Kernel)

~~~text
w(x, y) = exp( − ||x − y||² / (2σ²) )
~~~

- 가까울수록 가중치 큼
- 멀수록 거의 0

---

## 4️⃣ Step 2. Graph Laplacian

그래프 구조를 수학적으로 다루기 위해
다음 행렬들을 만든다.

- W : 인접 행렬 (weight matrix)
- D : 차수 행렬 (degree matrix)
- L : Laplacian = D − W

👉 이 Laplacian이 **Spectral Clustering의 핵심**

---

## 5️⃣ Step 3. Graph Cut 개념

### ▪ Cut이란?
- 그래프를 두 부분으로 나눌 때
- 끊어지는 엣지 가중치의 합

~~~text
Cut(A, B) = A와 B 사이 엣지 가중치 합
~~~

---

### ▪ 문제점 (Minimum Cut)
- 아주 작은 군집이 생기기 쉬움

---

### ▪ 해결책: Normalized Cut ⭐
- 군집 내부 연결도 함께 고려
- 균형 잡힌 분할 유도
[O
👉 Spectral Clustering은  
**Normalized Cut 문제를 푸는 과정**으로 볼 수 있다.

---

## 6️⃣ Step 4. 고유값 분해 (Spectral)

- Laplacian 행렬 고유값 분해
- 작은 고유값에 대응하는 고유벡터 사용
- 이 벡터 공간에서
  → **K-means 수행**

👉 핵심:
> “원래 공간이 아니라  
> **그래프 스펙트럼 공간에서 군집화**”

---

## 7️⃣ 왜 비선형 군집에 강한가?

- 거리 대신 연결 구조 사용
- 군집 내부는 촘촘히 연결
- 군집 사이는 느슨하게 연결

👉 모양이 꼬여 있어도
**연결만 잘 분리되면 군집 가능**

---

## 8️⃣ Spectral Clustering의 장단점

### ✅ 장점
- 비선형 구조에 매우 강함
- 거리 기반 방법의 한계 극복
- 군집 모양 가정이 거의 없음

---

### ❌ 단점
- 그래프 구성 비용 큼
- 파라미터 (k, σ) 민감
- 대규모 데이터에 비효율적

---

## 🎯 이 파일의 핵심 메시지

- Spectral Clustering은 **그래프 분할 문제**
- 거리보다 **연결 구조**가 중요
- 비선형 군집에서 압도적으로 강력
- 계산 비용을 감당할 수 있을 때 선택

---

## 🔗 다음 파일 예고
👉 `05_dbscan.md`  
- 거리도, K도 아닌  
- **밀도(density)**로 군집을 정의하는 방법

