---
layout: default
title: MES-ERP 통합공정 프로젝트
parent: Project
nav_order: 1
---

# MES-ERP 통합공정 프로젝트
{: .no_toc }

# SAP ERP-MES 통합공정 개발 프로젝트

> 1조 탄정엔지니어링
> 

> 김탄정, 이동혁
> 

# 자동차 조립 공정 개발 프로젝트

---

## Project Summary

---

### 🗃️ Table of Contents

---

### 💡 Objective

---

공정 개발부터 MES, ERP 프로그램을 개발하고 다루며 스마트 공정을 전반적으로 이해한다.

- **SAP ABAP 개발**
    
    설계 된 공정과의 연동을 위해 마스터 데이터, 공정 데이터 등을 입력, 조회, 삭제 할 수 있도록 개발 한다.
    
- **MES 공정 개발**
    
    제품 제작 공정을 개발하여 CODESYS를 이용해 공정을 실행하고, SQL SERVER로 ERP데이터와 CODESYS데이터를 연동하여 실행 한다.
    

### 📈 Value

---

SAP ERP 프로그램 사용자가 재료, 플랜트 등 마스터 데이터를 입력 후 생산 계획을 구성하여 입력하면 자동으로 SQL SEVER 연동을 위한 코드가 생성되어 실행 되게 한다.

### 🗺️ Milestones

---

- **ERP 데이터 테이블 구성- 2022/11/09**
    
    사용자 화면을 구성하기에 앞서 테이블을 생성하고 테이블 간 데이터 JOIN 관계를 구성함.
    
- **SAP 화면 디자인 - 2022/11/11**
    
    사용자 화면을 ABAP으로 프로그래밍 하기 전에 화면을 디자인 함.
    
- **BOM** **화면 ABAP 완료 - 2022/11/18**
    
    BOM, BOMROUTING, WORKPLAN CREATE & CHANGE, 조회 ALV화면 제작 완료
    
- **MASTER** **화면 ABAP 완료 - MM/DD/YYYY**
    
    Describe the milestone.
    
- **Milestone 4 - MM/DD/YYYY**
    
    Describe the milestone.
    

## 🔃 Project Process

---

### 📅 계획 표 설정

![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled.png)

### 🏭 공정 설계

![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%201.png)

1. 차량 하부는 외부에서 제작되어 컨테이너에 이송되어 오고, 차량의 외장을 가공하여 조립 하는것을 목표로 한다.
2. 외장에는 하부 파워트레인 이외에도 전장, 라이트 등 추가로 조립되어야 하는 부분이 있다.

| 공정 코드 | 공정 설명 | 공정 투입되는 자재 |
| --- | --- | --- |
| CP-L-WAREHOUSE-2 | 창고에 하부 파워트레인이 저장되어 있음 | 하부 파워트레인 |
| CP-L-UNLOADER | 트럭에 싣기 위해 컨테이너에 삽입 | 하부 파워트레인 컨테이너 |
| CP-L-LOADER | 컨테이너에서 파워트레인을 꺼냄 | 하부 파워트레인 |
| CP-L-LOADER-WRARHOUSE | 창고에 파워트레인을 저장 | 하부 파워트레인 |
| CP-L-WAREHOUSE-1 | 제작된 외장을 창고에 저장 | 외장 프레임 |
| CP-L-WAREHOUSE-1 | 저장되어있는 파워트레인을 외장과 결합 하기 위해 ZIG에 준비시킴 | 하부 파워트레인 |
| CP-P-ASRS | 철판 자재를 가져오고 가공된 외장프레임을 버퍼에 저장 | 철판 |
| CP-P-MEAS | 철판을 대략적인 차량 외장 형태로 압착 하여 제작 | 철판 |
| CP-P-DRILL | ZIG에 설치 시키기위한 핀 홀 등을 드릴링 함 | 외장 프레임 |
| CP-P-RCNC | 외장 프레임을 핀 홀 중심으로 ZIG에 안착 시킨 후 창문 등을 CNC로 가공 | 외장 프레임 |
| CP-P-ASRS-RASS-1,2 | 앞, 뒤 라이트를 다른 창고에서 가져와 조립 | 외장 프레임, 앞,뒤 라이트 |
| CP-P-CAM | 라이트를 조립 한 후 해당 전장 라인 정리 | 외장 프레임 |
| CP-P-MAG-BACK | 나머지 창문, 사이드미러, 백미러, 해당 전장 등을 부착 | 나머지 자재, 외장 프레임 |
| CP-P-HEAT | 라벨을 부착하기 위해 예열 | 외장 프레임 |
| CP-P-LABEL | 라벨 부착 | 외장 프레임, 라벨 |

### 🧑‍💼 MES 실행을 위한 JSON CODE, SQL CODE 분석

