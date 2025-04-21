
## **Data warehouse에서 데이터 분석하기**

Microsoft Fabric에서 Data warehouse는 대규모 분석을 위한 관계형 데이터베이스를 제공합니다. Lakehouse에 정의된 테이블에 대한 기본 읽기 전용 SQL endpoint와 달리, Data warehouse는 테이블의 데이터 삽입, 업데이트, 삭제 기능을 포함한 완전한 SQL 시맨틱(semantics)을 제공합니다.

이 랩은 완료하는 데 약 30분이 소요됩니다.

**참고:** 이 실습을 완료하려면 Microsoft Fabric 평가판이 필요합니다.

**Workspace 생성**

Fabric에서 데이터 작업을 시작하기 전에 Fabric 평가판이 활성화된 Workspace를 만듭니다.

1.  브라우저에서 Microsoft Fabric 홈페이지 (`https://app.fabric.microsoft.com/home?experience=fabric`)로 이동하고 Fabric 자격 증명으로 로그인합니다.
2.  왼쪽 메뉴 모음에서 **Workspaces** (아이콘 모양 🗇)를 선택합니다.
3.  Fabric 용량(Trial, Premium 또는 Fabric)을 포함하는 라이선싱 모드를 선택하여 원하는 이름으로 새 Workspace를 만듭니다.
4.  새 Workspace가 열리면 비어 있어야 합니다.

    *Fabric의 빈 Workspace 스크린샷.*

**Data warehouse 생성**

이제 Workspace가 있으므로 Data warehouse를 만들 차례입니다. Synapse Data Warehouse 홈페이지에는 새 warehouse를 만드는 바로 가기가 포함되어 있습니다.

1.  왼쪽 메뉴 모음에서 **Create**를 선택합니다. **New** 페이지의 **Data Warehouse** 섹션에서 **Warehouse**를 선택합니다. 원하는 고유한 이름을 지정합니다.

**참고:** **Create** 옵션이 사이드바에 고정되어 있지 않으면 먼저 줄임표(…) 옵션을 선택해야 합니다.

2.  약 1분 후 새 warehouse가 생성됩니다.

    *새 warehouse 스크린샷.*

**테이블 생성 및 데이터 삽입**

Warehouse는 테이블 및 기타 객체를 정의할 수 있는 관계형 데이터베이스입니다.

1.  새 warehouse에서 **T-SQL** 타일을 선택하고 다음 `CREATE TABLE` 문을 사용합니다.

    ```sql
    -- sql
    CREATE TABLE dbo.DimProduct
    (
        ProductKey INTEGER NOT NULL,
        ProductAltKey VARCHAR(25) NULL,
        ProductName VARCHAR(50) NOT NULL,
        Category VARCHAR(50) NULL,
        ListPrice DECIMAL(5,2) NULL
    );
    GO
    ```

2.  ▷ **Run** 버튼을 사용하여 SQL 스크립트를 실행합니다. 이 스크립트는 data warehouse의 `dbo` 스키마에 `DimProduct`라는 새 테이블을 만듭니다.
3.  도구 모음의 **Refresh** 버튼을 사용하여 보기를 새로 고칩니다. 그런 다음 **Explorer** 창에서 **Schemas** > **dbo** > **Tables**를 확장하고 `DimProduct` 테이블이 생성되었는지 확인합니다.
4.  **Home** 메뉴 탭에서 **New SQL Query** 버튼을 사용하여 새 쿼리를 만들고 다음 `INSERT` 문을 입력합니다.

    ```sql
    -- sql
    INSERT INTO dbo.DimProduct
    VALUES
    (1, 'RING1', 'Bicycle bell', 'Accessories', 5.99),
    (2, 'BRITE1', 'Front light', 'Accessories', 15.49),
    (3, 'BRITE2', 'Rear light', 'Accessories', 15.49);
    GO
    ```

