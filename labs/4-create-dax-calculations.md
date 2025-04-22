
## Power BI Desktop에서 DAX 계산 만들기

**실습 시나리오**

이 실습에서는 DAX(Data Analysis Expressions)를 사용하여 계산된 테이블, 계산된 열, 그리고 간단한 측정값을 만들어 보겠습니다.

**이 실습에서 배울 내용:**

*   계산된 테이블 만들기 (Create calculated tables)
*   계산된 열 만들기 (Create calculated columns)
*   측정값 만들기 (Create measures)

**예상 소요 시간:** 약 45분

---

**시작하기 (Get started)**

이 연습을 완료하려면 먼저 웹 브라우저를 열고 다음 URL을 입력하여 zip 폴더를 다운로드하세요:

[`https://github.com/MicrosoftLearning/mslearn-fabric/raw/main/Allfiles/Labs/14/14-create-dax.zip`](https://github.com/MicrosoftLearning/mslearn-fabric/raw/main/Allfiles/Labs/14/14-create-dax.zip)

다운로드한 zip 파일의 압축을 `C:\Users\Student\Downloads\14-create-dax` 폴더에 풀어주세요.

`14-Starter-Sales Analysis.pbix` 파일을 여세요.

*   **참고:** Power BI Desktop을 열 때 로그인 창이 나타나면 `Cancel`을 선택하여 닫아도 됩니다. 다른 정보성 창도 닫아주세요. 변경 사항을 적용하라는 메시지가 표시되면 `Apply Later`를 선택하세요.

---

**Salesperson 계산된 테이블 만들기 (Create the Salesperson calculated table)**

이 작업에서는 `Salesperson` 계산된 테이블을 만듭니다. 이 테이블은 `Sales` 테이블과 직접적인 관계를 갖게 됩니다.

> **주석:** 계산된 테이블(Calculated Table)은 DAX 수식을 사용하여 모델 내에 새 테이블을 만드는 기능입니다. 기존 테이블의 데이터를 기반으로 하거나 완전히 새로운 테이블을 생성할 수 있습니다. 이는 데이터 원본에는 없지만 분석에 필요한 특정 테이블 구조를 만들 때 유용합니다.

계산된 테이블은 먼저 테이블 이름을 입력하고, 등호(=)를 입력한 다음, 테이블을 반환하는 DAX 수식을 입력하여 만듭니다. 테이블 이름은 데이터 모델 내에서 고유해야 합니다.

수식 입력줄(Formula bar)은 유효한 DAX 수식을 입력하는 데 사용됩니다. 자동 완성, Intellisense, 색상 코딩과 같은 기능을 지원하여 빠르고 정확하게 수식을 입력할 수 있도록 도와줍니다.

1.  Power BI Desktop의 `Report` 뷰에서 `Modeling` 리본 메뉴의 `Calculations` 그룹 안에 있는 `New Table`을 선택하세요.

    ![Picture 1](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-100.png)

2.  리본 메뉴 바로 아래에 나타나는 수식 입력줄(formula bar)에 다음을 입력하세요:
    `Salesperson =`
    그런 다음 `Shift+Enter` 키를 눌러 줄을 바꾸고, 다음을 입력하세요:
    `'Salesperson (Performance)'`
    마지막으로 `Enter` 키를 누르세요.

    *   **참고:** 편의를 위해 이 실습의 모든 DAX 정의는 `14-create-dax\Snippets.txt` 파일에서 복사하여 사용할 수 있습니다.

    ![Picture 4](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-103.png)

    > **설명:** 이 테이블 정의는 `'Salesperson (Performance)'` 테이블의 복사본을 만듭니다. 중요한 점은 데이터만 복사되고, 가시성(visibility), 서식(formatting) 등과 같은 모델 속성은 복사되지 않는다는 것입니다.

3.  `Data` 창에서 새로 생성된 `Salesperson` 테이블 아이콘 앞에 계산기 모양이 추가된 것을 확인하세요. 이는 계산된 테이블임을 나타냅니다.

    ![Picture 10](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-109.png)

    > **원리 이해:** 계산된 테이블은 테이블을 반환하는 DAX 수식으로 정의됩니다. 계산된 테이블은 값을 구체화(materialize)하고 저장하기 때문에 데이터 모델의 크기를 증가시킨다는 점을 이해하는 것이 중요합니다. 이 테이블들은 수식의 종속성이 새로 고쳐질 때마다 다시 계산됩니다. 예를 들어, 이 데이터 모델의 경우 나중에 새로운 날짜 값이 테이블에 로드될 때 다시 계산될 것입니다.
    > Power Query에서 가져온 테이블과 달리, 계산된 테이블은 외부 데이터 원본에서 데이터를 로드하는 데 사용할 수 없습니다. 오직 데이터 모델에 이미 로드된 데이터를 기반으로 데이터를 변환할 수만 있습니다.

4.  `Model` 뷰로 전환하고, `Salesperson` 테이블이 사용 가능한지 확인하세요 (테이블을 찾으려면 뷰를 재설정해야 할 수도 있습니다).

5.  `Salesperson` 테이블의 `EmployeeKey` 열과 `Sales` 테이블의 `EmployeeKey` 열 사이에 관계(relationship)를 만드세요. (드래그 앤 드롭으로 연결)

6.  `Salesperson (Performance)` 테이블과 `Sales` 테이블 사이의 비활성(inactive) 관계를 마우스 오른쪽 버튼으로 클릭한 다음 `Delete`를 선택하세요. 삭제 확인 메시지가 나타나면 `Yes`를 선택하세요.

7.  `Salesperson` 테이블에서 다음 열들을 다중 선택(Ctrl 키 누른 채 클릭)한 다음 숨기세요 (`Is Hidden` 속성을 `Yes`로 설정):
    *   `EmployeeID`
    *   `EmployeeKey`
    *   `UPN`

    > **주석:** 분석에 직접 사용되지 않거나 중복되는 키 열 등을 숨기면 최종 사용자가 `Data` 창에서 필드를 선택할 때 혼란을 줄이고 모델을 더 깔끔하게 유지할 수 있습니다.

8.  모델 다이어그램에서 `Salesperson` 테이블을 선택하세요.

9.  `Properties` 창의 `Description` 상자에 다음을 입력하세요: `Salesperson related to Sales`

    > **설명:** 여기서 입력한 설명(Description)은 사용자가 `Data` 창에서 테이블이나 필드 위로 마우스 커서를 가져가면 도구 설명(tooltip)으로 나타납니다. 모델의 구조나 필드의 의미를 이해하는 데 도움을 줍니다.

10. `Salesperson (Performance)` 테이블에 대해서도 `Description`을 설정하세요: `Salesperson related to region(s)`

    이제 데이터 모델은 판매원을 분석할 때 두 가지 대안을 제공합니다. `Salesperson` 테이블은 판매원이 직접 수행한 판매를 분석하는 데 사용되고, `Salesperson (Performance)` 테이블은 해당 판매원에게 할당된 판매 지역에서 발생한 판매를 분석하는 데 사용됩니다.

---

**Date 테이블 만들기 (Create the Date table)**

이 작업에서는 `Date` 테이블을 만듭니다.

> **원리 이해:** Power BI에서 시간 기반 분석(Time Intelligence)을 효과적으로 수행하려면 별도의 'Date 테이블'을 사용하는 것이 매우 중요합니다. 이 테이블에는 날짜와 관련된 다양한 속성(연도, 분기, 월 등)이 포함되어 있어 DAX 시간 인텔리전스 함수들이 올바르게 작동하도록 돕습니다.

1.  `Table` 뷰로 전환하세요. `Home` 리본 탭의 `Calculations` 그룹에서 `New Table`을 선택하세요.

    ![Picture 5](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-104.png)

2.  수식 입력줄에 다음 DAX 코드를 입력하세요:

    ```dax
    Date =
    CALENDARAUTO(6)
    ```

    ![Picture 6](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-105.png)

    > **DAX 함수 설명:**
    > *   `CALENDARAUTO([fiscal_year_end_month])`: 이 함수는 모델 내의 모든 날짜 열을 스캔하여 가장 이른 날짜와 가장 늦은 날짜를 찾아냅니다. 그리고 그 범위 내의 모든 날짜를 포함하는 단일 열 테이블을 생성합니다. 전체 연도를 포함하도록 범위를 양쪽으로 확장합니다.
    > *   `6`이라는 인수는 회계 연도의 마지막 달이 6월(June)임을 의미합니다. 인수를 생략하면 기본값인 12(December)가 사용됩니다. 이 예제에서는 Adventure Works사의 회계 연도가 7월에 시작하여 6월에 끝나는 것을 반영합니다.

3.  생성된 테이블의 `Date` 열에 날짜 값들이 미국 지역 설정(mm/dd/yyyy 형식)으로 표시되는 것을 확인하세요.

    ![Picture 7](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-106.png)

4.  화면 왼쪽 하단의 상태 표시줄(status bar)에서 테이블 통계를 확인하세요. 1826개의 데이터 행이 생성되었으며, 이는 5개년 치의 전체 데이터에 해당함을 나타냅니다.

    ![Picture 9](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-108.png)

---

**계산된 열 만들기 (Create calculated columns)**

이 작업에서는 다양한 시간 기준으로 필터링하고 그룹화할 수 있도록 `Date` 테이블에 열을 더 추가합니다. 또한 다른 열의 정렬 순서를 제어하기 위한 계산된 열도 만듭니다.

*   **참고:** 편의를 위해 이 실습의 모든 DAX 정의는 `Snippets.txt` 파일에서 복사하여 사용할 수 있습니다.

> **원리 이해:** 계산된 열(Calculated Column)은 테이블 내의 각 행에 대해 DAX 수식을 평가하여 값을 계산하고 저장합니다. 계산된 테이블처럼 모델의 크기를 증가시키지만, 행 수준의 컨텍스트(row context)에서 계산이 필요할 때 유용합니다. 예를 들어, 날짜로부터 연도, 월, 분기 등을 추출하거나, 여러 열의 값을 조합하여 새로운 분류를 만드는 데 사용됩니다.

1.  `Date` 테이블이 선택된 상태에서, `Table Tools` 상황별 리본 메뉴의 `Calculations` 그룹에서 `New Column`을 선택하세요.

    ![Picture 11](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-110.png)

    계산된 열은 먼저 열 이름을 입력하고, 등호(=)를 입력한 다음, 단일 값 결과를 반환하는 DAX 수식을 입력하여 만듭니다. 열 이름은 해당 테이블 내에서 고유해야 합니다.

2.  수식 입력줄에 다음을 입력(또는 snippets 파일에서 복사)하고 `Enter` 키를 누르세요:

    ```dax
    Year =
    "FY" & YEAR('Date'[Date]) + IF(MONTH('Date'[Date]) > 6, 1, 0)
    ```

    > **DAX 수식 설명:**
    > *   `YEAR('Date'[Date])`: 해당 날짜의 연도를 추출합니다.
    > *   `MONTH('Date'[Date])`: 해당 날짜의 월을 추출합니다.
    > *   `IF(MONTH('Date'[Date]) > 6, 1, 0)`: 월이 6월 이후(7월~12월)이면 1을 반환하고, 그렇지 않으면 0을 반환합니다.
    > *   `YEAR(...) + IF(...)`: 연도 값에 위 IF 함수의 결과(0 또는 1)를 더합니다. 즉, 7월부터 12월까지는 연도에 1을 더합니다.
    > *   `"FY" & ...`: 계산된 회계 연도 숫자 앞에 "FY" 문자열을 붙입니다. (예: 2017년 7월 1일은 FY2018이 됩니다.)
    >
    > 이 수식은 Adventure Works의 회계 연도 계산 방식(7월 시작)을 반영합니다.

3.  `Snippets.txt` 파일의 정의를 사용하여 `Date` 테이블에 다음 두 개의 계산된 열을 추가로 만드세요:
    *   `Quarter`
    *   `Month`

    ```dax
    // Quarter 계산된 열 수식 예시 (실제 Snippets.txt 내용 확인 필요)
    Quarter =
    "FY" & FORMAT('Date'[Date], "YYYY") & " Q" & CEILING(MONTH('Date'[Date])/3, 1)
    // Month 계산된 열 수식 예시 (실제 Snippets.txt 내용 확인 필요)
    Month =
    FORMAT('Date'[Date], "yyyy-MM")
    ```
    *(주: 위 분기/월 예시 수식은 일반적인 방식이며, 실습 파일의 정확한 수식을 사용하세요.)*

4.  새로운 열들이 추가되었는지 확인하세요.

    ![Picture 14](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-113.png)

5.  계산 결과를 검증하기 위해 `Report` 뷰로 전환하세요.

6.  `Page 1` 옆의 더하기(+) 아이콘을 선택하여 새 보고서 페이지를 만드세요.

    ![Picture 15](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-114.png)

7.  `Visualizations` 창에서 `matrix` 시각적 개체 유형을 선택하여 새 보고서 페이지에 추가하세요.

    *   **팁:** 각 아이콘 위로 마우스 커서를 가져가면 시각적 개체 유형을 설명하는 도구 설명이 나타납니다.

    ![Picture 51](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-150.png)

8.  `Data` 창의 `Date` 테이블 안에서 `Year` 필드를 `Rows` 영역(well/area)으로 드래그하세요.

    ![Picture 17](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-116.png)

9.  `Month` 필드를 `Year` 필드 바로 아래의 `Rows` 영역으로 드래그하세요.

10. matrix 시각적 개체의 오른쪽 상단(또는 시각적 개체 위치에 따라 하단)에 있는 갈래 모양의 이중 화살표 아이콘(Expand all down one level in the hierarchy)을 선택하세요.

    ![Picture 19](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-118.png)

11. 연도가 월로 확장되고, 월이 시간순이 아닌 알파벳순으로 정렬되는 것을 확인하세요.

    ![Picture 20](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-119.png)

    > **원리 이해:** 기본적으로 텍스트 값은 알파벳순으로, 숫자는 가장 작은 값에서 가장 큰 값 순으로, 날짜는 가장 이른 날짜에서 가장 늦은 날짜 순으로 정렬됩니다. `Month` 열은 텍스트 형식(예: "January", "February")으로 생성되었을 가능성이 높으므로 알파벳순으로 정렬된 것입니다.

12. `Month` 필드의 정렬 순서를 사용자 지정하기 위해 `Table` 뷰로 전환하세요.

13. `Date` 테이블에 `MonthKey` 열을 추가하세요.

    ```dax
    MonthKey =
    (YEAR('Date'[Date]) * 100) + MONTH('Date'[Date])
    ```

    > **DAX 수식 설명:** 이 수식은 각 연도/월 조합에 대한 숫자 값을 계산합니다. 예를 들어, 2017년 7월은 `(2017 * 100) + 7 = 201707`이 됩니다. 이 숫자 값은 시간 순서대로 증가하므로 정렬에 사용하기에 적합합니다.

14. `Table` 뷰에서 새 열이 숫자 값(예: 2017년 7월은 201707 등)을 포함하는지 확인하세요.

    ![Picture 21](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-120.png)

15. 다시 `Report` 뷰로 전환하세요. `Data` 창에서 `Month` 필드를 선택하세요 (열 머리글을 클릭).

16. `Column Tools` 상황별 리본 메뉴의 `Sort` 그룹에서 `Sort by Column`을 선택한 다음, `MonthKey`를 선택하세요.

    ![Picture 22](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-121.png)

    > **설명:** `Sort by Column` 기능은 특정 열(여기서는 `Month`)을 표시할 때, 실제 정렬 기준으로는 다른 열(여기서는 `MonthKey`)의 값을 사용하도록 설정합니다. 이를 통해 사용자는 의미 있는 이름(월 이름)을 보면서도 원하는 순서(시간순)로 정렬된 결과를 얻을 수 있습니다.

17. matrix 시각적 개체에서 이제 월이 시간순으로 올바르게 정렬되었는지 확인하세요.

    ![Picture 23](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-122.png)

---

**Date 테이블 완성하기 (Complete the Date table)**

이 작업에서는 열을 숨기고 계층(hierarchy)을 만들어 `Date` 테이블 디자인을 완료합니다. 그런 다음 `Sales` 및 `Targets` 테이블과의 관계를 만듭니다.

1.  `Model` 뷰로 전환하세요. `Date` 테이블에서 `MonthKey` 열을 숨기세요 (`Is Hidden` 속성을 `Yes`로 설정).

    > **주석:** `MonthKey` 열은 `Month` 열의 정렬을 위한 보조적인 역할만 하므로, 최종 사용자가 보고서에서 직접 사용할 필요가 없습니다. 숨겨서 `Data` 창을 깔끔하게 유지합니다.

2.  `Data` 창(오른쪽)에서 `Date` 테이블을 선택하고, `Year` 열을 마우스 오른쪽 버튼으로 클릭한 다음 `Create hierarchy`를 선택하세요.

3.  새로 생성된 계층(hierarchy)의 이름을 마우스 오른쪽 버튼으로 클릭하고 `Rename`을 선택하여 `Fiscal`로 변경하세요.

4.  `Data` 창에서 다음 두 필드를 선택하고, 마우스 오른쪽 버튼을 클릭한 후 `Add to hierarchy` -> `Fiscal`을 선택하여 `Fiscal` 계층에 추가하세요:
    *   `Quarter`
    *   `Month`

    ![Picture 24](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-123.png)

    > **설명:** 계층(Hierarchy)은 관련된 열들을 논리적인 순서(예: 연도 -> 분기 -> 월)로 묶어주는 기능입니다. 시각적 개체에서 사용자가 드릴다운(drill-down) 기능을 통해 데이터를 더 상세하게 탐색할 수 있도록 도와줍니다.

5.  다음 두 개의 모델 관계(relationship)를 만드세요:
    *   `Date | Date` 와 `Sales | OrderDate` 연결
    *   `Date | Date` 와 `Targets | TargetMonth` 연결

    > **표기법 안내:** 실습에서는 필드를 참조할 때 `테이블 이름 | 필드 이름` 형식의 약식 표기법을 사용합니다. 예를 들어, `Sales | Unit Price`는 `Sales` 테이블의 `Unit Price` 필드를 의미합니다.

6.  다음 두 열을 숨기세요:
    *   `Sales | OrderDate`
    *   `Targets | TargetMonth`

    > **주석:** 이제 중앙 집중화된 `Date` 테이블이 있으므로, 원래의 날짜 열들(`OrderDate`, `TargetMonth`)은 숨겨서 사용자들이 항상 `Date` 테이블의 필드를 통해 날짜 관련 분석을 하도록 유도하는 것이 좋습니다. 이는 모델의 일관성을 유지하는 데 도움이 됩니다.

---

**Date 테이블 표시하기 (Mark the Date table)**

이 작업에서는 `Date` 테이블을 날짜 테이블로 공식 지정합니다.

1.  `Report` 뷰로 전환하세요. `Data` 창에서 `Date` 테이블 자체를 선택하세요 (테이블 이름을 클릭, 특정 필드가 아님).

2.  `Table Tools` 상황별 리본 메뉴의 `Calendars` 그룹에서 `Mark as Date Table`을 선택하세요.

3.  `Mark as date table` 창에서:
    *   `Mark as date table` 속성 슬라이더를 `Yes`로 켭니다.
    *   `Date column` 드롭다운 목록에서 `Date` 열을 선택합니다.
    *   `OK` (또는 `Save`)를 선택합니다.

    ![Mark as date table](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-125.png)

4.  Power BI Desktop 파일을 저장하세요.

    > **원리 이해:** `Mark as Date Table` 기능은 Power BI에게 이 테이블이 시간(날짜)을 정의하는 공식적인 테이블임을 알려줍니다. 이는 DAX의 내장된 시간 인텔리전스 함수들(예: `TOTALYTD`, `SAMEPERIODLASTYEAR` 등)이 올바르게 작동하기 위해 필수적입니다. 이 기능은 데이터 원본에 자체적인 날짜 차원 테이블이 없는 경우에 적합한 디자인 접근 방식입니다. 만약 데이터 웨어하우스를 사용하고 있다면, 데이터 모델에서 날짜 로직을 "재정의"하기보다는 해당 데이터 웨어하우스의 날짜 차원 테이블에서 날짜 데이터를 로드하는 것이 더 적절합니다.

---

**간단한 측정값 만들기 (Create simple measures)**

이 작업에서는 간단한 측정값(measure)을 만듭니다. 간단한 측정값은 주로 단일 열의 값을 집계하거나 테이블의 행 수를 계산합니다.

> **원리 이해:** 측정값(Measure)은 DAX 수식을 사용하여 정의되며, 보고서의 시각적 개체나 다른 계산에서 사용될 때 *동적으로* 계산됩니다. 계산된 열과 달리 측정값은 모델에 데이터를 저장하지 않으며, 대신 사용자의 상호작용(필터링, 슬라이싱 등)에 따라 실시간으로 컨텍스트(context)에 맞춰 결과를 계산합니다. 합계, 평균, 개수 등 집계 계산에 주로 사용됩니다.

1.  `Report` 뷰의 `Page 2`에서, `Data` 창의 `Sales | Unit Price` 필드를 matrix 시각적 개체로 드래그하세요.

    ![Picture 27](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-126.png)

2.  시각적 개체 필드 창(`Visualizations` 창 아래)의 `Values` 영역에서 `Unit Price`가 `Average of Unit Price` (단가의 평균)로 표시된 것을 확인하세요. `Unit Price` 옆의 아래쪽 화살표를 선택하여 사용 가능한 메뉴 옵션을 확인하세요.

    ![Picture 30](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-129.png)

    > **설명:** 숫자 형식의 열을 시각적 개체에 그냥 넣으면, 보고서 작성자는 보고서 디자인 시점에 해당 열 값을 어떻게 요약할지(합계, 평균, 개수 등 또는 요약 안 함) 결정할 수 있습니다. 하지만 이는 때때로 부적절한 보고로 이어질 수 있습니다. 예를 들어, 단가(Unit Price)를 합산하는 것은 의미가 없는 경우가 많습니다. 일부 데이터 모델러는 이런 우연에 맡기기보다는, 해당 열을 숨기고 대신 측정값에 정의된 집계 로직만 노출하는 것을 선호합니다. 이 실습에서도 이제 이 접근 방식을 따를 것입니다.

3.  측정값을 만들려면, `Data` 창에서 `Sales` 테이블을 마우스 오른쪽 버튼으로 클릭한 다음 `New Measure`를 선택하세요.

4.  수식 입력줄에 다음 측정값 정의를 추가하세요:

    ```dax
    Avg Price =
    AVERAGE(Sales[Unit Price])
    ```

    > **DAX 함수 설명:** `AVERAGE(테이블[열])` 함수는 지정된 열에 있는 모든 숫자의 산술 평균을 계산합니다.

5.  `Avg Price` 측정값을 matrix 시각적 개체에 추가하고, `Unit Price` 열과 동일한 결과(하지만 다른 서식)를 생성하는지 확인하세요.

6.  `Values` 영역에서 `Avg Price` 필드의 컨텍스트 메뉴(아래 화살표 클릭)를 열고, 집계 방식(aggregation technique)을 변경하는 것이 불가능하다는 것을 확인하세요.

    ![Picture 32](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-131.png)

    > **설명:** 측정값의 집계 동작은 DAX 수식 자체에 이미 정의되어 있으므로, 시각적 개체 수준에서 변경할 수 없습니다. 이는 데이터 모델러가 의도한 대로만 값이 집계되도록 보장하는 장점이 있습니다.

7.  `Snippets.txt` 파일의 정의를 사용하여 `Sales` 테이블에 다음 다섯 개의 측정값을 만드세요:
    *   `Median Price`
    *   `Min Price`
    *   `Max Price`
    *   `Orders`
    *   `Order Lines`

    ```dax
    // Median Price 예시
    Median Price = MEDIAN(Sales[Unit Price])

    // Min Price 예시
    Min Price = MIN(Sales[Unit Price])

    // Max Price 예시
    Max Price = MAX(Sales[Unit Price])

    // Orders 예시
    Orders = DISTINCTCOUNT(Sales[SalesOrderNumber])

    // Order Lines 예시
    Order Lines = COUNTROWS(Sales)
    ```

    > **DAX 함수 설명:**
    > *   `MEDIAN(테이블[열])`: 열 값들의 중간값을 반환합니다.
    > *   `MIN(테이블[열])`: 열 값들의 최소값을 반환합니다.
    > *   `MAX(테이블[열])`: 열 값들의 최대값을 반환합니다.
    > *   `DISTINCTCOUNT(테이블[열])`: 열에서 고유한(중복 제외) 값의 개수를 셉니다. `Orders` 측정값에서는 고유한 주문 번호의 개수를 세어 실제 주문 건수를 계산합니다.
    > *   `COUNTROWS(테이블)`: 테이블의 총 행 수를 셉니다. `Order Lines` 측정값에서는 `Sales` 테이블의 각 행이 주문의 한 라인(품목)에 해당하므로, 총 행 수를 세어 전체 주문 라인 수를 계산합니다.

8.  `Model` 뷰로 전환한 다음, 네 개의 가격 관련 측정값(`Avg Price`, `Max Price`, `Median Price`, `Min Price`)을 다중 선택하세요.

9.  선택된 측정값들에 대해 다음 요구 사항을 구성하세요:
    *   형식(Format)을 소수점 이하 두 자리(two decimal places)로 설정하세요. (`Measure Tools` 리본 메뉴 사용)
    *   `Pricing`이라는 이름의 표시 폴더(display folder)에 할당하세요 (`Properties` 창).

    ![Picture 33](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-132.png)

    > **주석:** 표시 폴더(Display Folder)는 `Data` 창에서 관련 측정값이나 열들을 그룹화하여 보여주는 기능입니다. 모델이 복잡해질수록 필드를 찾고 관리하기 쉽게 만들어 줍니다.

10. `Unit Price` 열을 숨기세요.

    이제 보고서 작성자는 `Unit Price` 열을 사용할 수 없습니다. 대신 모델에 추가한 가격 관련 측정값들을 사용해야 합니다. 이 디자인 접근 방식은 보고서 작성자가 가격을 부적절하게 집계(예: 합산)하는 것을 방지합니다.

11. `Order Lines` 와 `Orders` 측정값을 다중 선택한 다음, 다음 요구 사항을 구성하세요:
    *   형식(Format)에서 천 단위 구분 기호(thousands separator)를 사용하도록 설정하세요.
    *   `Counts`라는 이름의 표시 폴더에 할당하세요.

    ![Picture 36](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-135.png)

12. `Report` 뷰로 돌아와서, matrix 시각적 개체의 `Values` 영역에서 `Unit Price` 필드 옆의 `X`를 선택하여 제거하세요.

13. matrix 시각적 개체의 크기를 페이지 너비와 높이를 채우도록 늘리세요.

14. 다음 다섯 개의 측정값을 matrix 시각적 개체에 추가하세요:
    *   `Median Price`
    *   `Min Price`
    *   `Max Price`
    *   `Orders`
    *   `Order Lines`

15. 결과가 합리적으로 보이고 올바르게 서식이 지정되었는지 확인하세요.

    ![Picture 39](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-138.png)

---

**추가 측정값 만들기 (Create additional measures)**

이 작업에서는 더 복잡한 수식을 사용하는 측정값을 추가로 만듭니다.

1.  `Report` 뷰에서 `Page 1`을 선택하고 테이블 시각적 개체를 검토하여 `Target` 열의 합계를 확인하세요.

    ![Picture 41](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-140.png)

    > **관찰:** 현재 `Target` 열의 합계는 모든 판매원의 목표 금액을 단순히 더한 값일 것입니다. 전체 합계로서는 의미가 없을 수 있습니다.

2.  테이블 시각적 개체를 선택한 다음, `Visualizations` 창에서 `Target` 필드를 제거하세요.

3.  `Targets | Target` 열의 이름을 `Targets | TargetAmount`로 변경하세요.

    *   **팁:** `Data` 창에서 열 이름을 바꾸는 방법은 여러 가지가 있습니다: 열을 마우스 오른쪽 버튼으로 클릭하고 `Rename`을 선택하거나, 열 이름을 더블 클릭하거나, F2 키를 누르면 됩니다.

4.  `Targets` 테이블에 다음 측정값을 만드세요:

    ```dax
    Target =
    IF(
        HASONEVALUE('Salesperson (Performance)'[Salesperson]),
        SUM(Targets[TargetAmount])
    )
    ```

    > **DAX 함수 및 수식 설명:**
    > *   `HASONEVALUE(테이블[열])`: 지정된 열이 현재 필터 컨텍스트에서 단 하나의 고유한 값만 가지고 있는지 여부를 테스트합니다 (TRUE 또는 FALSE 반환). 테이블의 총합계 행 같은 경우에는 여러 판매원의 값이 포함되므로 FALSE를 반환하고, 각 판매원 행에서는 해당 판매원 한 명의 값만 있으므로 TRUE를 반환합니다.
    > *   `SUM(Targets[TargetAmount])`: `TargetAmount` 열의 합계를 계산합니다.
    > *   `IF(조건, 참일 때 결과, 거짓일 때 결과)`: `HASONEVALUE` 함수가 TRUE를 반환하면 (즉, 단일 판매원에 대한 컨텍스트이면) `TargetAmount`의 합계를 반환합니다. FALSE를 반환하면 (즉, 여러 판매원에 대한 컨텍스트, 예를 들어 총합계 행이면) 기본값인 `BLANK`를 반환합니다.
    > *   **결과:** 이 측정값은 각 개별 판매원의 목표 금액은 보여주지만, 여러 판매원의 목표 금액이 합산되어 의미 없는 값이 되는 총합계는 공백(BLANK)으로 표시합니다.

5.  `Target` 측정값의 형식을 소수점 이하 0자리(zero decimal places)로 지정하세요. (`Measure Tools` 리본 메뉴 사용)

6.  `TargetAmount` 열을 숨기세요. (`Data` 창에서 열을 마우스 오른쪽 버튼으로 클릭하고 `Hide` 선택)

7.  `Target` 측정값을 테이블 시각적 개체에 추가하세요.

8.  이제 `Target` 열의 총합계가 `BLANK`로 표시되는 것을 확인하세요.

    ![Picture 43](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-142.png)

9.  `Snippets.txt` 파일의 정의를 사용하여 `Targets` 테이블에 다음 두 개의 측정값을 만드세요:
    *   `Variance`
    *   `Variance Margin`

    ```dax
    // Variance 예시 (실제 판매액 측정값이 필요하며, 여기서는 Sales 테이블의 관련 측정값을 사용한다고 가정)
    // Variance = [Total Sales] - [Target]
    // Variance Margin 예시
    // Variance Margin = DIVIDE([Variance], [Target])
    ```
    *(주: 위 예시 수식은 가정이며, 실제 실습에서는 `Sales` 테이블에 관련 매출 측정값이 정의되어 있어야 합니다. 실습 파일의 정확한 수식을 사용하세요. `DIVIDE` 함수는 0으로 나누는 오류를 방지하는 데 유용합니다.)*

10. `Variance` 측정값의 형식을 소수점 이하 0자리로 지정하세요.

11. `Variance Margin` 측정값의 형식을 소수점 이하 두 자리의 백분율(percentage with two decimal places)로 지정하세요.

12. `Variance` 및 `Variance Margin` 측정값을 테이블 시각적 개체에 추가하세요.

13. 모든 열과 행이 보이도록 테이블 시각적 개체의 크기를 조절하세요.

    ![Picture 44](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-143.png)

    > **관찰:** 모든 판매원이 목표를 달성하지 못한 것처럼 보일 수 있지만, 이 테이블 시각적 개체는 아직 특정 기간으로 필터링되지 않았다는 점을 기억하세요.

14. `Data` 창의 오른쪽 상단 모서리에 있는 축소(`<<`) 및 확장(`>>`) 버튼을 차례로 클릭하여 창을 닫았다가 다시 여세요.

    > **설명:** 창을 접었다 펴면 내용이 새로고침되어 최신 상태로 정렬됩니다.

15. 이제 `Targets` 테이블이 목록의 맨 위에 나타나는 것을 확인하세요.

    ![Picture 46](https://microsoftlearning.github.io/mslearn-fabric/Instructions/Labs/14-create-dax-calculations_files/image-145.png)

    > **원리 이해:** Power BI는 보이는 측정값만 포함하고 모든 열이 숨겨진 테이블을 자동으로 목록 상단에 표시하는 경향이 있습니다. 이는 사용자가 측정값에 더 쉽게 접근할 수 있도록 돕기 위함입니다.

---

**실습 완료 (Lab complete)**

축하합니다! Power BI Desktop에서 DAX를 사용하여 계산된 테이블, 계산된 열, 그리고 다양한 측정값을 성공적으로 만들었습니다. 이 과정을 통해 데이터 모델을 더욱 강력하고 유연하게 만드는 방법을 배우셨기를 바랍니다.
