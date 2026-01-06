# 05_shell_practice.md
# Shell 실습 정리 (PS1 / Color / Redirection / PATH / alias / Script)

> 이 문서는 “쉘이 무엇이고 어떻게 커스터마이징/활용하는지”를 실습 위주로 정리한 파일이다.

---

## 1. 현재 쉘 확인

~~~bash
echo $SHELL
~~~

- 현재 사용 중인 쉘 경로 출력
- 예: `/bin/bash`

---

## 2. 프롬프트(PS1) 확인 및 변경

### 현재 프롬프트 확인
~~~bash
echo $PS1
~~~

출력 예:
~~~text
\[\e]0;\u@\h: \w\a\]${debian_chroot:+($debian_chroot)}\u@\h:\w\$
~~~

> 로컬에서는 길고, 원격 서버에서는 보통 짧게 설정되어 있음

---

### 프롬프트 백업

~~~bash
DEFAULT=$PS1
~~~

의미:
> 현재 프롬프트 설정값을 `DEFAULT` 변수에 저장 (백업)

---

### 프롬프트 변경 실습

~~~bash
PS1="\u $ "
~~~

결과:
~~~text
song $
~~~

~~~bash
PS1="Hello $ "
~~~

~~~bash
PS1="\u@\t: \w # "
~~~

결과 예:
~~~text
song@15:14:07: ~ #
~~~

---

### 원래대로 복구

~~~bash
PS1=$DEFAULT
~~~

> 이 변경은 **현재 터미널 세션에서만** 유지된다.

---

## 3. ANSI 컬러 출력

### 잘못된 예

~~~bash
echo "/e[31mHello"
~~~

→ 그냥 문자열로 출력됨

---

### 올바른 방법

~~~bash
echo -e "\e[31mHello"   # 빨강
echo -e "\e[33mHello"   # 노랑
echo -e "\e[32mHello"   # 초록
~~~

문제:
> 색을 초기화하지 않으면 **터미널 전체 색이 계속 바뀜**

---

### 반드시 초기화 코드 필요

~~~bash
echo -e "\e[33mHello\e[0m"
~~~

---

### 배경색

~~~bash
echo -e "\e[44mHello\e[0m"
~~~

---

### 글자색 + 배경색 동시에

~~~bash
echo -e "\e[31;46mHello\e[0m"
~~~

> ⚠️ 구분자는 반드시 `;` (세미콜론)

---

### PuTTY 설정

- 상단 우클릭 → Change Settings → Colours
- **Allow terminal to specify ANSI colours** 체크되어 있어야 색 출력됨

---

## 4. 리다이렉션 (>, >>, 2>&1)

### 덮어쓰기 / 이어쓰기

~~~bash
echo "hello" > hello.txt
echo "hi" >> hello.txt
cat hello.txt
~~~

결과:
~~~text
hello
hi
~~~

---

### /tmp 실습

~~~bash
ls /tmp
ls /tmp/*
~~~

에러:
~~~text
Permission denied
~~~

---

### 정상출력 + 에러 출력 같이 저장

~~~bash
ls /tmp/* > hello.txt 2>&1
~~~

의미:
- `>` : stdout(1) → hello.txt
- `2>&1` : stderr(2) → stdout(1)과 같은 곳으로

> 즉, **정상 출력 + 에러 메시지 전부 파일에 저장**

---

## 5. PATH 환경변수

### 현재 PATH 확인

~~~bash
echo $PATH
~~~

---

### 현재 디렉터리도 실행 경로에 추가

~~~bash
PATH=$PATH:./
echo $PATH
~~~

이후:

~~~bash
mycat readonly.txt
./mycat readonly.txt
~~~

둘 다 실행됨.

---

### PATH 원래대로 복구

~~~bash
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
~~~

이후:

~~~bash
mycat readonly.txt
~~~

~~~text
Command 'mycat' not found
~~~

---

## 6. alias

~~~bash
alias
alias ..="cd .."
alias ...="cd ../.."
~~~

---

### 영구 저장

~~~bash
nano ~/.bashrc
touch ~/.bash_aliases
alias
echo 'alias ..="cd .."' > ~/.bash_aliases
echo 'alias ...="cd ../.."' >> ~/.bash_aliases
cat ~/.bash_aliases
~~~

---

## 7. while read 실습 (/etc/passwd)

### 여러 줄 버전

~~~bash
while IFS=: read -r F1 F2 F3 F4 F5 F6 F7
do
  echo "사용자 $F1는 $F7 쉘을 사용하고 $F6 홈디렉토리를 사용합니다."
done
~~~

---

### 한 줄 버전

~~~bash
while IFS=: read -r F1 F2 F3 F4 F5 F6 F7 ; do echo "사용자 $F1는 $F7 쉘을 사용하고 $F6 홈디렉토리를 사용합니다."; done < /etc/passwd
~~~

---

## 8. 쉘 스크립트 기초

### 스크립트 작성

~~~bash
vi test1.sh
~~~

내용:
~~~bash
echo "hello"
ls -l
~~~

---

### 실행 안 됨

~~~bash
test1.sh
~~~

~~~text
command not found
~~~

---

### bash로 실행

~~~bash
bash test1.sh
~~~

---

### 실행권한 부여

~~~bash
chmod u+x test1.sh
~~~

---

### 직접 실행

~~~bash
./test1.sh
~~~

---

## 9. 핵심 요약

- `PS1` = 프롬프트 모양
- `DEFAULT=$PS1` = 프롬프트 백업
- `\e[31;46m` = ANSI 컬러 (세미콜론 필수)
- `>`, `>>`, `2>&1` = 출력 제어
- `PATH` = 실행 파일 검색 경로
- alias = 명령어 단축
- 스크립트는:
  - `bash script.sh` 또는
  - `chmod +x` 후 `./script.sh`

---

## 이 파일의 목적

> “쉘이 단순히 명령 치는 곳이 아니라,  
> **환경 / 자동화 / 출력 제어 / 커스터마이징**의 중심이라는 것을 이해하는 것”

