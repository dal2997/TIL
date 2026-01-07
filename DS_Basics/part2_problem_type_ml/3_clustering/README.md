# Chapter04 – Clustering (Unsupervised Learning) 🧩

이 챕터는 **정답(label)이 없는 데이터에서 구조를 찾는 방법**을 다룬다.  
“잘 맞췄는가?”가 아니라 **“어떻게 묶였고, 왜 그렇게 보이는가?”**가 핵심이다.

---

## 📌 이 챕터에서 다루는 핵심 질문

- Clustering에서 말하는 “거리”란 무엇인가?
- 거리 정의가 바뀌면 결과는 왜 달라지는가?
- K-means는 왜 잘 되는 경우와 망하는 경우가 명확한가?
- 비선형 구조는 왜 K-means로 안 되는가?
- Noise(이상치)를 포함한 데이터는 어떻게 다뤄야 하는가?
- 정답이 없는데, 군집 결과를 어떻게 평가할 수 있는가?

---

## 📂 폴더 구조

~~~text
Chapter04_Clustering/
├── README.md
├── 01_distance_and_similarity.md
├── 02_kmeans.md
├── 03_hierarchical_clustering.md
├── 04_spectral_clustering.md
├── 05_dbscan.md
├── 06_hdbscan.md
└── 07_clustering_evaluation.md
~~~

---

## 🧠 개념 파일 설명

### 01_distance_and_similarity.md
- Clustering의 출발점: **거리**
- Euclidean / Manhattan / Minkowski
- Cosine Distance (고차원·텍스트 데이터)
- Mahalanobis Distance (분산·상관 고려)
- **Scaling이 왜 필수에 가까운가**
- “거리 선택 = 데이터에 대한 가정”

---

### 02_kmeans.md
- K-means 알고리즘 동작 과정
- 중심점 업데이트의 의미
- K 선택 문제 (Elbow의 직관과 한계)
- 초기값 문제 (random vs k-means++)
- 언제 잘 되고, 언제 실패하는가

---

### 03_hierarchical_clustering.md
- Hierarchical clustering의 사고방식
- Dendrogram 해석
- Linkage 방식 차이
- K-means와의 근본적 차이
- 계산 복잡도 이슈

---

### 04_spectral_clustering.md
- Graph 기반 클러스터링 관점
- Similarity graph / Laplacian matrix
- Cut 개념 (min-cut, normalized cut)
- 비선형 구조에서 강한 이유
- “거리 공간이 아닌 연결 구조를 본다”

---

### 05_dbscan.md
- Density 기반 클러스터링
- ε (epsilon), minPts 개념
- Core / Border / Noise point
- 이상치(outlier)를 자연스럽게 처리하는 이유
- K-means와의 결정적 차이

---

### 06_hdbscan.md
- DBSCAN의 한계
- Density hierarchy 개념
- ε를 직접 정하지 않는 이유
- 클러스터 안정성(stability)
- 실무에서 DBSCAN보다 선호되는 이유

---

### 07_clustering_evaluation.md
- 비지도 학습에서 평가가 어려운 이유
- Internal vs Relative 평가
- Silhouette Score 해석
- Dunn Index 개념
- Elbow 방법의 한계
- “평가 지표를 맹신하면 안 되는 이유”

---

## 🎯 이 챕터를 관통하는 메시지

- Clustering은 **정답을 맞추는 문제가 아니다**
- 거리 정의와 가정이 결과를 결정한다
- 하나의 알고리즘이 모든 데이터에 맞을 수 없다
- 시각화 + 도메인 해석이 필수다

---

## 🔗 이전 / 다음 챕터 연결

- 이전: Chapter03 – Interpretable ML & XAI  
  → “설명 가능한 모델”
- 이후: Chapter05 – (다음 비지도 / 차원축소 챕터와 연결 가능)

---

