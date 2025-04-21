
## **Lab 05: Microsoft Fabric에서 파이프라인으로 데이터 수집하기**

Data lakehouse는 클라우드 규모 분석 솔루션을 위한 일반적인 분석 데이터 저장소입니다. 데이터 엔지니어의 핵심 작업 중 하나는 여러 운영 데이터 원본에서 lakehouse로 데이터를 수집하는 것을 구현하고 관리하는 것입니다. Microsoft Fabric에서는 파이프라인 생성을 통해 데이터 수집을 위한 ETL(Extract, Transform, Load) 또는 ELT(Extract, Load, Transform) 솔루션을 구현할 수 있습니다.

Fabric은 또한 Apache Spark를 지원하므로 코드를 작성하고 실행하여 데이터를 대규모로 처리할 수 있습니다. Fabric의 파이프라인 및 Spark 기능을 결합하여 외부 원본에서 lakehouse의 기반이 되는 OneLake 스토리지로 데이터를 복사한 다음, Spark 코드를 사용하여 사용자 지정 데이터 변환을 수행하고 분석을 위해 테이블에 로드하는 복잡한 데이터 수집 로직을 구현할 수 있습니다.

이 랩은 완료하는 데 약 45분이 소요됩니다.

**참고:** 이 실습을 완료하려면 Microsoft Fabric 평가판이 필요합니다.

**Workspace 생성**

Fabric에서 데이터 작업을 시작하기 전에 Fabric 평가판이 활성화된 Workspace를 만듭니다.

1.  브라우저에서 Microsoft Fabric 홈페이지 (`https://app.fabric.microsoft.com/home?experience=fabric`)로 이동하고 Fabric 자격 증명으로 로그인합니다.
2.  왼쪽 메뉴 모음에서 **Workspaces** (아이콘 모양 🗇)를 선택합니다.
3.  Fabric 용량(Trial, Premium 또는 Fabric)을 포함하는 라이선싱 모드를 선택하여 원하는 이름으로 새 Workspace를 만듭니다.
4.  새 Workspace가 열리면 비어 있어야 합니다.

    *Fabric의 빈 Workspace 스크린샷.*

**Lakehouse 생성**

이제 Workspace가 있으므로 데이터를 수집할 데이터 Lakehouse를 만들 차례입니다.

1.  왼쪽 메뉴 모음에서 **Create**를 선택합니다. **New** 페이지의 **Data Engineering** 섹션에서 **Lakehouse**를 선택합니다. 원하는 고유한 이름을 지정합니다.

**참고:** **Create** 옵션이 사이드바에 고정되어 있지 않으면 먼저 줄임표(…) 옵션을 선택해야 합니다.

2.  약 1분 후 **Tables** 또는 **Files**가 없는 새 lakehouse가 생성됩니다.
3.  왼쪽의 **Explorer** 창에서 **Files** 노드의 `...` 메뉴에서 **New subfolder**를 선택하고 `new_data`라는 하위 폴더를 만듭니다.

**Pipeline 생성**

데이터를 수집하는 간단한 방법은 파이프라인의 **Copy Data** 활동을 사용하여 원본에서 데이터를 추출하고 lakehouse의 파일에 복사하는 것입니다.

1.  Lakehouse의 **Home** 페이지에서 **Get data**를 선택한 다음 **New data pipeline**을 선택하고 `Ingest Sales Data`라는 새 데이터 파이프라인을 만듭니다.
2.  **Copy Data wizard**가 자동으로 열리지 않으면 파이프라인 편집기 페이지에서 **Copy Data** > **Use copy assistant**를 선택합니다.
3.  **Copy Data wizard**의 **Choose data source** 페이지에서 검색 창에 `HTTP`를 입력한 다음 **New sources** 섹션에서 **HTTP**를 선택합니다.

    *Choose data source 페이지 스크린샷.*

4.  **Connect to data source** 창에서 데이터 원본 연결에 대한 다음 설정을 입력합니다.
    *   **URL:** `https://raw.githubusercontent.com/MicrosoftLearning/dp-data/main/sales.csv`
    *   **Connection:** `Create new connection`
    *   **Connection name:** 고유한 이름을 지정합니다.
    *   **Data gateway:** `(none)`
    *   **Authentication kind:** `Anonymous`
5.  **Next**를 선택합니다. 그런 다음 다음 설정이 선택되었는지 확인합니다.
    *   **Relative URL:** 비워 둡니다.
    *   **Request method:** `GET`
    *   **Additional headers:** 비워 둡니다.
    *   **Binary copy:** 선택 해제합니다.
    *   **Request timeout:** 비워 둡니다.
    *   **Max concurrent connections:** 비워 둡니다.
6.  **Next**를 선택하고 데이터가 샘플링될 때까지 기다린 다음 다음 설정이 선택되었는지 확인합니다.
    *   **File format:** `DelimitedText`
    *   **Column delimiter:** `Comma (,)`
    *   **Row delimiter:** `Line feed (\n)`
    *   **First row as header:** 선택합니다.
    *   **Compression type:** `None`
