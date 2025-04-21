# **Microsoft Fabric Lakehouse에서 Medallion 아키텍처 생성**

이 실습에서는 Notebook을 사용하여 Fabric Lakehouse에 Medallion 아키텍처를 구축합니다. Workspace를 생성하고, Lakehouse를 만들고, Bronze 계층에 데이터를 업로드하고, 데이터를 변환하여 Silver Delta 테이블에 로드하고, 데이터를 추가 변환하여 Gold Delta 테이블에 로드한 다음, Semantic Model을 탐색하고 관계를 생성합니다.

이 실습은 완료하는 데 약 45분이 소요됩니다.

**참고:** 이 실습을 완료하려면 Microsoft Fabric 평가판이 필요합니다.

**Workspace 생성**

Fabric에서 데이터 작업을 시작하기 전에 Fabric 평가판이 활성화된 Workspace를 만듭니다.

1.  브라우저에서 Microsoft Fabric 홈페이지 (`https://app.fabric.microsoft.com/home?experience=fabric`)로 이동하고 Fabric 자격 증명으로 로그인합니다.
2.  왼쪽 메뉴 모음에서 **Workspaces** (아이콘 모양 🗇)를 선택합니다.
3.  Fabric 용량(Trial, Premium 또는 Fabric)을 포함하는 라이선싱 모드를 선택하여 원하는 이름으로 새 Workspace를 만듭니다.
4.  새 Workspace가 열리면 비어 있어야 합니다.
5.  Workspace 설정으로 이동하여 **Data model editing** 미리 보기 기능을 활성화합니다. 이를 통해 Power BI Semantic Model을 사용하여 Lakehouse의 테이블 간 관계를 생성할 수 있습니다.

    *Fabric의 Workspace 설정 페이지 스크린샷.*

**참고:** 미리 보기 기능을 활성화한 후 브라우저 탭을 새로 고쳐야 할 수 있습니다.

**Lakehouse 생성 및 Bronze 계층에 데이터 업로드**

이제 Workspace가 있으므로 분석할 데이터를 위한 데이터 Lakehouse를 만들 차례입니다.

1.  방금 만든 Workspace에서 **+ New item** 버튼을 클릭하여 **Sales**라는 이름의 새 **Lakehouse**를 만듭니다.
2.  약 1분 후 새 빈 Lakehouse가 생성됩니다. 다음으로 분석을 위해 일부 데이터를 데이터 Lakehouse로 가져옵니다. 이를 수행하는 방법은 여러 가지가 있지만, 이 실습에서는 텍스트 파일을 로컬 컴퓨터(또는 해당되는 경우 랩 VM)에 다운로드한 다음 Lakehouse에 업로드합니다.
3.  이 실습용 데이터 파일을 `https://github.com/MicrosoftLearning/dp-data/blob/main/orders.zip` 에서 다운로드합니다. 파일의 압축을 풀고 원래 이름으로 로컬 컴퓨터(또는 해당되는 경우 랩 VM)에 저장합니다. 3개 연도의 판매 데이터가 포함된 3개의 파일이 있어야 합니다: `2019.csv`, `2020.csv`, `2021.csv`.
4.  Lakehouse가 포함된 웹 브라우저 탭으로 돌아가서 **Explorer** 창의 **Files** 폴더에 대한 `...` 메뉴에서 **New subfolder**를 선택하고 `bronze`라는 이름의 폴더를 만듭니다.
5.  `bronze` 폴더의 `...` 메뉴에서 **Upload** 및 **Upload files**를 선택한 다음 로컬 컴퓨터(또는 해당되는 경우 랩 VM)에서 3개의 파일(`2019.csv`, `2020.csv`, `2021.csv`)을 Lakehouse에 업로드합니다. `Shift` 키를 사용하여 3개의 파일을 한 번에 업로드합니다.
6.  파일이 업로드된 후 `bronze` 폴더를 선택하고 다음과 같이 파일이 업로드되었는지 확인합니다.

    *Lakehouse에 업로드된 products.csv 파일 스크린샷.*

**데이터 변환 및 Silver Delta 테이블 로드**

이제 Lakehouse의 Bronze 계층에 일부 데이터가 있으므로 Notebook을 사용하여 데이터를 변환하고 Silver 계층의 Delta 테이블에 로드할 수 있습니다.

1.  데이터 레이크의 `bronze` 폴더 내용을 보면서 **Home** 페이지의 **Open notebook** 메뉴에서 **New notebook**을 선택합니다.
2.  몇 초 후 단일 셀이 포함된 새 Notebook이 열립니다. Notebook은 코드 또는 Markdown(서식 있는 텍스트)을 포함할 수 있는 하나 이상의 셀로 구성됩니다.
3.  Notebook이 열리면 Notebook 왼쪽 상단의 **Notebook xxxx** 텍스트를 선택하고 새 이름 **Transform data for Silver**를 입력하여 이름을 변경합니다.

    *Transform data for silver라는 이름의 새 Notebook 스크린샷.*

4.  Notebook에서 일부 간단한 주석 처리된 코드가 포함된 기존 셀을 선택합니다. 이 코드는 필요하지 않으므로 이 두 줄을 강조 표시하고 삭제합니다.

