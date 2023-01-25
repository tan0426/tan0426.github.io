---
layout: default
title: 15. 찾아서 볼 FIELD CATALOG 기능들
parent: abapstudy
nav_order: 15
---
# 15. 찾아서 볼 FIELD CATALOG 기능들

### 1. SCRTEXT_L, SCRTEXT_M, SCRTEXT_S
```abap
GS_FCAT-fieldname = 'ZCODE'.
*GS_FCAT-COLTEXT = '코드'.
GS_FCAT-SELTEXT_L = '코드롱롱롱'.
GS_FCAT-SELTEXT_M = '코드미드'.
GS_FCAT-SELTEXT_S = '코드숏'.
APPEND GS_FCAT TO GT_FCAT.
CLEAR GS_FCAT.
```

### 2. EMPHASIZE, HOTSPOT
```abap
GS_FCAT-EMPHASIZE = 'C300'. "C(COLOR) 3(노랑) 0(강조) 0(반전)
GS_FCAT-HOTSPOT = 'X'.
```

### 3. NO_OUT, TECH, EDIT, CHECKBOX
```abap
GS_FCAT-FIELDNAME = 'CHECK'.
GS_FCAT-COLTEXT = '체크박스'.
GS_FCAT-CHECKBOX = 'X'. "체크박스
GS_FCAT-EDIT = 'X'. "체크박스 에딧 설정
GS_FCAT-OUTPUTLEN = 8.
*  GS_FCAT-NO_OUT = 'X'. "필드 숨김(HIDE)
*  GS_FCAT-TECH = 'X'. "아예 레이아웃에서도 숨김
APPEND GS_FCAT TO GT_FCAT.
CLEAR GS_FCAT.
```

### 4. KEY, JUST
```abap
GS_FCAT-FIELDNAME = 'ZCODE'.
GS_FCAT-COLTEXT = '코드'.
GS_FCAT-KEY = 'X'. "ABAP에서는 TRUE/FULSE OR ON/OFF OR 1/0
GS_FCAT-JUST = 'C'. "C, R, L 정렬
APPEND GS_FCAT TO GT_FCAT.
CLEAR GS_FCAT.
```

### 5. ALV 필드 SEARCH HELP
```abap
  GS_FCAT-FIELDNAME = 'PLAN_DATE'.
  GS_FCAT-COLTEXT = '계획 날짜'.
  GS_FCAT-OUTPUTLEN = 15.
  GS_FCAT-EDIT = 'X'.
  GS_FCAT-REF_TABLE = 'ADCP'.
  GS_FCAT-REF_FIELD = 'DATE_FROM'.
  GS_FCAT-INTTYPE = 'D'.
  APPEND GS_FCAT TO GT_FCAT.
  CLEAR GS_FCAT.
```

### 6. CURRENCY

```abap
CASE GS_FIELDCAT-FIELDNAME.
      WHEN 'LIFNR'.
        LV_TEXT = '거래처번호'.
      WHEN 'NAME1'.
        LV_TEXT = '거래처명'.
      WHEN 'STCD2'.
        LV_TEXT = '사업자번호'.
      WHEN 'ZBANKN'.
        LV_TEXT = '은행명'.
      WHEN 'BANKN'.
        LV_TEXT = '계좌번호'.
        GS_FIELDCAT-EDIT = 'X'.
      WHEN 'DMBTR'.
        LV_TEXT = '금액합계'.
        GS_FIELDCAT-CURRENCY = 'KRW'.
    ENDCASE.
```

또는 GS_FIELDCAT-CFIELDNAME = '통화필드 명' 으로 통화 별로 변환 해 줄수 있다.
