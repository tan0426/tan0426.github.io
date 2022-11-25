---
layout: default
title: AT SELECTION-SCREEN으로 조건달기
parent: abapstudy
nav_order: 9
---
# AT SELECTION-SCREEN으로 조건달기

```abap
AT SELECTION-SCREEN ON S_ZSDATE.
  IF S_ZSDATE-HIGH IS INITIAL.
    MESSAGE '입사일자 HIGH값을 입력해주세요' TYPE 'E'.
  ENDIF.
```
값 미입력시 에러발생

```abap
SELECT-OPTIONS : S_ZEMPNM FOR ZDSUWONT02-ZEMPNM MODIF ID ID1,
                 S_ZGEN FOR ZDSUWONT02-ZGENDER MODIF ID ID1. "8자리 이하로 선언해줘어야 함
PARAMETERS : P_CHK AS CHECKBOX USER-COMMAND CHK1.
```

```abap
AT SELECTION-SCREEN OUTPUT.
  "LOOP AT SCREEN. -> MODIFY SCREEN
  "3. CHECKBOX 체크시 사원명, 성별 SELECT-OPTIONS 필드가 비활성화
  LOOP AT SCREEN.
    IF P_CHK = 'X'. "USER-COMMAND를 설정 해주었기때문에 먹힘
        IF SCREEN-GROUP1 = 'ID1'.
          SCREEN-INPUT = 0. "해당 그룹 필드들 비활성화
        ELSE.
          SCREEN-INTENSIFIED = 1.
        ENDIF.
    ENDIF.
    MODIFY SCREEN.
  ENDLOOP.
```
묶은 ID그룹이 체크박스 체크 시 비활성화 됨.

USER-COMMAND를 적어주었기 때문에 체크박스 체크 시 비활성화가 수행 됨.
