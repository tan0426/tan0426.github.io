---
layout: default
title: 26. 헤더라인 있는 테이블 오류 수정 예시
parent: abapstudy
nav_order: 26
---

# 26. 헤더라인 있는 테이블 오류 수정 예시

## 1. 테이블 입력 선언 

```abap
TABLES : SFLIGHT, SBOOK, SCARR.

PARAMETERS : P_CARRID LIKE SFLIGHT-CARRID,
             P_CONNID LIKE SFLIGHT-CONNID.

SELECT-OPTIONS : S_FLDATE FOR SFLIGHT-FLDATE.

DATA : BEGIN OF LT_PAYSUM OCCURS 0,
        CARRNAME LIKE SCARR-CARRNAME,
        CARRID LIKE SFLIGHT-CARRID,
        CONNID LIKE SFLIGHT-CONNID,
        FLDATE LIKE SFLIGHT-FLDATE,
        BOOKID LIKE SBOOK-BOOKID,
        LOCCURAM LIKE SBOOK-LOCCURAM,
       END OF LT_PAYSUM.
DATA : LV_LOC TYPE SBOOK-LOCCURAM.

SELECT C~CARRNAME
       A~CARRID
       A~CONNID
       A~FLDATE
       B~BOOKID
       B~LOCCURAM

  FROM SFLIGHT AS A
  JOIN SBOOK AS B
    ON A~CARRID = B~CARRID
   AND A~CONNID = B~CONNID
   AND A~FLDATE = B~FLDATE
  JOIN SCARR AS C
    ON A~CARRID = C~CARRID
  INTO CORRESPONDING FIELDS OF TABLE LT_PAYSUM
 WHERE A~CARRID = P_CARRID
   AND A~CONNID = P_CONNID
   AND A~FLDATE IN S_FLDATE.

CLEAR LV_LOC.

SORT LT_PAYSUM[].

data : lr_table type ref to cl_salv_table.
data : lv_lines   type i,
       lv_columns type i.
field-symbols : <ft> type any table,
                <fs> type any.
************************************************************************
data : begin of lt_result occurs 0."표시 내역을 담을 테이블
  include structure lt_paysum.
data : end of lt_result.
assign : lt_result[] to <ft>,
         lt_result   to <fs>.
"select가 필요할 경우
*select * from sflight into corresponding fields of table <ft> up to 10 rows.
lt_result[] = lt_paysum[].
************************************************************************

describe : table <ft> lines  lv_lines,
           field <fs> length lv_columns in byte mode.

cl_salv_table=>factory(
  importing
    r_salv_table = lr_table
  changing
    t_table = <ft> ).

lr_table->set_screen_popup( start_column = 1
                            end_column = lv_columns
                            start_line = 1
                            end_line = lv_lines ).
lr_table->display( ).
```

여기서 헤더라인 내역 lt_paysum이 lt_result에 입력이 되지 않아 select join 된것이 뜨지 않는 경우가 있다.
그래서 lt_result[] = lt_paysum[] 으로 입력을 해 준다.
