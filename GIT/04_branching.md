# 🌿 브랜치(Branch) 기본기

## ✅ 브랜치를 쓰는 이유
- main을 보호
- 기능/실험을 분리
- PR 단위로 리뷰/병합 가능

---

## 기본 명령
브랜치 확인:

``` bash
git branch
git branch -a
git branch -r
```

브랜치 생성/이동:

``` bash
git switch -c feature/fizzbuzz
```

브랜치 삭제(로컬):

``` bash
git branch -D fb
```

처음 push(-u):

``` bash
git push -u origin feature/fizzbuzz
```

---

## 🤯 origin/HEAD -> origin/main 이거 뭐냐?
원격 저장소의 기본 브랜치가 `main`이라는 뜻(원격 포인터)


## 브랜치 이동: checkout vs switch

### git checkout
- 과거의 **만능 명령**
- 브랜치 이동, 파일 복구, 커밋 이동까지 전부 담당
- 기능이 많아 헷갈리기 쉬움

### git switch (권장)
- **브랜치 이동 전용**
- 의도가 명확해서 실수 줄어듦

~~~
git checkout -b abc
git switch -c abc
~~~

### 브랜치가 없을 때 발생하는 에러

~~~
git checkout abc
# error: pathspec 'abc' did not match any file(s) known to git
~~~

→ `abc`라는 브랜치가 존재하지 않기 때문
