# 📁 GIT 폴더 사용법 (내 기준 / 복습 최적화)

이 폴더는 **날짜별 일기(TIL)** 가 아니라,  
TIL에서 뽑아낸 내용을 **주제별로 다시 찾기 쉽게** 정리한 “지식 저장소”다.

---

## ✅ 이 폴더에서 지키는 규칙

### 1) 날짜 금지
- 날짜별 기록은 `TIL/251231.md` 같은 형태로만 둔다.
- GIT 폴더는 “주제별”만 쌓는다.

### 2) 파일명 = 찾는 키워드
- 파일명만 보고 “여기 열면 나오겠다”가 떠야 한다.
- 예) `12_fork_workflow.md` → fork/origin/upstream이 다 들어있어야 함

### 3) 복붙 안정성 규칙(가장 중요)
- 코드블록은 반드시 이렇게 쓴다:

``` bash
git status
git branch
```

- 닫는 ``` 다음에는 **항상 빈 줄 1줄**
- 리스트는 항상 `- ` (하이픈 + 공백)

---

## 📌 추천 루틴 (내가 자주 헤매서 만든 흐름)

### 작업 시작 10초 체크
``` bash
git status
git branch
git remote -v
```

- 현재 브랜치 확인
- 변경사항 확인
- 원격 연결 확인

### main 최신화(안전)
``` bash
git switch main
git pull --ff-only
```

### 작업 브랜치 생성
``` bash
git switch -c feature/<task>
```

### PR 전 동기화(충돌 줄이기)
``` bash
git fetch origin main
git merge FETCH_HEAD
```

---

## 🧨 “내가 실제로 당했던 사고” 체크리스트

- add 안 하고 commit 했다고 착각
- `FETCH_HEAD` 오타로 시간 낭비
- 한 폴더에서 remote만 바꿔가며 다른 프로젝트 작업 → 히스토리 꼬임
- pre-commit이 파일을 고쳤는데 add 다시 안 해서 커밋 실패 반복

