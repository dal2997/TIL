# 05. CNN & Feature Learning 🖼️

이 파일은 **왜 Fully Connected NN이 이미지에서 실패했고,  
왜 CNN이 ‘feature extractor’가 되었는지**를 정리한다.

---

## 5.1 FC Network의 구조적 한계
- 모든 픽셀을 일렬로 펼침
- **공간 정보(위치, 이웃 관계) 소실**
- 파라미터 수 폭증
- 작은 위치 변화에도 예측 불안정

👉 이미지의 “구조”를 전혀 활용 못함

---

## 5.2 CNN의 핵심 질문
> “이미지는  
> **가까운 픽셀끼리 의미가 있지 않을까?**”

이 질문이 CNN의 출발점이다.

---

## 5.3 Convolution의 핵심 아이디어
- **Local connectivity**
  - 가까운 영역만 본다
- **Weight sharing**
  - 같은 패턴은 어디서든 동일하게 감지
- **Sliding window**
  - 전체 이미지를 훑으며 특징 추출

👉 파라미터 수 ↓  
👉 일반화 성능 ↑

---

## 5.4 Filter는 무엇을 배우는가
- 초반 레이어:
  - edge, 방향, 색 변화
- 중간 레이어:
  - 눈, 코, 질감
- 후반 레이어:
  - 객체, 의미 있는 부분

👉 CNN은 **계층적 feature learner**

---

## 5.5 Feature Map의 의미
- 하나의 filter → 하나의 feature map
- “이 패턴이 어디에 얼마나 있는가”

👉 값이 크다 = 해당 패턴 강함

---

## 5.6 Pooling의 역할
- 중요 특징만 남김
- 위치 변화에 둔감
- 계산량 감소

종류:
- Max Pooling
- Average Pooling
- Global Average Pooling (GAP)

---

## 5.7 CNN 구조의 전형적 흐름
~~~text
Input
 → Conv + Activation
 → (Pooling)
 → Conv + Activation
 → …
 → Flatten / GAP
 → Fully Connected
 → Softmax
~~~

👉 CNN = **특징 추출기**  
👉 FC = **판단기**

---

## 5.8 왜 CNN이 일반화에 강한가
- 공간 구조 반영
- 파라미터 공유
- 불필요한 자유도 감소

👉 Regularization 효과가 구조에 내재

---

## 5.9 CNN의 한계
- 회전/왜곡에 완전 불변 아님
- 전역 관계 파악 어려움
- 큰 receptive field 필요

→ 이후:
- Deeper CNN
- Skip connection
- Attention / Transformer로 확장

---

## 한 줄 요약
- CNN은 이미지를 “픽셀”이 아니라  
  **구조와 패턴**으로 본다
- Conv는 연산이 아니라 **가정의 구현**
- CNN은 feature engineering을 **모델이 대신하는 방식**

