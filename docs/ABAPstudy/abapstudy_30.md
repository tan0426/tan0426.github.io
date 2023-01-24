---
layout: default
title: 30. BDC (Batch Data Communication)
parent: abapstudy
nav_order: 30
---

# BDC (Batch Data Communication)
BDC는 사용자가 Macro를 사용하여 SAP프로그램을 자동으로 수행하는 것과 같은 형태의 기능과 유사.

즉 단순 반복작업을 최소화하여 백그라운드로 수행 가능하도록 해줌. 
사용자가 직접 프로그램에서 정해진 시나리오대로 작업하는 절차를 레코딩 수행하는 것으로 직접 DB table에 입력 하는것이 아님.

## BDC 구성 요소
![image](./abapstudy_img/abapstudy_40.png)

## 1. BDC 준비물
BDCDATA : SHDB라는 레코딩 정보를 생성하는 프로그램에서 발췌된 value들을 해당 structure에 입력

![image](./abapstudy_img/abapstudy_41.png)

BDCMSGCOLL : BDC수행이 완료 된 후, 성공이나 실패 여부에 대한 Message 정보를 담아주며 "성공 유무에 대한 이유 정보"를 같이 return 해줌.

![image](./abapstudy_img/abapstudy_42.png)

CTUPARAMS
![image](./abapstudy_img/abapstudy_43.png)

DISMODE: MODE 옵션과 동일

UPDMODE: UPDATE 옵션과 동일

CATTMODE: CATT 모드설정

                   ' ' CATT 사용 안 함

                   'N' single-screen control이 없는 CATT

                   'A' single-screen control이 있는 CATT

DEFSIZE:  기본 윈도우 사이즈 설정

RACOMMIT:COMMIT WORK에서 트랜잭션 종료하지 않음.

NOBINPT:Batch Input Mode 사용 안 함.

NOBIEND:DISMODE가 'E' 상태일 때만 설정이 가능하며, 시스템 에레가 발생하면 Background에서 수행 중인 BDC가 Foreground로 전환되고 스크린이 조회 됨.

CTU_PARAMS의 옵션이 DISMODE와 CALL TRANSACTION의 <MODE> 는 같은 기능을 수행하며, 중복해서 사용할 수 없다. UPDMODE 속성도 동일.

<mode> : 조회 모드 설정.

 A   화면을 조회하면서 트랜잭션 수행

 E   에러가 발생할 경우에만 화면 조회

 N   화면을 표시하지 않음

<update> : update mode

 S    Synchronous Update

 A    Asynchronous Update

 L    Local Update

TYPES : GTY_BDC TYPE BDCDATA,

              GTY_MSG TYPE BDCMSGCOLL.
