# Linux 파일 권한(chmod) 기본 실습

## 1. 테스트 파일 생성
~~~bash
pwd
touch readonly.txt
echo "hello" > readonly.txt
cat readonly.txt
~~~

## 2. others 읽기 권한 제거
~~~bash
chmod o-r readonly.txt
~~~

의미:
- others(그 외 사용자)의 read 권한 제거

## 3. 그룹 write 권한 제거
~~~bash
chmod g-w readonly.txt
~~~

## 4. 권한 확인
~~~bash
ls -al readonly.txt
~~~

결과 예시:
~~~text
-rw-r----- 1 song song 6 Jan  6 13:53 readonly.txt
~~~

### 해석
- user(song): read, write
- group: read
- others: 접근 불가

---

## 핵심 요약
- `u / g / o` : user / group / others
- `+ / -` : 권한 추가 / 제거
- `r w x` 조합을 직접 제어 가능