**참고:** Notebook을 사용하면 Python, Scala, SQL 등 다양한 언어로 코드를 실행할 수 있습니다. 이 실습에서는 PySpark와 SQL을 사용합니다. 코드를 문서화하기 위해 서식 있는 텍스트와 이미지를 제공하는 Markdown 셀을 추가할 수도 있습니다.

5.  다음 코드를 셀에 붙여넣습니다.

    ```python
    # code
    from pyspark.sql.types import *
        
    # 테이블 스키마 생성
    orderSchema = StructType([
        StructField("SalesOrderNumber", StringType()),
        StructField("SalesOrderLineNumber", IntegerType()),
        StructField("OrderDate", DateType()),
        StructField("CustomerName", StringType()),
        StructField("Email", StringType()),
        StructField("Item", StringType()),
        StructField("Quantity", IntegerType()),
        StructField("UnitPrice", FloatType()),
        StructField("Tax", FloatType())
        ])
        
    # Lakehouse의 bronze 폴더에서 모든 파일 가져오기
    df = spark.read.format("csv").option("header", "true").schema(orderSchema).load("Files/bronze/*.csv")
        
    # 데이터 프레임의 처음 10개 행을 표시하여 데이터 미리 보기
    display(df.head(10))
    ```

6.  셀 왼쪽의 **▷ (Run cell)** 버튼을 사용하여 코드를 실행합니다.

**참고:** 이 Notebook에서 Spark 코드를 처음 실행하는 것이므로 Spark 세션을 시작해야 합니다. 즉, 첫 번째 실행은 완료하는 데 1분 정도 걸릴 수 있습니다. 이후 실행은 더 빠릅니다.

7.  셀 명령이 완료되면 셀 아래의 출력을 검토합니다. 다음과 유사해야 합니다.

    | Index | SalesOrderNumber | SalesOrderLineNumber | OrderDate  | CustomerName   | Email                       | Item                   | Quantity | UnitPrice | Tax     |
    | :---- | :--------------- | :------------------- | :--------- | :------------- | :-------------------------- | :--------------------- | :------- | :-------- | :------ |
    | 1     | SO49172          | 1                    | 2021-01-01 | Brian Howard   | brian23@adventure-works.com | Road-250 Red, 52       | 1        | 2443.35   | 195.468 |
    | 2     | SO49173          | 1                    | 2021-01-01 | Linda Alvarez  | linda19@adventure-works.com | Mountain-200 Silver, 38 | 1        | 2071.4197 | 165.7136|
    | …     | …                | …                    | …          | …              | …                           | …                      | …        | …         | …       |

    실행한 코드는 `bronze` 폴더의 CSV 파일에서 데이터를 Spark DataFrame으로 로드한 다음 DataFrame의 처음 몇 행을 표시했습니다.

**참고:** 출력 창 왼쪽 상단의 `...` 메뉴를 선택하여 셀 출력 내용을 지우고, 숨기고, 자동 크기 조정할 수 있습니다.

8.  이제 PySpark DataFrame을 사용하여 열을 추가하고 일부 기존 열의 값을 업데이트하여 데이터 유효성 검사 및 정리를 위한 열을 추가합니다. `+` 버튼을 사용하여 새 코드 블록을 추가하고 다음 코드를 셀에 추가합니다.

    ```python
    # code
    from pyspark.sql.functions import when, lit, col, current_timestamp, input_file_name
        
    # IsFlagged, CreatedTS 및 ModifiedTS 열 추가
    df = df.withColumn("FileName", input_file_name()) \
        .withColumn("IsFlagged", when(col("OrderDate") < '2019-08-01',True).otherwise(False)) \
        .withColumn("CreatedTS", current_timestamp()).withColumn("ModifiedTS", current_timestamp())
        
    # CustomerName이 null이거나 비어 있으면 "Unknown"으로 업데이트
    df = df.withColumn("CustomerName", when((col("CustomerName").isNull() | (col("CustomerName")=="")),lit("Unknown")).otherwise(col("CustomerName")))
    ```

    코드의 첫 번째 줄은 PySpark에서 필요한 함수를 가져옵니다. 그런 다음 DataFrame에 새 열을 추가하여 원본 파일 이름, 주문이 해당 회계 연도 이전에 발생했는지 여부 플래그, 행이 생성 및 수정된 시점을 추적할 수 있도록 합니다.
    마지막으로 `CustomerName` 열이 null이거나 비어 있는 경우 "Unknown"으로 업데이트합니다.

9.  **▷ (Run cell)** 버튼을 사용하여 셀을 실행하여 코드를 실행합니다.

