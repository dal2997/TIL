# 🧹 .gitignore (사고 예방 장치)

## ✅ 왜 중요?
- OS/IDE 찌꺼기 파일 커밋 방지
- 비밀키/환경변수(.env) 유출 방지

---

## 예시 템플릿
``` gitignore
# Python
__pycache__/
*.pyc
venv/

# OS
.DS_Store
Thumbs.db

# Secrets
.env
*.pem
```

---

## 🔥 이미 추적된 파일은 ignore에 넣어도 계속 추적됨
캐시 제거 후 다시 add:

``` bash
git rm -r --cached .
git add .
git commit -m "conf: apply .gitignore"
```

