# Linux Users & Groups 실습 정리

## 1. 사용자 / 그룹 생성
~~~bash
sudo adduser user2
sudo addgroup developers
~~~

## 2. 그룹에 사용자 추가
~~~bash
sudo usermod -aG developers song
sudo usermod -aG developers user2
~~~

- `-aG` : 기존 그룹 유지한 채 추가
- 재로그인 필요 (or `newgrp`)

## 3. 사용자 정보 확인
~~~bash
id
~~~

출력 예시:
- uid / gid
- 소속 그룹들 확인 가능

---

## 4. 홈 디렉터리 접근 권한 이슈
~~~bash
ls -ld /home /home/song
~~~

- `/home/song` 이 `700`이면
  → 다른 유저(user2)는 접근 불가
- 강의 환경과 다른 이유

### 해결 (강의와 동일하게)
~~~bash
sudo chmod 755 /home/song
~~~

---

## 핵심 요약
- `cd`는 **읽기(r)가 아니라 실행(x) 권한**이 필요
- 홈 디렉터리 권한에 따라 다른 유저 접근 가능/불가
- 강의 ≠ 내 로컬 환경 → 권한부터 확인

