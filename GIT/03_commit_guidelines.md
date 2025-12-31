# ✍️ 커밋 규칙 (메시지 / 단위 / 습관)

## ✅ 커밋은 “미래의 나에게 남기는 메모”
좋은 커밋 = 되돌리기 쉬움 + 리뷰 쉬움 + 협업 쉬움

---

## 🏷️ 커밋 메시지 태그 (수업/실습에서 쓴 것들)
- `feat:` 기능 추가
- `fix:` 버그 수정
- `docs:` 문서 추가/수정
- `refactor:` 리팩토링
- `conf:` 설정 변경

예시:
- `conf: Create .gitignore`
- `docs: Create README.md`
- `feat: Add fizz and buzz features`

---

## 🧱 커밋 단위 원칙 (핵심)
- 한 커밋 = 한 논리
- 관련 없는 변경은 분리
- 커밋 메시지로 “무엇을/왜”가 보이게

---

## 🔧 amend (마지막 커밋 수정)
``` bash
git commit --amend
```

주의:
- 이미 push한 커밋을 amend하면 히스토리 꼬일 수 있음(팀 작업 시 특히)

