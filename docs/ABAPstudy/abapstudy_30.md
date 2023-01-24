---
layout: default
title: 30. BDC, BAPI, 엑셀 
parent: abapstudy
nav_order: 30
---

# BDC (Batch Data Communication)
BDC는 사용자가 Macro를 사용하여 SAP프로그램을 자동으로 수행하는 것과 같은 형태의 기능과 유사.

즉 단순 반복작업을 최소화하여 백그라운드로 수행 가능하도록 해줌. 
사용자가 직접 프로그램에서 정해진 시나리오대로 작업하는 절차를 레코딩 수행하는 것으로 직접 DB table에 입력 하는것이 아님.

### BDC 구성 요소
![image](./abapstudy_img/abapstudy_40.png)

### 1. BDC 준비물
BDCDATA : SHDB라는 레코딩 정보를 생성하는 프로그램에서 발췌된 value들을 해당 structure에 입력

![image](./abapstudy_img/abapstudy_41.png)

BDCMSGCOLL : BDC수행이 완료 된 후, 성공이나 실패 여부에 대한 Message 정보를 담아주며 "성공 유무에 대한 이유 정보"를 같이 return 해줌.

![image](./abapstudy_img/abapstudy_42.png)

CTUPARAMS
![image](./abapstudy_img/abapstudy_43.png)

DISMODE: MODE 옵션과 동일

UPDMODE: UPDATE 옵션과 동일

CATTMODE: CATT 모드설정

                   ' ' CATT 사용 안 함

                   'N' single-screen control이 없는 CATT

                   'A' single-screen control이 있는 CATT

DEFSIZE:  기본 윈도우 사이즈 설정

RACOMMIT:COMMIT WORK에서 트랜잭션 종료하지 않음.

NOBINPT:Batch Input Mode 사용 안 함.

NOBIEND:DISMODE가 'E' 상태일 때만 설정이 가능하며, 시스템 에레가 발생하면 Background에서 수행 중인 BDC가 Foreground로 전환되고 스크린이 조회 됨.

CTU_PARAMS의 옵션이 DISMODE와 CALL TRANSACTION의 MODE 는 같은 기능을 수행하며, 중복해서 사용할 수 없다. UPDMODE 속성도 동일.

mode : 조회 모드 설정.

 A   화면을 조회하면서 트랜잭션 수행

 E   에러가 발생할 경우에만 화면 조회

 N   화면을 표시하지 않음
update : update mode

 S    Synchronous Update

 A    Asynchronous Update

 L    Local Update

TYPES : GTY_BDC TYPE BDCDATA,

              GTY_MSG TYPE BDCMSGCOLL.
  
  
## BDC 예시

### run BDC

선언 해 준 인터널 테이블, 불러 올 엑셀 의 필드 순서와 갯수를 같게 해 주어야 한다.

그리고 해당 트랜잭션에 레고드가 저장 되어 있어야 한다.
```abap
form run_bdc .
  tables : zco01.

  data: l_except(1) type c.

  data: gt_zco06 like table of zco01 with header line.

  data : l_index(10) type c.

  loop at gt_itab into gs_itab.
    clear : gt_bdcdata, gt_bdcdata[].
    clear : gt_msgtab, gt_msgtab[].

    perform dynpro using : 'X'   'SAPMKAUF'     '0100',
                           ' '   'COAS-AUART'   gs_itab-auart,
                           ' '   'BDC_OKCODE'   '/00'.

    perform dynpro using : 'X'   'SAPMKAUF'     '0600',
                           ' '   'COAS-KTEXT'   gs_itab-ktext,
                           ' '   'COAS-BUKRS'   gs_itab-bukrs,
                           ' '   'COAS-GSBER'   gs_itab-gsber,
                           ' '   'COAS-WERKS'   gs_itab-werks,
                           ' '   'COAS-SCOPE'   gs_itab-scope,
                           ' '   'BDC_OKCODE'   '=SICH'.

   call transaction 'KO01' using gt_bdcdata
                           mode 'A'.

  endloop.

endform.                    " EXECUTE_BDC
```

엑셀 다운로드 버튼 USER-COMMAND.

불러 올 엑셀 양식은 SMW0에 저장 한다.

![image](./abapstudy_img/abapstudy_44.png)

```abap
form user_command . " Excel Form 다운로드

  data:    l_file  type rlgrap-filename ,
           l_name  type string ,
           wwwdata_item like wwwdatatab .

  clear : wwwdata_item.
  select single *
    into corresponding fields of wwwdata_item
    from wwwdata
    where objid = 'ZINTERN02_01' . "SMW0에 저장해 둔 엑셀 양식을 불러옴.

  data: l_filename     type string,
               table             type string_table,
               title               type string,
               fullpath         type string,
               filename       type string,
               path             type string,
              action           type i.

  """""""""""""다운로드 엑셀 형식. 타이틀, 저장 위치 등
  title = '투자오더 Excel Layout.XLS' .
  l_filename = '투자오더 Excel Layout.XLS' .

  call method cl_gui_frontend_services=>file_save_dialog
    exporting
      window_title      = title
      default_extension = 'xls'
      default_file_name = l_filename
      file_filter       = 'Only Excel Files (*.xls)|*.XLS|'
      initial_directory = 'C:\'
    changing
      fullpath          = fullpath
      filename          = filename
      path              = path
      user_action       = action.
  if sy-subrc <> 0.
    message id sy-msgid type sy-msgty number sy-msgno
               with sy-msgv1 sy-msgv2 sy-msgv3 sy-msgv4.
  endif.
  p_file = fullpath  .
  """"""""""""""""""""""""""""""""""

  check not l_file is initial .

  call function 'DOWNLOAD_WEB_OBJECT'
    exporting
      key         = wwwdata_item
      destination = l_file.

  l_name = l_file .

  call method cl_gui_frontend_services=>execute
    exporting
      document               = l_name
    exceptions
      cntl_error             = 1
      error_no_gui           = 2
      bad_parameter          = 3
      file_not_found         = 4
      path_not_found         = 5
      file_extension_unknown = 6
      error_execute_failed   = 7
      synchronous_failed     = 8
      not_supported_by_gui   = 9
      others                 = 10.
  if sy-subrc <> 0.
    message id sy-msgid type sy-msgty number sy-msgno
               with sy-msgv1 sy-msgv2 sy-msgv3 sy-msgv4.
    exit .
  endif.

endform.                    " HANDLE_USER_COMMAND
```

