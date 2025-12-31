# 🔀 Merge vs Rebase

## ✅ Merge
- 두 브랜치의 히스토리를 합친다
- merge commit이 생길 수 있음
- 팀 협업에서 가장 흔하고 안전

``` bash
git switch main
git merge feature/fizzbuzz
```

---

## ✅ Rebase
- 내 브랜치 커밋을 “다시 얹어” 히스토리를 깔끔하게 만든다
- 충돌이 반복될 수 있고, 히스토리 재작성 위험

``` bash
git switch feature/fizzbuzz
git rebase main
```

---

## 🔥 실전 룰
- 이미 공유(push)한 커밋은 rebase로 재작성하지 말기
- 개인 로컬 정리용으로만 rebase를 적극 사용