10. 다음으로 Delta Lake 형식을 사용하여 `sales` 데이터베이스의 `sales_silver` 테이블에 대한 스키마를 정의합니다. 새 코드 블록을 만들고 다음 코드를 셀에 추가합니다.

    ```python
    # code
    # sales_silver 테이블 스키마 정의
    # 참고: 아래 .tableName("sales.sales_silver") 에서 "sales"는 
    # Lakehouse 이름(예: "Sales")으로 변경해야 할 수 있습니다. 
    # 예: .tableName("Sales.sales_silver")
        
    from pyspark.sql.types import *
    from delta.tables import *
        
    DeltaTable.createIfNotExists(spark) \
        .tableName("sales.sales_silver") \ # 필요시 "Sales.sales_silver" 등으로 수정
        .addColumn("SalesOrderNumber", StringType()) \
        .addColumn("SalesOrderLineNumber", IntegerType()) \
        .addColumn("OrderDate", DateType()) \
        .addColumn("CustomerName", StringType()) \
        .addColumn("Email", StringType()) \
        .addColumn("Item", StringType()) \
        .addColumn("Quantity", IntegerType()) \
        .addColumn("UnitPrice", FloatType()) \
        .addColumn("Tax", FloatType()) \
        .addColumn("FileName", StringType()) \
        .addColumn("IsFlagged", BooleanType()) \
        .addColumn("CreatedTS", DateType()) \
        .addColumn("ModifiedTS", DateType()) \
        .execute()
    ```

11. **▷ (Run cell)** 버튼을 사용하여 셀을 실행하여 코드를 실행합니다.

12. Lakehouse 탐색기 창의 **Tables** 섹션에서 `...`를 선택하고 **Refresh**를 선택합니다. 이제 새 `sales_silver` 테이블이 나열되어야 합니다. ▲ (삼각형 아이콘)는 Delta 테이블임을 나타냅니다.

**참고:** 새 테이블이 표시되지 않으면 몇 초간 기다렸다가 **Refresh**를 다시 선택하거나 전체 브라우저 탭을 새로 고칩니다.

13. 이제 Delta 테이블에서 **upsert** 작업을 수행하여 특정 조건에 따라 기존 레코드를 업데이트하고 일치하는 항목이 없을 때 새 레코드를 삽입합니다. 새 코드 블록을 추가하고 다음 코드를 붙여넣습니다.

    ```python
    # code
    # SalesOrderNumber, OrderDate, CustomerName 및 Item 열로 정의된 조건에 따라
    # 기존 레코드를 업데이트하고 새 레코드를 삽입합니다.
    
    from delta.tables import *
        
    deltaTable = DeltaTable.forPath(spark, 'Tables/sales_silver')
        
    dfUpdates = df
        
    deltaTable.alias('silver') \
      .merge(
        dfUpdates.alias('updates'),
        # 병합 조건: silver 테이블과 updates DataFrame 간의 SalesOrderNumber, OrderDate, CustomerName, Item 이 모두 일치하는 경우
        'silver.SalesOrderNumber = updates.SalesOrderNumber and silver.OrderDate = updates.OrderDate and silver.CustomerName = updates.CustomerName and silver.Item = updates.Item'
      ) \
       .whenMatchedUpdate(set =
        {
            # 조건이 일치했을 때 업데이트할 내용 정의 (현재는 비어 있음)
            # 필요하다면 여기에 업데이트 로직을 추가할 수 있습니다.
            # 예: "ModifiedTS": "updates.ModifiedTS"  
        }
      ) \
     .whenNotMatchedInsert(values =
        {
          # 조건이 일치하지 않았을 때 (새 레코드일 때) 삽입할 값 정의
          "SalesOrderNumber": "updates.SalesOrderNumber",
          "SalesOrderLineNumber": "updates.SalesOrderLineNumber",
          "OrderDate": "updates.OrderDate",
          "CustomerName": "updates.CustomerName",
          "Email": "updates.Email",
          "Item": "updates.Item",
          "Quantity": "updates.Quantity",
          "UnitPrice": "updates.UnitPrice",
          "Tax": "updates.Tax",
          "FileName": "updates.FileName",
          "IsFlagged": "updates.IsFlagged",
          "CreatedTS": "updates.CreatedTS",
          "ModifiedTS": "updates.ModifiedTS"
        }
      ) \
      .execute()
    ```

14. **▷ (Run cell)** 버튼을 사용하여 셀을 실행하여 코드를 실행합니다.

    이 작업은 특정 열의 값을 기반으로 테이블의 기존 레코드를 업데이트하고 일치하는 항목이 없을 때 새 레코드를 삽입할 수 있으므로 중요합니다. 이는 기존 레코드 및 새 레코드에 대한 업데이트를 포함할 수 있는 원본 시스템에서 데이터를 로드할 때 일반적인 요구 사항입니다.

    이제 추가 변환 및 모델링 준비가 된 Silver Delta 테이블에 데이터가 있습니다.

**SQL 엔드포인트를 사용하여 Silver 계층의 데이터 탐색**

이제 Silver 계층에 데이터가 있으므로 **SQL analytics endpoint**를 사용하여 데이터를 탐색하고 일부 기본 분석을 수행할 수 있습니다. 이는 SQL에 익숙하고 데이터의 기본 탐색을 수행하려는 경우 유용합니다. 이 실습에서는 Fabric의 SQL 엔드포인트 보기를 사용하지만 SQL Server Management Studio (SSMS) 및 Azure Data Explorer와 같은 다른 도구를 사용할 수도 있습니다.

