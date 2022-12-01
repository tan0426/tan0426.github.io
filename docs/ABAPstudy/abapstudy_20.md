---
layout: default
title: DUMP를 피하기 위한 TRY CATCH문
parent: abapstudy
nav_order: 20
---
# DUMP를 피하기 위한 TRY CATCH문

```abap
TRY .

EXEC SQL.
  DELETE FROM sapout.BOM WHERE REMARK_DC='{ KTJ }'
ENDEXEC.
"INSERT
EXEC SQL.
  INSERT INTO sapout.BOM
              (ITEMPARENT_CD, ITEMCHILD_CD, BOM_SQ, JUST_QT, USE_YN, REMARK_DC)
         VALUES (125, 13, 1, 1, 1, '{ FFFF }')
ENDEXEC.

CATCH CX_SY_NATIVE_SQL_ERROR INTO LX_EXEC. "덤프 안뜨게 하기 위함
  LV_MSG = LX_EXEC->GET_TEXT(  ).
  MESSAGE LV_MSG TYPE 'S' DISPLAY LIKE 'E'.
ENDTRY.
```
