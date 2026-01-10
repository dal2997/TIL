# 🧩 KMeans 클러스터링 결과 해석 (Marketing Segmentation)
> 파일명 추천(DS_Basics 흐름 기준): `10_KMeans_Marketing_Segmentation.md`  
> 목적: KMeans로 만든 `label`(군집)별 **평균 프로필 표**를 기반으로 고객군을 해석하고, 실행 아이디어까지 정리한다.

---

## 1) 이 표를 읽는 법 (핵심)
- `label` = KMeans가 만든 **클러스터 번호**(숫자 자체엔 의미 없음, 그냥 그룹 ID)
- 각 행 = 해당 label에 속한 고객들의 **평균값**
- `Education_*`, `Marital_Status_*` 처럼 0~1로 보이는 값은 보통 **비율(=원-핫 평균)** 로 해석
  - 예) `Marital_Status_Partner = 1.0` → 그 군집은 거의 전원이 Partner 쪽
  - 예) `Education_Graduate = 0.89` → 그 군집의 89%가 Graduate

---

## 2) 📊 label별 평균 프로필(원본 표)
> (groupby mean 결과)

| label | Year_Birth | Income | Recency | MntWines | MntFruits | MntMeatProducts | MntFishProducts | MntSweetProducts | MntGoldProds | NumDealsPurchases | NumWebPurchases | NumCatalogPurchases | NumStorePurchases | NumWebVisitsMonth | Complain | cust_age | Total_mnt | Children | Education_Graduate | Education_Postgraduate | Education_Undergraduate | Marital_Status_Partner | Marital_Status_Single |
|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| 0 | 1966.57 | 55288.24 | 48.66 | 372.92 | 20.31 | 162.52 | 26.28 | 19.58 | 33.18 | 2.44 | 4.21 | 2.84 | 5.98 | 5.23 | 0.00 | 23.48 | 634.78 | 1.02 | 0.00 | 1.00 | 0.00 | 1.0 | 0.0 |
| 1 | 1969.84 | 52615.57 | 51.15 | 278.52 | 33.20 | 183.38 | 43.94 | 31.51 | 50.84 | 2.27 | 4.00 | 2.83 | 5.76 | 5.25 | 0.01 | 24.21 | 621.40 | 0.87 | 1.00 | 0.00 | 0.00 | 0.0 | 1.0 |
| 2 | 1967.85 | 70686.75 | 47.71 | 538.74 | 60.39 | 354.82 | 90.36 | 65.22 | 84.29 | 2.13 | 5.81 | 5.20 | 8.68 | 3.76 | 0.01 | 24.46 | 1193.82 | 0.54 | 0.89 | 0.00 | 0.11 | 1.0 | 0.0 |
| 3 | 1968.06 | 51554.01 | 46.81 | 341.37 | 22.14 | 165.10 | 33.90 | 24.26 | 40.55 | 2.25 | 4.17 | 2.60 | 5.82 | 5.38 | 0.01 | 24.33 | 627.32 | 0.94 | 0.00 | 0.79 | 0.21 | 0.0 | 1.0 |
| 4 | 1971.94 | 35920.86 | 50.23 | 75.43 | 7.73 | 36.18 | 12.32 | 8.02 | 25.40 | 2.43 | 2.84 | 0.76 | 3.75 | 6.46 | 0.02 | 24.37 | 165.08 | 1.21 | 0.75 | 0.00 | 0.25 | 1.0 | 0.0 |

---

## 3) 🧠 클러스터별 해석(라벨링)
> `Total_mnt(총지출)`, `Income(소득)`, `구매채널 횟수`, `방문(Visits)`을 중심으로 프로필을 잡는 게 직관적이다.

### ✅ label 2 — “고소득 + VIP 헤비 스펜더(매출 핵심)”
- **Income 최고(70,686)**, **Total_mnt 압도적 1위(1,193.82)**
- Store/Catalog/Web 구매 횟수도 높음(특히 Store 8.68, Catalog 5.20)
- `NumWebVisitsMonth`는 낮은 편(3.76) → **많이 구경 안 하고도 구매 전환이 잘 됨**
- Partner 비중 높고, 자녀는 상대적으로 적음(Children 0.54)