- **SQL 코드 리뷰**
    - usp_executeProcess
        
        ```sql
        ALTER PROC [dbo].[usp_executeProcess]
        	@ORDER DATE = null
        AS
        
        	SET NOCOUNT ON;
        
        	EXEC dbo.usp_updateErpMasterData @ORDER=@ORDER;
        	EXEC dbo.usp_updateMesMasterData;
        	EXEC usp_initMesTables;
        
        	DECLARE @OCount INT;
        	DECLARE @k INT;
        	DECLARE @i INT;
        
        	SELECT @OCount=COUNT(*) FROM tblOrder;
        	SET @k = 1;
        
        	WHILE @k <= @OCount
        	BEGIN
        		SELECT @i=ONo 
        		FROM tblOrder 
        		WHERE NOT ([Enabled] = 1) 
        		ORDER BY ONo;
        
        		IF @i IS NOT NULL EXEC usp_enableOrder @ONo=@i;
        		SET @k = @k + 1;
        	END
        ```
        
        1. tblOrder에서 ONo를 한 줄 씩 읽어 옴.
        2. usp_updateErpMasterData에서 더존에서 마스터 데이터를 잃어 옴.
        3. usp_updateMesMasterData에서 Box, Container, Buffer, ResourceMap, Order, Step, Step정보를 순서대로 테이블에 join 한다. **ERP에서 필요한 CODE는 이 테이블에서 찾아 볼 수 있다.**
    - usp_viewOrderPos
        
        ```sql
        USE [DigitalTwinMesDB]
        GO
        /****** Object:  StoredProcedure [dbo].[usp_viewOrderPos]    Script Date: 2022-11-22 오후 4:40:36 ******/
        SET ANSI_NULLS ON
        GO
        SET QUOTED_IDENTIFIER ON
        GO
        -- =============================================
        -- Author:		<Author,,Name>
        -- Create date: <Create Date,,>
        -- Description:	<Description,,>
        -- =============================================
        ALTER PROCEDURE [dbo].[usp_viewOrderPos]
        	-- Add the parameters for the stored procedure here
        AS
        BEGIN
        	-- SET NOCOUNT ON added to prevent extra result sets from
        	-- interfering with SELECT statements.
        	SET NOCOUNT ON;
        
            -- Insert statements for procedure here
        	select ONo,OPos,[Start],[End],StepNo,ResourceID,o.OpNo,op.[Description],AssemblyCode,PNo,StepPNo,OrderPNo,o.WO_CD,p.ITEM_NM AS [WO_NM]
        	from tblOrderPos as o join mesin.WORKORDER as w on  o.WO_CD=w.WO_CD
        		join mesin.PART as p ON w.ITEM_CD=p.ITEM_CD
        		join dbo.tblOperation as op on o.OpNo=op.OpNo
        END
        ```
        
        PLC를 가동시켜 서버로 얻은 코드를 ERP로 가져온 코드 테이블과 join시켜 MES가 실행 될 수 있도록 한다.
        
        그래서 더존BOM에 코드를 넣고 usp_executeProcess를 실행해 MES 실행 테이블에 이 코드를 넣어 둔 후 PLC 공정을 실행하고, usp_viewOrderPos를 실행해 공정 코드를 join시켜 다시 usp_executeProcess로 MES를 실행하는 순서로 진행 한다.
        

- **BOM ROUTING JASON CODE**
    - **모품목 411**
        
        
        | 품번코드 | 품명 | 정미수량 | JSON CODE |
        | --- | --- | --- | --- |
        | 410 | GRBK1740-FRAME | 1 | {"S":10, "O":212, "R":11, "N":20, "F":1} |
        | 410 | GRBK1740-FRAME | 0 | {"S":20, "O":115, "R":12, "N":30} |
        | 410 | GRBK1740-FRAME | 0 | {"S":30, "O":122, "R":13, "N":40} |
        | 410 | GRBK1740-FRAME | 0 | {"S":40, "O":193, "R":14, "N":50} |
        | 410 | GRBK1740-FRAME | 1 | {"S":50, "O":320, "A":304,"R":15, "N":60} |
        | 130 | GRBK1740-MIRROR | 1 | {"A":304, "R":15, "B":31, "P1":410} |
        | 120 | GRBK1740-LIGHT | 2 | {"A":304, "R":15, "B":31, "P1":410} |

- **WORK CENTER (공정) JASON CODE**
    
    
    | 창고명 | JSON CODE |
    | --- | --- |
    | CP-L-WAREHOUSE-1-B1 | {"R":1,"B":1,"T":1,"S":2,"RO":4,"CO":5,"SR":11,"SB":1} |
    | CP-L-WAREHOUSE-1-B2 | {"R":1,"B":2,"T":21,"S":1,"RO":1,"CO":4,"SR":6,"SB":1} |
    | CP-L-LOADER-B1 | {"R":2,"B":1,"T":21,"S":1,"RO":1,"CO":4} |
    | CP-L-LOADER-B2 | {"R":2,"B":2,"T":22,"S":1,"RO":1,"CO":2} |
    | CP-L-LOADER-B3 | {"R":2,"B":3,"T":21,"S":1,"RO":1,"CO":4, "SR":5,"SB":1} |
    | CP-L-LOADER-1011 | {"R":2,"B":4,"T":21,"S":1,"RO":1,"CO":4} |
    | CP-L-UNLOADER-B1 | {"R":3,"B":1,"T":21,"S":1,"RO":1,"CO":4} |
    | CP-L-UNLOADER-B2 | {"R":3,"B":2,"T":21,"S":1,"RO":1,"CO":4} |
    | CP-L-WAREHOUSE-2-B1 | {"R":4,"B":1,"T":1,"S":2,"RO":4,"CO":5} |
    | CP-L-Loader-WAREHOUSE | {"R":5,"B":1,"T":21,"S":1,"RO":1,"CO":4, "SR":2,"SB":3} |
    | CP-L-WAREHOUSE-1-WAREHOUSE | {"R":6,"B":1,"T":21,"S":1,"RO":1,"CO":4, "SR":1,"SB":2} |
    | CP-P-ASRS-B1 | {"R":11,"B":1,"T":1,"S":2,"RO":4,"CO":5,"SR":1,"SB":1} |
    | CP-P-RASS-B1 | {"R":15,"B":1,"T":21,"S":1,"RO":1,"CO":3,"SR":22,"SB":1} |
    | CP-P-MAG-BACK-B1 | {"R":17,"B":1,"T":21,"S":1,"RO":1,"CO":1,"SR":23,"SB":1} |
    | CP-P-WAREHOUSE-ASSEMBLY-B1 | {"R":21,"B":1,"T":21,"S":1,"RO":1,"CO":8} |
    | CP-P-WAREHOUSE-ASSEMBLY-B2 | {"R":21,"B":2,"T":1,"S":2,"RO":4,"CO":5} |
    | CP-P-WAREHOUSE-ASSEMBLY-B3 | {"R":21,"B":3,"T":1,"S":1,"RO":4,"CO":5} |
    | CP-P-WAREHOUSE-ASSEMBLY-B4 | {"R":21,"B":4,"T":1,"S":1,"RO":4,"CO":5} |
    | CP-P-ASRS-RASS-B1 | {"R":22,"B":1,"T":21,"S":1,"RO":1,"CO":3,"SR":15,"SB":1} |
    | CP-P-ASRS-MAG-BACK-B1 | {"R":23,"B":1,"T":21,"S":1,"RO":1,"CO":1,"SR":17,"SB":1} |
    | CP-P-RASS-2-B1 | {"R":31,"B":1,"T":21,"S":1,"RO":1,"CO":3,"SR":32,"SB":1} |
    | CP-P-ASRS-RASS-2-B1 | {"R":32,"B":1,"T":21,"S":1,"RO":1,"CO":3,"SR":31,"SB":1} |
    | CP-P-WAREHOUSE-Assembly-2-B1 | {"R":34,"B":1,"T":21,"S":1,"RO":1,"CO":8} |
    | CP-P-WAREHOUSE-Assembly-2-B2 | {"R":34,"B":2,"T":1,"S":2,"RO":4,"CO":5} |
    | CP-P-WAREHOUSE-Assembly-2-B3 | {"R":34,"B":3,"T":1,"S":1,"RO":4,"CO":5} |
    | CP-P-WAREHOUSE-Assembly-2-B4 | {"R":34,"B":4,"T":1,"S":1,"RO":4,"CO":5} |
    | CP-P-MAG-BACK-PRO-B1 | {"R":35,"B":1,"T":21,"S":1,"RO":1,"CO":1,"SR":36,"SB":1} |
    | CP-P-ASRS-MAG-BACK-PRO-B1 | {"R":36,"B":1,"T":21,"S":1,"RO":1,"CO":1,"SR":35,"SB":1} |

