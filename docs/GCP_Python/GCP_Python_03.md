---
layout: default
title: 3. my python program running process
parent: GCP & Python
nav_order: 3
---

# 3. my python program running process

우선 제일 먼저 해당 python 파일이 있는 폴더에 들어가 주어야 한다.

### 1. 실행중인 process를 찾는다.

```sh
$ pidof python3
```

을 하면 내 python program과 이 program log를 실시간으로 nohup.out에 넣어주는 프로그램 2가지 process가 나올것이다.

### 2. 이 두 PID를 kill 해 준다.

```sh
$ kill -9 [PID code]
```

### 3. 로그를 확인 후 리셋 해 준다.

```sh
$ cat nohup.out
$ cat /dev/null > nohup.out
```

### 4. 다시 재실행

```sh
$ nohup python3 upbit_cointrade.py &
$ nohup python3 -u upbit_cointrade.py &

$ cat nohup.out
start
```