1.  Workspace로 다시 이동하면 이제 여러 항목이 나열되어 있음을 알 수 있습니다. **Sales SQL analytics endpoint**를 선택하여 SQL 분석 엔드포인트 보기에서 Lakehouse를 엽니다.

    *Lakehouse의 SQL 엔드포인트 스크린샷.*

2.  리본에서 **New SQL query**를 선택하면 SQL 쿼리 편집기가 열립니다. Lakehouse 탐색기 창의 기존 쿼리 이름 옆에 있는 `...` 메뉴 항목을 사용하여 쿼리 이름을 변경할 수 있습니다.
3.  다음으로 두 개의 SQL 쿼리를 실행하여 데이터를 탐색합니다.
4.  다음 쿼리를 쿼리 편집기에 붙여넣고 **Run**을 선택합니다.

    ```sql
    -- sql
    -- 참고: 아래 FROM sales_silver 에서 "sales"는 
    -- Lakehouse 이름(예: "Sales")으로 변경해야 할 수 있습니다. 
    -- 예: FROM Sales.sales_silver
    SELECT YEAR(OrderDate) AS Year
        , CAST (SUM(Quantity * (UnitPrice + Tax)) AS DECIMAL(12, 2)) AS TotalSales
    FROM sales_silver -- 필요시 "Sales.sales_silver" 등으로 수정
    GROUP BY YEAR(OrderDate) 
    ORDER BY YEAR(OrderDate)
    ```

    이 쿼리는 `sales_silver` 테이블의 각 연도별 총 매출을 계산합니다. 결과는 다음과 같아야 합니다.

    *Lakehouse에서 SQL 쿼리 결과 스크린샷.*

5.  다음으로 어떤 고객이 가장 많이 구매하는지(수량 기준) 검토합니다. 다음 쿼리를 쿼리 편집기에 붙여넣고 **Run**을 선택합니다.

    ```sql
    -- sql
    -- 참고: 아래 FROM sales_silver 에서 "sales"는 
    -- Lakehouse 이름(예: "Sales")으로 변경해야 할 수 있습니다. 
    -- 예: FROM Sales.sales_silver
    SELECT TOP 10 CustomerName, SUM(Quantity) AS TotalQuantity
    FROM sales_silver -- 필요시 "Sales.sales_silver" 등으로 수정
    GROUP BY CustomerName
    ORDER BY TotalQuantity DESC
    ```

    이 쿼리는 `sales_silver` 테이블에서 각 고객이 구매한 총 품목 수량을 계산한 다음 수량 기준으로 상위 10명의 고객을 반환합니다.

    Silver 계층에서의 데이터 탐색은 기본 분석에 유용하지만, 더 고급 분석 및 보고를 가능하게 하려면 데이터를 추가로 변환하고 스타 스키마로 모델링해야 합니다. 다음 섹션에서 이를 수행합니다.

**Gold 계층용 데이터 변환**

Bronze 계층에서 데이터를 가져와 변환하고 Silver Delta 테이블에 성공적으로 로드했습니다. 이제 새 Notebook을 사용하여 데이터를 추가로 변환하고, 스타 스키마로 모델링하고, Gold Delta 테이블에 로드합니다.

이 모든 작업을 단일 Notebook에서 수행할 수도 있지만, 이 실습에서는 Bronze에서 Silver로, 그런 다음 Silver에서 Gold로 데이터를 변환하는 프로세스를 보여주기 위해 별도의 Notebook을 사용합니다. 이는 디버깅, 문제 해결 및 재사용에 도움이 될 수 있습니다.

1.  Workspace 홈페이지로 돌아가서 **Transform data for Gold**라는 새 Notebook을 만듭니다.
2.  Lakehouse 탐색기 창에서 **Add**를 선택한 다음 이전에 만든 **Sales** Lakehouse를 선택하여 Sales Lakehouse를 추가합니다. **Add Lakehouse** 창에서 **Existing Lakehouse without Schema**를 선택합니다. 탐색기 창의 **Tables** 섹션에 `sales_silver` 테이블이 나열되어야 합니다.
3.  기존 코드 블록에서 주석 처리된 텍스트를 제거하고 다음 코드를 추가하여 DataFrame에 데이터를 로드하고 스타 스키마 빌드를 시작한 다음 실행합니다.

    ```python
    # code
    # Gold 계층 생성을 위한 시작점으로 데이터 프레임에 데이터 로드
    # 참고: 아래 "Sales.sales_silver" 에서 "Sales"는 
    # 실제 Lakehouse 이름인지 확인하세요.
    df = spark.read.table("Sales.sales_silver") # 필요시 Lakehouse 이름 확인/수정
    ```

4.  새 코드 블록을 추가하고 다음 코드를 붙여넣어 날짜 차원 테이블을 생성하고 실행합니다.

    ```python
    # code
    from pyspark.sql.types import *
    from delta.tables import*
        
    # dimdate_gold 테이블 스키마 정의
    # 참고: 아래 .tableName("sales.dimdate_gold") 에서 "sales"는 
    # Lakehouse 이름(예: "Sales")으로 변경해야 할 수 있습니다.
    # 예: .tableName("Sales.dimdate_gold")
    DeltaTable.createIfNotExists(spark) \
        .tableName("sales.dimdate_gold") \ # 필요시 "Sales.dimdate_gold" 등으로 수정
        .addColumn("OrderDate", DateType()) \
        .addColumn("Day", IntegerType()) \
        .addColumn("Month", IntegerType()) \
        .addColumn("Year", IntegerType()) \
        .addColumn("mmmyyyy", StringType()) \
        .addColumn("yyyymm", StringType()) \
        .execute()
    ```

