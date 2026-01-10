# setuid 실습과 실행 권한 이해

## 1. cat 바이너리 복사
~~~bash
cp /bin/cat .
mv cat mycat
~~~

~~~bash
ls -al mycat
~~~

기본 상태:
~~~text
-rwxr-xr-x 1 song song ...
~~~

---

## 2. 권한 없는 파일 접근 시도 (실패)
~~~bash
user2$ ./mycat readonly.txt
~~~

~~~text
Permission denied
~~~

---

## 3. setuid 설정
~~~bash
chmod u+s mycat
ls -l mycat
~~~

~~~text
-rwsr-xr-x 1 song song ...
~~~

`s` 의미:
- 실행 시 **파일 소유자(song)의 권한으로 실행**

---

## 4. setuid 적용 후 실행 (성공)
~~~bash
user2$ ./mycat readonly.txt
~~~

~~~text
hello
~~~

---

## 5. setuid 제거
~~~bash
chmod u-s mycat
~~~

다시 실행:
~~~bash
user2$ ./mycat readonly.txt
~~~

~~~text
Permission denied
~~~

---

## 핵심 요약
- setuid는 **매우 강력 → 보안 리스크**
- 일반 바이너리엔 거의 쓰지 않음
- 시스템 명령어(passwd 등)에서만 제한적으로 사용

