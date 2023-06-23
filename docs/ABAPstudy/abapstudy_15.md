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
FORM FIELD_CATALOG_100  USING    PS_FCAT TYPE LVC_S_FCAT
                                 PT_FCAT TYPE LVC_T_FCAT.
  DEFINE _FCAT_MAC.
    PS_FCAT-FIELDNAME = &1.
    GV_TEXT = &2.
    PS_FCAT-OUTPUTLEN = &3.
    PS_FCAT-EDIT = &4.
    PS_FCAT-REF_FIELD = &5.
    PS_FCAT-CFIELDNAME = &5.

    IF GV_TEXT IS NOT INITIAL.
      PS_FCAT-COLTEXT =   GV_TEXT.
      PS_FCAT-SCRTEXT_L = GV_TEXT.
      PS_FCAT-SCRTEXT_M = GV_TEXT.
      PS_FCAT-SCRTEXT_S = GV_TEXT.
    ENDIF.

    APPEND PS_FCAT TO PT_FCAT.
    CLEAR PS_FCAT.
  END-OF-DEFINITION.

  _FCAT_MAC 'EBELN' '구매문서 번호'   '' ''  ''.
  _FCAT_MAC 'BUKRS' '회사코드'        '' ''  ''.
  _FCAT_MAC 'WAERS' '통화'            '' ''  ''.
  _FCAT_MAC 'NETPR' '구매문서의 단가' '' 'X' 'WAERS'.

ENDFORM.                    " FIELD_CATALOG_100
```

또는 GS_FIELDCAT-CFIELDNAME = '통화필드 명' 으로 통화 별로 변환 해 줄수 있다.

## 통화를 변경하는 다른 방법들

### WRITE
 
```abap
WRITE GS_DATA-PRICE CURRENCY 'WAERS' TO GS_DATA-PRICE DECIMALS 0. 
```

이 경우 만약 SAP에서 1.25값이라면, 통화필드값을 따라 (만약 KRW이라면) *100이 되어서 필드안에 들어간다.

### FUNCTION

```abap
*&---------------------------------------------------------------------*
*&      Form  AMT_CONV_TO_EXTERNAL
*&---------------------------------------------------------------------*
FORM amt_conv_to_external USING    p_waers
                          CHANGING p_amount.

  DATA:
    l_internal TYPE bapicurr-bapicurr,
    l_external TYPE bapicurr-bapicurr.

  l_internal = p_amount.  "금액이 들어간다.

  CALL FUNCTION 'BAPI_CURRENCY_CONV_TO_EXTERNAL'
    EXPORTING
      currency        = p_waers   "통화가 들어간다. KRW 겠지
      amount_internal = l_internal "100.00이 들어간다.
    IMPORTING
      amount_external = l_internal. "10000이 나온다.

p_amount = l_internal. 다시 원래 INTERFACE 할 필드에 넣은것이다.
*  p_external = l_external.

ENDFORM.                    " AMT_CONV_TO_EXTERNAL
```