**참고:** 언제든지 `display(df)` 명령을 실행하여 작업 진행 상황을 확인할 수 있습니다. 이 경우 `display(dfdimDate_gold)`를 실행하여 `dimDate_gold` DataFrame의 내용을 확인합니다.

5.  새 코드 블록에서 다음 코드를 추가하고 실행하여 날짜 차원 `dimdate_gold`용 DataFrame을 만듭니다.

    ```python
    # code
    from pyspark.sql.functions import col, dayofmonth, month, year, date_format
        
    # dimDate_gold용 데이터 프레임 생성
        
    dfdimDate_gold = df.dropDuplicates(["OrderDate"]).select(col("OrderDate"), \
            dayofmonth("OrderDate").alias("Day"), \
            month("OrderDate").alias("Month"), \
            year("OrderDate").alias("Year"), \
            date_format(col("OrderDate"), "MMM-yyyy").alias("mmmyyyy"), \
            date_format(col("OrderDate"), "yyyyMM").alias("yyyymm"), \
        ).orderBy("OrderDate")

    # 데이터 프레임의 처음 10개 행을 표시하여 데이터 미리 보기

    display(dfdimDate_gold.head(10))
    ```

6.  데이터를 변환하면서 Notebook에서 어떤 일이 발생하는지 이해하고 지켜볼 수 있도록 코드를 새 코드 블록으로 분리하고 있습니다. 다른 새 코드 블록에서 다음 코드를 추가하고 실행하여 새 데이터가 들어올 때 날짜 차원을 업데이트합니다.

    ```python
    # code
    from delta.tables import *
        
    deltaTable = DeltaTable.forPath(spark, 'Tables/dimdate_gold')
        
    dfUpdates = dfdimDate_gold
        
    deltaTable.alias('gold') \
      .merge(
        dfUpdates.alias('updates'),
        'gold.OrderDate = updates.OrderDate'
      ) \
        .whenMatchedUpdate(set =
        {
            # 이 실습에서는 업데이트할 내용 없음
        }
      ) \
      .whenNotMatchedInsert(values =
        {
          "OrderDate": "updates.OrderDate",
          "Day": "updates.Day",
          "Month": "updates.Month",
          "Year": "updates.Year",
          "mmmyyyy": "updates.mmmyyyy",
          "yyyymm": "updates.yyyymm"
        }
      ) \
      .execute()
    ```

    이제 날짜 차원이 설정되었습니다. 이제 고객 차원을 만듭니다.

7.  고객 차원 테이블을 구축하려면 새 코드 블록을 추가하고 다음 코드를 붙여넣고 실행합니다.

    ```python
    # code
    from pyspark.sql.types import *
    from delta.tables import *
        
    # customer_gold 차원 델타 테이블 생성
    # 참고: 아래 .tableName("sales.dimcustomer_gold") 에서 "sales"는 
    # Lakehouse 이름(예: "Sales")으로 변경해야 할 수 있습니다.
    # 예: .tableName("Sales.dimcustomer_gold")
    DeltaTable.createIfNotExists(spark) \
        .tableName("sales.dimcustomer_gold") \ # 필요시 "Sales.dimcustomer_gold" 등으로 수정
        .addColumn("CustomerName", StringType()) \
        .addColumn("Email",  StringType()) \
        .addColumn("First", StringType()) \
        .addColumn("Last", StringType()) \
        .addColumn("CustomerID", LongType()) \
        .execute()
    ```

8.  새 코드 블록에서 다음 코드를 추가하고 실행하여 중복 고객을 제거하고, 특정 열을 선택하고, "CustomerName" 열을 분할하여 "First" 및 "Last" 이름 열을 만듭니다.

    ```python
    # code
    from pyspark.sql.functions import col, split
        
    # customer_silver 데이터 프레임 생성 (Gold 계층을 위한 준비 단계)
        
    dfdimCustomer_silver = df.dropDuplicates(["CustomerName","Email"]).select(col("CustomerName"),col("Email")) \
        .withColumn("First",split(col("CustomerName"), " ").getItem(0)) \
        .withColumn("Last",split(col("CustomerName"), " ").getItem(1)) 
        
    # 데이터 프레임의 처음 10개 행을 표시하여 데이터 미리 보기

    display(dfdimCustomer_silver.head(10))
    ```

    여기서는 중복 제거, 특정 열 선택, "CustomerName" 열을 분할하여 "First" 및 "Last" 이름 열 생성과 같은 다양한 변환을 수행하여 새 DataFrame `dfdimCustomer_silver`를 만들었습니다. 결과는 "CustomerName" 열에서 추출한 별도의 "First" 및 "Last" 이름 열을 포함하여 정리되고 구조화된 고객 데이터가 있는 DataFrame입니다.

