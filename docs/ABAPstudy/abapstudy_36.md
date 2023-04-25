---
layout: default
title: 36. SIMPLE ALV (SALV)
parent: abapstudy
nav_order: 36
---

# 36. SIMPLE ALV (SALV)

ALV를 매우 간단하게 만들 수 있는 기능.

### SALV 예시

```abap
FORM SALV.
  DATA : LO_SALV_TAB TYPE REF TO CL_SALV_TABLE.

  DATA : LO_COLUMNS TYPE REF TO CL_SALV_COLUMNS_TABLE,
         LO_COLUMN TYPE REF TO CL_SALV_COLUMN.

  DATA : ORF_1 TYPE REF TO CX_ROOT.
  
  "SALV의 FACTORY 호출
  CALL METHOD CL_SALV_TABLE=>FACTORY
    IMPORTING
      R_SALV_TABLE = LO_SALV_TAB
    CHANGING
      T_TABLE      = GT_DISPLAY2.

  TRY .
      "COLUMN 너비 최적화
      LO_COLUMNS = LO_SALV_TAB->GET_COLUMNS( ).
      LO_COLUMNS->SET_OPTIMIZE( ABAP_TRUE ).
      
      "SALV DISPLAY
      LO_SALV_TAB->DISPLAY( ).
    CATCH CX_SY_ZERODIVIDE INTO ORF_1.
      MESSAGE 'ERROR1' TYPE 'S' DISPLAY LIKE 'E'.
    CATCH CX_ROOT INTO ORF_1.
      MESSAGE 'ERROR2' TYPE 'S' DISPLAY LIKE 'E'.
  ENDTRY.
ENDFORM.                    " SALV
```

## 1. 열 너비 최적화

```abap
DATA : LO_COLUMNS TYPE REF TO CL_SALV_COLUMNS_TABLE,
       LO_COLUMN TYPE REF TO CL_SALV_COLUMN.

LO_COLUMNS = LO_SALV_TAB->GET_COLUMNS( ).
LO_COLUMNS->SET_OPTIMIZE( ABAP_TRUE ).
```

## 2. 필드명 설정

SALV 클래스의 COLUMNS_TABLE을 참조한 LO_COLUMNS, COLUMN을 참조한 LO_COLUMN, TABLE 자체를 참조한 LO_SALV_TAB를 선언해 준다.

```abap
DATA : LO_SALV_TAB TYPE REF TO CL_SALV_TABLE.

DATA : LO_COLUMNS TYPE REF TO CL_SALV_COLUMNS_TABLE,
       LO_COLUMN TYPE REF TO CL_SALV_COLUMN.
```

필드명을 지정해주기 위해서는 우선
LO_COLUMNS는 SALV_TAB 클래스의 GET_COLUMNS 메소드를 호출하여 불러와 COLUMNS_TABLE을 만들어준다.
그다음 이 테이블에서 각 필드들을 GET_COLUMN으로 불러 온 후 SET_TEXT를 해 준다.

```abap
TRY .
    LO_COLUMNS = LO_SALV_TAB->GET_COLUMNS( ).

    LO_COLUMN = LO_COLUMNS->GET_COLUMN( 'ZCODE' ).
    LO_COLUMN->SET_LONG_TEXT( '코드값' ).
    LO_COLUMN->SET_MEDIUM_TEXT( '코드값' ).
    LO_COLUMN->SET_SHORT_TEXT( '코드값' ).

    LO_COLUMNS->SET_OPTIMIZE( ABAP_TRUE ).
    LO_SALV_TAB->DISPLAY( ).
    
  CATCH CX_SY_ZERODIVIDE INTO ORF_1.
    MESSAGE 'ERROR1' TYPE 'S' DISPLAY LIKE 'E'.
  CATCH CX_ROOT INTO ORF_1.
    MESSAGE 'ERROR2' TYPE 'S' DISPLAY LIKE 'E'.
ENDTRY.
```