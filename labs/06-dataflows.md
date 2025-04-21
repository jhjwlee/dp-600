
## **Microsoft Fabric에서 Dataflows (Gen2) 생성 및 사용**

Microsoft Fabric에서 Dataflows (Gen2)는 다양한 데이터 원본에 연결하고 Power Query Online에서 변환을 수행합니다. 그런 다음 Data Pipelines에서 사용하여 데이터를 lakehouse 또는 다른 분석 저장소로 수집하거나 Power BI 보고서용 데이터 세트를 정의하는 데 사용할 수 있습니다.

이 랩은 Dataflows (Gen2)의 다양한 요소를 소개하도록 설계되었으며, 엔터프라이즈에 존재할 수 있는 복잡한 솔루션을 만드는 것은 아닙니다. 이 랩은 완료하는 데 약 30분이 소요됩니다.

**참고:** 이 실습을 완료하려면 Microsoft Fabric 평가판이 필요합니다.

**Workspace 생성**

Fabric에서 데이터 작업을 시작하기 전에 Fabric 평가판이 활성화된 Workspace를 만듭니다.

1.  브라우저에서 Microsoft Fabric 홈페이지 (`https://app.fabric.microsoft.com/home?experience=fabric`)로 이동하고 Fabric 자격 증명으로 로그인합니다.
2.  왼쪽 메뉴 모음에서 **Workspaces** (아이콘 모양 🗇)를 선택합니다.
3.  Fabric 용량(Trial, Premium 또는 Fabric)을 포함하는 라이선싱 모드를 선택하여 원하는 이름으로 새 Workspace를 만듭니다.
4.  새 Workspace가 열리면 비어 있어야 합니다.

    *Fabric의 빈 Workspace 스크린샷.*

**Lakehouse 생성**

이제 Workspace가 있으므로 데이터를 수집할 데이터 lakehouse를 만들 차례입니다.

1.  왼쪽 메뉴 모음에서 **Create**를 선택합니다. **New** 페이지의 **Data Engineering** 섹션에서 **Lakehouse**를 선택합니다. 원하는 고유한 이름을 지정합니다.

**참고:** **Create** 옵션이 사이드바에 고정되어 있지 않으면 먼저 줄임표(…) 옵션을 선택해야 합니다.

2.  약 1분 후 새 빈 lakehouse가 생성됩니다.

    *새 lakehouse.*

**데이터 수집을 위한 Dataflow (Gen2) 생성**

이제 lakehouse가 있으므로 일부 데이터를 수집해야 합니다. 한 가지 방법은 ETL(추출, 변환, 로드) 프로세스를 캡슐화하는 dataflow를 정의하는 것입니다.

1.  Workspace의 홈페이지에서 **Get data** > **New Dataflow Gen2**를 선택합니다. 몇 초 후 새 dataflow를 위한 Power Query 편집기가 다음과 같이 열립니다.
    *새 dataflow.*
2.  **Import from a Text/CSV file**을 선택하고 다음 설정을 사용하여 새 데이터 원본을 만듭니다.
    *   **Link to file:** 선택됨
    *   **File path or URL:** `https://raw.githubusercontent.com/MicrosoftLearning/dp-data/main/orders.csv`
    *   **Connection:** `Create new connection`
    *   **data gateway:** `(none)`
    *   **Authentication kind:** `Anonymous`
3.  **Next**를 선택하여 파일 데이터를 미리 보고 **Create**를 선택하여 데이터 원본을 만듭니다. Power Query 편집기는 데이터 원본과 데이터 서식 지정을 위한 초기 쿼리 단계 세트를 다음과 같이 보여줍니다.
    *Power Query 편집기의 쿼리.*
4.  도구 모음 리본에서 **Add column** 탭을 선택합니다. 그런 다음 **Custom column**을 선택하고 새 열을 만듭니다.
5.  **New column name**을 `MonthNo`로 설정하고, **Data type**을 `Whole Number`로 설정한 다음, 다음 수식을 추가합니다: `Date.Month([OrderDate])` - 다음과 같이 표시됩니다.

    *Power Query 편집기의 Custom column.*

6.  **OK**를 선택하여 열을 만들고 사용자 지정 열을 추가하는 단계가 쿼리에 어떻게 추가되는지 확인합니다. 결과 열은 데이터 창에 표시됩니다.
    *사용자 지정 열 단계가 있는 쿼리.*

**팁:** 오른쪽의 **Query Settings** 창에서 **Applied Steps**에 각 변환 단계가 포함되어 있음을 확인하세요. 하단에서 **Diagram flow** 버튼을 토글하여 단계의 시각적 다이어그램을 켤 수도 있습니다.

단계는 위아래로 이동하거나, 기어 아이콘을 선택하여 편집할 수 있으며, 각 단계를 선택하여 미리 보기 창에서 변환이 적용되는 것을 볼 수 있습니다.

7.  `OrderDate` 열의 데이터 유형이 `Date`로 설정되고 새로 생성된 `MonthNo` 열의 데이터 유형이 `Whole Number`로 설정되었는지 확인하고 확정합니다.

