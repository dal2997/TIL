# ✅ TIL/LINUX/ 4번부터 “전체 리눅스 명령어” 정리 (기초~응용)

> 너는 이미 1~3번 파일을 작성했으니, 아래는 `04~08`로 “전반적인 명령어”를 **기초 + 세세 + 응용 + 내가 중요하게 보는 포인트**까지 한 번에 정리해줄게.  
> (원하면 다음 턴에 `09~`로 네트워크/디버깅/서비스(systemd)/로그 심화까지 이어감)

---

## 4) TIL/LINUX/04_shell_navigation_and_help.md

# Shell / Navigation / Help (기초~응용)

## 0. 용어 한 방 정리
- **쉘(shell)**: 명령어를 입력받아 실행하는 프로그램 (bash, zsh 등)
- **경로(path)**:
  - 절대경로: `/home/song`
  - 상대경로: `./src`, `../`
- **현재 위치**: `pwd`
- **홈**: `~` (= `/home/<user>`)

---

## 1. 이동/탐색 기본 7종
~~~bash
pwd                # 현재 경로
ls                 # 목록
ls -al             # 숨김 포함 + 상세
cd /path           # 이동
cd ..              # 상위 이동
cd -               # 직전 경로로 이동
tree -L 2          # 트리 형태(없으면: sudo apt install tree)
~~~

### ls 옵션 자주 쓰는 조합
~~~bash
ls -l              # 권한/소유/크기/시간
ls -a              # 숨김 파일(.bashrc 등)
ls -h              # 사람이 읽기 좋은 단위 (K/M/G)
ls -t              # 최신 수정순
ls -alh            # 실무 기본 세트
~~~

---

## 2. 도움말 / 문서 검색 (man, help, apropos)
### “이 명령 뭔데?” 1순위
~~~bash
man ls             # 매뉴얼
ls --help          # 간단 옵션 설명
help cd            # bash 내장 명령 도움말(cd 같은 건 man보다 help가 정확)
~~~

### man 검색
~~~bash
man -k printf      # 키워드 포함 문서 찾기(= apropos)
man -k '^printf'   # printf로 '시작'하는 문서만(정규식)
~~~

### man 섹션(중요)
- `1` : 커맨드(예: `printf(1)`)
- `3` : 라이브러리 함수(예: `printf(3)`)

~~~bash
man 1 printf
man 3 printf
~~~

---

## 3. 히스토리/재실행/편의 기능 (강추)
~~~bash
history            # 명령 기록
!123               # 123번 명령 재실행
!!                 # 직전 명령 재실행
sudo !!            # “아 sudo 붙일걸” 할 때
Ctrl + R           # 히스토리 검색(진짜 개꿀)
~~~

### 별칭(alias)으로 생산성 올리기
~~~bash
alias ll='ls -alh'
alias gs='git status'
~~~

영구 적용:
~~~bash
echo "alias ll='ls -alh'" >> ~/.bashrc
source ~/.bashrc
~~~

---

## 4. 파이프/리다이렉션/서브쉘 (기초에서 제일 중요)
### 표준입출력
- `>`  : 덮어쓰기
- `>>` : 이어쓰기
- `|`  : 왼쪽 출력 → 오른쪽 입력

~~~bash
echo "hi" > a.txt
echo "again" >> a.txt
cat a.txt | grep hi
~~~

### 오류 출력도 같이 받기
~~~bash
cmd > out.txt 2> err.txt
cmd > all.txt 2>&1
cmd 2>&1 | tee all.txt      # 화면에도 보고 파일에도 저장
~~~

---

## 5. 환경변수 / PATH (기초지만 실무 핵심)
~~~bash
echo $SHELL
echo $PATH
which python
type -a python
~~~

임시 설정:
~~~bash
export MYVAR=hello
echo $MYVAR
~~~

---

## 내가 중요하게 보는 포인트 ✅
- `man/help`로 스스로 확인하는 습관이 “실력 차이”
- `|`, `>`, `2>&1`, `tee`는 리눅스 작업의 70%
- `Ctrl+R`, `sudo !!`는 생산성 치트키

---

## 5) TIL/LINUX/05_files_and_text_tools.md

# Files & Text Tools (파일 다루기 + 텍스트 처리)

## 1. 파일/디렉터리 생성/복사/이동/삭제
~~~bash
touch a.txt                 # 빈 파일 생성
mkdir dir                   # 폴더 생성
mkdir -p a/b/c              # 중간 폴더까지 한 번에
cp src.txt dst.txt          # 복사
cp -r dir1 dir2             # 폴더 복사
mv old new                  # 이름 변경/이동
rm file                     # 삭제
rm -r dir                   # 폴더 삭제
rm -rf dir                  # 강제 삭제(주의)
~~~

