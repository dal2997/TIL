# 🧨 Conflict 해결 (실전 루틴)

## 충돌 마커 의미
``` diff
<<<<<<< HEAD
내 브랜치 내용
=======
상대 브랜치 내용
>>>>>>> feature-branch
```

---

## ✅ merge 충돌 해결 루틴
1) 파일 열어서 마커 제거 + 최종 정답 만들기  
2) add  
3) commit

``` bash
vi main.py
git add main.py
git commit
```

---

## ✅ rebase 중 충돌이면
``` bash
vi main.py
git add main.py
git rebase --continue
```

---

## ✅ “한쪽만 채택 / 둘 다 합치기” 판단
- 한쪽만 채택: 더 최신/정답이 확실할 때
- 둘 다 살리고 수동 합치기: 대부분 이게 정답(내가 자주 만남)

