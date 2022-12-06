---
layout: default
title: 25. 찾아 볼 ALV LAYOUT, STYLE
parent: abapstudy
nav_order: 25
---
# 25. 찾아 볼 ALV LAYOUT, STYLE
            
1. 특정 조건에 EDIT ENABLE, DISABLE.

```abap
*&---------------------------------------------------------------------*
*& Form GET_DATA
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*& -->  p1        text
*& <--  p2        text
*&---------------------------------------------------------------------*
FORM get_data .

  SELECT *
    INTO CORRESPONDING FIELDS OF TABLE GT_WORKPL
    FROM ZTJ_WORKPLAN.

  LOOP AT GT_WORKPL INTO GS_WORKPL.
    IF GS_WORKPL-PLAN_CONFIRM = 'X'.
      GS_STYLE-FIELDNAME = 'PLAN_DATE'.
      GS_STYLE-STYLE = CL_GUI_ALV_GRID=>MC_STYLE_DISABLED.
      APPEND GS_STYLE TO GS_WORKPL-STYLE.
      CLEAR GS_STYLE.

      GS_STYLE-FIELDNAME = 'PLAN_QT'.
      GS_STYLE-STYLE = CL_GUI_ALV_GRID=>MC_STYLE_DISABLED.
      APPEND GS_STYLE TO GS_WORKPL-STYLE.
      CLEAR GS_STYLE.
    ENDIF.
    MODIFY GT_WORKPL FROM GS_WORKPL.
    CLEAR GS_WORKPL.
  ENDLOOP.

ENDFORM.

*&---------------------------------------------------------------------*
*& Form FIELD_CATALOG
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*& -->  p1        text
*& <--  p2        text
*&---------------------------------------------------------------------*
  GS_FCAT-FIELDNAME = 'PLAN_DATE'.
  GS_FCAT-COLTEXT = '계획 날짜'.
  GS_FCAT-OUTPUTLEN = 15.
  GS_FCAT-EDIT = 'X'.
  GS_FCAT-REF_TABLE = 'ADCP'.
  GS_FCAT-REF_FIELD = 'DATE_FROM'.
  GS_FCAT-INTTYPE = 'D'.
  APPEND GS_FCAT TO GT_FCAT.
  CLEAR GS_FCAT.

  GS_LAYOUT-STYLEFNAME = 'STYLE'.
  
 *&---------------------------------------------------------------------*
*& Form DISPLAY_ALV
*&---------------------------------------------------------------------*
*& text
*&---------------------------------------------------------------------*
*& -->  p1        text
*& <--  p2        text
*&---------------------------------------------------------------------*

  CALL METHOD gc_grid->set_table_for_first_display
    EXPORTING
      is_layout                     = GS_LAYOUT
    CHANGING
      it_outtab                     = GT_WORKPL
      it_fieldcatalog               = GT_FCAT.
  ENDFORM.
```

![Untitled](./abapstudy_img/abapstudy_28.PNG)
