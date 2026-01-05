# ⏪ 되돌리기(Undo) 치트키 모음

## 1) 작업 파일 되돌리기 (restore)
``` bash
git restore main.py
```

## 2) 스테이징만 취소(unstage)
``` bash
git restore --staged main.py
```

## 3) stash (임시 저장)
``` bash
git stash
git stash list
git stash pop
```

## 4) 마지막 커밋 수정(amend)
``` bash
git commit --amend
```

---

## 내가 쓰는 기준
- “파일만 되돌림” → restore
- “add만 취소” → restore --staged
- “브랜치 이동해야 함” → stash
- “방금 커밋이 아쉬움” → amend

## checkout 대신 restore 사용

Git 최신 버전에서는 역할이 분리됨

- 브랜치 이동 → `git switch`
- 파일 되돌리기 → `git restore`

### 파일 변경 되돌리기

~~~
git restore file.txt
~~~

### staging 취소

~~~
git restore --staged file.txt
~~~

