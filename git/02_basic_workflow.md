# 🔁 Git 기본 작업 흐름 (Add → Commit → Push)

Git에서 가장 기본이 되는 작업 사이클이다.  
이 흐름만 정확히 이해하면, 대부분의 Git 사용이 설명된다.

---

## ✅ 기본 개념 한 줄 요약
- `git add` : **이번 커밋에 포함할 변경사항 선택**
- `git commit` : 선택된 변경사항을 **스냅샷으로 저장**
- `git push` : 로컬 커밋을 **원격 저장소(GitHub)에 업로드**

---

## 📌 작업 시작 전 습관 (매번)

``` bash
git status
git branch
git remote -v
```

- 현재 브랜치 확인
- 변경사항 존재 여부 확인
- 원격 저장소 연결 상태 확인

---

## 1) 저장소 생성 / 가져오기

### 새 프로젝트 시작

``` bash
git init
```

### 원격 저장소 복제

``` bash
git clone <원격_URL>
```

### 폴더 이름을 바꿔서 clone (실제 사용 예시)

``` bash
git clone https://github.com/dal2997/second-repo.git ./second-repo-copy
```

---

## 2) 작업 → Staging (add)

파일 수정 후 상태 확인

``` bash
git status
```

특정 파일만 스테이징

``` bash
git add main.py
```

모든 변경사항 스테이징

``` bash
git add .
```

### ⚠️ 자주 헷갈리는 포인트
- `git add`는 저장이 아니라 **“이번 커밋에 포함할 변경 선택”**
- add 안 하면 commit에 **절대 포함 안 됨**

---

## 3) 커밋 (commit)

### 메시지 한 줄 커밋

``` bash
git commit -m "docs: update notes"
```

### Vim이 열리는 커밋

``` bash
git commit
```

Vim 기본 조작:
- 입력: `i`
- 저장+종료: `Esc` → `:wq`
- 저장 안 하고 종료: `Esc` → `:q!`

---

## 4) 원격으로 올리기 (push)

``` bash
git push
```

처음 올릴 때 upstream 설정까지 같이:

``` bash
git push -u origin main
```

---

## 🔄 pull vs fetch + merge (내가 쓰는 방식)

`git pull`은 내부적으로 `fetch + merge`를 한 번에 수행한다.  
나는 꼬이는 걸 피하려고 보통 아래처럼 나눠서 사용한다.

``` bash
git fetch origin main
git merge FETCH_HEAD
```

---

## ❌ 자주 했던 실수 메모
- add 안 하고 commit 했다고 착각
- 브랜치 확인 안 하고 작업
- remote 확인 안 해서 엉뚱한 저장소에 push

