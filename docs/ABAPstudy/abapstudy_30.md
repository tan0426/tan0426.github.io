---
layout: default
title: 30. BDC (Batch Data Communication)
parent: abapstudy
nav_order: 30
---

# BDC (Batch Data Communication)
BDC는 사용자가 Macro를 사용하여 SAP프로그램을 자동으로 수행하는 것과 같은 형태의 기능과 유사.

즉 단순 반복작업을 최소화하여 백그라운드로 수행 가능하도록 해줌. 
사용자가 직접 프로그램에서 정해진 시나리오대로 작업하는 절차를 레코딩 수행하는 것으로 직접 DB table에 입력 하는것이 아님.

## BDC 구성 요소
![image](https://user-images.githubusercontent.com/106587677/214050895-3391b827-8cc1-4759-b361-3474e3ec7262.png)


## 엑셀 업로드

```abap
call function 'KD_GET_FILENAME_ON_F4'
       changing
        file_name = p_file.
```

파라메터에서 파일 관리자를 띄움

## function key 설정

```abap
initialization. 
  selection-screen function key 1.  초기화면(1000) 버튼 추가
*  SELECTION-SCREEN FUNCTION KEY 2.
  
  data: lv_ftext(30),  "FunctionKey Text (1) Display
            lv_internal type icon_int.

  select single internal into lv_internal
     from icon
    where name = 'ICON_XLS'.

  concatenate lv_internal text-t01
          into lv_ftext separated by space.

  sscrfields-functxt_01 = lv_ftext.
*  SSCRFIELDS-FUNCTXT_02 = LV_FTEXT.

  gv_repid = sy-repid.
```

# function key command 설정

```abap
AT SELECTION-SCREEN.
  CASE SSCRFIELDS-UCOMM.
      WHEN 'FC01'.
      
          " Excel Form 다운로드
          DATA:    L_FILE  TYPE RLGRAP-FILENAME ,
           L_NAME  TYPE STRING ,
           WWWDATA_ITEM LIKE WWWDATATAB .

          SELECT SINGLE *
            INTO CORRESPONDING FIELDS OF WWWDATA_ITEM
            FROM WWWDATA
            WHERE OBJID = 'ZTEST02' . "저장된 데이터 오브젝트 id

          DATA: L_FILENAME     TYPE STRING,
               TABLE             TYPE STRING_TABLE,
               TITLE               TYPE STRING,
               FULLPATH         TYPE STRING,
               FILENAME       TYPE STRING,
               PATH             TYPE STRING,
              ACTION           TYPE I.
          
          """""""""""""""" 다운로드할 엑셀 정보 가져오는 코드
          TITLE = '투자오더 Excel Layout.XLS' .
          L_FILENAME = '투자오더 Excel Layout.XLS' .

          CALL METHOD CL_GUI_FRONTEND_SERVICES=>FILE_SAVE_DIALOG
            EXPORTING
              WINDOW_TITLE      = TITLE
              DEFAULT_EXTENSION = 'xls'
              DEFAULT_FILE_NAME = L_FILENAME
              FILE_FILTER       = 'Only Excel Files (*.xls)|*.XLS|'
              INITIAL_DIRECTORY = 'C:\Users\User\Desktop\교육'
            CHANGING
              FULLPATH          = FULLPATH
              FILENAME          = FILENAME
              PATH              = PATH
              USER_ACTION       = ACTION.
          IF SY-SUBRC <> 0.
            MESSAGE ID SY-MSGID TYPE SY-MSGTY NUMBER SY-MSGNO
                       WITH SY-MSGV1 SY-MSGV2 SY-MSGV3 SY-MSGV4.
          ENDIF.
          P_FILE = FULLPATH  .
          """"""""""""""""

          CHECK NOT L_FILE IS INITIAL .

          CALL FUNCTION 'DOWNLOAD_WEB_OBJECT' "웹 템플릿 다운로드
            EXPORTING
              KEY         = WWWDATA_ITEM
              DESTINATION = L_FILE.

          L_NAME = L_FILE .

          CALL METHOD CL_GUI_FRONTEND_SERVICES=>EXECUTE
            EXPORTING
              DOCUMENT               = L_NAME
            EXCEPTIONS
              CNTL_ERROR             = 1
              ERROR_NO_GUI           = 2
              BAD_PARAMETER          = 3
              FILE_NOT_FOUND         = 4
              PATH_NOT_FOUND         = 5
              FILE_EXTENSION_UNKNOWN = 6
              ERROR_EXECUTE_FAILED   = 7
              SYNCHRONOUS_FAILED     = 8
              NOT_SUPPORTED_BY_GUI   = 9
              OTHERS                 = 10.
          IF SY-SUBRC <> 0.
            MESSAGE ID SY-MSGID TYPE SY-MSGTY NUMBER SY-MSGNO
                       WITH SY-MSGV1 SY-MSGV2 SY-MSGV3 SY-MSGV4.
            EXIT .
          ENDIF.
          
  ENDCASE.
```

## RUN BDC

```abap
tables : zco01.

  data: l_except(1) type c.

  data: gt_zco06 like table of zco01 with header line.

  data : l_index(10) type c.

  loop at gt_itab into gs_itab.
*    CLEAR : GT_BDCDATA, GT_BDCDATA[].
*    CLEAR : GT_MSGTAB, GT_MSGTAB[].
*
*    PERFORM DYNPRO USING : 'X'   'SAPMKAUF'     '0100',
*                           ' '   'COAS-AUART'   gs_itab-AUART,
*                           ' '   'BDC_OKCODE'   '/00'.
*
*    PERFORM DYNPRO USING : 'X'   'SAPMKAUF'     '0600',
*                           ' '   'COAS-KTEXT'   gs_itab-KTEXT,
*                           ' '   'COAS-BUKRS'   gs_itab-BUKRS,
*                           ' '   'COAS-GSBER'   gs_itab-GSBER,
*                           ' '   'COAS-WERKS'   gs_itab-WERKS,
*                           ' '   'COAS-SCOPE'   gs_itab-SCOPE,
*                           ' '   'BDC_OKCODE'   '=SICH'.
*
*   call TRANSACTION 'KO01' USING GT_BDCDATA
*                           mode 'A'.

    l_index = gs_itab-kstar.

  call function 'CONVERSION_EXIT_ALPHA_INPUT'
    exporting
      input         = l_index
   importing
      output        = l_index.
          .

  gt_zco06-kstar = l_index.        " 계정코드
  gt_zco06-wog05 = gs_itab-wog05.  " 금액

  append gt_zco06.

  call function 'ZINTERN_BUDGET'
    exporting
      m_gjahr        = gs_itab-gjahr
      m_perbl        = gs_itab-perbl
      m_kostl        = gs_itab-kostl
*      m_kstar        = GS_ZCO06-kstar
*      m_wog05        = GS_ZCO06-wog05
*   IMPORTING
*     E_EXCEPT       = l_index
    tables
      zco06          = gt_zco06
            .
  clear gt_zco06.

  endloop.
```
