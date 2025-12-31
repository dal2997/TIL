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

