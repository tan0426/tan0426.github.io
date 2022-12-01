---
layout: default
title: MODULE POOL에서 TABLE CONTROL 학습
parent: abapstudy
nav_order: 18
---
# MODULE POOL에서 TABLE CONTROL 학습

1. 간단한 화면 구성

MAIN PROGRAM에 테이블 컨트롤을 선언해 준다.
```ABAP
CONTROLS TC100 TYPE TABLEVIEW USING SCREEN 100.
```
```ABAP
PROCESS BEFORE OUTPUT.
 MODULE STATUS_0100.
 "ABAP > SCREEN
 LOOP WITH CONTROL TC100.
   MODULE INIT_TC100.
   MODULE MOVE_ABAP_TO_SCREEN.
 ENDLOOP.

PROCESS AFTER INPUT.
"SCREEN > ABAP
 LOOP WITH CONTROL TC100.
   MODULE MOVE_SCREEN_TO_ABAP.
 ENDLOOP.
 MODULE USER_COMMAND_0100.
 ```
 PBO와 PAI에 LOOP을 돌려 한줄씩 적어넣는 느낌인것 같다.
 ```ABAP
 MODULE status_0100 OUTPUT.
 SET PF-STATUS 'STATUS100'.
 SET TITLEBAR 'T100'.

 CLEAR GT_DATA.
 SELECT *
   INTO CORRESPONDING FIELDS OF TABLE GT_DATA
   FROM ZDSUWONT02.

 SORT GT_DATA.

 "테이블 컨트롤 스크롤 생성 -> 뿌려줄 인터널테이블 총 라인수
 DESCRIBE TABLE GT_DATA LINES TC100-LINES.

ENDMODULE.
```
PBO의 버튼설정을 해놓은 모듈에 함께 SELECT를 해서 넣는다.

DESCRIBE TABLE을 해야 테이블에 맞추어 스크롤이 생성되는것 같다.

```ABAP
MODULE init_tc100 OUTPUT.
  LOOP AT SCREEN.
    IF SCREEN-NAME = 'GS_DATA-ZEMPNO'.
      SCREEN-INPUT = 0.
    ENDIF.

    MODIFY SCREEN.
  ENDLOOP.
ENDMODULE.

*&---------------------------------------------------------------------*
*& Module MOVE_ABAP_TO_SCREEN OUTPUT
*&---------------------------------------------------------------------*
*&
*&---------------------------------------------------------------------*
MODULE move_abap_to_screen OUTPUT.
  READ TABLE GT_DATA INTO GS_DATA INDEX TC100-CURRENT_LINE.
ENDMODULE.
*&---------------------------------------------------------------------*
*&      Module  MOVE_SCREEN_TO_ABAP  INPUT
*&---------------------------------------------------------------------*
*       text
*----------------------------------------------------------------------*
MODULE move_screen_to_abap INPUT.
  MODIFY GT_DATA FROM GS_DATA INDEX TC100-CURRENT_LINE.
ENDMODULE.
```
스크린 입력을 0으로 MODIFY해주는 INITIAL 조건 모듈을 지정해 준다.
```ABAP
MODULE init_tc100 OUTPUT.
  LOOP AT SCREEN.
    IF SCREEN-GROUP1 = 'GR1'.
      SCREEN-INPUT = 0.
    ENDIF.

    MODIFY SCREEN.
  ENDLOOP.
ENDMODULE.

이런식으로 그룹 전체를 설정 해 줄 수도 있다.

PBO의 ABAP TO SCREEN 모듈에는 LOOP돌린 데이터 한줄을 CURRENT LINE으로 읽어내고

PAI의  SCREEN TO ABAP모듈에 MODIFY한다.

2. 필수값 설정, SAVE, REFRESH, 특정 필드값에따른 입력 설정

```ABAP
PROCESS BEFORE OUTPUT.
 MODULE STATUS_0100.
 MODULE INIT_SCREEN.
 LOOP WITH CONTROL TC100.
   MODULE MOVE_ABAP_TO_SCREEN.
   MODULE INIT_TC100.
 ENDLOOP.

PROCESS AFTER INPUT.
 LOOP WITH CONTROL TC100.
   MODULE MOVE_SCREEN_TO_ABAP.
 ENDLOOP.
 MODULE EXIT_COMMAND AT EXIT-COMMAND.
 MODULE USER_COMMAND_0100.
 ```
 STATUS_0100모듈에는 버튼설정과 테이블 스크롤을 설정한다. (DESCRIBE TABLE)
 
 ```ABAP
 MODULE init_screen OUTPUT.
  LOOP AT SCREEN.
    IF SCREEN-NAME = 'ZSDATE_LOW'.
      SCREEN-REQUIRED = 1.
    ENDIF.
    MODIFY SCREEN.
  ENDLOOP.
ENDMODULE.
```
INIT_SCREEN은 스크린을 LOOP로 돌려 ZSDATE_LOW 입력부를 REQUIRED로 필수값 설정을 한다.
```ABAP
MODULE init_tc100 OUTPUT.
  LOOP AT SCREEN.
    "사원번호 칼럼 비활성화
    IF SCREEN-NAME = 'GS_DATA-ZEMPNO'.
      SCREEN-INPUT = 0.
    ENDIF.

    "퇴사자 라인 비활성화
    IF GS_DATA-ZDELE IS NOT INITIAL.
      IF SCREEN-GROUP1 = 'GR1'.
        SCREEN-INPUT = 0.
      ENDIF.
    ENDIF.
    MODIFY SCREEN.
  ENDLOOP.
ENDMODULE.
```
INIT_TC100은 ZEMPNO값의 인풋을 0으로 막고, ZELE(퇴사자처리) 가 X로 되어있으면 인풋을 0이로 하는것을 설정한다.