➡️ 한 줄 요약: **VIP/충성 고객군 — 리텐션/업셀 최우선**

---

### ✅ label 4 — “저소득 + 저지출 라이트 고객(구경러 가능성)”
- **Income 최저(35,920)**, **Total_mnt 최저(165.08)**
- 구매 횟수도 낮음(Store 3.75 / Catalog 0.76 / Web 2.84)
- 방문은 가장 많음(Visits 6.46) → **관심 대비 구매 전환이 낮을 수 있음**
- Children 가장 높음(1.21) → 가정형/가격 민감 가능

➡️ 한 줄 요약: **저지출·고방문 군 — 전환(쿠폰/혜택) 실험 대상**

---

### ✅ label 0 — “중상 지출 + 균형 구매(카탈로그/매장도 잘 씀)”
- Income 55,288 / Total_mnt 634.78(중상)
- 구매채널이 비교적 고르게 나옴(Web 4.21 / Catalog 2.84 / Store 5.98)
- Partner 비중 높음
- 교육은 Postgraduate가 1.00로 찍혀 있어(표본/전처리 영향 가능) 해석 시 주의

➡️ 한 줄 요약: **중상 소비의 균형형 — 채널 믹스 캠페인 적합**

---

### ✅ label 1 — “싱글 중심 중간 소비층(평균적 구매자)”
- Income 52,615 / Total_mnt 621.40(중간 이상)
- `Marital_Status_Single = 1.0` → 싱글 성향 강함
- 채널 구매도 평균적(Store 5.76, Web 4.00, Catalog 2.83)

➡️ 한 줄 요약: **싱글 중심의 표준 고객군 — 메시지/상품 세분화 테스트 적합**

---

### ✅ label 3 — “싱글 중심 중간 소비 + 학력 mix”
- Income 51,554 / Total_mnt 627.32(중간 이상)
- Single 1.0, 교육은 Postgraduate 0.79 / Undergraduate 0.21
- 구매 패턴은 label 1과 유사

➡️ 한 줄 요약: **싱글 중간 소비층(학력 mix) — label1과 비교 타깃**

---

## 4) 💡 실행 아이디어(제안점)
### 🎯 VIP(label 2)
- 프리미엄 번들/멤버십/고가 제품 추천(업셀)
- 리텐션(재구매 유도), VIP 전용 혜택
- 방문 적고 구매 높음 → **개인화 추천/리마인드** 효율 좋을 가능성

### 🎯 저지출·고방문(label 4)
- 쿠폰/무료배송 임계값/장바구니 리마인드로 전환 개선 실험
- 가격 민감 가정(Income↓, Children↑) → 생활형/가성비 중심 번들

### 🎯 중간층(label 0/1/3)
- 채널별 행동이 다를 수 있으니 A/B 테스트
  - Web 구매 유도 vs Store 유도 vs Catalog 타깃
- label1 vs label3은 비슷해 보이므로,
  - “정말 분리할 가치가 있는지”는 추가 검증 권장(아래 참고)

---

## 5) 체크하면 좋은 추가 검증(추천)
- **클러스터 크기 확인**: 너무 작은 군집이면 해석이 불안정할 수 있음
- **Silhouette만 맹신 금지**: 값이 낮아도(혹은 모양이 달라도) 비즈니스적으로 의미 있는 군집이 나올 수 있음
- **안정성(재현성) 확인**: `random_state` 바꿔도 비슷한 군집이 나오는지
- **해석 우선 변수 선정**: Total_mnt/Income/채널별 구매횟수 중심으로 설명이 가장 깔끔함

---

## 6) (선택) pandas 결과를 Markdown 표로 자동 출력하는 방법
> 다음처럼 하면 groupby 결과를 바로 md로 뽑아서 문서에 붙여넣기 쉽다.

~~~python
summary = round(mkt_df.groupby("label").mean(), 2)
print(summary.to_markdown())
~~~

---

