---
layout: default
title: 30. FOR ALL ENTRIES
parent: abapstudy
nav_order: 30
---

# 30. FOR ALL ENTRIES

FOR ALL ENTRIES 구문은 인터널 테이블의 서브 쿼리와 유사함. WHERE구문의 조건은 인터널 테이블에 존재하는 필드만 필드만 가능.

## 주의점
1. 인터널 테이블의 컬럼과 비교 대상 테이블의 컬럼 타입이 같아야 함.
2. LIKE, BBETWEEN, IN과 같은 비교 구문은 사용 할 수 없음.
3. 인터널테이블의 중복된 값은 하나만 남음.(UNIQUE KEY 기준)
4. 인터널테이블이 NULL이면 모든 데이터를 읽음.
5. 인터널테이블의 수가 많으면 LOOP수가 증가하게 되므로 쿼리 속도가 느려짐

```abap
SELECT *
  INTO CORRESPONDING FIELDS OF TABLE GT_DATA
  FROM BKPF
  FOR ALL ENTRIES IN GT_BSEG
 WHERE BUKRS = GT_BSEG-BUKRS AND BELNR = GT_BSEG-BELNR
   AND GJAHR = GT_BSEG-GJAHR
   AND BUKRS = P_BUKRS
   AND GJAHR = P_GJAHR
   AND BLART IN S_BLART
   AND BELNR IN S_BELNR
   AND BLDAT IN S_BLDAT.
```
