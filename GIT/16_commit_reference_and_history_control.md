# Git 커밋 지정(리셋/리베이스/체리픽/쇼/디프) — 상대참조 `~` / `^` 완전정리

> 목표: “원하는 커밋을 정확히 찍어서” 확인(show), 비교(diff), 되돌림(reset), 정리(rebase), 골라담기(cherry-pick)까지 한 번에.

---

## 0) 커밋을 지정하는 방법 (치트키)

### A. 해시로 지정
- 전체 해시 대신 **앞 7~10자리**만으로 보통 충분

~~~bash
git show a1b2c3d
~~~

### B. 이름으로 지정 (브랜치/태그)
- `main`, `feature/login`, `v1.2.0` 같은 이름도 “커밋 포인터”임

~~~bash
git show main
git show v1.2.0
~~~

### C. 상대참조로 지정 (오늘의 핵심)
- `HEAD~N` : 첫 번째 부모 기준으로 N칸 뒤
- `HEAD^N` : N번째 부모 선택(머지에서 갈림길 선택)

~~~bash
git show HEAD~2
git show HEAD^1
git show HEAD^2
~~~

---

## 1) 상대참조 연산자 핵심: `~` vs `^`

### 1-1) `~` (틸드): “직선으로 몇 칸 뒤”
- `HEAD~1` = `HEAD~` = HEAD의 부모
- `HEAD~2` = 부모의 부모

~~~bash
git show HEAD~1
git diff HEAD~1..HEAD
~~~

✅ 특징
- 머지가 있어도 기본은 **first-parent(첫 번째 부모)만 따라감**
- “최근 몇 커밋 전”을 찍을 때 가장 많이 씀

---

### 1-2) `^` (캐럿): “부모를 고르는 연산자”
#### 일반 커밋(부모 1개)
- `HEAD^` = `HEAD~1` (사실상 동일)

~~~bash
git show HEAD^
~~~

#### 머지 커밋(부모 2개 이상)일 때 진짜 유용
- `HEAD^1` : 첫 번째 부모 (보통 merge 시점의 **기존 브랜치**)
- `HEAD^2` : 두 번째 부모 (보통 merge로 **들어온 브랜치**)

~~~bash
git show HEAD^1
git show HEAD^2
~~~

✅ 팁: 머지 커밋에서 “들어온 브랜치 쪽만 보고 싶다” → `^2`

---

### 1-3) 조합 가능: `^` 다음에 `~`
- “갈림길을 고른 뒤, 그쪽에서 몇 칸 더 뒤”

~~~bash
git show HEAD^2~3
~~~

---

## 2) 실전 1: 확인(Show / Log) — 원하는 커밋을 눈으로 보기

### 2-1) 특정 커밋 상세 보기
~~~bash
git show HEAD~1
git show a1b2c3d
~~~

### 2-2) 그래프로 히스토리 보기(머지 구조 확인)
~~~bash
git log --oneline --graph --decorate -n 30
~~~

### 2-3) “어느 부모가 ^1 / ^2인지” 확인하는 법
1) 머지 커밋을 찾고
2) 각각을 찍어본다

~~~bash
git show HEAD
git show HEAD^1
git show HEAD^2
~~~

---

## 3) 실전 2: 비교(Diff) — 범위를 정확히 찍기

### 3-1) 두 커밋 사이 변경 비교
~~~bash
git diff A..B
~~~

예)
~~~bash
git diff HEAD~3..HEAD
~~~

### 3-2) “이 커밋 하나가 가져온 변경분만” 보고 싶을 때
- 특정 커밋이 도입한 변경을 보고 싶다면 보통 `show`가 더 직관적

~~~bash
git show HEAD~1
~~~

### 3-3) 파일 하나만 비교
~~~bash
git diff HEAD~1..HEAD -- path/to/file.txt
~~~

---

## 4) 실전 3: 되돌림(Reset) — 커밋을 기준으로 “어디까지” 되돌릴지

> reset은 **로컬에서 히스토리 자체를 되돌리는 강한 명령**이라 shared 브랜치(main 등)에서는 특히 주의.