5.  새 쿼리를 실행하여 `DimProduct` 테이블에 세 개의 행을 삽입합니다.
6.  쿼리가 완료되면 **Explorer** 창에서 `DimProduct` 테이블을 선택하고 세 개의 행이 테이블에 추가되었는지 확인합니다.
7.  **Home** 메뉴 탭에서 **New SQL Query** 버튼을 사용하여 새 쿼리를 만듭니다. 그런 다음 `https://raw.githubusercontent.com/MicrosoftLearning/dp-data/main/create-dw.txt` 에서 Transact-SQL 코드를 복사하여 새 쿼리 창에 붙여넣습니다.
8.  쿼리를 실행합니다. 이 쿼리는 간단한 데이터 웨어하우스 스키마를 만들고 일부 데이터를 로드합니다. 스크립트 실행에는 약 30초가 소요됩니다.
9.  도구 모음의 **Refresh** 버튼을 사용하여 보기를 새로 고칩니다. 그런 다음 **Explorer** 창에서 data warehouse의 `dbo` 스키마에 이제 다음 네 개의 테이블이 포함되어 있는지 확인합니다.
    *   `DimCustomer`
    *   `DimDate`
    *   `DimProduct`
    *   `FactSalesOrder`

**팁:** 스키마 로드에 시간이 걸리면 브라우저 페이지를 새로 고치세요.

**데이터 모델 정의**

관계형 데이터 웨어하우스는 일반적으로 팩트(fact) 테이블과 차원(dimension) 테이블로 구성됩니다. 팩트 테이블에는 비즈니스 성과 분석을 위해 집계할 수 있는 숫자 측정값(예: 판매 수익)이 포함되고, 차원 테이블에는 데이터를 집계할 수 있는 엔터티의 속성(예: 제품, 고객 또는 시간)이 포함됩니다. Microsoft Fabric data warehouse에서는 이러한 키를 사용하여 테이블 간의 관계를 캡슐화하는 데이터 모델을 정의할 수 있습니다.

1.  도구 모음에서 **Model** 레이아웃 버튼을 선택합니다.
2.  모델 창에서 data warehouse의 테이블을 재정렬하여 `FactSalesOrder` 테이블이 다음과 같이 가운데에 오도록 합니다.

    *데이터 웨어하우스 모델 페이지 스크린샷.*

**참고:** `frequently_run_queries`, `long_running_queries`, `exec_sessions_history`, `exec_requests_history` 뷰는 Fabric에서 자동으로 생성된 `queryinsights` 스키마의 일부입니다. 이는 SQL 분석 엔드포인트의 과거 쿼리 활동에 대한 전체적인 뷰를 제공하는 기능입니다. 이 기능은 이 실습의 범위를 벗어나므로 해당 뷰는 지금은 무시해야 합니다.

3.  `FactSalesOrder` 테이블에서 `ProductKey` 필드를 끌어 `DimProduct` 테이블의 `ProductKey` 필드에 놓습니다. 그런 다음 다음 관계 세부 정보를 확인합니다.
    *   **From table:** `FactSalesOrder`
    *   **Column:** `ProductKey`
    *   **To table:** `DimProduct`
    *   **Column:** `ProductKey`
    *   **Cardinality:** `Many to one (*:1)`
    *   **Cross filter direction:** `Single`
    *   **Make this relationship active:** 선택됨
    *   **Assume referential integrity:** 선택 해제됨
4.  프로세스를 반복하여 다음 테이블 간에 다대일(Many-to-one) 관계를 만듭니다.
    *   `FactSalesOrder.CustomerKey` → `DimCustomer.CustomerKey`
    *   `FactSalesOrder.SalesOrderDateKey` → `DimDate.DateKey`
5.  모든 관계가 정의되면 모델은 다음과 같아야 합니다.

    *관계가 있는 모델 스크린샷.*

**Data warehouse 테이블 쿼리**

Data warehouse는 관계형 데이터베이스이므로 SQL을 사용하여 테이블을 쿼리할 수 있습니다.

**팩트 및 차원 테이블 쿼리**
관계형 데이터 웨어하우스의 대부분 쿼리는 관련된 테이블 간(JOIN 절 사용) 데이터를 집계하고 그룹화하는 것(집계 함수 및 GROUP BY 절 사용)을 포함합니다.

1.  새 **SQL Query**를 만들고 다음 코드를 실행합니다.

    ```sql
    -- sql
    SELECT  d.[Year] AS CalendarYear,
             d.[Month] AS MonthOfYear,
             d.MonthName AS MonthName,
            SUM(so.SalesTotal) AS SalesRevenue
    FROM FactSalesOrder AS so
    JOIN DimDate AS d ON so.SalesOrderDateKey = d.DateKey
    GROUP BY d.[Year], d.[Month], d.MonthName
    ORDER BY CalendarYear, MonthOfYear;
    ```

    날짜 차원의 속성을 사용하면 팩트 테이블의 측정값을 여러 계층 수준(이 경우 연도 및 월)에서 집계할 수 있습니다. 이는 데이터 웨어하우스에서 일반적인 패턴입니다.

