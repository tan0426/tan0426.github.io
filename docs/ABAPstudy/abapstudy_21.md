---
layout: default
title: ALV DATA CHANGED
parent: abapstudy
nav_order: 21
---
# ALV DATA CHANGED
ALV의 EDIT된 값 변경하고 ENTER또는 버튼 클릭 시 그 변경된 값을 가져오는 방법

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

CALL METHOD
INSTANCE : GC_GRID
CLASS/INTERFACE : CL_GUI_ALV_GRID
METHOD : REGISTER_EDIT_EVENT
