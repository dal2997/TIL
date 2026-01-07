# 03. Hierarchical Clustering 🌳

이 파일은 **군집을 계층 구조(tree)로 바라보는 방법**을 설명한다.  
K-means처럼 “몇 개로 나눌지”를 먼저 정하지 않아도 된다는 점이 핵심이다.

---

## 1️⃣ Hierarchical Clustering의 기본 아이디어

Hierarchical Clustering(HC)는 다음 질문에서 출발한다.

> “데이터를  
> 가장 비슷한 것부터 차례대로 묶어보면  
> 어떤 구조가 드러날까?”

즉,
- 한 번에 K개로 자르지 않고
- **점진적으로 묶거나 나누는 방식**

---

## 2️⃣ 계층적 구조: Dendrogram

### ▪ Dendrogram이란?
- 군집이 **어떤 순서로 합쳐졌는지**를 보여주는 트리
- y축: 거리(또는 비유사도)
- x축: 데이터 포인트

👉 **어디서 자르느냐에 따라 군집 수가 달라진다**

---

## 3️⃣ Hierarchical Clustering의 종류

### ▪ Agglomerative (Bottom-up) ⭐
- 각 데이터가 하나의 군집으로 시작
- 가장 가까운 것부터 계속 합침
- **실무에서 가장 많이 사용**

---

### ▪ Divisive (Top-down)
- 전체 데이터를 하나로 시작
- 점점 쪼갬
- 계산량 큼, 잘 쓰이지 않음

---

## 4️⃣ Agglomerative Clustering의 단계

### Step 1. 거리 행렬 계산
- 모든 데이터 쌍 간 거리 계산
- Euclidean, Cosine 등 선택

---

### Step 2. 가장 가까운 두 군집 병합
- 단일 데이터 ↔ 데이터
- 데이터 ↔ 군집
- 군집 ↔ 군집

---

### Step 3. 거리 재계산 (Linkage)
- 병합된 군집과 나머지 군집 간 거리 계산

---

### Step 4. 반복
- 하나의 군집만 남을 때까지 반복
- 그 과정이 dendrogram으로 기록됨

---

## 5️⃣ Linkage 방식 (군집 간 거리 정의)

Linkage는
> “군집과 군집 사이의 거리를 어떻게 정의할 것인가”

를 의미한다.

---

### ▪ Single Linkage
- 두 군집 중 **가장 가까운 점**
- 장점: 비선형 구조 포착 가능
- 단점: chaining 현상 (줄줄이 엮임)

---

### ▪ Complete Linkage
- 두 군집 중 **가장 먼 점**
- 장점: 조밀한 군집
- 단점: 이상치에 민감

---

### ▪ Average Linkage
- 모든 점 쌍의 평균 거리
- 타협형, 안정적

---

### ▪ Ward Linkage ⭐
- 군집 병합 시 **분산 증가 최소화**
- K-means와 철학 유사
- Euclidean 거리 전제

---

## 6️⃣ K-means와의 근본적 차이

| 항목 | K-means | Hierarchical |
|---|---|---|
| K 사전 지정 | 필요 | 불필요 |
| 구조 | 평면 | 트리 |
| 거리 정보 | 최종만 사용 | 전체 병합 과정 사용 |
| 계산 복잡도 | O(nk) | O(n³) |

---

## 7️⃣ Hierarchical Clustering의 장점

- K를 미리 정하지 않아도 됨
- 군집 구조를 **시각적으로 이해 가능**
- 작은 데이터셋에서 강력

---

## 8️⃣ Hierarchical Clustering의 한계

- 계산 비용 큼 (대규모 데이터 불리)
- 한번 병합되면 되돌릴 수 없음
- 노이즈에 민감

---

## 🎯 이 파일의 핵심 메시지

- Hierarchical Clustering은  
  **군집을 “과정”으로 본다**
- Dendrogram은 결과이자 설명 도구
- K를 고민하기 전에 구조를 보고 싶을 때 적합

---

## 🔗 다음 파일 예고
👉 `04_spectral_clustering.md`  
- 거리 대신 **그래프 연결 구조**로 군집을 바라보는 방법

