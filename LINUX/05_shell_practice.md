# ✅ TIL/LINUX/ 05~10 (04 이후 확장: “전체적인” 리눅스 명령어 기초~응용)
# - 04에서 다룬 내용(PS1/컬러/리다이렉션/PATH/alias/스크립트 입문)은 여기서 반복하지 않음
# - 1~3 실습(/work setgid, setuid 등)에서 “다음 단계로 연결되는” 명령어 위주로 확장

---

## 05) TIL/LINUX/05_files_and_directories_advanced.md

# Files & Directories (기초 → 실무형)

## 1) 경로/탐색
~~~bash
pwd
ls
ls -l
ls -alh
cd /path
cd ..
cd -
tree -L 2        # 없으면: sudo apt install tree
~~~

## 2) 생성/복사/이동/삭제
~~~bash
touch a.txt
mkdir dir
mkdir -p a/b/c

cp src.txt dst.txt
cp -r dir1 dir2

mv old new
mv file /tmp/

rm file
rm -r dir
rm -rf dir       # ⚠️ 주의: 강제 삭제
~~~

### ⚠️ rm -rf 사고 방지 루틴(중요)
~~~bash
pwd
ls -alh
# 삭제 대상이 맞는지 확인 후 rm 실행
~~~

## 3) 파일 내용 보기(상황별)
~~~bash
cat file.txt          # 짧은 파일
less file.txt         # 긴 파일(검색: /pattern, 종료: q)
head -n 20 file.txt
tail -n 50 file.txt
tail -f app.log       # 로그 따라가기
~~~

## 4) 파일 메타/타입
~~~bash
file mycat
stat mycat
~~~

## 5) 권한 확인 기본 패턴(실무)
~~~bash
ls -ld /path /path/to/dir
ls -l file
~~~
- 디렉터리 접근 불가(cd Permission denied)는 대부분 **x(실행/진입) 권한 문제**.

---

## 06) TIL/LINUX/06_search_and_text_tools.md

# Search & Text Tools (grep/find + 텍스트 파이프라인)

## 1) grep (파일 내용 검색)
~~~bash
grep "ERROR" app.log
grep -n "ERROR" app.log
grep -i "error" app.log
grep -r "TODO" .                 # 재귀
grep -E "err|fail" app.log        # 정규식 OR
~~~

## 2) find (파일 자체 검색) — grep보다 더 자주 씀
~~~bash
find . -name "*.py"
find . -type f -size +100M
find . -type f -mtime -3          # 최근 3일 수정
~~~

### find + grep 콤보(프로젝트에서 핵심)
~~~bash
find . -name "*.py" -print0 | xargs -0 grep -n "TODO"
~~~

## 3) 정렬/집계
~~~bash
wc -l file.txt
sort file.txt
sort file.txt | uniq
sort file.txt | uniq -c
~~~

## 4) cut/awk/sed (응용 입문)
~~~bash
cut -d: -f1 /etc/passwd
awk -F: '{print $1, $7}' /etc/passwd
sed 's/foo/bar/g' file.txt
sed -n '1,10p' file.txt
~~~

---

## 07) TIL/LINUX/07_archives_packages_and_download.md

# Packages / Archives / Download (실무 기본)

## 1) apt 패키지 관리(Ubuntu)
~~~bash
sudo apt update
sudo apt install tree htop
sudo apt remove tree
apt list --installed | grep tree
~~~

## 2) 압축/해제 (tar 필수)
~~~bash
tar -czf backup.tar.gz mydir/
tar -xzf backup.tar.gz
~~~

## 3) zip
~~~bash
zip -r backup.zip mydir/
unzip backup.zip
~~~

## 4) 다운로드(curl/wget)
~~~bash
curl -I https://example.com
curl -L -o file.zip https://example.com/file.zip
wget -O file.zip https://example.com/file.zip
~~~

---

## 08) TIL/LINUX/08_process_jobs_and_ports.md

# Process / Jobs / Ports (개발/서버 필수)

## 1) 프로세스 확인
~~~bash
ps aux
top
htop     # 없으면: sudo apt install htop
~~~

## 2) 프로세스 찾기
~~~bash
ps aux | grep python
pgrep -a python
~~~

## 3) 종료(시그널)
~~~bash
kill <PID>          # SIGTERM(정상 종료 요청)
kill -9 <PID>       # SIGKILL(최후 수단)
pkill -f "uvicorn"  # 패턴 종료(주의)
~~~

## 4) 잡 제어(터미널 작업 체력)
~~~bash
sleep 1000 &
jobs
fg %1
bg %1
# Ctrl+Z: 일시정지
disown %1           # 터미널 종료해도 유지
~~~

## 5) 포트/리스닝 확인(개발자 필수)
~~~bash
ss -lntp
sudo lsof -i :8000
~~~

---

## 09) TIL/LINUX/09_systemd_and_logs.md

# systemd / Logs (운영 기본)

## 1) 서비스 상태/관리
~~~bash
systemctl status <service>
sudo systemctl start <service>
sudo systemctl stop <service>
sudo systemctl restart <service>
sudo systemctl enable <service>      # 부팅 시 자동 시작
sudo systemctl disable <service>
~~~

## 2) 로그 보기(journalctl)
~~~bash
journalctl -u <service> -n 200
journalctl -u <service> -f            # 실시간
journalctl -xe                         # 최근 에러 중심
~~~

## 3) 커널/부팅 로그
~~~bash
dmesg | tail -n 100
~~~

---

## 10) TIL/LINUX/10_shell_script_next_level.md

# Shell Script (입문 다음 단계: “안전하게 쓰는 법”)

## 1) shebang + 실행권한
~~~bash
#!/usr/bin/env bash
echo "hello"
~~~
~~~bash
chmod u+x script.sh
./script.sh
~~~

## 2) 안전 옵션(강추)
~~~bash
set -euo pipefail
# -e: 에러 나면 종료
# -u: 정의 안 된 변수 사용 시 에러
# -o pipefail: 파이프 중간 실패도 잡기
~~~

## 3) 인자/변수/기본값
~~~bash
name="${1:-world}"
echo "hello, $name"
~~~

## 4) 종료코드(엄청 중요)
~~~bash
command
echo $?     # 직전 명령 exit code (0=성공)
~~~

## 5) 조건문/케이스
~~~bash
if [[ -f "file.txt" ]]; then
  echo "exists"
else
  echo "missing"
fi

case "${1:-}" in
  start) echo "start";;
  stop)  echo "stop";;
  *)     echo "usage: $0 {start|stop}"; exit 1;;
esac
~~~

## 6) 루프(파일/라인 처리)
~~~bash
while IFS=: read -r user _ uid gid gecos home shell; do
  echo "사용자 $user 는 $shell 을 쓰고 $home 을 홈으로 사용"
done < /etc/passwd
~~~

## 7) 따옴표(사고 방지 포인트)
- 변수는 웬만하면 `"${var}"`로 감싸기
- 공백/특수문자 들어간 경로에서 깨짐 방지

---

# ✅ “1~3 실습”과 연결되는 핵심 포인트(요약)
- 협업 폴더 권한: `/work`에 `g+w`와 `g+s(setgid)`를 주면 그룹 협업이 편해짐. :contentReference[oaicite:2]{index=2}
- setuid는 동작 원리 이해용으로 좋지만, 실제 운영에선 보안 리스크가 큼(실습 후 반드시 원복). :contentReference[oaicite:3]{index=3}