⚠️ 실무 팁:
- `rm -rf` 치기 전에 `pwd` 확인 + 대상 `ls`로 재확인

---

## 2. 파일 내용 보기 (cat/less/head/tail)
~~~bash
cat file                    # 전체 출력(짧은 파일용)
less file                   # 스크롤/검색(긴 파일용)  (q로 종료)
head -n 20 file             # 앞 20줄
tail -n 50 file             # 뒤 50줄
tail -f app.log             # 로그 실시간 보기
~~~

---

## 3. 검색/필터링 (grep)
~~~bash
grep "ERROR" app.log
grep -n "ERROR" app.log     # 줄번호 포함
grep -i "error" app.log     # 대소문자 무시
grep -r "TODO" .            # 현재 폴더 아래 재귀 검색
grep -E "err|fail" app.log  # 정규식(OR)
~~~

---

## 4. “텍스트 처리 4대장” (응용의 핵심)
### sort / uniq / wc
~~~bash
sort file
sort -u file                # 중복 제거 포함
uniq -c file                # 연속 중복 카운트(대개 sort와 같이 씀)
wc -l file                  # 줄 수
~~~

### cut / awk / sed
- `cut`: 구분자로 필드 뽑기(간단)
- `awk`: 컬럼 처리/계산(강력)
- `sed`: 치환/삭제(치트)

~~~bash
cut -d',' -f1 data.csv                 # CSV 1열
awk -F',' '{print $1, $3}' data.csv    # 1열 3열 출력
sed 's/foo/bar/g' file                 # foo -> bar 전체 치환
sed -n '1,10p' file                    # 1~10줄만 출력
~~~

---

## 5. 찾기 (find) — 실무에서 grep보다 더 씀
~~~bash
find . -name "*.py"
find . -type f -size +100M
find . -type f -mtime -3               # 최근 3일 수정
find . -type f -name "*.log" -delete   # 삭제(주의)
~~~

find + grep 조합:
~~~bash
find . -name "*.py" -print0 | xargs -0 grep -n "TODO"
~~~

---

## 6. 링크(중요!)
- 심볼릭 링크: 바로가기 같은 느낌
~~~bash
ln -s /path/to/real target_link
ls -l
~~~

---

## 내가 중요하게 보는 포인트 ✅
- 긴 파일은 `less`가 정답 (`cat`은 실수 유발)
- `grep -r`, `find`는 프로젝트에서 무조건 씀
- `awk/sed`는 “한 방 변환”에 최강 (익숙해지면 생산성 폭발)

---

## 6) TIL/LINUX/06_permissions_ownership_and_umask.md

# Permissions / Ownership / umask (기초+실무)

## 1. 권한 문자열 읽기 (ls -l)
예:
~~~text
-rw-r----- 1 song developers 123 Jan  6  file.txt
~~~

### 앞 1글자
- `-` 파일
- `d` 디렉터리
- `l` 링크

### 뒤 9글자: user/group/others
- `rw-` : 읽기/쓰기
- `r-x` : 읽기/실행(디렉터리는 “진입 가능”)
- `---` : 없음

---

## 2. chmod (심볼/숫자)
### 심볼 방식
~~~bash
chmod o-r file
chmod g+w dir
chmod u+x script.sh
~~~

### 숫자 방식(암기하면 빠름)
- r=4, w=2, x=1
- 예: 7=4+2+1(rwx), 5=4+1(r-x)

~~~bash
chmod 755 dir     # rwx r-x r-x
chmod 644 file    # rw- r-- r--
chmod 700 secret  # rwx --- ---
~~~

---

## 3. chown/chgrp (소유자/그룹)
~~~bash
sudo chown song:developers /work
sudo chown -R song:developers /work
sudo chgrp developers /work
~~~

---

## 4. 디렉터리 권한: r/w/x의 진짜 의미(시험에 나옴)
- 디렉터리에서:
  - `r`: 목록 보기 가능
  - `x`: **진입/탐색 가능(cd 가능)** ← 핵심
  - `w`: 생성/삭제 가능(단, x도 필요)

---

## 5. setgid / sticky bit (응용)
### setgid(디렉터리) — “그룹 고정”
~~~bash
chmod g+s /work
~~~
의미:
- 그 폴더 안에서 만든 파일이 **생성자 그룹이 아니라 폴더의 그룹**을 따름

### sticky bit(/tmp가 대표) — “남의 파일 삭제 방지”
~~~bash
chmod +t /shared
~~~