### 📊 사용자 화면 구성을 고려한 테이블 관계 세팅

대략적인 테이블 구성을 디자인 한다. 

1. 김탄정 - BOM, BOM Routing, Work Plan (이하 BOM DATA)
2. 이동혁 - BP, Material, Plant Master Data (이하 MASTER DATA)
- **BOM DATA TABLE**
    - 테이블들의 join 관계를 설정
    
    ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%202.png)
    
    - 각각의 테이블을 입력
    
    ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%203.png)
    
    ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%204.png)
    
    ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%205.png)
    
    ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%206.png)
    
    ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%207.png)
    
- **MASTER DATA TABLE**
    - 테이블들의 join 관계를 설정
    
    ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%208.png)
    
    - 각각의 테이블을 입력
    
    ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%209.png)
    
    ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%2010.png)
    
    ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%2011.png)
    
    ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%2012.png)
    
    ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%2013.png)
    
    ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%2014.png)
    

### ✏️ 사용자 입력, 수정 화면 설계

사용자가 입력 할 화면 구성을 디자인 한다.

- **BOM create or change 화면**
    
    ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%2015.png)
    
    1. MITEM CODE를 넣고 DISPLAY 했을 때 없는 코드이면 에러 메세지. (오른쪽에 제품 명 띄워주면 좋을 것 같다. DISPLAY 하기 전에는 아래 입력 창 막혀있어야 한다.)
    2. ITEM CODE를 넣고 ENTER 하면 COMPONENT가 자동으로 생성 됨.
    3. MITEM CODE를 넣고 DISPLAY를 클릭하면 아래에 얽혀있는 재료들이 뜸.(없으면 안뜸)
    4. 값을 모두 넣고 CREATE를 하면 테이블의 MITEM CODE 데이터가 있는지 확인하고 없으면 INSERT, 있으면 UPDATE.
    5. REFRESH를 하면 모든 값들이 빈칸으로 초기화 됨.
    6. MITEM CODE를 입력하고 엔터 후 DELETE를 클릭했을 때 하위 자재가 있으면 DELETE, 없으면 에러 메세지.
    
- **BOM ROUTING create or change 화면**
    
    ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%2016.png)
    
    1. MITEM CODE를 넣고 ENTER 하면 오른쪽에 제품 이름이 뜸.(BOM이 없는 아이템이면 에러 메세지.)
    2. ITEM CODE는 MITEM CODE에 따라 SEARCH HELP가 생성됨.
    3. STEP_NO는 SEARCH HELP로 선택 가능하고, 선택 하고 나면 OP_NO와 ASS_CODE는 자동으로 생성된다.
    4. RES_ID도 SEARCH HELP로 선택.
    5. 나머지 코드를 모두 입력하면 JSONCODE가 자동으로 생성된다.
    6. 나머지 버튼 수행 방식은 이전과 같다. 다만 DELETE의 경우에는 JSONCODE가 생성되어 있지 않을 시 삭제한다.
- **WORK PLAN create or change 화면**
    
    ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%2017.png)
    
    1. MITEM CODE를 입력하고 ENTER 하면 COMPONENT, DESCRIPTION이 자동 생성 됨.
    2. PLAN_DATE, PLAN_QT를 입력하고 PLAN_CONFIRM은 체크박스로 한다.
    3. 체크박스가 입력되면 나중에 ALV로 조회 할 시 체크 됨을 확인 한다. (초록색 라이트로)
    4. 나머지 버튼 수행 방식은 이전과 같다.
    5. 플랜 테이블이 저장되어 있고 PLAN_CONFIRM이 체크 된 후 CONFIRM을 누르면 ALV로 조회 시 그린 라이트를 생성한다.

### 🖥️ 사용자 조회 화면 디자인

사용자가 입력 한 데이터를 ALV 화면으로 조회 할 수 있도록 한다.

- **BOM Display 화면**
    
    ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%2018.png)
    
    1. MITEM CODE를 입력하고 ENTER를 입력하면 오른쪽에 제품 이름이 뜸.
    2. IMITEM CODE를 입력하고 ENTER를 누른 후 DISPLAY를 클릭하면 ALV화면에 아래쪽 테이블이 쭉 뜸.