2.  집계에 두 번째 차원을 추가하도록 쿼리를 다음과 같이 수정합니다.

    ```sql
    -- sql
    SELECT  d.[Year] AS CalendarYear,
            d.[Month] AS MonthOfYear,
            d.MonthName AS MonthName,
            c.CountryRegion AS SalesRegion,
           SUM(so.SalesTotal) AS SalesRevenue
    FROM FactSalesOrder AS so
    JOIN DimDate AS d ON so.SalesOrderDateKey = d.DateKey
    JOIN DimCustomer AS c ON so.CustomerKey = c.CustomerKey
    GROUP BY d.[Year], d.[Month], d.MonthName, c.CountryRegion
    ORDER BY CalendarYear, MonthOfYear, SalesRegion;
    ```

3.  수정된 쿼리를 실행하고 결과를 검토합니다. 이제 결과에는 연도, 월 및 판매 지역별로 집계된 판매 수익이 포함됩니다.

**View 생성**
Microsoft Fabric의 Data warehouse는 관계형 데이터베이스에서 익숙한 기능 중 상당수를 갖추고 있습니다. 예를 들어, 뷰(View)나 저장 프로시저(Stored Procedure)와 같은 데이터베이스 객체를 만들어 SQL 로직을 캡슐화할 수 있습니다.

1.  이전에 만든 쿼리를 다음과 같이 수정하여 뷰를 만듭니다 (뷰를 만들려면 `ORDER BY` 절을 제거해야 함).

    ```sql
    -- sql
    CREATE VIEW vSalesByRegion
    AS
    SELECT  d.[Year] AS CalendarYear,
            d.[Month] AS MonthOfYear,
            d.MonthName AS MonthName,
            c.CountryRegion AS SalesRegion,
           SUM(so.SalesTotal) AS SalesRevenue
    FROM FactSalesOrder AS so
    JOIN DimDate AS d ON so.SalesOrderDateKey = d.DateKey
    JOIN DimCustomer AS c ON so.CustomerKey = c.CustomerKey
    GROUP BY d.[Year], d.[Month], d.MonthName, c.CountryRegion;
    ```

2.  쿼리를 실행하여 뷰를 만듭니다. 그런 다음 데이터 웨어하우스 스키마를 새로 고치고 새 뷰가 **Explorer** 창에 나열되는지 확인합니다.
3.  새 **SQL query**를 만들고 다음 `SELECT` 문을 실행합니다.

    ```sql
    -- code (SQL이지만 code 블록 사용 권장)
    SELECT CalendarYear, MonthName, SalesRegion, SalesRevenue
    FROM vSalesByRegion
    ORDER BY CalendarYear, MonthOfYear, SalesRegion;
    ```

**Visual query 생성**
SQL 코드를 작성하는 대신 그래픽 쿼리 디자이너를 사용하여 data warehouse의 테이블을 쿼리할 수 있습니다. 이 경험은 Power Query online과 유사하며, 코드를 사용하지 않고 데이터 변환 단계를 만들 수 있습니다. 더 복잡한 작업의 경우 Power Query의 M (Mashup) 언어를 사용할 수 있습니다.

1.  **Home** 메뉴에서 **New SQL query** 아래 옵션을 확장하고 **New visual query**를 선택합니다.
2.  `FactSalesOrder`를 캔버스로 끕니다. 테이블 미리 보기가 아래의 **Preview** 창에 표시됩니다.
3.  `DimProduct`를 캔버스로 끕니다. 이제 쿼리에 두 개의 테이블이 있습니다.
4.  캔버스의 `FactSalesOrder` 테이블에서 `(+)` 버튼을 사용하여 **Merge queries**를 수행합니다. *캔버스에서 FactSalesOrder 테이블이 선택된 스크린샷.*
5.  **Merge queries** 창에서 **DimProduct**를 병합할 오른쪽 테이블로 선택합니다. 두 쿼리 모두에서 `ProductKey`를 선택하고 기본 **Left outer** 조인 유형을 그대로 두고 **OK**를 클릭합니다.
6.  **Preview**에서 새 `DimProduct` 열이 `FactSalesOrder` 테이블에 추가된 것을 확인합니다. 열 이름 오른쪽의 화살표를 클릭하여 열을 확장합니다. `ProductName`을 선택하고 **OK**를 클릭합니다.

    *Preview 창에서 DimProduct 열이 확장되고 ProductName이 선택된 스크린샷.*