9.  다음으로 고객의 ID 열을 만듭니다. 새 코드 블록에 다음을 붙여넣고 실행합니다.

    ```python
    # code
    from pyspark.sql.functions import monotonically_increasing_id, col, when, coalesce, max, lit
        
    # 기존 Gold 테이블에서 현재 최대 CustomerID 가져오기
    # 참고: 아래 "Sales.dimCustomer_gold" 에서 "Sales"는 
    # 실제 Lakehouse 이름인지 확인하세요.
    dfdimCustomer_temp = spark.read.table("Sales.dimCustomer_gold") # 필요시 Lakehouse 이름 확인/수정
        
    # MAXCustomerID가 없으면 (테이블이 비어 있으면) 0으로 시작
    MAXCustomerID = dfdimCustomer_temp.select(coalesce(max(col("CustomerID")),lit(0)).alias("MAXCustomerID")).first()[0]
        
    # Silver 데이터 중 Gold 테이블에 아직 없는 새로운 고객만 필터링 (left_anti join)
    dfdimCustomer_gold = dfdimCustomer_silver.join(dfdimCustomer_temp,(dfdimCustomer_silver.CustomerName == dfdimCustomer_temp.CustomerName) & (dfdimCustomer_silver.Email == dfdimCustomer_temp.Email), "left_anti")
        
    # 새로운 고객에게 순차적으로 증가하는 ID 할당 (기존 MAX ID + 1 부터 시작)
    dfdimCustomer_gold = dfdimCustomer_gold.withColumn("CustomerID",monotonically_increasing_id() + MAXCustomerID + 1)

    # 데이터 프레임의 처음 10개 행을 표시하여 데이터 미리 보기

    display(dfdimCustomer_gold.head(10))
    ```

    여기서는 `left anti join`을 수행하여 `dimCustomer_gold` 테이블에 이미 존재하는 중복 항목을 제외하여 고객 데이터(`dfdimCustomer_silver`)를 정리 및 변환한 다음 `monotonically_increasing_id()` 함수를 사용하여 고유한 `CustomerID` 값을 생성합니다.

10. 이제 새 데이터가 들어올 때 고객 테이블이 최신 상태로 유지되도록 합니다. 새 코드 블록에 다음을 붙여넣고 실행합니다.

    ```python
    # code
    from delta.tables import *

    deltaTable = DeltaTable.forPath(spark, 'Tables/dimcustomer_gold')
        
    dfUpdates = dfdimCustomer_gold
        
    deltaTable.alias('gold') \
      .merge(
        dfUpdates.alias('updates'),
        'gold.CustomerName = updates.CustomerName AND gold.Email = updates.Email'
      ) \
       .whenMatchedUpdate(set =
        {
            # 이 실습에서는 업데이트할 내용 없음
        }
      ) \
     .whenNotMatchedInsert(values =
        {
          "CustomerName": "updates.CustomerName",
          "Email": "updates.Email",
          "First": "updates.First",
          "Last": "updates.Last",
          "CustomerID": "updates.CustomerID"
        }
      ) \
      .execute()
    ```

11. 이제 제품 차원을 만들기 위해 해당 단계를 반복합니다. 새 코드 블록에 다음을 붙여넣고 실행합니다.

    ```python
    # code
    from pyspark.sql.types import *
    from delta.tables import *
        
    # dimproduct_gold 차원 델타 테이블 생성
    # 참고: 아래 .tableName("sales.dimproduct_gold") 에서 "sales"는 
    # Lakehouse 이름(예: "Sales")으로 변경해야 할 수 있습니다.
    # 예: .tableName("Sales.dimproduct_gold")
    DeltaTable.createIfNotExists(spark) \
        .tableName("sales.dimproduct_gold") \ # 필요시 "Sales.dimproduct_gold" 등으로 수정
        .addColumn("ItemName", StringType()) \
        .addColumn("ItemID", LongType()) \
        .addColumn("ItemInfo", StringType()) \
        .execute()
    ```

12. `product_silver` DataFrame을 만들기 위해 다른 코드 블록을 추가합니다.

    ```python
    # code
    from pyspark.sql.functions import col, split, lit, when
        
    # product_silver 데이터 프레임 생성 (Gold 계층을 위한 준비 단계)
        
    dfdimProduct_silver = df.dropDuplicates(["Item"]).select(col("Item")) \
        .withColumn("ItemName",split(col("Item"), ", ").getItem(0)) \
        .withColumn("ItemInfo",when((split(col("Item"), ", ").getItem(1).isNull() | (split(col("Item"), ", ").getItem(1)=="")),lit("")).otherwise(split(col("Item"), ", ").getItem(1))) 
        
    # 데이터 프레임의 처음 10개 행을 표시하여 데이터 미리 보기

    display(dfdimProduct_silver.head(10))
    ```

