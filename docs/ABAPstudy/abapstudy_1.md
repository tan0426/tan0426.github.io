---
layout: default
title: 1. system 변수와 수 정리
parent: abapstudy
nav_order: 1
---
# 1. system 변수와 수 정리

![Untitled](./abapstudy_img/abapstudy_1.png)
![Untitled](./abapstudy_img/abapstudy_2.png)

### SY-CPROG : runtime, main program
호출된절차에서는호출프로그램의이름이고, 그렇지 않으면 현재 프로그램의 이름. 외부에서 호출된 프로시저가 다른 외부 프로시저를 호출하는 경우 SY-CPROG는 첫번째 기본 프로그램의 이름을 유지하고, 추가 호출자의 기본 프로그램 이름을 지정하지 않음.

### SY-REPID : program, name of abap/4 program
최근 abap 프로그램 이름.
