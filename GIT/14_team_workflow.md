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


## 주요 Git 브랜치 전략 요약

### Git Flow
- develop / release / hotfix 포함
- 릴리즈 단위 개발에 적합

### GitHub Flow
- main 중심
- PR + CI 기반
- 잦은 배포에 적합

### GitLab Flow
- GitHub Flow + 환경/릴리즈 브랜치

### Trunk Based Development
- main(trunk)에 매우 자주 병합
- feature flag 필수

### One Flow
- Git Flow 단순화
- develop 제거, main 중심