---

## 6. umask (실무에서 은근 중요)
“새 파일/폴더가 만들어질 때 기본 권한”을 결정

~~~bash
umask
~~~

예:
- umask 022 → 파일 644, 폴더 755 (흔한 기본값)
- umask 027 → 파일 640, 폴더 750 (조금 더 보안)

---

## 내가 중요하게 보는 포인트 ✅
- `cd Permission denied` = 대개 **디렉터리 x 권한** 문제
- 협업 폴더는 `setgid`로 그룹 고정하면 사고 줄어듦
- `/tmp` 구조(sticky bit)는 보안 기초

---

## 7) TIL/LINUX/07_process_jobs_and_monitoring.md

# Process / Jobs / Monitoring (기초~운영감각)

## 1. 프로세스 보기
~~~bash
ps aux
top
htop                     # 없으면: sudo apt install htop
~~~

### 특정 프로세스 찾기
~~~bash
ps aux | grep python
pgrep -a python
~~~

---

## 2. 종료/시그널 (kill)
~~~bash
kill <PID>              # 기본 SIGTERM(정상 종료 요청)
kill -9 <PID>           # SIGKILL(강제 종료, 최후 수단)
pkill -f "uvicorn"      # 패턴으로 죽이기(주의)
~~~

---

## 3. 백그라운드/잡 제어(진짜 중요)
~~~bash
sleep 1000 &
jobs
fg %1
bg %1
Ctrl+Z                  # 일시정지
disown %1               # 터미널 꺼도 유지
~~~

---

## 4. 리소스/디스크/메모리
~~~bash
free -h
df -h
du -sh .
du -sh * | sort -h
~~~

---

## 5. 포트 확인(개발자 필수)
~~~bash
ss -lntp
sudo lsof -i :8000
~~~

---

## 내가 중요하게 보는 포인트 ✅
- `kill -9`는 습관적으로 쓰면 안 됨(데이터 손상 위험)
- `jobs/fg/bg/Ctrl+Z`는 CLI 작업의 기본 체력
- 포트/프로세스 확인(`ss`, `lsof`)은 서버 디버깅의 시작

---

## 8) TIL/LINUX/08_packages_archives_and_remote.md

# Packages / Archives / Remote (기초~실무)

## 1. 패키지 관리(ubuntu)
~~~bash
sudo apt update
sudo apt install tree
sudo apt remove tree
apt list --installed | grep tree
~~~

---

## 2. 압축/해제 (tar, gzip, zip)
### tar.gz 만들기/풀기
~~~bash
tar -czf backup.tar.gz mydir/     # 압축 생성
tar -xzf backup.tar.gz            # 압축 해제
~~~

### zip
~~~bash
zip -r backup.zip mydir/
unzip backup.zip
~~~

---

## 3. 다운로드/요청 (curl/wget)
~~~bash
curl -I https://example.com
curl -L -o file.zip https://example.com/file.zip
wget https://example.com/file.zip
~~~

---

## 4. 원격 접속/전송 (ssh/scp) — 개발자 필수
~~~bash
ssh user@server
scp local.txt user@server:/tmp/
scp -r mydir user@server:/tmp/
~~~

---

## 5. 권한 문제 해결 패턴(실전)
### “왜 안 돼?” 체크리스트
1) 내가 누구냐: `whoami`, `id`
2) 경로 권한: `ls -ld /path /path/to/dir`
3) 소유/그룹: `ls -l`
4) 실행권한(x): 스크립트면 `chmod +x`
5) sudo 필요: 시스템 경로면 `sudo`

~~~bash
whoami
id
ls -ld /home/song
~~~

---

## 내가 중요하게 보는 포인트 ✅
- `apt update` 없이 install하면 꼬이는 경우 많음
- `tar -czf / -xzf`는 리눅스 기본기
- 원격 작업은 `ssh/scp`부터 시작

---

# ✅ 다음 확장 (원하면 이어서 만들어줌)
- 09) systemd / 서비스 관리: `systemctl`, `journalctl`
- 10) 로그/디버깅: `dmesg`, `journalctl -xe`, `strace` 기초
- 11) 네트워크 심화: `ip a`, `ip r`, `ping`, `traceroute`, `dig`, `nc`
- 12) 쉘 스크립팅 기초: 변수/조건/반복/함수 + 실전 템플릿

원하면 “너가 요즘 하는 것(uvicorn/fastapi, git, venv, docker 여부)” 기준으로  
09~12를 **실습 예제까지 포함해서** 커리큘럼 형태로 바로 이어갈게.

