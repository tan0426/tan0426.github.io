---
layout: default
title: 21. ALV DATA CHANGED
parent: abapstudy
nav_order: 21
---

# 21. ALV DATA CHANGED
ALV의 EDIT된 값 변경하고 ENTER또는 버튼 클릭 시 그 변경된 값을 가져오는 방법

정의부

```abap
class lcl_event_receiver definition.
    public section.
        methods:
            handle_data_changed for event data_changed of cl_gui_alv_grid
                importing er_data_changed
                    e_onf4 e_onf4_before e_onf4_after e_ucomm sender.
endclass. "LCL_EVENT_RECEIVER DEFINITION
```

실행부

```abap
class lcl_event_receiver implementation.
    method handle_data_changed.
        perform event_data_changed using er_data_changed
                                         e_onf4
                                         e_onf4_before
                                         e_onf4_after
                                         e_ucomm
                                         sender.
    endmethod.                    "HANDLE_DATA_CHANGED
endclass. "LCL_EVENT_RECEIVER IMPLEMENTATION
```

핸들러 불러오기.

```abap
DATA : GO_EVENT_RECEIVER TYPE REF TO LCL_EVENT_RECEIVER.
CREATE OBJECT GO_EVENT_RECEIVER.
    SET HANDLER GO_EVENT_RECEIVER->HANDLE_DATA_CHANGED FOR GR_GRID1.
```

```abap
CALL METHOD gc_grid->register_edit_event
    EXPORTING
      i_event_id = CL_GUI_ALV_GRID=>MC_EVT_MODIFIED "엔터 = MC_EVT_ENTER, MODIFYED는 엔터이외에도..
*    EXCEPTIONS
*      error      = 1
*      others     = 2
          .
  IF sy-subrc <> 0.
*   Implement suitable error handling here
  ENDIF.
```
GRID에 테이블을 얹어 주기 전에 적어준다.

CALL METHOD

INSTANCE : GC_GRID

CLASS/INTERFACE : CL_GUI_ALV_GRID

METHOD : REGISTER_EDIT_EVENT

## DATA CHANGED 예시

```abap
DATA LS_MOD_CELLS  TYPE LVC_S_MODI.
  DATA LS_DEL_ROWS   TYPE LVC_S_MOCE.
  DATA L_VALUE(30) TYPE C.
  DATA LT_ZT2302_03 LIKE TABLE OF ZT2302_03 WITH HEADER LINE.

  SELECT BUKRS BUTXT
    FROM ZT2302_03
    INTO CORRESPONDING FIELDS OF TABLE LT_ZT2302_03.

  CASE PR_SENDER.
    WHEN GR_GRID1.
      LOOP AT PR_DATA_CHANGED->MT_GOOD_CELLS INTO LS_MOD_CELLS.
        IF LS_MOD_CELLS-FIELDNAME = 'MENGE'.
          READ TABLE GT_DISPLAY INDEX LS_MOD_CELLS-ROW_ID.
          IF SY-SUBRC = 0.
            GT_DISPLAY-MENGE = LS_MOD_CELLS-VALUE.
            GT_DISPLAY-BRTWR = LS_MOD_CELLS-VALUE * GT_DISPLAY-NETPR.
            MODIFY GT_DISPLAY INDEX SY-TABIX.
            CLEAR  GT_DISPLAY.
          ENDIF.
          PERFORM REFRESH_GRID_0100.
        ENDIF.
        CLEAR LS_MOD_CELLS.
      ENDLOOP.
    WHEN GR_GRID2.
      LOOP AT PR_DATA_CHANGED->MT_GOOD_CELLS INTO LS_MOD_CELLS.
        READ TABLE GT_DATA2 WITH KEY BUKRS = LS_MOD_CELLS-VALUE.
        IF SY-SUBRC = 0.

          GT_DISPLAY2-BUKRS = GT_DATA2-BUKRS.
          GT_DISPLAY2-BUTXT = GT_DATA2-BUTXT.

          CALL METHOD PR_DATA_CHANGED->MODIFY_CELL
            EXPORTING
              I_ROW_ID    = LS_MOD_CELLS-ROW_ID
              I_FIELDNAME = 'BUTXT'
              I_VALUE     = GT_DATA2-BUTXT.
        ENDIF.
        CLEAR LS_MOD_CELLS.
      ENDLOOP.

    WHEN OTHERS.
  ENDCASE.
```
LS_MOD_CELLS-FIELDNAME이 특정 필드의 값이 바뀌었을 때만 GT_DISPLAY를 다시 뿌리도록 하여 DATA CHANGED가 실행 될 때 마다 바뀌는 LS_MOD_CELLS의 값을 제제 하였다.

값이 바뀐 후에는 PERFORM REFRESH_GRID_0100 로 GRID를 REFRESH 하여 값이 바뀌는 것을 확인 할 수 있도록 했다.
이때 이 REFRESH PERFORM문은 다음과 같은 로직으로,

```abap
DATA: LV_INDEX(1000).

  GS_STABLE-ROW = ABAP_TRUE. "Row
  GS_STABLE-COL = ABAP_TRUE. "column

*  GS_STABLE-ROW = 'X'. "Row
*  GS_STABLE-COL = 'X'. "column

  DESCRIBE TABLE GT_DISPLAY LINES LV_INDEX.
  CONDENSE LV_INDEX NO-GAPS.
  CONCATENATE 'Lines(' LV_INDEX ')' INTO GS_LAYOUT-GRID_TITLE.

  CALL METHOD GR_GRID1->SET_FRONTEND_LAYOUT
    EXPORTING
      IS_LAYOUT = GS_LAYOUT.

  CALL METHOD GR_GRID1->REFRESH_TABLE_DISPLAY
    EXPORTING
      IS_STABLE      = GS_STABLE
*      I_SOFT_REFRESH = SPACE
    .
```

값이 바뀐 후 다시 SELECT 해오는 REFRESH가 아닌 GRID만을 REFRESH하는 PERFORM 문이다.


