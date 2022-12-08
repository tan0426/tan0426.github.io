---
layout: default
title: 4. table 정리
parent: abapstudy
nav_order: 4
---
# 4. table 정리

table의 종류에는 세 가지가 있다고 한다.

standard table, sorted table, hashed table.

그 중 standard table에 대해 배웠다.

standard table은

1. 순차적인 index를 가지고 ( 그냥 적힌 순서대로 나열된다는 뜻인 듯. index1하면 첫번째줄 데이터를 찾을 수 있는 정도?),

2. key는 항상 non-unique로 선언해야 하며, (unique key로 설정하면 중복되는 값 입력이 불가능 하다고 한다. 그런데 standard table에서는 이러한 unique key 설정이 불가하다. with key로만 적어주면 자동으로 non-unique key가 된다.)

3. binary search 옵션을 사용 가능하다고 한다. (read table itab with key col binary search.로 사용 가능.)

```abap
TYPES : BEGIN OF TY_STR,
          COL1 TYPE C LENGTH 10,
          COL2 TYPE C LENGTH 10,
          COL3 TYPE C LENGTH 10,
        END OF TY_STR.
DATA : GS_DATA TYPE TY_STR.
DATA : GT_DATA LIKE STANDARD TABLE OF GS_DATA.

GS_DATA-COL1 = 'aa'.
GS_DATA-COL2 = 'aa'.
GS_DATA-COL3 = 1.
APPEND GS_DATA TO GT_DATA.
CLEAR GS_DATA.

GS_DATA-COL1 = 'bb'.
GS_DATA-COL2 = 'bb'.
GS_DATA-COL3 = 1.
APPEND GS_DATA TO GT_DATA.
CLEAR GS_DATA.

GS_DATA-COL1 = 'cc'.
GS_DATA-COL2 = 'cc'.
GS_DATA-COL3 = 1.
APPEND GS_DATA TO GT_DATA.
CLEAR GS_DATA.

GS_DATA-COL1 = 'cc'.
GS_DATA-COL2 = 'cc'.
GS_DATA-COL3 = 1.
APPEND GS_DATA TO GT_DATA.
CLEAR GS_DATA.

SORT GT_DATA BY COL1.
READ TABLE GT_DATA WITH KEY COL1 = 'aa' INTO GS_DATA BINARY SEARCH.
WRITE : /SY-SUBRC.
```
빨리 찾을 수 있는? 한 방법 인 것 같다.

sorted table은

1. key 값으로 항상 정렬 되어 있다. (unique, non-unique 모두 설정 가능하고 unique key 설정하면 중복되는값은 쌓이지 않고 non-unique key 설정하면 중복되는값도 쌓인다.)

2. sorted와 같이 사용 할 수 없고,  append가 아닌 insert로 데이터를 추가해야 한다. 실수로 append처리 했더니 덤프가 발생했다. 덤프 발생 시 해당 내요을 확인하니 append쪽 에러로 표시를 해 주더라.
            
```abap
TYPES : BEGIN OF TY_STR,
          COL1 TYPE C LENGTH 10,
          COL2 TYPE C LENGTH 10,
          COL3 TYPE C LENGTH 10,
        END OF TY_STR.
DATA : GS_DATA TYPE TY_STR.
DATA : GT_DATA LIKE SORTED TABLE OF GS_DATA WITH NON-UNIQUE DEFAULT KEY.

GS_DATA-COL1 = 'aa'.
GS_DATA-COL2 = 'aa'.
GS_DATA-COL3 = 1.
INSERT GS_DATA INTO TABLE GT_DATA.
CLEAR GS_DATA.

GS_DATA-COL1 = 'bb'.
GS_DATA-COL2 = 'bb'.
GS_DATA-COL3 = 1.
INSERT GS_DATA INTO TABLE GT_DATA.
CLEAR GS_DATA.

GS_DATA-COL1 = 'cc'.
GS_DATA-COL2 = 'cc'.
GS_DATA-COL3 = 1.
INSERT GS_DATA INTO TABLE GT_DATA.
CLEAR GS_DATA.

GS_DATA-COL1 = 'cc'.
GS_DATA-COL2 = 'cc'.
GS_DATA-COL3 = 1.
INSERT GS_DATA INTO TABLE GT_DATA.
CLEAR GS_DATA.

CL_DEMO_OUTPUT=>DISPLAY( GT_DATA ).
```

3. index나 key로 해당 행을 찾을 수 있다.

hashed table은

1. 순차적인 index를 사용하지 않아서 read table - index 구문이 사용 불가.

2. 메모리에 저장된 주소값으로 데이터를 읽는다고 한다.

3. 항상 unique key로 선언해야 한다.

```abap
TYPES : BEGIN OF TY_STR,
          COL1 TYPE C LENGTH 10,
          COL2 TYPE C LENGTH 10,
          COL3 TYPE I,
        END OF TY_STR.
DATA : GS_DATA TYPE TY_STR.
DATA : GT_DATA LIKE HASHED TABLE OF GS_DATA WITH NON-UNIQUE DEFAULT KEY.

GS_DATA-COL1 = 'aa'.
GS_DATA-COL2 = 'aa'.
GS_DATA-COL3 = 1.
INSERT GS_DATA INTO TABLE GT_DATA.
CLEAR GS_DATA.

GS_DATA-COL1 = 'bb'.
GS_DATA-COL2 = 'bb'.
GS_DATA-COL3 = 1.
INSERT GS_DATA INTO TABLE GT_DATA.
CLEAR GS_DATA.

GS_DATA-COL1 = 'cc'.
GS_DATA-COL2 = 'cc'.
GS_DATA-COL3 = 1.
INSERT GS_DATA INTO TABLE GT_DATA.
CLEAR GS_DATA.

GS_DATA-COL1 = 'cc'.
GS_DATA-COL2 = 'cc'.
GS_DATA-COL3 = 1.
INSERT GS_DATA INTO TABLE GT_DATA.
CLEAR GS_DATA.

READ TABLE GT_DATA INTO GS_DATA WITH KEY COL1 = 'dd'.
IF SY-SUBRC = 0.
  CL_DEMO_OUTPUT=>DISPLAY( GT_DATA ).
ENDIF.
```

다른 테이블과 생성 방식은 비슷한데, index로 한줄 한줄 읽어오는것이 안된다는게 독특한 점인듯 하다.

## 인터널 테이블 중복값 제거 후 정리 예시

```abap
SORT GT_DATA BY WAREID ITEM_CODE TOTALQT.

DELETE ADJACENT DUPLICATES FROM GT_DATA COMPARING WAREID ITEM_CODE TOTALQT.
```
