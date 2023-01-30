---
layout: default
title: 31. DROP DOWN LIST 
parent: abapstudy
nav_order: 31
---

# 31. DROP DOWN LIST (드롭다운 리스트)

드롭다운 리스트는 PBO에 리스트를 등록하고 FIELD CATALOG에 'DRDN_HNDL'을 '1'로 지정해주어 드롭다운을 생성 할 수 있다.

## DROP DOWN LIST 예시

DB에 저장된 값은 드롭다운 리스트에 추가 하지 않기 위해서 조건을 걸고 PERFROM EROPDOWN_LIST을 구성함.

### PBO
```abap
*&---------------------------------------------------------------------*
*&      Module  ALV_INIT_DISPLAY_0100  OUTPUT
*&---------------------------------------------------------------------*
*       text
*----------------------------------------------------------------------*
MODULE ALV_INIT_DISPLAY_0100 OUTPUT.
  "-- 화면의  GRID가 BOUND되었는지 확인한다.
  IF GR_DOCK1 IS INITIAL.

    "-- GRID의 INSTANCE를 생성한다.
    PERFORM CREATE_INSTANCE_0100.

    "-- GRID의 LAYOUT 속성을 정의한다.
    PERFORM INIT_LAYOUT_0100.

    "-- ALV Standard toolbar button cotrol
*    PERFORM SET_GRID_EXCLUDE_0100.

    "-- ALV Sort
*    perform alv_sort_0100.

    "-- Field Attribute을 사용자의 요구사항에 맞게 변경
    PERFORM APPEND_FIELDCAT_0100.

    "-- ALV Events 등록
    PERFORM REGIST_ALV_EVENT_0100.

    PERFORM DROPDOWN_LIST.

    "-- ALV Display
    PERFORM DISPLAY_ALV_GRID_0100.
*
**    "-- ALV Title
*    *    PERFORM DISPLAY_ALV_TITLE_0100.

  ELSE.

    PERFORM REFRESH_GRID_0100.
    PERFORM REFRESH_GRID2_0100.
    PERFORM REFRESH_GRID3_0100.

  ENDIF.
ENDMODULE.                 " ALV_INIT_DISPLAY_0100  OUTPUT
```

```abap
*&---------------------------------------------------------------------*
*&      Form  DROPDOWN_LIST
*&---------------------------------------------------------------------*
*       text
*----------------------------------------------------------------------*
*  -->  p1        text
*  <--  p2        text
*----------------------------------------------------------------------*
FORM DROPDOWN_LIST .
  DATA : GT_DROPDOWN TYPE LVC_T_DRAL,
         GS_DROPDOWN TYPE LVC_S_DRAL.
  DATA : LT_DATA2 LIKE TABLE OF GT_DATA2 WITH HEADER LINE,
         LT_ZT2302_03 LIKE TABLE OF ZT2302_03 WITH HEADER LINE.

  CLEAR: GT_DROPDOWN, GS_DROPDOWN.

  LOOP AT GT_DATA2.
    READ TABLE GT_DISPLAY2 WITH KEY BUKRS = GT_DATA2-BUKRS.
    IF SY-SUBRC = 0.
    ELSE.
      MOVE-CORRESPONDING GT_DATA2 TO LT_DATA2.
      APPEND LT_DATA2.
      CLEAR : GT_DATA2, LT_DATA2.
    ENDIF.
  ENDLOOP.

  IF LT_DATA2[] IS NOT INITIAL.
    LOOP AT LT_DATA2.

      GS_DROPDOWN-HANDLE    = 1.
      GS_DROPDOWN-INT_VALUE = LT_DATA2-BUKRS.
      GS_DROPDOWN-VALUE     = LT_DATA2-BUKRS.
      APPEND GS_DROPDOWN TO GT_DROPDOWN.
      CLEAR GS_DROPDOWN.

    ENDLOOP.
  ENDIF.

  CLEAR LT_DATA2[].

  CALL METHOD GR_GRID2->SET_DROP_DOWN_TABLE
    EXPORTING
      IT_DROP_DOWN_ALIAS = GT_DROPDOWN.
ENDFORM.                    " DROPDOWN_LIST
```

필드 카탈로그에 드롭다운 필드에 GS_FIELDCAT-DRDN_HNDL = 1 추가.

```abap
*&---------------------------------------------------------------------*
*&      Form  MODIFY_FIELDCATLOG_DATA_0100_2
*&---------------------------------------------------------------------*
*       text
*----------------------------------------------------------------------*
*  -->  p1        text
*  <--  p2        text
*----------------------------------------------------------------------*
FORM MODIFY_FIELDCATLOG_DATA_0100_2 .
  DATA:  LV_TEXT(50).
  CLEAR: LV_TEXT.
  "--- Change Fieldcat.
  LOOP AT GT_FIELDCAT2 INTO GS_FIELDCAT2.

    "-- Change fieldcat Attribute

    GS_FIELDCAT2-COL_OPT = ABAP_TRUE.

    CASE GS_FIELDCAT2-FIELDNAME.
      WHEN 'BUKRS'.
        LV_TEXT = '회사코드'.
        GS_FIELDCAT2-EDIT = 'X'.
        GS_FIELDCAT2-DRDN_HNDL = '1'.
      WHEN 'BUTXT'.
        LV_TEXT = '회사코드명'.
    ENDCASE.

    "-- Common attribute
    IF LV_TEXT IS NOT INITIAL.
      GS_FIELDCAT2-COLTEXT   = LV_TEXT.
      GS_FIELDCAT2-SCRTEXT_L = LV_TEXT.
      GS_FIELDCAT2-SCRTEXT_M = LV_TEXT.
      GS_FIELDCAT2-SCRTEXT_S = LV_TEXT.
    ENDIF.

    MODIFY GT_FIELDCAT2 FROM GS_FIELDCAT2.
  ENDLOOP.

ENDFORM.                    " MODIFY_FIELDCATLOG_DATA_0100_2
```

만약에 테이블이 변경 될 때마다 드롭다운 리스트가 바뀔 경우에는 ALV REFRESH 해주는 PERFORM문 등에 드롭다운리스트 PERFORM문을 넣어주어야 한다.

```abap
*&---------------------------------------------------------------------*
*&      Form  ALV_REFRESH_GRID1
*&---------------------------------------------------------------------*
*       text
*----------------------------------------------------------------------*
*  -->  p1        text
*  <--  p2        text
*----------------------------------------------------------------------*
FORM ALV_REFRESH_GRID .
  CLEAR : GT_DISPLAY[], GT_DISPLAY2[], GT_DISPLAY3[].

  PERFORM SELECCT_DATA.
  PERFORM PROCESS_DATA.
  
  PERFORM DISABLED_STYLE.
  PERFORM DROPDOWN_LIST.
  
  PERFORM REFRESH_GRID_0100.
  PERFORM REFRESH_GRID2_0100.
  PERFORM REFRESH_GRID3_0100.
ENDFORM.                    " ALV_REFRESH_GRID1
```
SELECT와 PROCESS를 모두 거친 후 DROP DOWN LIST를 재구성하고, 그 후 GRID를 다시 REFRESH하는 순서로 해 주어야 드롭다운리스트가 실시간으로 업데이트 된다.