- **BOM ROUTING Display 화면**
    
    ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%2019.png)
    
    1. MITEM CODE를 입력하고 ENTER를 입력하면 오른쪽에 제품 이름이 뜸.
    2. IMITEM CODE를 입력하고 ENTER를 누른 후 DISPLAY를 클릭하면 ALV화면에 아래쪽 테이블이 쭉 뜸.
- **WORK PLAN Display 화면**
    
    ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%2020.png)
    
    1. MITEM CODE를 입력하고 DISPLAY를 클릭하면 ALV화면에 아래쪽 테이블이 쭉 뜸.
    2. PLAN이 CONFIRM되어 있으면 그린 라이트를 띄워줌.

### ⌨️ 스크린 생성 (ABAP)

- **BOM create or change screen**
    
    ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%2021.png)
    
    - **SEARCH HELP & AUTO COMPLITE LIST**
        
        
        - **서치 헬프 필드**
        
        | CODE | NAME |
        | --- | --- |
        | PLANTCODE | 창고 코드 |
        | MITEMCODE | 모품목 코드 |
        | ITEMCODE | 재료 코드 |
        
        - **자동 완성 필드**
        
        | CODE | NAME |
        | --- | --- |
        | COMPONENT | 재료 명 |
        | UOM | 단위 |
    - **입력 화면 설명**
        1. 창고 코드를 입력하고 ENTER를 클릭하여야 모품목 코드를 입력 할 수 있다.
        2. 재료 코드를 입력하면 재료 명과 단위가 자동으로 생성된다. 단위는 EA를 DEFAULT로 하고 변경 할 수 있다.
    - **버튼 기능 설명**
        
        
        | 버튼 | 기능 | 세부 기능 |
        | --- | --- | --- |
        | DISPLAY | 조회 | 모품목 코드가 품목 테이블에 있는 제품이면 BOM이 있는 제품인지 아닌지 확인 후 BOM이 있다면 내부 품목을 조회, 없다면 메세지 출력  |
        | CREATE | 생성 또는 변경 | 모품목 코드에 하위 제품 BOM이 있다면 데이터를 변경, 없다면 생성한다. 이때 변경 로그 또는 생성 로그를 기록 한다. |
        | REFRESH | 입력 초기화 | 입력한 값을 모두 초기화 한다. |
        | DELETE | 데이터 삭제 | 삭제 버튼 클릭 시 팝업 경고창을 띄운 후 삭제 동의를 한다면 삭제 처리 한다. 여기서는 삭제 됨을 확인하기 위해 플래그를 사용하지 않고 데이터베이스에서 데이터를 완전 삭제 한다. |
    - **코드 설명**
        - MAIN PROGRAM
            
            ```abap
            REPORT ZDKTJ_BOM.
            
            INCLUDE ZDKTJ_BOM_TOP. "데이터 선언부
            INCLUDE ZDKTJ_BOM_SCR. "SELECT SCREEN
            INCLUDE ZDKTJ_BOM_PBO. "PBO
            INCLUDE ZDKTJ_BOM_PAI. "PAI
            INCLUDE ZDKTJ_BOM_F01. "나머지 프로그램
            
            INITIALIZATION.
            
            START-OF-SELECTION.
            
            CALL SCREEN 100.
            ```
            
            INCLUDE 에 코드를 정리 한 후 100 스크린을 띄운다.
            
        - SCREEN 100
            
            ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%2022.png)
            
            ```abap
            PROCESS BEFORE OUTPUT.
             MODULE STATUS_0100. "스크린에 처음 띄워 줄 버튼 설정
             MODULE INIT_SCREEN.
             LOOP WITH CONTROL TC100.
               MODULE MOVE_ABAP_TO_SCREEN.
               MODULE INIT_TC100.
             ENDLOOP.
            ```
            
            1. MODULE STATUS_0100 : 화면에 처음 띄워 줄 버튼과 제목을 지정 해줌.
            2. MODULE INIT_SCREEN : 창고 코드를 입력 하여야 모품목 코드를 입력 할 수 있게 활성화.
                
                ```abap
                LOOP AT SCREEN.
                  IF GS_BOM-PLANTCODE IS NOT INITIAL. "창고코드 적히면 입력 활성화
                    IF SCREEN-NAME = 'GS_BOM-MITEMCODE'.
                      SCREEN-INPUT = 1. "입력 활성화
                      SCREEN-REQUIRED = '1'. "필수값 활성화
                    ENDIF.
                  ELSE.
                    IF SCREEN-NAME = 'GS_BOM-MITEMCODE'.
                      SCREEN-INPUT = 0.
                      SCREEN-REQUIRED = 1.
                    ENDIF.
                  ENDIF.
                  MODIFY SCREEN.
                ENDLOOP.
                ```
                
            3. MODULE MOVE_ABAP_TO_SCREEN : 테이블 TC100의 CURRENT_LINE에 해당하는 인터널 테이블(GT_BOM) 라인을 한줄 읽어 WORK AREA GS_BOM에 넣는다. INIT_TC100은 컨트롤 테이블 초기 세팅이지만 사용하지 않았다.
            
            ```abap
            PROCESS AFTER INPUT.
             FIELD GS_BOM-PLANTCODE MODULE CHECK_FIELD_PLANTCODE ON REQUEST.
             FIELD GS_BOM-MITEMCODE MODULE CHECK_FIELD_MITEMCODE ON REQUEST.
            
             MODULE USER_COMMAND_0100.
             LOOP WITH CONTROL TC100.
               MODULE MOVE_SCREEN_TO_ABAP. "TC100테이블 값 입력 방식 설정
             ENDLOOP.
            
             "우선적으로 BACK, CANC, EXIT 버튼을 동작 시키기 위한 모듈
             MODULE EXIT_COMMAND AT EXIT-COMMAND.
            
             PROCESS ON VALUE-REQUEST. "서치헬프
             FIELD GS_BOM-PLANTCODE MODULE F4_PLANTCODE.
             FIELD GS_BOM-MITEMCODE MODULE F4_MITEMCODE.
             FIELD GS_BOM-ITEMCODE MODULE F4_TABLECONTROL_FIELD.
            ```
            
            1. MODULE CHECK_FIELD_PLANTCODE & MITEMCODE : 창고 또는 모품목 코드가 입력되면 각각 테이블에서 이름을 찾아와 옆에 띄워주기 위한 모듈.
            2. MODULE USER_COMMAND_0100 : DISPLAY, CREATE, REFRESH, DELETE 버튼의 기능을 설정. (다음 코드는 CREATE 버튼 일부만 가져 옴.  나머지는 생략한다.)
                
                ```abap
                WHEN 'CREA'.
                  LOOP AT GT_BOM INTO GS_BOM.
                    IF GS_BOM-ITEMCODE IS NOT INITIAL
                    AND GS_BOM-COMPONENT IS NOT INITIAL
                    AND GS_BOM-UOM IS NOT INITIAL
                    AND GS_BOM-QUANTITY IS NOT INITIAL. 
                      "값이 모두 입력되었는지 확인
                      "BOM이 있으면 변경, 아니면 입력
                      SELECT SINGLE *
                        INTO CORRESPONDING FIELDS OF GT_ZTJ_BOM
                        FROM ZTJ_BOM
                        WHERE MITEMCODE = GS_BOM-MITEMCODE.
                      IF SY-SUBRC = 0.
                        MOVE-CORRESPONDING GS_BOM TO GT_ZTJ_BOM.
                
                        "변경 로그
                        GT_ZTJ_BOM-AEDAT = SY-DATUM.
                        GT_ZTJ_BOM-AEZET = SY-UZEIT.
                        GT_ZTJ_BOM-AENAM = SY-UNAME.
                        APPEND GT_ZTJ_BOM.
                        CLEAR GT_ZTJ_BOM.
                
                        "변경 되면 인터널 테이블에서 DB테이블로 입력
                        IF GT_ZTJ_BOM[] IS NOT INITIAL.
                          MODIFY ZTJ_BOM FROM TABLE GT_ZTJ_BOM.
                          COMMIT WORK.
                
                          IF SY-SUBRC = 0.
                            MESSAGE '변경 성공' TYPE 'S'.
                          ENDIF.
                        ENDIF.
                
                      ELSE.
                        MOVE-CORRESPONDING GS_BOM TO GT_ZTJ_BOM.
                
                        "생성 로그
                        GT_ZTJ_BOM-ERDAT = SY-DATUM.
                        GT_ZTJ_BOM-ERZET = SY-UZEIT.
                        GT_ZTJ_BOM-ERNAM = SY-UNAME.
                        APPEND GT_ZTJ_BOM.
                        CLEAR GT_ZTJ_BOM.
                
                        "저장 되면 인터널 테이블에서 DB테이블로 입력
                        IF GT_ZTJ_BOM[] IS NOT INITIAL.
                          MODIFY ZTJ_BOM FROM TABLE GT_ZTJ_BOM.
                          COMMIT WORK.
                
                          IF SY-SUBRC = 0.
                            MESSAGE '저장 성공' TYPE 'S'.
                          ENDIF.
                        ENDIF.
                
                      ENDIF.
                    ENDIF.
                  ENDLOOP.
                ```
                
            3. PROCESS ON VALUE-REQUEST. 이후 모듈들은 CALL FUNCTION인 F4IF_INT_TABLE_VALUE_REQUEST를 이용한 서치헬프 모듈이다.
                
                ```abap
                "서치 헬프 가져올 테이블 지정
                  DATA : BEGIN OF LS_HELP,
                          MITEMCODE TYPE ZTJ_ITEM-ITEM_CODE,
                          COMPONENT TYPE ZTJ_ITEM-COMPONENT,
                         END OF LS_HELP,
                         LT_HELP LIKE TABLE OF LS_HELP.
                  DATA : LS_RETURN LIKE DDSHRETVAL,
                         LT_RETURN LIKE TABLE OF DDSHRETVAL.
                
                  SELECT ITEM_CODE AS MITEMCODE, COMPONENT
                    INTO CORRESPONDING FIELDS OF TABLE @LT_HELP
                    FROM ZTJ_ITEM.
                
                  SORT LT_HELP.
                
                  "SEARCH HELP FUNCTION
                  CALL FUNCTION 'F4IF_INT_TABLE_VALUE_REQUEST'
                    EXPORTING
                      retfield               = 'MITEMCODE'
                     VALUE_ORG              = 'S'
                    tables
                      value_tab              = LT_HELP
                     RETURN_TAB             = LT_RETURN
                
                  "서치헬프 창에서 선택하면 값을 가져옴
                  IF LT_RETURN IS NOT INITIAL.
                    LOOP AT LT_RETURN INTO LS_RETURN.
                      GS_BOM-MITEMCODE = LS_RETURN-FIELDVAL.
                    ENDLOOP.
                  ENDIF.
                ```
                
