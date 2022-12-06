---
layout: default
title: 11. 모듈풀 학습
parent: abapstudy
nav_order: 11
---
# 11. 모듈풀 학습

ANDROID STUDIO로 모바일 어플리케이션 개발 공부 할때는 화면을 HTML로 구성하고,

버튼의 기능 등등을 자바로 따로 코딩을 해서 구성을 했었는데, 아밥에서도 비슷한 방식으로 진행하는 것 같다.

다만 그 개념에 대해 깊이 있게 책을 읽어본다던지 한 것은 아니지만, 약간 개념이 다른것 같아 혼동되는 점이 있어 그 부분을 확실하게 공부 할 필요가 있는 것 같다.

아밥의 화면 구성은 다음과 같다. 다음과 같은 구분을 적어줘야 적절하게 화면에 실행 되므로 꼭 적어주는게 좋다.

(INITIOALIZATION과 START OF SELECTION을 적지 않고 적으니 구분이 안되서 그런지 화면이 안떴다 ㅋ)

# 1. 스크린을 생성하지 않은 화면 구성법
1-1. 데이터 선언

1-2. INITIALIZATION

 화면 실행 전에 미리 한번만 띄워 놓는 부분인것 같다.

```abap
INITIALIZATION.
  S_ZSDATE-HIGH = SY-DATUM. "첫번째 조건. 오늘날짜 값
  APPEND S_ZSDATE.
```

SELECT OPTIONS 등에 미리 값을 넣어둔다던지,

```abap
INITIALIZATION.
  TOP-OF-PAGE.
  WRITE : /20(20) SY-ULINE.
  WRITE : /20(1) SY-VLINE NO-GAP.
  WRITE : 21(18) '부양 가족 수' NO-GAP CENTERED.
  WRITE : 39(1) SY-VLINE NO-GAP.
  WRITE : /20(20) SY-ULINE.
```
화면을 띄울 때 필드명을 띄우는것 처럼 미리 지정을 해둔다던지 하는 방식인것같다.

1-3. AT SELECTION SCREEN OUTPUT

SELECTION SCREEN으로 지정된 값들이 특정 행위를 통해 변하는? 것을 나타내는 부분인것 같다.

```abap
AT SELECTION-SCREEN OUTPUT.
  "LOOP AT SCREEN. -> MODIFY SCREEN
  "3. CHECKBOX 체크시 사원면, 성별 SELECT-OPTIONS 필드가 비활성화
  LOOP AT SCREEN.
    IF P_CHK = 'X'. "USER-COMMAND를 설정 해주었기때문에 먹힘
      IF SCREEN-GROUP1 = 'ID1'.
        SCREEN-INPUT = 0. "해당 그룹 필드들 비활성화
      ELSE.
        SCREEN-INTENSIFIED = 1.
      ENDIF.
    ENDIF.
    MODIFY SCREEN.
  ENDLOOP.
```
1-4. START-OF-SELECTION

SELECTION SCREEN부분 등등에서 값을 할당하고 화면에 띄워주는 부분을 어떻게 띄워 줄 것인이 SELECT해서 값을 넣는 부분. 
```abap
START-OF-SELECTION.
SELECT *
  INTO CORRESPONDING FIELDS OF TABLE GT_ZSUWONT02
  FROM ZDSUWONT02
  WHERE ZSDATE IN S_ZSDATE
  AND ZEMPNO IN S_ZSDATE
  AND ZEMPNM IN S_ZEMPNM
  AND ZDEPT IN S_ZDEPT
  AND ZGENDER IN S_ZGEN.
```
```abap
SET PF-STATUS 'STATUS1000'.
```
이 부분에 스테이터스 값으로 버튼을 구성하여 화면이 뜬 후 버튼을 넣어 둘 수 있다. 이 버튼은 END-OF-SELECTION에서 실행을 설정 할 수 있다.

1-5. END-OF-SELECTION

보통은 안적어줘도 SELECT 한 후 끝에 WRITE등등으로 뿌려주기만 하면 뜨긴 하는 부분이다.

```abap
AT USER-COMMAND.
  CASE SY-UCOMM.
    WHEN 'LEAV'.
      LEAVE PROGRAM.
    WHEN 'BACK'.
      LEAVE TO SCREEN 0.
  ENDCASE.
```
이 부분에 USER-COMMAND를 이용하여 화면이 뜬 후 생성할 버튼의 기능을 설정 할 수 있다.

# 2. 모듈풀 화면 구성
# PBO, PAI


```abap
PROCESS BEFORE OUTPUT.
  MODULE STATUS_0100.
PROCESS AFTER INPUT.
  MODULE USER_COMMAND_0100.
```
간단히 생각하면 스크린(여기서는 100번)을 띄우기 전은 PBO, 스크린 띄운 후는 PAI인듯 한데 실습을 심화하다보면 약간 헷갈릴 때가 있다. 일단 그렇게 생각하고 학습을 진행하는게 좋을것같다.

실행

모듈풀은 스크린을 띄우려면 꼭 트랜잭션코드를 생성해서 띄워야 한다.

그렇게 하지 않았더니 그냥 실행하면 화면이 뜨지 않는다.

2-1. PBO
```abap
*&---------------------------------------------------------------------*
*& Module STATUS_0100 OUTPUT
*&---------------------------------------------------------------------*
*&
*&---------------------------------------------------------------------*
MODULE status_0100 OUTPUT.
 SET PF-STATUS 'STATUS100'.
 SET TITLEBAR 'T100'.
ENDMODULE.
```
기본적으로는 스크린 띄우기 전 버튼과 제목을 생성한다.

2-2. PAI

```abap
*&---------------------------------------------------------------------*
*&      Module  USER_COMMAND_0100  INPUT
*&---------------------------------------------------------------------*
*       text
*----------------------------------------------------------------------*
MODULE user_command_0100 INPUT.
  CASE OK_CODE.
    WHEN 'BACK'.
      LEAVE TO SCREEN 0.
  	WHEN 'EXIT'.
      LEAVE PROGRAM.
  ENDCASE.
ENDMODULE.
```
기본적으로는 스크린을 띄운 후 버튼의 기능을 구성함.

```abap
MODULE user_command_0100 INPUT.
  CASE OK_CODE.
    WHEN 'BACK'.
      LEAVE TO SCREEN 0.
    WHEN 'DISP'.

      SELECT SINGLE ZEMPNM ZDEPT ZSDATE
        INTO ( ZDSUWONT02-ZEMPNM, ZDSUWONT02-ZDEPT, ZDSUWONT02-ZSDATE )
        FROM ZDSUWONT02
        WHERE ZEMPNO = ZDSUWONT02-ZEMPNO.

*  	WHEN OTHERS.
  ENDCASE.
  CLEAR OK_CODE.
ENDMODULE.
```
스크린 띄운 후 버튼을 누를 경우 정보가 뜨는 행위를 이와같이 SELECT하여 나타 낼 수 있다.
