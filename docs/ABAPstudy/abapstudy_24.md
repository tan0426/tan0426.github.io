---
layout: default
title: 24. ALV 선택한 LINE 받아오기
parent: abapstudy
nav_order: 24
---
# 24. ALV 선택한 LINE 받아오기
            
ALV FIELD CATALOG에서 EDIT을 했을때 왼쪽에 나타나는 체크포인트를 이용해 클릭 하면 그 클릭한 라인을 받아오는 프로그래밍을 했다.

```abap
*&---------------------------------------------------------------------*
*& Include          ZDKTJ_WORKPLAN_ALV_TOP
*&---------------------------------------------------------------------*
DATA : GT_INDEX TYPE LVC_T_ROW, "ALV 선택라인 저장 인터널테이블
       GS_INDEX LIKE LINE OF GT_INDEX, "ALV 선택라인 저장 스트럭쳐
       GD_LINES TYPE I. "선택한 라인수를 저장할 변수

*&---------------------------------------------------------------------*
*& Form ALV_CURRENT_LINE
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*& -->  p1        text
*& <--  p2        text
*&---------------------------------------------------------------------*
FORM alv_current_line .
  CALL METHOD gc_grid->get_selected_rows
    IMPORTING
      et_index_rows = GT_INDEX
*      et_row_no     =
      .

  DESCRIBE TABLE GT_INDEX LINES GD_LINES. "한 라인만 선택하기 위해서 몇 라인을 선택했는지 저장
ENDFORM.
```

GET_SELECTED_ROWS METHOD를 이용해 GT_INDEX로 라인을 받아온다.

```abap
WHEN 'CONF'.
  LOOP AT GT_ALV_LINE.
    SELECT SINGLE *
      FROM ZTJ_WORKPLAN
      INTO CORRESPONDING FIELDS OF GT_ZTJ_WORKPLAN
      WHERE PLAN_CODE = GT_ALV_LINE-PLAN_CODE.

    IF SY-SUBRC = 0.
      IF GT_ALV_LINE-PLAN_CONFIRM IS NOT INITIAL.
        GT_ZTJ_WORKPLAN-PLAN_CONFIRM = 'X'.
        APPEND GT_ZTJ_WORKPLAN.
        CLEAR GT_ZTJ_WORKPLAN.
        IF GT_ZTJ_WORKPLAN[] IS NOT INITIAL.
          MODIFY ZTJ_WORKPLAN FROM TABLE GT_ZTJ_WORKPLAN.
          COMMIT WORK.

          IF SY-SUBRC = 0.
            MESSAGE 'SUCCESS' TYPE 'S'.
          ENDIF.
        ENDIF.
      ENDIF.
    ENDIF.
  ENDLOOP.
```

이러한 방식으로 CONFIRM 버튼을 클릭하면 플래그를 세우도록 했다.
