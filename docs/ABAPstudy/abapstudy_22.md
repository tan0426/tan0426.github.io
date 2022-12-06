---
layout: default
title: SUB SCREEN 생성
parent: abapstudy
nav_order: 22
---
# SUB SCREEN 생성
            
![Untitled](./abapstudy_img/abapstudy_24.png)

SCREEN 100 내에 ALV와 PARAMETERS를 함께 넣기 위해 SUBSCREEN을 이용했다.

SELECT SCREEN INCLUDE에 다음과 같이 SUBSCREEN을 생성한다.

```abap
*&---------------------------------------------------------------------*
*& Include          ZDKTJ_WORKPLAN_ALV_SCR
*&---------------------------------------------------------------------*
SELECTION-SCREEN BEGIN OF SCREEN 1004 AS SUBSCREEN.
  SELECTION-SCREEN BEGIN OF BLOCK BLOCK1 WITH FRAME TITLE TEXT-A01.
    PARAMETERS : P_MCODE TYPE ZTJ_WORKPLAN-MITEMCODE MATCHCODE OBJECT ZSH_ITEM_TEAM1.
  SELECTION-SCREEN END OF BLOCK BLOCK1.
SELECTION-SCREEN END OF SCREEN 1004.
```

여기에서 해당 PARAMETERS에 SEARCH HELP를 넣어주고 싶으면 MATCHCODE OBJECT를 이용한다. 이를 이용하기 위해서는 TCODE-SE11에서 SEARCH HELP를 생성해야 한다. SEARCH HELP에 넣기위한 ELEMENT들도 만들어주어야 한다.

![Untitled](./abapstudy_img/abapstudy_26.png)

SEARCH HELP롤 가져 올 테이블을 해당 필드에도 ELEMENT를 지정해주어야 한다.

![Untitled](./abapstudy_img/abapstudy_27.png)

SCREEN LAYOUT에서 SUBSCREEN을 생성할 부위를 지정한다.

![Untitled](./abapstudy_img/abapstudy_25.png)

PBO와 PAI에 SUB SCREEN을 불러와준다.

```abap
PROCESS BEFORE OUTPUT.
 MODULE STATUS_0100.
 MODULE INIT_ALV_DISPLAY.

 CALL SUBSCREEN SUB01 INCLUDING SY-REPID '1004'. "1004는 생성 한 SUBSCREEN 번호

PROCESS AFTER INPUT.
 MODULE USER_COMMAND_0100.

 CALL SUBSCREEN SUB01.

 MODULE EXIT_COMMAND AT EXIT-COMMAND.
```
