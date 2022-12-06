---
layout: default
title: ALV EDIT에서 값 받아오기
parent: abapstudy
nav_order: 23
---
# ALV EDIT에서 값 받아오기
            
ALV에서 값을 입력 한 후 DB에 저장 하고자 하는데, ALV에 입력한 값이 관련 인터널 테이블로 업데이트가 안된다.

이를 하게 해주는 METHOD는 다음과 같이 ALV GRID를 뿌려주기 전에 적어준다.

```abap
*&---------------------------------------------------------------------*
*& Form DISPLAY_ALV
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*& -->  p1        text
*& <--  p2        text
*&---------------------------------------------------------------------*
FORM display_alv .
  CALL METHOD gc_grid->register_edit_event
    EXPORTING
      i_event_id = CL_GUI_ALV_GRID=>MC_EVT_ENTER "엔터 = MC_EVT_ENTER, MODIFYED는 엔터이외에도..
*    EXCEPTIONS
*      error      = 1
*      others     = 2
          .

  CALL METHOD gc_grid->set_table_for_first_display
*    EXPORTING
*      is_layout                     = GS_LAYOUT
    CHANGING
      it_outtab                     = GT_WORKPL
      it_fieldcatalog               = GT_FCAT.
ENDFORM.
```

MC_EVT_ENTER는 ENTER 시에 업데이트, MC_EVT_MODIFYED는 엔터이외에도 업데이트 한다.

MODIFIED로 하였더니 ENTER하고 SAVE를 클릭 했을 때 SAVE 완료 MESSAGE가 뜨지 않았다.(첫번째에는 꼭 안뜨고 두번째에는 떴다)