- **BOM ROUTING create or change screen**
    
    ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%2023.png)
    
    - **SEARCH HELP & AUTO COMPLITE LIST**
        
        
        - **서치 헬프 필드**
        
        | CODE | NAME |
        | --- | --- |
        | MITEMCODE | 모품목 코드 |
        | ITEMCODE | 품목 코드 |
        | STEP_NO | 재료 코드 |
        | NEXTSTEP | 다음 스텝 |
        | RES_ID | 창고 코드O |
        
        - **자동 완성 필드**
        
        | CODE | NAME |
        | --- | --- |
        | OP_NO | 공정 번호 |
        | ASS_NO | 조립 번호 |
        | DESCRIPTION | 설명 |
        | JSONCODE | JSON CODE |
    - **입력 화면 설명**
        1. 모품목 코드는 필수값이며 입력 하고 ENTER를 클릭하면 아래에 품목명이 나타난다.
        2. 품목 코드를 서치 헬프로 입력한 후 ENTER를 클릭하면 단위가 디폴트 EA로 입력된다.
        3. 스텝 코드를 서치헬프로 입력하면 공정, 조립 번호, 공정 설명이 자동으로 입력된다. 이때 스텝번호화 공정 번호는 연결되어 있다고 가정한 테이블을 이용한다. (따로 입력해서 저장 할 수 있으나 본 프로젝트는 학습이 목적이므로 묶어서 이용하도록 했다.)
        4. 창고 번호와 다음 스텝 번호를 입력한 후 ENTER를 클릭하고, CREATE를 하면 JASON CODE가 자동으로 생성되며 ZTJ_BOMROUTING 테이블에 저장된다.
        5. DISPLAY를 하기 위해서는 REFRESH로 테이블을 청소 한 후 띄워준다. 이때 자동 생성 된 JSON코드가 나타남을 확인 할 수 있다.
        6. DELETE를 하면 입력된 모품목 코드를 중심으로 ZTJ_BOMROUTING 테이블에서 데이터가 삭제된다.
    - **버튼 기능 설명**
        
        
        | 버튼 | 기능 | 세부 기능 |
        | --- | --- | --- |
        | DISPLAY | 조회 | 모품목 코드가 품목 테이블에 있는 제품이면 BOMROUTING이 있는 제품인지 아닌지 확인 후 BOMROUTING이 있다면 조회, 없다면 경고 메세지를 생성한다. |
        | CREATE | 생성 또는 변경 | 모품목 코드에 하위 제품 BOMROUTING이 있다면 데이터를 변경, 없다면 생성한다. 이때 변경 로그 또는 생성 로그를 기록 한다. 만약 BOM이 없는 품목이면 에러메세지가 나타나면서 생성 되지 않는다. |
        | REFRESH | 입력 초기화 | 입력한 값을 모두 초기화 한다. |
        | DELETE | 데이터 삭제 | 삭제 버튼 클릭 시 팝업 경고창을 띄운 후 삭제 동의를 한다면 삭제 처리 한다. 여기서는 삭제 됨을 확인하기 위해 플래그를 사용하지 않고 데이터베이스에서 데이터를 완전 삭제 한다. |
    - **코드 설명**
        - MAIN PROGRAM
            
            INCLUDE 에 코드를 정리 한 후 100 스크린을 띄운다. (위와 같다)
            
        - SCREEN 100
            
            ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%2024.png)
            
            USER-COMMAND 모듈에서 CREATE 버튼을 클릭 시 JSON CODE가 자동 생성되는 코드는 CONCATENATE 명령어를 이용하였다. 다음은 WHEN ‘CREA’ 버튼 동작 코드이다.
            
            ```abap
            IF LV_MITEMCODE IS NOT INITIAL.
              IF GS_BOMROUT-ITEMCODE IS NOT INITIAL
              AND GS_BOMROUT-STEP_NO IS NOT INITIAL
              AND GS_BOMROUT-RES_ID IS NOT INITIAL
              AND GS_BOMROUT-NEXTSTEP IS NOT INITIAL
              AND GS_BOMROUT-INPUT_QT IS NOT INITIAL.
                SELECT SINGLE *
                  INTO CORRESPONDING FIELDS OF GT_ZTJ_BOMROUT
                  FROM ZTJ_BOMROUTING
                  WHERE MITEMCODE = GS_BOMROUT-MITEMCODE.
                IF SY-SUBRC = 0.
                  MOVE-CORRESPONDING GS_BOMROUT TO GT_ZTJ_BOMROUT.
            
                  CONCATENATE '{"S":' GS_BOMROUT-STEP_NO ',' '"O":' GS_BOMROUT-OP_NO 
                  ',' '"A":' GS_BOMROUT-ASS_CODE ',' '"R":' GS_BOMROUT-RES_ID ',' 
                  '"N":' GS_BOMROUT-NEXTSTEP '}'
                  INTO LV_JSONCODE.
            
                  "변경 로그
                  GT_ZTJ_BOMROUT-AEDAT = SY-DATUM.
                  GT_ZTJ_BOMROUT-AEZET = SY-UZEIT.
                  GT_ZTJ_BOMROUT-AENAM = SY-UNAME.
                  APPEND GT_ZTJ_BOMROUT.
            
                  GT_ZTJ_BOMROUT-JSONCODE = LV_JSONCODE.
                  APPEND GT_ZTJ_BOMROUT.
                  CLEAR GT_ZTJ_BOMROUT.
            
                  "변경 되면 인터널 테이블에서 DB테이블로 입력
                  IF GT_ZTJ_BOMROUT[] IS NOT INITIAL.
                    MODIFY ZTJ_BOMROUTING FROM TABLE GT_ZTJ_BOMROUT.
                    COMMIT WORK.
                    IF SY-SUBRC = 0.
                      MESSAGE '변경 성공' TYPE 'S'.
                    ENDIF.
                  ENDIF.
                ELSE.
                  MOVE-CORRESPONDING GS_BOMROUT TO GT_ZTJ_BOMROUT.
            
            -------------------------------------------------- 생성로그 부분 생략
            ```
            