7.  **Preview data**를 선택하여 수집될 데이터 샘플을 확인합니다. 그런 다음 데이터 미리 보기를 닫고 **Next**를 선택합니다.
8.  **Connect to data destination** 페이지에서 다음 데이터 대상 옵션을 설정한 다음 **Next**를 선택합니다.
    *   **Root folder:** `Files`
    *   **Folder path name:** `new_data`
    *   **File name:** `sales.csv`
    *   **Copy behavior:** `None`
9.  다음 파일 형식 옵션을 설정하고 **Next**를 선택합니다.
    *   **File format:** `DelimitedText`
    *   **Column delimiter:** `Comma (,)`
    *   **Row delimiter:** `Line feed (\n)`
    *   **Add header to file:** 선택합니다.
    *   **Compression type:** `None`
10. **Copy summary** 페이지에서 복사 작업의 세부 정보를 검토한 다음 **Save + Run**을 선택합니다.

    다음과 같이 **Copy Data** 활동이 포함된 새 파이프라인이 생성됩니다.

    *Copy Data 활동이 있는 파이프라인 스크린샷.*

11. 파이프라인 실행이 시작되면 파이프라인 디자이너 아래의 **Output** 창에서 상태를 모니터링할 수 있습니다. ↻ (**Refresh**) 아이콘을 사용하여 상태를 새로 고치고 성공할 때까지 기다립니다.
12. 왼쪽 메뉴 모음에서 lakehouse를 선택합니다.
13. **Home** 페이지의 **Lakehouse explorer** 창에서 **Files**를 확장하고 `new_data` 폴더를 선택하여 `sales.csv` 파일이 복사되었는지 확인합니다.

**Notebook 생성**

1.  Lakehouse의 **Home** 페이지에 있는 **Open notebook** 메뉴에서 **New notebook**을 선택합니다.
2.  몇 초 후 단일 셀이 포함된 새 Notebook이 열립니다. Notebook은 코드 또는 Markdown(서식 있는 텍스트)을 포함할 수 있는 하나 이상의 셀로 구성됩니다.
3.  Notebook에서 일부 간단한 코드가 포함된 기존 셀을 선택한 다음 기본 코드를 다음 변수 선언으로 바꿉니다.

    ```python
    # code
    table_name = "sales"
    ```

4.  셀의 `...` 메뉴(오른쪽 상단)에서 **Toggle parameter cell**을 선택합니다. 이렇게 하면 셀에 선언된 변수가 파이프라인에서 Notebook을 실행할 때 매개변수로 처리되도록 셀이 구성됩니다.
5.  매개변수 셀 아래에서 **+ Code** 버튼을 사용하여 새 코드 셀을 추가합니다. 그런 다음 다음 코드를 추가합니다.

    ```python
    # code
    from pyspark.sql.functions import *

    # 새로운 판매 데이터 읽기
    df = spark.read.format("csv").option("header","true").load("Files/new_data/*.csv")

    ## 월 및 연도 열 추가
    df = df.withColumn("Year", year(col("OrderDate"))).withColumn("Month", month(col("OrderDate")))

    # FirstName 및 LastName 열 파생
    df = df.withColumn("FirstName", split(col("CustomerName"), " ").getItem(0)).withColumn("LastName", split(col("CustomerName"), " ").getItem(1))

    # 열 필터링 및 재정렬
    # 참고: 원본 CSV에 EmailAddress 대신 Email 이 있다면 아래 코드 수정 필요
    # 예: df = df["SalesOrderNumber", ..., "Email", "Item", ...]
    df = df["SalesOrderNumber", "SalesOrderLineNumber", "OrderDate", "Year", "Month", "FirstName", "LastName", "EmailAddress", "Item", "Quantity", "UnitPrice", "TaxAmount"]

    # 데이터를 테이블로 로드
    df.write.format("delta").mode("append").saveAsTable(table_name)
    ```

    이 코드는 **Copy Data** 활동에 의해 수집된 `sales.csv` 파일의 데이터를 로드하고, 일부 변환 로직을 적용하고, 변환된 데이터를 테이블로 저장합니다. 테이블이 이미 존재하면 데이터를 추가(append)합니다.

6.  Notebook이 다음과 유사한지 확인한 다음 도구 모음의 **▷ Run all** 버튼을 사용하여 포함된 모든 셀을 실행합니다.

    *매개변수 셀과 데이터 변환 코드가 있는 Notebook 스크린샷.*

**참고:** 이 세션에서 Spark 코드를 처음 실행하는 것이므로 Spark 풀을 시작해야 합니다. 즉, 첫 번째 셀은 완료하는 데 1분 정도 걸릴 수 있습니다.