13. 이제 `dimProduct_gold` 테이블의 ID를 만듭니다. 다음 구문을 새 코드 블록에 추가하고 실행합니다.

    ```python
    # code
    from pyspark.sql.functions import monotonically_increasing_id, col, lit, max, coalesce
        
    # 기존 Gold 테이블에서 현재 최대 ItemID 가져오기
    # dfdimProduct_temp = dfdimProduct_silver # 이 줄은 주석 처리하거나 삭제해도 됩니다.
    # 참고: 아래 "Sales.dimProduct_gold" 에서 "Sales"는 
    # 실제 Lakehouse 이름인지 확인하세요.
    dfdimProduct_temp = spark.read.table("Sales.dimProduct_gold") # 필요시 Lakehouse 이름 확인/수정
        
    # MAXProductID가 없으면 0으로 시작
    MAXProductID = dfdimProduct_temp.select(coalesce(max(col("ItemID")),lit(0)).alias("MAXItemID")).first()[0]
        
    # Silver 데이터 중 Gold 테이블에 아직 없는 새로운 제품만 필터링
    dfdimProduct_gold = dfdimProduct_silver.join(dfdimProduct_temp,(dfdimProduct_silver.ItemName == dfdimProduct_temp.ItemName) & (dfdimProduct_silver.ItemInfo == dfdimProduct_temp.ItemInfo), "left_anti")
        
    # 새로운 제품에게 순차적으로 증가하는 ID 할당
    dfdimProduct_gold = dfdimProduct_gold.withColumn("ItemID",monotonically_increasing_id() + MAXProductID + 1)
        
    # 데이터 프레임의 처음 10개 행을 표시하여 데이터 미리 보기

    display(dfdimProduct_gold.head(10))
    ```

    이 코드는 테이블의 현재 데이터를 기반으로 사용 가능한 다음 제품 ID를 계산하고, 이 새 ID를 제품에 할당한 다음, 업데이트된 제품 정보를 표시합니다.

14. 다른 차원에서 수행한 것과 유사하게, 새 데이터가 들어올 때 제품 테이블이 최신 상태로 유지되도록 해야 합니다. 새 코드 블록에 다음을 붙여넣고 실행합니다.

    ```python
    # code
    from delta.tables import *
        
    deltaTable = DeltaTable.forPath(spark, 'Tables/dimproduct_gold')
                
    dfUpdates = dfdimProduct_gold
                
    deltaTable.alias('gold') \
      .merge(
            dfUpdates.alias('updates'),
            'gold.ItemName = updates.ItemName AND gold.ItemInfo = updates.ItemInfo'
            ) \
            .whenMatchedUpdate(set =
            {
                  # 이 실습에서는 업데이트할 내용 없음
            }
            ) \
            .whenNotMatchedInsert(values =
             {
              "ItemName": "updates.ItemName",
              "ItemInfo": "updates.ItemInfo",
              "ItemID": "updates.ItemID"
              }
              ) \
              .execute()
    ```

    이제 차원이 구축되었으므로 마지막 단계는 팩트 테이블을 만드는 것입니다.

15. 새 코드 블록에 다음 코드를 붙여넣고 실행하여 팩트 테이블을 만듭니다.

    ```python
    # code
    from pyspark.sql.types import *
    from delta.tables import *
        
    # factsales_gold 팩트 델타 테이블 생성
    # 참고: 아래 .tableName("sales.factsales_gold") 에서 "sales"는 
    # Lakehouse 이름(예: "Sales")으로 변경해야 할 수 있습니다.
    # 예: .tableName("Sales.factsales_gold")
    DeltaTable.createIfNotExists(spark) \
        .tableName("sales.factsales_gold") \ # 필요시 "Sales.factsales_gold" 등으로 수정
        .addColumn("CustomerID", LongType()) \
        .addColumn("ItemID", LongType()) \
        .addColumn("OrderDate", DateType()) \
        .addColumn("Quantity", IntegerType()) \
        .addColumn("UnitPrice", FloatType()) \
        .addColumn("Tax", FloatType()) \
        .execute()
    ```

16. 새 코드 블록에 다음 코드를 붙여넣고 실행하여 판매 데이터를 고객 및 제품 정보(고객 ID, 품목 ID, 주문 날짜, 수량, 단가 및 세금 포함)와 결합하는 새 DataFrame을 만듭니다.

    ```python
    # code
    from pyspark.sql.functions import col, split, lit, when # split, lit, when 추가
        
    # 조인을 위해 Gold 차원 테이블 로드
    # 참고: 아래 테이블 이름에서 "Sales"는 실제 Lakehouse 이름인지 확인하세요.
    dfdimCustomer_temp = spark.read.table("Sales.dimCustomer_gold") # 필요시 Lakehouse 이름 확인/수정
    dfdimProduct_temp = spark.read.table("Sales.dimProduct_gold") # 필요시 Lakehouse 이름 확인/수정
        
    # 원본 DataFrame (df - Silver 계층 데이터)에 조인 키로 사용할 ItemName, ItemInfo 추가
    # (이전 단계에서 df가 업데이트 되었을 수 있으므로 다시 정의할 필요가 있을 수 있습니다.
    #  만약 df가 이전에 사용한 Silver 데이터 로드 결과 그대로라면 이 부분을 유지합니다.)
    df = df.withColumn("ItemName",split(col("Item"), ", ").getItem(0)) \
        .withColumn("ItemInfo",when((split(col("Item"), ", ").getItem(1).isNull() | (split(col("Item"), ", ").getItem(1)=="")),lit("")).otherwise(split(col("Item"), ", ").getItem(1))) \
        
        
    # Sales_gold 데이터 프레임 생성 (팩트 테이블 데이터)
    # 원본 데이터(df)를 고객 차원, 제품 차원과 조인
    dffactSales_gold = df.alias("df1").join(dfdimCustomer_temp.alias("df2"),(df.CustomerName == dfdimCustomer_temp.CustomerName) & (df.Email == dfdimCustomer_temp.Email), "left") \
            .join(dfdimProduct_temp.alias("df3"),(df.ItemName == dfdimProduct_temp.ItemName) & (df.ItemInfo == dfdimProduct_temp.ItemInfo), "left") \
        .select(col("df2.CustomerID") \
            , col("df3.ItemID") \
            , col("df1.OrderDate") \
            , col("df1.Quantity") \
            , col("df1.UnitPrice") \
            , col("df1.Tax") \
        ).orderBy(col("df1.OrderDate"), col("df2.CustomerID"), col("df3.ItemID"))
        
    # 데이터 프레임의 처음 10개 행을 표시하여 데이터 미리 보기
        
    display(dffactSales_gold.head(10))
    ```

