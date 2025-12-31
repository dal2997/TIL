# 🪝 pre-commit 훅 (커밋 막히는 원인 1위)

## 설치 흐름
``` bash
pip install pre-commit
pre-commit sample-config > .pre-commit-config.yaml
pre-commit install
```

---

## 내가 계속 당했던 패턴(중요)
훅이 파일을 고치면 → 커밋 중단 → 다시 add 필요

``` bash
git add .
git commit
# 훅이 수정함
git add .
git commit
```

---

## 훅 잠깐 스킵
``` bash
git commit -m "msg" -n
```

---

## 훅 제거/복구
``` bash
rm .git/hooks/pre-commit
pre-commit install
```

---

## Python 버전/환경 꼬일 때
- 가능한 한 프로젝트별 가상환경 사용(venv)
- 훅 캐시 정리:

``` bash
pre-commit clean
pre-commit gc
```