# BAPI (Buisness Application Programming Interface) : 학습 심화 필요

이하 밥피는 BDC처럼 자동으로 무언가를 불러 와주는 프로그램인데, BDC와 달리 레코드에 저장 해 둘 필요 없고, 레코드를 불러 올 필요 없이 바로 입력 해 주는 프로그램이다.

편리한 대신 function내에 파라메터들이 많아서 오류를 찾기가 힘들고 적절한 예제가 잘 없다...

다음 예제 프로그램 에서도 gt_zco06이 따로 선언되어야 하고, 가져오는 테이블도 만들어져 있는 테이블이고,
function도 예제를 위한 커스텀 function이라 다른 테이블은 불러오지를 못한다..

```abap
form run_bapi .

  tables : zco01.

  data: l_except(1) type c.

  data: gt_zco06 like table of zco01 with header line.

  data : l_index(10) type c.


  loop at gt_itab into gs_itab.

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
      m_gjahr  = gs_itab-gjahr    " 연도
      m_perbl  = gs_itab-perbl    " 월
      m_kostl  = gs_itab-kostl    " 코스트센터
    importing
      e_except = l_except
    tables
      zco06    = gt_zco06.

  clear : gt_zco06, gt_zco06[].

  endloop.

endform.                    " RUN_BAPI
```

물론 이 프로그램 이전에 엑셀에서 값을 읽어오기 위한 get data 와 read data가 필요하다.

```abap
form get_data_section.

  call function 'KCD_EXCEL_OLE_TO_INT_CONVERT'
    exporting
      filename    = p_file
      i_begin_col = 1
      i_begin_row = 1
      i_end_col   = 10
      i_end_row   = 5000
    tables
      intern      = g_xls_t.

  perform read_item_data.

endform.                    " GET_DATA_SECTION
```

```abap
form read_item_data.

  data: w_cnt  type i,
        w_row   like g_xls_t-row,
        w_col1  like g_xls_t-col, w_col2  like g_xls_t-col,
        w_col3  like g_xls_t-col, w_col4  like g_xls_t-col,
        w_col5  like g_xls_t-col. "W_COL6  LIKE G_XLS_T-COL.
*        W_COL7  LIKE G_XLS_T-COL, W_COL8  LIKE G_XLS_T-COL,
*        W_COL9  LIKE G_XLS_T-COL, W_COL10  LIKE G_XLS_T-COL.


  w_row = g_xls_t-row.                 " Header값, 총 row 수

  loop at g_xls_t.
    w_col1  =  g_xls_t-col.
    w_col2  =  w_col1 + 1.
    w_col3  =  w_col2 + 1.
    w_col4  =  w_col3 + 1.
    w_col5  =  w_col4 + 1.
*    W_COL6  =  W_COL5 + 1.
*    W_COL7  =  W_COL6 + 1.
*    W_COL8  =  W_COL7 + 1.
*    W_COL9  =  W_COL8 + 1.
*    W_COL10  =  W_COL9 + 1.

    w_cnt = g_xls_t-row + 1.           "처음 텍스트를 제외하기 위해서
    g_cnt = w_row - g_xls_t-row.       "실데이타가 들어있는 시작 row
    "AT FIRST 기능
    exit.
  endloop.

  g_start_row = w_cnt.                 "탭제외

  clear : gt_itab[].

  do g_cnt times.

    perform convert_excel_data :

    tables g_xls_t using g_start_row w_col1  gs_itab-gjahr,
    tables g_xls_t using g_start_row w_col2  gs_itab-perbl,
    tables g_xls_t using g_start_row w_col3  gs_itab-kostl,
    tables g_xls_t using g_start_row w_col4  gs_itab-kstar,
    tables g_xls_t using g_start_row w_col5  gs_itab-wog05.
*    TABLES G_XLS_T USING G_START_ROW W_COL6  GS_ITAB-scope.
*    TABLES G_XLS_T USING G_START_ROW W_COL7  GS_ITAB-KOSAR,
*    TABLES G_XLS_T USING G_START_ROW W_COL8  GS_ITAB-KHINR,
*    TABLES G_XLS_T USING G_START_ROW W_COL9  GS_ITAB-BUKRS,
*    TABLES G_XLS_T USING G_START_ROW W_COL10 GS_ITAB-GSBER.
    add  1  to g_start_row.

    append gs_itab to gt_itab.  clear  gs_itab.

  enddo.

endform.                    " READ_ITEM_DATA
```