17. 이제 새 코드 블록에서 다음 코드를 실행하여 판매 데이터가 최신 상태로 유지되도록 합니다.

    ```python
    # code
    from delta.tables import *
        
    deltaTable = DeltaTable.forPath(spark, 'Tables/factsales_gold')
        
    dfUpdates = dffactSales_gold
        
    deltaTable.alias('gold') \
      .merge(
        dfUpdates.alias('updates'),
        # 복합 키를 사용하여 병합 조건 정의
        'gold.OrderDate = updates.OrderDate AND gold.CustomerID = updates.CustomerID AND gold.ItemID = updates.ItemID'
      ) \
       .whenMatchedUpdate(set =
        {
            # 일반적으로 팩트 테이블은 업데이트보다 삽입 위주이지만, 필요시 여기에 업데이트 로직 추가 가능
            # 예: "Quantity": "updates.Quantity" 
        }
      ) \
     .whenNotMatchedInsert(values =
        {
          "CustomerID": "updates.CustomerID",
          "ItemID": "updates.ItemID",
          "OrderDate": "updates.OrderDate",
          "Quantity": "updates.Quantity",
          "UnitPrice": "updates.UnitPrice",
          "Tax": "updates.Tax"
        }
      ) \
      .execute()
    ```

    여기서는 Delta Lake의 `merge` 작업을 사용하여 `factsales_gold` 테이블을 새 판매 데이터(`dffactSales_gold`)와 동기화하고 업데이트합니다. 이 작업은 기존 데이터와 새 데이터 간의 주문 날짜, 고객 ID 및 품목 ID를 비교하여 일치하는 레코드를 업데이트하고 필요에 따라 새 레코드를 삽입합니다.

    이제 보고 및 분석에 사용할 수 있는 큐레이션되고 모델링된 Gold 계층이 준비되었습니다.

**Semantic Model 생성**

Workspace에서 이제 Gold 계층을 사용하여 보고서를 만들고 데이터를 분석할 수 있습니다. Workspace에서 직접 **Semantic Model**에 액세스하여 보고를 위한 관계 및 측정값을 생성할 수 있습니다.

**참고:** Lakehouse를 만들 때 자동으로 생성되는 기본 Semantic Model은 사용할 수 없습니다. 이 실습에서 만든 Gold 테이블을 포함하는 새 Semantic Model을 Lakehouse 탐색기에서 만들어야 합니다.

1.  Workspace에서 **Sales** Lakehouse로 이동합니다.
2.  Lakehouse 탐색기 보기의 리본에서 **New semantic model**을 선택합니다.
3.  새 Semantic Model에 이름 **Sales_Gold**를 할당합니다.
4.  Semantic Model에 포함할 변환된 Gold 테이블을 선택하고 **Confirm**을 선택합니다.
    *   `dimdate_gold`
    *   `dimcustomer_gold`
    *   `dimproduct_gold`
    *   `factsales_gold`
5.  그러면 Fabric에서 Semantic Model이 열리고, 여기서 다음과 같이 관계 및 측정값을 생성할 수 있습니다.

    *Fabric의 Semantic Model 스크린샷.*

여기서부터 귀하 또는 데이터 팀의 다른 구성원은 Lakehouse의 데이터를 기반으로 보고서 및 대시보드를 만들 수 있습니다. 이러한 보고서는 Lakehouse의 Gold 계층에 직접 연결되므로 항상 최신 데이터를 반영합니다.

**리소스 정리**

이 실습에서는 Microsoft Fabric Lakehouse에서 Medallion 아키텍처를 만드는 방법을 배웠습니다.

Lakehouse 탐색을 마쳤으면 이 실습을 위해 만든 Workspace를 삭제할 수 있습니다.

1.  왼쪽 막대에서 Workspace 아이콘을 선택하여 포함된 모든 항목을 봅니다.
2.  도구 모음의 `...` 메뉴에서 **Workspace settings**를 선택합니다.
3.  **General** 섹션에서 **Remove this workspace**를 선택합니다.

---