- **WORK PLAN create or change screen**
    
    ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%2025.png)
    
    - **SEARCH HELP & AUTO COMPLITE LIST**
        
        
        - **서치 헬프 필드**
        
        | CODE | NAME |
        | --- | --- |
        | MITEMCODE | 모품목 코드 |
        | PLAN_DATE | 계획 날짜 |
        
        - **자동 완성 필드**
        
        | CODE | NAME |
        | --- | --- |
        | COMPONENT | 공정 번호 |
        | DESCRIPTION | 공정 설명 |
        | DESCRIPTION | 설명 |
        | JSONCODE | JSON CODE |
        | PLAN_CONFIRM | 계획 실행 여부 |
    - **입력 화면 설명**
        1. 모품목 코드는 필수값이며 입력 하고 ENTER를 클릭하면 아래에 품목명이 나타나며 비활성화 되어 있던 제작 날짜와 제작 수량이 입력 가능하도록 활성화 된다.
        2. DISPLAY를 클릭했을 때 계획이 없는 모품목이면 에러메세지가 뜨고 계획이 있으면 컨트롤 테이블에 데이터가 보여진다.
        3. 추가를 클릭하면 계획이 한 줄 추가되고 제작 날짜와 제작 수량을 모두 작성 한 후 CREATE를 클릭하면 DB 테이블에 데이터가 입력된다.
        4. REFRESH를 클릭하면 입력한 값들이 모두 지워진다.
        5. DELETE를 클릭하면 만약 계획이 하나 일 경우 모품목 코드를 중심으로 DB 테이블에서 데이터가 모두 삭제되고, 계획이 여러 개 일 경우 체크가 되어있는 계획이 삭제된다.
        6. 체크를 한 후 CONFIRM을 클릭하면 그 계획이 실행 됨을 표시해 준다.
    - **버튼 기능 설명**
        
        
        | 버튼 | 기능 | 세부 기능 |
        | --- | --- | --- |
        | DISPLAY | 조회 | 모품목 코드가 품목 테이블에 있는 제품이면 WORKPLAN이 있는 제품인지 아닌지 확인 후 WORKPLAN이 있다면 조회, 없다면 경고 메세지를 생성한다. |
        | CREATE | 생성 또는 변경 | 모품목 코드에 하위 계획이 있다면 데이터를 변경, 없다면 생성한다. 이때 변경 로그 또는 생성 로그를 기록 한다. 만약 BOMROUTING이 없는 품목이면 에러메세지가 나타나면서 생성 되지 않는다. |
        | REFRESH | 입력 초기화 | 입력한 값을 모두 초기화 한다. |
        | DELETE | 데이터 삭제 | 삭제 버튼 클릭 시 팝업 경고창을 띄운 후 삭제 동의를 한다면 삭제 처리 한다. 계획이 하나만 있다면 전체 삭제, 여러개 있다면 체크 표시 한 계획만 삭제한다. 여기서는 삭제 됨을 확인하기 위해 플래그를 사용하지 않고 데이터베이스에서 데이터를 완전 삭제 한다. |
        | CONFIRM | 계획 실행 여부 | 체크를 한 후 CONFIRM을 클릭하면 실행 여부 표시 ‘X’가 된다. |
    - **코드 설명**
        - MAIN PROGRAM
            
            INCLUDE 에 코드를 정리 한 후 100 스크린을 띄운다. (위와 같다)
            
        - SCREEN 100
            
            ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%2026.png)
            
            USER-COMMAND 모듈에서 추가 버튼 클릭 시 BOMROUTING이 있는 모품목만 추가 되도록 하기 위해서 SELECT INNERJOIN을 이용했다. 품목군도 불러오기 위해 품목군 테이블을 추가로 JOIN 했다.
            
            ```abap
            WHEN 'PUSH'.
              IF GS_WORKPL-MITEMCODE IS NOT INITIAL.
                SELECT SINGLE ITEM~COMPONENT ITEMC~ICNAME
                  INTO CORRESPONDING FIELDS OF GS_DATA
                  FROM ZTJ_ITEM AS ITEM
                  JOIN ZTJ_BOMROUTING AS ROUT
                  ON ITEM~ITEM_CODE = ROUT~MITEMCODE
                  JOIN ZTJ_ITEMC AS ITEMC
                  ON ITEM~ITEM_GROUP = ITEMC~ICODE
                  WHERE ITEM~ITEM_CODE = GS_WORKPL-MITEMCODE.
            
                IF SY-SUBRC = 0.
                  MOVE-CORRESPONDING GS_DATA TO GS_WORKPL.
                  APPEND GS_WORKPL TO GT_WORKPL.
                  IF SY-SUBRC = 0.
                    MESSAGE '추가 완료' TYPE 'S'.
                    CLEAR GS_WORKPL.
                  ENDIF.
                ELSE.
                  MESSAGE 'BOM ROUTING이 없는 품목' TYPE 'S' DISPLAY LIKE 'E'.
                ENDIF.
              ELSE.
                MESSAGE '모품목 코드가 입력되지 않음' TYPE 'S' DISPLAY LIKE 'E'.
              ENDIF.
            ```
            
            DELETE 버튼 클릭 시 DB데이터가 한 줄 이면 모품목 코드를 중심으로 삭제, 여러 개 이면 체크 한 줄만 삭제 하도록 했다.
            
            ```abap
            WHEN 'DELE'.
              SELECT *
                INTO CORRESPONDING FIELDS OF TABLE GT_ZTJ_WORKPL
                FROM ZTJ_WORKPLAN
                WHERE MITEMCODE = GS_WORKPL-MITEMCODE.
            
              IF LINES( GT_ZTJ_WORKPL ) > 1.
                LOOP AT GT_WORKPL INTO GS_WORKPL.
                  IF GS_WORKPL-CHK IS NOT INITIAL.
                    CLEAR LV_ANSWER.
                    CALL FUNCTION 'POPUP_TO_CONFIRM' "FUNCTION 내용 생략
            
                    IF LV_ANSWER = '1'.
                      "삭제 처리
                      DELETE FROM ZTJ_WORKPLAN WHERE MITEMCODE = GS_WORKPL-MITEMCODE
                                               AND PLAN_DATE = GS_WORKPL-PLAN_DATE.
                      COMMIT WORK. "COMMIT WORK AND WAIT도 가능한듯.
                      IF SY-SUBRC = 0.
                        MESSAGE '삭제 완료' TYPE 'S'.
                      ENDIF.
                    ENDIF.
                  ENDIF.
                ENDLOOP.
            
              ELSEIF LINES( GT_ZTJ_WORKPL ) = 1.
                CLEAR LV_ANSWER.
                CALL FUNCTION 'POPUP_TO_CONFIRM' "FUNCTION 내용 생략
                          .
                IF sy-subrc <> 0.
            *   Implement suitable error handling here
                ENDIF.
            
                IF LV_ANSWER = '1'.
                  "삭제 처리
                  DELETE FROM ZTJ_WORKPLAN WHERE MITEMCODE = GS_WORKPL-MITEMCODE.
                  COMMIT WORK. "COMMIT WORK AND WAIT도 가능한듯.
                  IF SY-SUBRC = 0.
                    MESSAGE '삭제 완료' TYPE 'S'.
                  ENDIF.
                ENDIF.
              ENDIF.
            ```
            