7.  관리자의 요청에 따라 단일 제품에 대한 데이터를 보고 싶다면 이제 `ProductName` 열을 사용하여 쿼리의 데이터를 필터링할 수 있습니다. `ProductName` 열을 필터링하여 **Cable Lock** 데이터만 확인합니다.
8.  여기서 **Visualize results** 또는 **Download Excel file**을 선택하여 이 단일 쿼리의 결과를 분석할 수 있습니다. 이제 관리자가 요청한 내용을 정확히 볼 수 있으므로 결과를 더 이상 분석할 필요가 없습니다.

**데이터 시각화**
단일 쿼리 또는 data warehouse의 데이터를 쉽게 시각화할 수 있습니다. 시각화하기 전에 보고서 디자이너에게 친숙하지 않은 열 및/또는 테이블을 숨깁니다.

1.  **Model** 레이아웃 버튼을 선택합니다.
2.  보고서 생성에 필요하지 않은 팩트 및 차원 테이블의 다음 열을 숨깁니다. 이는 모델에서 열을 제거하는 것이 아니라 보고서 캔버스 보기에서 숨기는 것입니다.
    *   **FactSalesOrder**
        *   `SalesOrderDateKey`
        *   `CustomerKey`
        *   `ProductKey`
    *   **DimCustomer**
        *   `CustomerKey`
        *   `CustomerAltKey`
    *   **DimDate**
        *   `DateKey`
        *   `DateAltKey`
    *   **DimProduct**
        *   `ProductKey`
        *   `ProductAltKey`
3.  이제 보고서를 만들고 이 데이터 세트를 다른 사람이 사용할 수 있도록 준비되었습니다. **Reporting** 메뉴에서 **New report**를 선택합니다. 그러면 Power BI 보고서를 만들 수 있는 새 창이 열립니다.
4.  **Data** 창에서 `FactSalesOrder`를 확장합니다. 숨긴 열이 더 이상 표시되지 않는 것을 확인합니다.
5.  `SalesTotal`을 선택합니다. 그러면 열이 보고서 캔버스에 추가됩니다. 열이 숫자 값이므로 기본 시각적 개체는 세로 막대형 차트입니다.
6.  캔버스의 세로 막대형 차트가 활성 상태인지 확인(회색 테두리와 핸들이 있음)한 다음 `DimProduct` 테이블에서 `Category`를 선택하여 세로 막대형 차트에 범주를 추가합니다.
7.  **Visualizations** 창에서 차트 유형을 세로 막대형 차트에서 **묶은 가로 막대형 차트(Clustered bar chart)**로 변경합니다. 그런 다음 범주를 읽을 수 있도록 필요에 따라 차트 크기를 조정합니다.

    *가로 막대형 차트가 선택된 Visualizations 창 스크린샷.*

8.  **Visualizations** 창에서 **Format your visual** 탭을 선택하고 **General** 하위 탭의 **Title** 섹션에서 **Text**를 **Total Sales by Category**로 변경합니다.
9.  **File** 메뉴에서 **Save**를 선택합니다. 그런 다음 이전에 만든 Workspace에 보고서를 **Sales Report**로 저장합니다.
10. 왼쪽의 메뉴 허브에서 Workspace로 다시 이동합니다. 이제 Workspace에 세 개의 항목(data warehouse, 기본 semantic model, 생성한 보고서)이 저장되어 있음을 확인합니다.

    *세 개의 항목이 나열된 Workspace 스크린샷.*

**리소스 정리**

이 실습에서는 여러 테이블을 포함하는 data warehouse를 만들었습니다. SQL을 사용하여 테이블에 데이터를 삽입하고 쿼리했으며, visual query 도구도 사용했습니다. 마지막으로 data warehouse의 기본 데이터 세트에 대한 데이터 모델을 향상시키고 이를 보고서의 원본으로 사용했습니다.

Data warehouse 탐색을 마쳤으면 이 실습을 위해 만든 Workspace를 삭제할 수 있습니다.

1.  왼쪽 막대에서 Workspace 아이콘을 선택하여 포함된 모든 항목을 봅니다.
2.  도구 모음의 `...` 메뉴에서 **Workspace settings**를 선택합니다.
3.  **General** 섹션에서 **Remove this workspace**를 선택합니다.

---
