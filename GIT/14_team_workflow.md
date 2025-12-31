# 🧭 팀 작업 루틴 (덜 꼬이는 흐름)

## 시작 10초 루틴
``` bash
git status
git branch
git remote -v
```

## main 최신화(안전)
``` bash
git switch main
git pull --ff-only
```

## 작업 브랜치 생성
``` bash
git switch -c feature/<task>
```

## PR 전 main 동기화(충돌 줄이기)
``` bash
git fetch origin main
git merge FETCH_HEAD
```

## 머지 후 정리
``` bash
git switch main
git branch -D feature/<task>
```

---

## 🚫 절대 금지(너가 실제로 겪었던 사고)
- 한 폴더에서 remote만 바꿔가며 다른 프로젝트 작업 ❌  
레포 1개 = 폴더 1개