**Dataflow를 위한 데이터 대상 추가**

1.  도구 모음 리본에서 **Home** 탭을 선택합니다. 그런 다음 **Add data destination** 드롭다운 메뉴에서 **Lakehouse**를 선택합니다.

**참고:** 이 옵션이 회색으로 표시되면 이미 데이터 대상이 설정되어 있을 수 있습니다. Power Query 편집기 오른쪽의 **Query settings** 창 하단에서 데이터 대상을 확인하세요. 대상이 이미 설정된 경우 기어 아이콘을 사용하여 변경할 수 있습니다.

2.  **Connect to data destination** 대화 상자에서 연결을 편집하고 Power BI 조직 계정을 사용하여 로그인하여 dataflow가 lakehouse에 액세스하는 데 사용하는 ID를 설정합니다.

    *데이터 대상 구성 페이지.*

3.  **Next**를 선택하고 사용 가능한 Workspace 목록에서 Workspace를 찾아 이 실습 시작 시 생성한 lakehouse를 선택합니다. 그런 다음 `orders`라는 새 테이블을 지정합니다.

    *데이터 대상 구성 페이지.*

4.  **Next**를 선택하고 **Choose destination settings** 페이지에서 **Use automatic settings** 옵션을 비활성화하고, **Append**를 선택한 다음 **Save settings**를 선택합니다.

**참고:** 데이터 유형 업데이트는 Power Query 편집기를 사용하는 것이 좋지만, 원하는 경우 이 페이지에서도 수행할 수 있습니다.

*데이터 대상 설정 페이지.*

5.  메뉴 모음에서 **View**를 열고 **Diagram view**를 선택합니다. Power Query 편집기의 쿼리에서 Lakehouse 대상이 아이콘으로 표시되는 것을 확인합니다.

    *Lakehouse 대상이 있는 쿼리.*

6.  **Publish**를 선택하여 dataflow를 게시합니다. 그런 다음 Workspace에서 `Dataflow 1` dataflow가 생성될 때까지 기다립니다.

**Pipeline에 Dataflow 추가**

Dataflow를 파이프라인의 활동으로 포함할 수 있습니다. 파이프라인은 데이터 수집 및 처리 활동을 오케스트레이션하는 데 사용되며, 단일 예약된 프로세스에서 dataflow를 다른 종류의 작업과 결합할 수 있습니다. 파이프라인은 Data Factory 경험을 포함한 몇 가지 다른 경험에서 생성할 수 있습니다.

1.  Fabric 지원 Workspace에서 여전히 **Data Engineering** 경험에 있는지 확인합니다. **+ New item** > **Data pipeline**을 선택한 다음, 메시지가 표시되면 `Load data`라는 새 파이프라인을 만듭니다.
2.  파이프라인 편집기가 열립니다.

    *빈 데이터 파이프라인.*

**팁:** **Copy Data wizard**가 자동으로 열리면 닫으세요!

3.  **Add pipeline activity**를 선택하고 파이프라인에 **Dataflow** 활동을 추가합니다.
4.  새 `Dataflow1` 활동이 선택된 상태에서 **Settings** 탭의 **Dataflow** 드롭다운 목록에서 `Dataflow 1`(이전에 만든 dataflow)을 선택합니다.

    *Dataflow 활동이 있는 파이프라인.*

5.  **Home** 탭에서 🖫 (**Save**) 아이콘을 사용하여 파이프라인을 저장합니다.
6.  ▷ **Run** 버튼을 사용하여 파이프라인을 실행하고 완료될 때까지 기다립니다. 몇 분 정도 걸릴 수 있습니다.

    *성공적으로 완료된 dataflow가 있는 파이프라인.*

7.  왼쪽 가장자리의 메뉴 모음에서 lakehouse를 선택합니다.
8.  **Tables**의 `...` 메뉴에서 **refresh**를 선택합니다. 그런 다음 **Tables**를 확장하고 dataflow에 의해 생성된 `orders` 테이블을 선택합니다.

    *Dataflow에 의해 로드된 테이블.*

**팁:** Power BI Desktop에서는 Power BI dataflows (Legacy) 커넥터를 사용하여 dataflow로 수행한 데이터 변환에 직접 연결할 수 있습니다.

추가 변환을 수행하고, 새 데이터 세트로 게시하고, 특수 데이터 세트를 위해 의도된 대상에게 배포할 수도 있습니다.

*Power BI 데이터 원본 커넥터*

**리소스 정리**

Microsoft Fabric에서 dataflow 탐색을 마쳤으면 이 실습을 위해 만든 Workspace를 삭제할 수 있습니다.

1.  브라우저에서 Microsoft Fabric으로 이동합니다.
2.  왼쪽 막대에서 Workspace 아이콘을 선택하여 포함된 모든 항목을 봅니다.
3.  **Workspace settings**를 선택하고 **General** 섹션에서 아래로 스크롤하여 **Remove this workspace**를 선택합니다.
4.  **Delete**를 선택하여 Workspace를 삭제합니다.

---