### 4-1) reset 옵션 3종 (개념)
- `--soft`  : 커밋만 취소, 스테이징은 유지
- `--mixed` : (기본) 커밋 취소 + 스테이징 취소, 워킹은 유지
- `--hard`  : 커밋/스테이징/워킹 전부 해당 시점으로 (변경 날아감)

### 4-2) 최근 1개 커밋만 “커밋만 취소(내용은 남김)”
~~~bash
git reset --soft HEAD~1
~~~

### 4-3) 최근 1개 커밋 취소하고 스테이징도 내리기(내용은 남김)
~~~bash
git reset HEAD~1
~~~

### 4-4) 특정 시점으로 완전 되돌리기(주의)
~~~bash
git reset --hard HEAD~2
~~~

✅ 안전망(진짜 추천)
- reset 전에 현재 상태를 임시로 태그/브랜치로 박아두기

~~~bash
git branch backup-before-reset
~~~

---

## 5) 실전 4: 히스토리 정리(Rebase) — 커밋을 “정돈”하기

### 5-1) 다른 브랜치 위로 내 브랜치를 올려 정리
~~~bash
git switch my-branch
git rebase main
~~~

### 5-2) 인터랙티브 리베이스(커밋 메시지 수정/스쿼시/순서 변경)
- 최근 N개 커밋을 대상으로 정리

~~~bash
git rebase -i HEAD~5
~~~

여기서 자주 하는 것
- `pick` 그대로 둠
- `reword` 커밋 메시지 수정
- `squash` 여러 커밋 합치기

⚠️ 주의
- 이미 원격에 푸시해서 **다른 사람이 기반으로 쓰는 브랜치**를 rebase하면 충돌/혼란 큼

---

## 6) 실전 5: 골라담기(Cherry-pick) — 특정 커밋만 가져오기

### 6-1) 커밋 하나만 가져오기
~~~bash
git cherry-pick a1b2c3d
~~~

### 6-2) 범위로 여러 커밋 가져오기
- `A..B`는 “A 다음부터 B까지” 의미 (A는 제외)

~~~bash
git cherry-pick A..B
~~~

예)
~~~bash
git cherry-pick HEAD~3..HEAD
~~~

### 6-3) 충돌 나면
~~~bash
git status
# 충돌 해결 후
git add .
git cherry-pick --continue
~~~

---

## 7) 커밋 범위 표기 정리 (자주 헷갈리는 부분)

### `A..B`
- A는 제외, B까지 포함(사실상 “A 이후 ~ B”)

### `A...B` (세 점)
- 두 브랜치의 “서로 다른” 커밋들(대칭차)
- 로그 비교할 때 유용

~~~bash
git log --oneline A..B
git log --oneline A...B
~~~

---

## 8) Detached HEAD 주의 (checkout/switch로 커밋 직접 이동할 때)

### 커밋 자체로 이동(확인용)
~~~bash
git switch --detach HEAD~2
~~~

- 여기서 커밋하면 브랜치에 매달리지 않은 상태가 될 수 있음
- 작업을 계속할 거면 새 브랜치로 붙이는 게 안전

~~~bash
git switch -c temp-work
~~~

---

## 9) “최근에도 많이 쓰나?” 결론

- ✅ `~` / `^`는 지금도 현역 (커밋 지정의 표준 문법)
- ✅ 특히 `show`, `diff`, `reset`, `rebase -i`, `cherry-pick`에서 자주 사용
- 🔸 브랜치 이동은 요즘 `git switch` 권장이라 `checkout` 빈도는 줄었지만,
  상대참조 문법 자체는 그대로 많이 쓴다

---

## 10) 미니 치트시트 (바로 복붙용)

~~~bash
# 히스토리 보기
git log --oneline --graph --decorate -n 30

# 커밋 확인
git show HEAD~1
git show HEAD^2

# 비교
git diff HEAD~3..HEAD
git diff HEAD~1..HEAD -- path/to/file.txt

# reset
git reset --soft HEAD~1
git reset HEAD~1
git reset --hard HEAD~2

# rebase
git rebase main
git rebase -i HEAD~5

# cherry-pick
git cherry-pick a1b2c3d
git cherry-pick HEAD~3..HEAD
~~~

