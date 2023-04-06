---
layout: default
title: 8. SELECT-OPTIONS
parent: abapstudy
nav_order: 8
---
# 8. SELECT-OPTIONS

## SELECT OPTIONS에 SEARCH HELP 달기

```abap
AT SELECTION-SCREEN ON VALUE-REQUEST FOR S_ZEMPNO-LOW.
  PERFORM F4_ZEMPNO.
```
SELECT-OPTIONS의 LOW부분 창에 SEARCH HELP를 지정하는 코드.

```abap
FORM f4_zempno USING PARA.
  DATA : BEGIN OF LS_ZEMPNO,
          ZEMPNO TYPE ZDSUWONT02-ZEMPNO,
          ZEMPNM TYPE ZDSUWONT02-ZEMPNM,
         END OF LS_ZEMPNO,
         LT_ZEMPNO LIKE TABLE OF LS_ZEMPNO.
  DATA : LT_RETURN TYPE TABLE OF DDSHRETVAL,
         LS_RETURN TYPE DDSHRETVAL.
         
  SELECT ZEMPNO ZEMPNM
    INTO CORRESPONDING FIELDS OF TABLE LT_ZEMPNO
    FROM ZDSUWONT02.
    
    "서치헬프 FUNCTION
    CALL FUNCTION 'F4IF_INT_TABLE_VALUE_REQUEST'
      EXPORTING
*       DDIC_STRUCTURE         = ' '
        retfield               = 'ZEMPNO'
*       PVALKEY                = ' '
*       DYNPPROG               = ' '
*       DYNPNR                 = ' '
*       DYNPROFIELD            = ' '
*       STEPL                  = 0
*       WINDOW_TITLE           =
*       VALUE                  = ' '
       VALUE_ORG              = 'S'
*       MULTIPLE_CHOICE        = ' '
*       DISPLAY                = ' '
*       CALLBACK_PROGRAM       = ' '
*       CALLBACK_FORM          = ' '
*       CALLBACK_METHOD        =
*       MARK_TAB               =
*     IMPORTING
*       USER_RESET             =
      tables
        value_tab              = LT_ZEMPNO
*       FIELD_TAB              =
       RETURN_TAB             = LT_RETURN "선택하면 값이 필드에 들어감
*       DYNPFLD_MAPPING        =
     EXCEPTIONS
       PARAMETER_ERROR        = 1
       NO_VALUES_FOUND        = 2
       OTHERS                 = 3
              .
    IF sy-subrc <> 0.
* Implement suitable error handling here
    ENDIF.

ENDFORM.
```

![Untitled](./abapstudy_img/abapstudy_7.png)
![Untitled](./abapstudy_img/abapstudy_8.png)
![Untitled](./abapstudy_img/abapstudy_9.png)

DDSHRETVAL는 서치헬프로 띄우는 전형적인 형식의 테이블 인것같다.

그래서 리턴 받는 테이블(LT_RETURN)은 이 테이블(DDSHRETVAL)을 참고하는 것 같다.

## SELECT-OPTIONS 상위 TITLE (1000 SCREEN) 바꾸기

### 1. PROGRAM CODE 로 TITLE 제어

```abap
INITIALIZATION
  SY-TITLE = 'TITLE TEST'
```

### 2. 속성 이용

ABAP 편집기의 툴바 [이동->속성] 에서 설정.