- **BOM, ROUTING, WORK PLAN 통합 ALV 조회 화면**
    
    
    ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%2027.png)
    
    ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%2028.png)
    
    ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%2029.png)
    
    ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%2030.png)
    
    - **SEARCH HELP & AUTO COMPLITE LIST**
        - **서치 헬프 필드**
        
        | CODE | NAME |
        | --- | --- |
        | MITEMCODE | 모품목 코드 |
    - **입력 화면 설명**
        1. 각각 데이터가 존재하는 모품목 코드를 입력한 후 조회 버튼을 클릭하면 해당 모품목에 해당하는 BOM, BOM ROUTING, WORK PLAN ALV 테이블이 조회 된다.
    - **버튼 기능 설명**
        
        
        | 버튼 | 기능 | 세부 기능 |
        | --- | --- | --- |
        | 조회 | ALV 테이블 조회 | 해당 모품목 코드에 해당하는 데이터들이 ALV화면에 조회된다. |
    - **코드 설명**
        - MAIN PROGRAM
            
            INCLUDE 에 코드를 정리 한 후 100 스크린을 띄운다. (위와 같다)
            
        - SCREEN 100
            
            ![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%2031.png)
            
            USER-COMMAND 모듈에서 조회 버튼을 각각 클릭 시 생성되는 스크린은 각각 다르게 하여 ALV화면을 구성하였다.
            
            ```abap
            SAVE_OK = OK_CODE.
            
            CASE OK_CODE.
              WHEN 'PUSH1'.
                IF GS_BOM-MITEMCODE IS NOT INITIAL.
                  CALL SCREEN 200.
                ENDIF.
              WHEN 'PUSH2'.
                IF GS_BOMROUT-MITEMCODE IS NOT INITIAL.
                  CALL SCREEN 300.
                ENDIF.
              WHEN 'PUSH3'.
                IF GS_WORKPL-MITEMCODE IS NOT INITIAL.
                  CALL SCREEN 400.
                ENDIF.
            ENDCASE.
            CLEAR OK_CODE.
            ```
            
            버튼을 클릭 후 해당 스크린으로 이동하면 ALV화면이 뜬다.
            
            ```abap
            PROCESS BEFORE OUTPUT.
             MODULE STATUS_0200.
             MODULE DISPLAY_ALV_0200.
            
            PROCESS AFTER INPUT.
             MODULE USER_COMMAND_0200.
            ```
            
            DOCKING CONTAINER에 그리드를 그린 후 이 그리드에 테이블을 넣었다.
            
            ```abap
            DATA : GT_ZTJ_BOM LIKE TABLE OF ZTJ_BOM WITH HEADER LINE,
                     GT_ZTJ_BOMROUT LIKE TABLE OF ZTJ_BOMROUTING WITH HEADER LINE,
                     GT_ZTJ_WORPL LIKE TABLE OF ZTJ_WORKPLAN WITH HEADER LINE.
            
              CREATE OBJECT gc_docking
                EXPORTING
                  repid                       = SY-REPID
                  dynnr                       = SY-DYNNR
                  extension                   = 5000.
            
              CREATE OBJECT gc_grid
                EXPORTING
                  i_parent          = GC_DOCKING.
            
              GS_FCAT-FIELDNAME = 'ITEMCODE'.
              GS_FCAT-COLTEXT = '품목 코드'.
              APPEND GS_FCAT TO GT_FCAT.
              CLEAR GS_FCAT.
             "GS_FCAT 이하 생략-------------------------
            
              SELECT *
                INTO CORRESPONDING FIELDS OF TABLE GT_ZTJ_BOM
                FROM ZTJ_BOM
                WHERE MITEMCODE = GS_BOM-MITEMCODE.
            
              CALL METHOD gc_grid->set_table_for_first_display
            
                CHANGING
                  it_outtab                     = GT_ZTJ_BOM[]
                  it_fieldcatalog               = GT_FCAT.
            
              CLEAR : GT_ZTJ_BOM, GT_ZTJ_BOM[], GT_FCAT.
            ```
            

