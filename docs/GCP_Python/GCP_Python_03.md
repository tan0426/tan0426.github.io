---
layout: default
title: 3. TMUX
parent: GCP & Python
nav_order: 3
---

# 3. TMUX

리눅스에서 하나의 창이 아닌 여러 창을 함께 사용 할 때 사용하는 것. 리눅스 원격 연결이 꺼져도 서버가 꺼지지 않는 이상 tmux로 돌려놓은 코드는 다운되지 않는다.

gcp에서 nohup이 자꾸 안먹어서 사용해보았다. (확인 중)

| Code or Command | Description | Example |
| --- | --- | --- |
| tmux | tmux에 들어간다 tmux 입력 후 python3 [filename]을 하면 해당 프로그램 실행 되고 바로 창을 끄면 된다. | tmux -> python3 test.py|
| tmux ls | 실행중인 tmux 세션 확인 |
| tmux a -t [session_number] | 해당 번호 tmux 들어감 | tmux a -t 0 |
| tmux kill-session -t [session_number] | 특정 세션 강제 종료 | tmux kill-session -t 0 |
