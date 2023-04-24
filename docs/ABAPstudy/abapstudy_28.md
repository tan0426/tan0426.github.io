---
layout: default
title: 28. FIELD-SYMBOL
parent: abapstudy
nav_order: 28
---

# 28. FIELD-SYMBOL

1. FIELD SYMBOL은 DATA OBJECT에 동적인 접근이 가능하게 하는 기능.
2. 주소를 가지고 주소값에 해당하는 메모리의 값을 작동.
3. FIELD SYMBOL을 선언하여 사용한다고 하여서 필드 심벌의 공간을 가지는 것이 아니라 연결된 메모리의 값으로 작동.
4. FIELD SYMBOL의 데이터 이름과 속성은 실행 시에 결정이 됨.
5. FIELD SYMBOL은 모든 데이터 오브젝트에 지정될 수 있음.
6. 일단 FIELD SYMBOL이 할당되면 필드와 필드 심벌 간에는 차이가 없다.
7. MOVE와 같은 명령어도 동일하게 사용 가능.
8. FIELD SYMBOL은 TYPE을 명시하여 선언할 수도 있으며, 타입 없이 생성해도 됨.
9. TYPE이 명시되지 않으면 할당되는 필드(오브젝트)의 타입을 그대로 상속받음.


변수명(A)            ----ASSIGN(별명 지정)----> FIELD SYMBOL(B)

메모리 주소(100번지) ----동일 영역------------> 메모리 주소(100번지)

변수값(ABC)          ----동일 영역------------> 변수값(ABC


프로그램을 실행하여 FIELD SYMBOL이 필드에 할당되는 순간 메모리 주소를 가지게 되고 FIELD SYMBOL의 변수값은 
LINK된 필드의 메모리 주소값을 가지게 됨.

### FIELD SYMBOL 구문 선언 1

```abap
FIELD-SYMBOL <FS> [TYPE]
```

FIELD SYMBOL 변수는 <>를 사용하여 선언하고 [TYPE] 구문에는 타입을 선언하는 구문이며, 옵션사항.

### FIELD SYMBOL 구문 선언2

```abap
FIELD-SYMBOLS <FS1>.
FIEDL-SYMBOLS <FS2> TYPE ANY [TABLE].
FIELD-SYMBOLS <FS3> LIKE GT_TAB.
```

FIELD-SYMBOLS [FS2] TYPE ANY TABLE을 사용하기 위해서는 ASSIGN 구문에서 할당할 인터널 테이블이 TABLE 타입으로 선언되어야 함.
이렇게 선언된 FIELD SYMBOL은 그 자체가 인터널 테이블이 되어 READ와 같은 구문을 사용 할 수 있음.
  
FIELD SYMBOL을 READ 하기 위해서는 TYPE ANY TABLE로 선언하여야 FIELD SYMBOL이 인터널 테이블 역할을 수행하게 되고 (FNAME)을 사용하여
동적 구문을 이용함.

### FIELD SYMBOL 구문  선언3
STRUCTURE B의 필드 A 내용을 FILED SYMBOL C에 넣으라는 뜻.

```abap
ASSIGN COMPONENT A OF STRUCTURE B TO <C>
```

```abap
READ TABLE <FS> WITH TABLE KEY (FNAME) =  'X' INTO WA.
```
  
### FIELD SYMBOL 사용 예 1

```abap
DATA: BEGIN OF line,
        col1(1) TYPE c,
        col2(1) TYPE c VALUE 'X',
      END OF line.

FIELD-SYMBOLS <fs> LIKE LINE.

ASSIGN line TO <fs>.

MOVE <fs>-col2 TO <fs>-col1.
```

[FS]-CO2와 같이 STRUCTURE처럼 사용하고자 할 경우에는 FIELD SYMBOL을 선언할 경우에 LIKE, 또는 TYPE을 선언하여야 함.

### FIELD SYMBOL 사용 예 2

```abap
TYPES: BEGIN OF line,
         col1(1) TYPE c,
         col2(1) TYPE c, " VALUE 'X',
       END OF line.

FIELD-SYMBOLS <fs> type LINE.
MOVE <fs>-col2 TO <fs>-col1.
```

TYPE을 선언하여 FIELD SYMBOL을 선언하면 이미 TYPE이 FIELD SYMBOL에 지정되어 있어서 ASSIGN 구문이 필요하지 않음.
  
### 최종 활용 예

```abap
FIELD-SYMBOLS <FIELD>.
DATA :  FNAME(10), SUM LIKE COSP-WTG001.
  
DO 12 TIMES.  
  CC = SY-INDEX.
  CONCATENATE ‘COSP-WGT0' CC INTO FNAME. 
  ASSIGN (FNAME) TO <FIELD>.
  SUM = SUM + <FIELD>.
  CLEAR : FNAME, <FIELD>.
ENDDO.
```
  
LOOP 구문을 이용하여 FIELD SYMBOL에 데이터를 MOVE 시키기 위해서는 ASSIGNING 구문을 사용.
  
```abap
FIELD-SYMBOLS: <ls_fcat> TYPE lvc_s_fcat.

FIELD-SYMBOLS: <fname>, <fname2>.

LOOP AT lt_fieldcat ASSIGNING <ls_fcat>.
  
      IF <ls_fcat>-fieldname CS '_C'.

        ASSIGN  COMPONENT <ls_fcat>-fieldname OF
        STRUCTURE  gt_display TO <fname>.
        l_len = STRLEN(  <ls_fcat>-fieldname ).
        l_len = l_len - 2.
        l_fd_nm = <ls_fcat>-fieldname+0(l_len).
        ASSIGN  COMPONENT l_fd_nm OF
        STRUCTURE  gt_display
                TO <fname2>.

ENDLOOP.
```

## 실습

```abap
DATA : BEGIN OF gs_data.
        INCLUDE STRUCTURE zt2302_01.
DATA : END OF gs_data,
       gt_data LIKE TABLE OF gs_data.

FIELD-SYMBOLS <field>.

DATA : fname(100), sum LIKE zt2302_01-mon01.
DATA : cc(2).

DO 12 TIMES.
  cc = sy-index.

  IF strlen( cc ) = 1.
    CONCATENATE '0' cc INTO cc.
  ELSE.
  ENDIF.

  CONCATENATE 'gs_data-MON' cc INTO fname.
  ASSIGN (fname) TO <field>.

  <field> = sy-index.
  APPEND gs_data to gt_data.
  CLEAR gs_data.
ENDDO.

WRITE : / 'matnr',
        'mon01',
        'mon02',
        'mon03',
        'mon04',
        'mon05',
        'mon06',
        'mon07',
        'mon08',
        'mon09',
        'mon10',
        'mon11',
        'mon12'.

LOOP AT gt_data INTO gs_data.
  WRITE : / gs_data-matnr,
          gs_data-mon01,
          gs_data-mon02,
          gs_data-mon03,
          gs_data-mon04,
          gs_data-mon05,
          gs_data-mon06,
          gs_data-mon07,
          gs_data-mon08,
          gs_data-mon09,
          gs_data-mon10,
          gs_data-mon11,
          gs_data-mon12.
ENDLOOP.
```
  
  
  