7.  Notebook 실행이 완료되면 왼쪽의 **Lakehouse explorer** 창에서 **Tables**의 `...` 메뉴에서 **Refresh**를 선택하고 `sales` 테이블이 생성되었는지 확인합니다.
8.  Notebook 메뉴 모음에서 ⚙️ **Settings** 아이콘을 사용하여 Notebook 설정을 봅니다. 그런 다음 Notebook의 **Name**을 `Load Sales`로 설정하고 설정 창을 닫습니다.
9.  왼쪽의 허브 메뉴 모음에서 lakehouse를 선택합니다.
10. **Explorer** 창에서 보기를 새로 고칩니다. 그런 다음 **Tables**를 확장하고 `sales` 테이블을 선택하여 포함된 데이터의 미리 보기를 확인합니다.

**Pipeline 수정**

이제 데이터를 변환하고 테이블에 로드하는 Notebook을 구현했으므로 Notebook을 파이프라인에 통합하여 재사용 가능한 ETL 프로세스를 만들 수 있습니다.

1.  왼쪽의 허브 메뉴 모음에서 이전에 만든 **Ingest Sales Data** 파이프라인을 선택합니다.
2.  **Activities** 탭의 **All activities** 목록에서 **Delete data**를 선택합니다. 그런 다음 새 **Delete data** 활동을 **Copy data** 활동의 왼쪽에 배치하고 다음과 같이 해당 **On completion** 출력을 **Copy data** 활동에 연결합니다.

    *Delete data 및 Copy data 활동이 있는 파이프라인 스크린샷.*

3.  **Delete data** 활동을 선택하고 디자인 캔버스 아래 창에서 다음 속성을 설정합니다.
    *   **General:**
        *   **Name:** `Delete old files`
    *   **Source**
        *   **Connection:** 사용자의 lakehouse
        *   **File path type:** `Wildcard file path`
        *   **Folder path:** `Files / new_data`
        *   **Wildcard file name:** `*.csv`
        *   **Recursively:** 선택됨
    *   **Logging settings:**
        *   **Enable logging:** 선택 해제됨

    이 설정은 `sales.csv` 파일을 복사하기 전에 기존 `.csv` 파일이 삭제되도록 합니다.

4.  파이프라인 디자이너의 **Activities** 탭에서 **Notebook**을 선택하여 파이프라인에 **Notebook** 활동을 추가합니다.
5.  **Copy data** 활동을 선택한 다음 다음과 같이 해당 **On Completion** 출력을 **Notebook** 활동에 연결합니다.

    *Copy Data 및 Notebook 활동이 있는 파이프라인 스크린샷.*

6.  **Notebook** 활동을 선택한 다음 디자인 캔버스 아래 창에서 다음 속성을 설정합니다.
    *   **General:**
        *   **Name:** `Load Sales notebook`
    *   **Settings:**
        *   **Notebook:** `Load Sales`
        *   **Base parameters:** 다음 속성으로 새 매개변수를 추가합니다.

| Name       | Type   | Value     |
| :--------- | :----- | :-------- |
| table_name | String | new_sales |

`table_name` 매개변수는 Notebook으로 전달되어 매개변수 셀의 `table_name` 변수에 할당된 기본값을 재정의합니다.

7.  **Home** 탭에서 🖫 (**Save**) 아이콘을 사용하여 파이프라인을 저장합니다. 그런 다음 ▷ **Run** 버튼을 사용하여 파이프라인을 실행하고 모든 활동이 완료될 때까지 기다립니다.

    *Dataflow 활동이 있는 파이프라인 스크린샷.* (실제로는 Notebook 활동임)

**참고:** `Spark SQL queries are only possible in the context of a lakehouse. Please attach a lakehouse to proceed:` 오류 메시지가 표시되는 경우: Notebook을 열고 왼쪽 창에서 생성한 lakehouse를 선택한 다음 **Remove all Lakehouses**를 선택하고 다시 추가합니다. 파이프라인 디자이너로 돌아가서 ▷ **Run**을 선택합니다.

8.  포털 왼쪽 가장자리의 허브 메뉴 모음에서 lakehouse를 선택합니다.
9.  **Explorer** 창에서 **Tables**를 확장하고 `new_sales` 테이블을 선택하여 포함된 데이터의 미리 보기를 확인합니다. 이 테이블은 파이프라인에 의해 실행될 때 Notebook에서 생성되었습니다.

이 실습에서는 파이프라인을 사용하여 외부 원본에서 lakehouse로 데이터를 복사한 다음 Spark Notebook을 사용하여 데이터를 변환하고 테이블에 로드하는 데이터 수집 솔루션을 구현했습니다.

**리소스 정리**

이 실습에서는 Microsoft Fabric에서 파이프라인을 구현하는 방법을 배웠습니다.

Lakehouse 탐색을 마쳤으면 이 실습을 위해 만든 Workspace를 삭제할 수 있습니다.

1.  왼쪽 막대에서 Workspace 아이콘을 선택하여 포함된 모든 항목을 봅니다.
2.  **Workspace settings**를 선택하고 **General** 섹션에서 아래로 스크롤하여 **Remove this workspace**를 선택합니다.
3.  **Delete**를 선택하여 Workspace를 삭제합니다.

---
