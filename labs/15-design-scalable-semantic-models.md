## 확장 가능한 Semantic Model 디자인하기

**실습 시나리오**

이 연습에서는 DAX 함수를 사용하여 데이터 모델의 유연성과 효율성을 향상시키는 방법을 배웁니다. 특히 계산 그룹(Calculation groups) 및 필드 매개 변수(Field parameters)와 같은 기능을 활용합니다. 이러한 기능들을 함께 사용하면 여러 시각적 개체나 복잡한 DAX 표현식 없이도 대화형 보고서를 만들 수 있으며, 매우 유연하고 확장 가능한 Semantic Model을 구축할 수 있습니다.

**이 연습에서 배울 내용:**

*   DAX 함수를 사용하여 관계(relationship) 동작을 수정하는 방법
*   계산 그룹(Calculation groups)을 만들고 동적 시간 인텔리전스(Time Intelligence) 계산에 적용하는 방법
*   필드 매개 변수(Field parameters)를 만들어 다양한 필드와 측정값을 동적으로 선택하고 표시하는 방법

**예상 소요 시간:** 약 30분

---

**시작하기 전에 (Before you start)**

1.  다음 URL에서 `Sales Analysis` 시작 파일을 다운로드하고 로컬에 저장하세요:
    [`https://github.com/MicrosoftLearning/mslearn-fabric/raw/main/Allfiles/Labs/15/15-scalable-semantic-models.zip`](https://github.com/MicrosoftLearning/mslearn-fabric/raw/main/Allfiles/Labs/15/15-scalable-semantic-models.zip)

2.  다운로드한 zip 파일의 압축을 `C:\Users\Student\Downloads\15-scalable-semantic-models` 폴더에 풀어주세요.

3.  `15-Starter-Sales Analysis.pbix` 파일을 여세요.

4.  변경 사항을 적용하라는 경고 창이 나타나면 무시하고 닫으세요 (`Discard changes`를 선택하지 마세요).

---

**관계 작업하기 (Work with relationships)**

이 작업에서는 미리 개발된 Power BI Desktop 솔루션을 열어 데이터 모델에 대해 알아봅니다. 그런 다음 활성 모델 관계의 동작을 탐색합니다.

1.  Power BI Desktop 왼쪽에서 `Model` 뷰로 전환하세요.

    ![Model view 아이콘](https://learn.microsoft.com/ko-kr/training/modules/design-power-bi-semantic-models-dax/media/model-view-icon.png) *(참고: 실제 실습 환경의 아이콘을 참조하세요)*

2.  모델 다이어그램을 사용하여 모델 디자인을 검토하세요.

    ![Model view 다이어그램 예시](https://learn.microsoft.com/ko-kr/training/modules/design-power-bi-semantic-models-dax/media/relationships.png) *(참고: 실제 실습 파일의 다이어그램을 확인하세요)*

3.  `Date` 테이블과 `Sales` 테이블 사이에 세 개의 관계가 있는 것을 확인하세요.

    > **원리 이해:** `Date` 테이블의 `Date` 열은 고유한 값을 가지며 관계의 "일(One)" 측면을 나타냅니다. `Date` 테이블의 어떤 열에 필터가 적용되면, 이 세 관계 중 *하나*를 통해 `Sales` 테이블로 필터가 전파됩니다. 기본적으로는 활성(Active) 관계를 통해서만 필터가 전파됩니다.

4.  세 개의 관계 각각에 마우스 커서를 올려 `Sales` 테이블에서 관계의 "다(Many)" 측면 열이 강조 표시되는 것을 확인하세요. (`OrderDate`, `DueDate`, `ShipDate`)

5.  `Date` 와 `OrderDate` 사이의 관계가 활성(실선으로 표시) 상태인 것을 확인하세요. 현재 모델 디자인은 `Date` 테이블이 **역할 수행 차원(Role-playing dimension)**임을 나타냅니다. 이 차원은 주문 날짜(Order Date), 마감 날짜(Due Date) 또는 배송 날짜(Ship Date)의 역할을 수행할 수 있습니다. 어떤 역할을 수행하는지는 보고서의 분석 요구 사항에 따라 달라집니다.

    > **주석:** 역할 수행 차원은 하나의 차원 테이블(여기서는 `Date`)이 여러 팩트 테이블 열(여기서는 `Sales` 테이블의 `OrderDate`, `DueDate`, `ShipDate`)과 연결되어 각각 다른 의미(역할)로 사용될 때를 말합니다.

6.  나중에 DAX를 사용하여 이러한 비활성(Inactive) 관계들을 활용할 것입니다. 다른 날짜 열에 대해 활성 관계를 만들기 위해 별도의 테이블을 추가 생성할 필요 없이 이를 처리하는 방법을 배웁니다.

---

**날짜별 판매 데이터 시각화하기 (Visualize sales data by date)**

이 작업에서는 연도별 총 판매액을 시각화하고 비활성 관계를 사용하는 방법을 알아봅니다.

1.  `Report` 뷰로 전환하세요.

    ![Report view 아이콘](https://learn.microsoft.com/ko-kr/training/modules/design-power-bi-semantic-models-dax/media/report-view-icon.png) *(참고: 실제 실습 환경의 아이콘을 참조하세요)*

2.  `Table` 시각적 개체를 추가하려면 `Visualizations` 창에서 `Table` 시각적 개체 아이콘을 선택하세요.

    ![Table visual 아이콘 선택](https://learn.microsoft.com/ko-kr/training/modules/design-power-bi-semantic-models-dax/media/visualizations-pane-table-selected.png) *(참고: 실제 실습 환경의 아이콘을 참조하세요)*

3.  `Table` 시각적 개체에 열을 추가하려면, 오른쪽에 있는 `Data` 창에서 먼저 `Date` 테이블을 확장하세요.

4.  `Year` 열을 `Table` 시각적 개체 안으로 드래그 앤 드롭하세요.

5.  `Sales` 테이블을 확장한 다음, `Total Sales` 열을 `Table` 시각적 개체 안으로 드래그 앤 드롭하세요.

6.  `Table` 시각적 개체를 검토하세요.

    ![Year 및 Total Sales가 포함된 Table visual](https://learn.microsoft.com/ko-kr/training/modules/design-power-bi-semantic-models-dax/media/table-visual-year-total-sales.png) *(참고: 실제 실습 파일의 결과를 확인하세요)*

    > **설명:** 테이블 시각적 개체는 `Total Sales` 열의 합계를 `Year`별로 그룹화하여 보여줍니다. 그런데 여기서 `Year`는 무엇을 의미할까요? `Date` 테이블과 `Sales` 테이블 사이에 `OrderDate` 열에 대한 **활성 관계**가 있기 때문에, 여기서 `Year`는 주문이 이루어진 회계 연도를 의미합니다.

---

**비활성 관계 사용하기 (Use inactive relationships)**

이 작업에서는 `USERELATIONSHIP` 함수를 사용하여 비활성 관계를 특정 계산 중에만 일시적으로 활성화합니다.

1.  `Data` 창에서 `Sales` 테이블을 마우스 오른쪽 버튼으로 클릭한 다음 `New measure`를 선택하세요.

    ![New measure 선택](https://learn.microsoft.com/ko-kr/training/modules/design-power-bi-semantic-models-dax/media/data-pane-sales-new-measure.png) *(참고: 실제 실습 환경의 메뉴를 참조하세요)*

2.  리본 메뉴 아래에 있는 수식 입력줄(formula bar)에서 기존 텍스트를 다음 측정값 정의로 바꾸고 `Enter` 키를 누르세요.

    ```dax
    Sales Shipped =
    CALCULATE (
        SUM ('Sales'[Sales]),
        USERELATIONSHIP('Date'[Date], 'Sales'[ShipDate])
    )
    ```

    > **DAX 함수 설명:**
    > *   `CALCULATE(표현식, 필터1, 필터2, ...)`: 기존 필터 컨텍스트를 수정한 후 표현식을 평가하는 매우 강력하고 중요한 DAX 함수입니다.
    > *   `SUM('Sales'[Sales])`: `Sales` 테이블의 `Sales` 열 합계를 계산하는 기본 표현식입니다.
    > *   `USERELATIONSHIP(일_측면_열, 다_측면_열)`: `CALCULATE` 함수 내에서 사용될 때, 지정된 비활성 관계를 해당 `CALCULATE` 계산 동안에만 활성 상태로 만듭니다. 여기서는 `Date[Date]`와 `Sales[ShipDate]` 간의 비활성 관계를 활성화합니다.
    >
    > **결과:** 이 수식은 `Sales` 합계를 계산하되, 기본 활성 관계(`OrderDate` 기준) 대신 `ShipDate` 기준의 관계를 사용하여 계산합니다.

3.  `Sales Shipped` 측정값을 `Table` 시각적 개체에 추가하세요.

4.  모든 열이 완전히 보이도록 `Table` 시각적 개체의 너비를 넓히세요. 총합계(Total) 행의 값은 같지만, `Total Sales`와 `Sales Shipped`의 각 연도별 판매액은 다른 것을 관찰하세요. 이 차이는 주문이 특정 연도에 접수되었지만 다음 해에 배송되었거나 아직 배송되지 않았기 때문에 발생합니다.

    ![Year, Total Sales, Sales Shipped가 포함된 Table](https://learn.microsoft.com/ko-kr/training/modules/design-power-bi-semantic-models-dax/media/table-visual-year-total-sales-sales-shipped.png) *(참고: 실제 실습 파일의 결과를 확인하세요)*

    > **원리 이해:** 관계를 일시적으로 활성 상태로 설정하는 측정값을 만드는 것은 역할 수행 차원을 다루는 한 가지 방법입니다. 하지만 많은 측정값에 대해 역할 수행 버전을 만들어야 할 때는 이 작업이 지루해질 수 있습니다. 예를 들어, 10개의 판매 관련 측정값이 있고 3개의 역할 수행 날짜(주문, 마감, 배송)가 있다면 총 30개의 측정값을 만들어야 할 수도 있습니다. **계산 그룹(Calculation Groups)**을 사용하면 이 과정을 훨씬 쉽게 만들 수 있습니다.

---

**계산 그룹 만들기 (Create calculation groups)**

이 작업에서는 시간 인텔리전스(Time Intelligence) 분석을 위한 계산 그룹을 만듭니다.

> **원리 이해:** 계산 그룹은 기존 측정값에 적용될 수 있는 재사용 가능한 계산 로직(DAX 수식)의 모음입니다. 이를 통해 동일한 패턴의 계산(예: YTD, PY, YoY)을 여러 기본 측정값에 대해 반복적으로 만들 필요 없이 한 번만 정의하고 적용할 수 있어 모델을 훨씬 간결하고 관리하기 쉽게 만듭니다.

1.  `Model` 뷰로 전환하세요.

2.  `Model` 뷰의 `Modeling` 리본 메뉴에서 `Calculation group` 버튼을 선택하여 새로운 계산 그룹 테이블, 그룹 열, 그리고 기본 항목을 만듭니다. 경고 창이 나타나면 `Yes`를 선택하여 계산 그룹 생성을 확인하세요.

    ![Calculation group 버튼](https://learn.microsoft.com/ko-kr/training/modules/design-power-bi-semantic-models-dax/media/calculation-group-button.png) *(참고: 실제 실습 환경의 리본 메뉴를 참조하세요)*

    > **중요 참고 (Implicit Measures):** 암시적 측정값(Implicit measure)은 `Report` 뷰에서 `Data` 창의 데이터 열을 시각적 개체에 직접 사용할 때 발생합니다. 시각적 개체는 이를 SUM, AVERAGE, MIN, MAX 등 기본 집계로 요약할 수 있게 해주며, 이것이 암시적 측정값이 됩니다. **계산 그룹을 만들면 Power BI Desktop은 더 이상 암시적 측정값을 만들지 않습니다.** 즉, 이제부터 데이터 열을 집계하려면 반드시 명시적인 측정값(Explicit measure)을 만들어야 합니다.

3.  계산 그룹의 이름을 `Time Calculations`로, 계산 열(새로 생긴 테이블의 유일한 열)의 이름을 `Yearly Calculations`로 변경하세요. (`Properties` 창 또는 `Data` 창에서 이름 변경 가능)

4.  `Data` 창의 `Model` 탭에서, 계산 그룹과 함께 자동으로 생성된 계산 항목(`Calculation Item`)을 선택하세요.

5.  해당 항목의 수식을 다음으로 바꾸고 적용(Commit)하세요:

    ```dax
    Year-to-Date (YTD) = CALCULATE(SELECTEDMEASURE(), DATESYTD('Date'[Date]))
    ```

    > **DAX 함수 설명:**
    > *   `SELECTEDMEASURE()`: 계산 그룹이 적용되는 컨텍스트의 현재 측정값을 참조하는 특수 함수입니다. 예를 들어, 이 계산 그룹을 'Total Sales' 측정값에 적용하면 `SELECTEDMEASURE()`는 'Total Sales'를 나타냅니다.
    > *   `DATESYTD('Date'[Date])`: 현재 필터 컨텍스트에서 해당 연도의 시작일부터 마지막 날짜까지의 모든 날짜를 포함하는 테이블을 반환합니다 (Year-to-Date).

6.  `Calculation items` 필드(또는 `Yearly Calculations` 열)를 마우스 오른쪽 버튼으로 클릭하고 `New calculation item`을 선택하세요.

7.  새 항목에 다음 DAX 수식을 사용하세요:

    ```dax
    Previous Year (PY) = CALCULATE(SELECTEDMEASURE(), PREVIOUSYEAR('Date'[Date]))
    ```

    > **DAX 함수 설명:**
    > *   `PREVIOUSYEAR('Date'[Date])`: 현재 필터 컨텍스트의 날짜를 기준으로 정확히 1년 전의 모든 날짜를 포함하는 테이블을 반환합니다.

8.  다음 DAX 수식으로 세 번째 항목을 만드세요:

    ```dax
    Year-over-Year (YoY) Growth =
    VAR MeasurePriorYear =
        CALCULATE(
            SELECTEDMEASURE(),
            SAMEPERIODLASTYEAR('Date'[Date])
        )
    RETURN
        DIVIDE(
            (SELECTEDMEASURE() - MeasurePriorYear),
            MeasurePriorYear
        )
    ```

    > **DAX 수식 설명:**
    > *   `VAR MeasurePriorYear = ...`: 전년도 동기간의 측정값을 계산하여 `MeasurePriorYear`라는 변수에 저장합니다.
    > *   `SAMEPERIODLASTYEAR('Date'[Date])`: 현재 필터 컨텍스트의 날짜 세트와 동일한 기간이지만 1년 전인 날짜 세트를 반환합니다.
    > *   `RETURN DIVIDE(...)`: 현재 측정값과 전년도 동기간 측정값의 차이를 계산하고, 그 차이를 전년도 값으로 나눕니다 (성장률 계산). `DIVIDE` 함수는 0으로 나누는 오류를 안전하게 처리합니다 (결과로 BLANK 반환).

9.  마지막 계산 항목(`YoY Growth`)은 백분율 값만 반환해야 하므로, 영향을 받는 측정값의 형식을 변경하기 위해 동적 서식 문자열(Dynamic format string)이 필요합니다.

10. `YoY Growth` 항목의 `Properties` 창에서 `Dynamic format string` 기능을 활성화(Enable)하세요.

11. 수식 입력줄에서 왼쪽 필드가 `Format`으로 설정되어 있는지 확인하고, 다음 서식 문자열을 작성하세요: `"0.##%"`

    > **설명:** 동적 서식 문자열을 사용하면 계산 항목 자체가 적용될 때 값의 표시 형식을 결정할 수 있습니다. 여기서는 소수점 두 자리까지 표시하는 백분율 형식을 지정합니다.

12. 계산 그룹이 다음과 같이 보이는지 확인하세요:

    ![완성된 Calculation group 예시](https://learn.microsoft.com/ko-kr/training/modules/design-power-bi-semantic-models-dax/media/calculation-group-complete.png) *(참고: 실제 실습 파일의 결과를 확인하세요)*

---

**측정값에 계산 그룹 적용하기 (Apply a calculation group to measures)**

이 작업에서는 계산 항목이 시각적 개체의 측정값에 어떻게 영향을 미치는지 시각화합니다.

1.  `Report` 뷰로 전환하세요.

2.  캔버스 하단에서 `Overview` 탭을 선택하세요.

3.  캔버스에 이미 생성되어 있는 `matrix` 시각적 개체를 선택하고, `Data` 창에서 `Yearly Calculations` 계산 열(from `Time Calculations` table)을 `Visualizations` 창의 `Columns` 필드로 드래그하세요.

    ![Matrix에 Yearly Calculations 추가](https://learn.microsoft.com/ko-kr/training/modules/design-power-bi-semantic-models-dax/media/matrix-visual-calculation-group.png) *(참고: 실제 실습 파일의 결과를 확인하세요)*

    > **관찰:** 이제 matrix는 각 계산 항목(`YTD`, `PY`, `YoY Growth`)에 대한 판매 수치 세트를 보여줍니다. 원래 `Values` 영역에 있던 측정값(예: `Total Sales`)에 대해 각 계산 항목이 적용된 결과가 열 방향으로 펼쳐진 것입니다.

    > **주석:** 이 모든 정보를 하나의 시각적 개체에서 한 번에 보는 것은 읽기 어려울 수 있습니다. 따라서 한 번에 하나의 판매 수치(예: YTD만 보거나 PY만 보기)만 표시하도록 시각적 개체를 제한하는 것이 편리할 수 있습니다. 이를 위해 **필드 매개 변수(Field Parameters)**를 사용할 수 있습니다. *수정: 원문은 한 번에 하나의 계산 항목(YTD, PY, YoY)이 아니라, 한 번에 하나의 기본 측정값(Total Sales, Profit 등)을 선택하는 시나리오로 보입니다. 아래 필드 매개변수 생성 부분을 보면 이것이 맞습니다.*

---

**필드 매개 변수 만들기 (Create field parameters)**

이 작업에서는 필드 매개 변수를 만들어 시각적 개체를 동적으로 변경합니다.

> **원리 이해:** 필드 매개 변수(Field Parameters)는 사용자가 보고서에서 보고 싶은 필드(열 또는 측정값)를 동적으로 선택할 수 있게 해주는 기능입니다. 슬라이서(Slicer)나 다른 컨트롤과 연결되어, 사용자의 선택에 따라 시각적 개체에 표시되는 필드를 즉시 변경할 수 있습니다. 이는 여러 버전의 시각적 개체를 만들거나 복잡한 북마크(Bookmark)를 설정할 필요 없이 사용자에게 높은 유연성을 제공합니다.

1.  상단 리본 메뉴에서 `Modeling` 탭을 선택한 다음, `New parameter` 버튼을 확장하고 `Fields`를 선택하세요.

    ![New parameter -> Fields 선택](https://learn.microsoft.com/ko-kr/training/modules/design-power-bi-semantic-models-dax/media/new-parameter-fields.png) *(참고: 실제 실습 환경의 리본 메뉴를 참조하세요)*

2.  `Parameters` 창에서:
    *   매개 변수 이름을 `Sales Figures`로 변경하세요.
    *   `Add slicer to this page` 옵션이 선택되어 있는지 확인하세요.
    *   `Sales` 테이블에서 다음 필드들을 추가하세요:
        *   `Total Sales` (측정값)
        *   `Profit` (측정값)
        *   `Profit Margin` (측정값)
        *   `Orders` (측정값)

    ![Field parameter 설정 창](https://learn.microsoft.com/ko-kr/training/modules/design-power-bi-semantic-models-dax/media/field-parameter-sales.png) *(참고: 실제 실습 파일의 필드 목록을 확인하세요)*

3.  `Create`를 선택하세요.

4.  슬라이서가 생성되면, `matrix` 시각적 개체를 선택하고 `Visualizations` 창의 `Values` 영역에서 기존 필드들을 모두 제거한 다음, 대신 새로 생성된 `Sales Figures` 필드 매개 변수를 추가하세요.

    ![Matrix Values에 Sales Figures 추가](https://learn.microsoft.com/ko-kr/training/modules/design-power-bi-semantic-models-dax/media/matrix-visual-field-parameter.png) *(참고: 실제 실습 파일의 결과를 확인하세요)*

5.  슬라이서에서 다른 판매 수치(`Total Sales`, `Profit`, `Profit Margin`, `Orders`)를 선택해보고, 각 항목을 선택할 때마다 matrix가 어떻게 변경되는지 확인하세요.

    > **결합 효과 확인:** 이제 사용자는 `Sales Figures` 슬라이서를 통해 보고 싶은 기본 측정값(예: `Profit`)을 선택할 수 있습니다. 그러면 `matrix`는 선택된 측정값(`Profit`)에 대해 계산 그룹(`Time Calculations`)의 각 항목(`PY`, `YoY Growth`, `YTD`)이 적용된 결과를 보여줍니다. 즉, **필드 매개 변수는 어떤 기본 측정값을 볼지 결정하고, 계산 그룹은 해당 측정값에 어떤 시간 기반 계산을 적용할지 결정합니다.**

    ![Calculation group과 Field parameter가 적용된 Matrix (Profit 선택됨)](https://learn.microsoft.com/ko-kr/training/modules/design-power-bi-semantic-models-dax/media/matrix-visual-field-parameter-calculation-group.png) *(참고: 실제 실습 파일의 결과를 확인하세요)*

---

**필드 매개 변수 편집하기 (Edit field parameters)**

이 작업에서는 `Sales Figures` 필드 매개 변수의 DAX 표현식을 직접 수정하여 편집합니다.

1.  캔버스 하단에서 `Salesperson Performance` 탭을 선택하세요. 차트를 `Sales by Month`와 `Target by Month` 간에 전환하는 묶은 세로 막대형 차트(clustered bar chart)가 있는 것을 확인하세요. (이전에는 아마도 북마크 버튼으로 구현되었을 것입니다.)

    > **배경 설명:** 이전에는 북마크 버튼을 사용하여 각 옵션(Sales 또는 Target)에 따라 시각적 개체 유형이나 표시되는 측정값을 변경했을 수 있습니다. 하지만 분석하려는 측정값이 많아지면 각 측정값마다 북마크 버튼을 만들어야 하므로 매우 시간이 많이 걸릴 수 있습니다. 대신, 분석하려는 모든 측정값을 포함하는 필드 매개 변수를 사용하면 슬라이서로 빠르게 전환할 수 있습니다.

    ![변경 전 Salesperson Performance 페이지](https://learn.microsoft.com/ko-kr/training/modules/design-power-bi-semantic-models-dax/media/salesperson-performance-page-before.png) *(참고: 실제 실습 파일의 상태를 확인하세요)*

2.  막대 차트 시각적 개체를 선택하고, `Visualizations` 창의 `X-axis` (또는 Y-axis일 수 있음, 값 축 확인)에 있는 `Total Sales` 필드를 `Sales Figures` 필드 매개 변수로 교체하세요.

3.  `Slicer` 시각적 개체를 새로 만들고, `Sales Figures` 매개 변수를 `Field` 영역으로 드래그하세요.

4.  이 시각적 개체에서는 여전히 `Target by Month`를 평가해야 하는데, 이 `Target` 측정값은 현재 `Sales Figures` 필드 매개 변수에 포함되어 있지 않습니다.

5.  `Data` 창에서 `Sales Figures` 매개 변수를 선택하세요. 수식 입력줄에 이 매개 변수를 정의하는 DAX 코드가 나타납니다. 이 DAX 표현식에 `Target` 필드를 다음과 같이 추가하세요:

    ```dax
    Sales Figures = {
      ("Total Sales", NAMEOF('Sales'[Total Sales]), 0),
      ("Profit", NAMEOF('Sales'[Profit]), 1),
      ("Profit Margin", NAMEOF('Sales'[Profit Margin]), 2),
      ("Orders", NAMEOF('Sales'[Orders]), 3),
      ("Target", NAMEOF('Targets'[Target]), 4)
     }
    ```

    > **DAX 수식 설명 (Field Parameter):**
    > *   필드 매개 변수는 내부적으로 DAX를 사용하여 테이블을 생성합니다. 각 행은 슬라이서에 표시될 옵션을 나타냅니다.
    > *   각 행은 일반적으로 `("표시 이름", NAMEOF(참조할 필드), 순서)` 형식의 튜플(tuple)로 구성됩니다.
    > *   `NAMEOF(테이블[필드])`: 필드 이름을 문자열로 안전하게 가져오는 함수입니다. 필드 이름이 변경되어도 자동으로 업데이트됩니다.
    > *   마지막 줄에 `("Target", NAMEOF('Targets'[Target]), 4)`를 추가하여 `Targets` 테이블의 `Target` 측정값을 매개 변수 옵션에 포함시켰습니다. 순서(4)는 슬라이서에 표시될 순서를 지정합니다.

6.  변경 사항을 적용(Commit, Enter 키 누르기)하고, 슬라이서에서 다른 `Sales figures`를 선택할 때 시각적 개체가 올바르게 변경되는지 확인하세요. 이제 `Target`도 선택할 수 있어야 합니다.

7.  (만약 있었다면) 기존의 북마크 버튼들을 삭제하고, 보고서 페이지의 최종 상태를 관찰하세요.

    ![Field parameter가 적용된 최종 Salesperson Performance 페이지](https://learn.microsoft.com/ko-kr/training/modules/design-power-bi-semantic-models-dax/media/salesperson-performance-page-after.png) *(참고: 실제 실습 파일의 최종 상태를 확인하세요)*

---

**실습 완료 (Lab complete)**

축하합니다! 이 연습을 통해 DAX 함수, 계산 그룹, 필드 매개 변수를 활용하여 유연하고 확장 가능한 Semantic Model을 디자인하는 방법을 배우셨습니다.

연습을 마치려면 Power BI Desktop을 닫으세요. 파일은 저장할 필요 없습니다.
