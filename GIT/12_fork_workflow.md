# 🍴 Fork 협업: origin / upstream 완전 정리

## ✅ 왜 포크(Fork)를 하냐?
- 원본 레포를 보호(직접 push 못 하게)
- 권한 없는 사람도 기여 가능
- PR로만 반영 → 리뷰/검증 가능

---

## 용어 고정
- `origin` = **내 fork 저장소**
- `upstream` = **원본(팀) 저장소**

---

## remote 확인
``` bash
git remote -v
```

출력 예시(네 케이스):
``` bash
origin  https://github.com/dal2997/251231-100prisoners.git (fetch)
origin  https://github.com/dal2997/251231-100prisoners.git (push)
upstream        https://github.com/End-of-2025b/251231-100prisoners.git (fetch)
upstream        https://github.com/End-of-2025b/251231-100prisoners.git (push)
```

---

## upstream 추가
``` bash
git remote add upstream https://github.com/End-of-2025b/251231-100prisoners.git
```

## origin URL 수정(잘못 연결했을 때)
``` bash
git remote set-url origin https://github.com/dal2997/251231-100prisoners.git
```

---

## 🔥 원본(upstream)에서 변경 가져와서 내 fork(origin)에 반영 (정석 루틴)
1) upstream에서 가져오기(fetch)
2) 내 main에 합치기(merge)
3) 내 fork로 올리기(push)

``` bash
git switch main
git fetch upstream main
git merge FETCH_HEAD
git push origin main
```

---

## 자주 하는 실수
- upstream을 origin이라고 착각해서 fetch/push 방향을 반대로 함
- main이 아닌 브랜치에서 upstream main을 merge하려다 꼬임

## origin / upstream 개념 정리

- origin : **내 GitHub 레포**
- upstream : **원본(공식) 레포**

### remote 확인

~~~
git remote -v
~~~

### 원본 레포 최신 내용 반영

~~~
git fetch upstream
git merge upstream/main
git push origin main
~~~

⚠️ remote 이름 `upstream`과
⚠️ 브랜치 추적 개념 `upstream`은 **완전히 다른 개념**