### ▶️ MES 실행

```sql
use DigitalTwinMesDB

exec usp_viewOrderPos

exec usp_viewStepDef @OrderPno=1010; --주문제품의 세부 생산공정 계획 보기

exec usp_viewBufferPos @ResourceID=21, @BufNo=1 --창고의 재고현황을 모니터링

exec usp_viewBoxPos @BoxID=1 --Box의 내용물 모니터링

exec usp_viewStep @OrderPno=50 --특정 품목의 스텝별 작업 진행여부 모니터링
```

![Untitled](SAP%20ERP-MES%20%E1%84%90%E1%85%A9%E1%86%BC%E1%84%92%E1%85%A1%E1%86%B8%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A5%E1%86%BC%20%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8C%E1%85%A6%E1%86%A8%E1%84%90%E1%85%B3%2076ed9f7dfe9545e6911758ff6f09a9bf/Untitled%2032.png)

각 공정별로 실행이 잘 됨을 확인 할 수 있다.

## 🏁 Outcome

---

MES를 실행하기 위해 제작한 SAP 스크린에서 데이터를 삽입하고 JSONCODE를 얻어 냄.

## 🔗 Related Documents

---

[스팩 프젝 ABAP코드 정리](https://www.notion.so/ABAP-0e12e32897a140ab934b5fad9a8d2c94)
