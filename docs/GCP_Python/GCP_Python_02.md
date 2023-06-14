---
layout: default
title: 2. Ubuntu Command
parent: GCP & Python
nav_order: 2
---

# 2. Ubuntu Command

| Code or Command | Description | Example |
| --- | --- | --- |
| python3 -V | 현재 python 버젼 확인 |
| wget https://bootstrap.pypa.io/get-pip.py | pip install 주소 |
| sudo python3 get-pip.py | pip install |
| mkdir [folder] |  파일 생성 | mkdir test |
| ls | 하위 파일 조회 |
| cd [folder] | 해당 폴더로 이동 | cd test, cd ..|
| vi [file] | 해당 파일 열기(없는 파일이면 생성) | vi test.py |
| :wq | 파일 내에서 저장 후 나가기 |
| :q | 파일 내에서 그냥 나가기 |
| python3 [file] | 해당 파이썬 파일 실행 | python3 test.py |
| ctrl + C | 실행중인 파일 실행종료 |
| nohup python3 [file] & | nohup 프로세스 유지, & 백그라운드 실행 | nohup python3 test.py & |
| nohup python3 -u [file] | python파일의 print된 문구와 로그를 실시간으로 저장. | nohup python3 -u test.py |
| cat nohup.out | 로그 확인 |
| cat /dev/null > nohup.out | nohup.out에 저장된 로그 내용 삭제 |
| ps, ps -A | 현재 실행중인 프로세스 ID 출력. ps -A는 전체 출력 |  ps -ef '|' grep [file] |
| kill -9 [PID] | 실행중인 백그라운드 파일 종료 | kill -9 1234 |
| rm [file] | 파일 삭제 | rm test.py |
| pidof python3 | 실행중인 모든 프로세스 PID 출력 |
