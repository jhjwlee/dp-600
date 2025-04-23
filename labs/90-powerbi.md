네, 그럼 Microsoft Fabric Analytics Engineer (DP-600) 과정의 슬라이드 내용을 한국어로 번역하고, Power BI 전문가로서 필요한 부연 설명을 추가하여 친절하게 안내해 드리겠습니다. 주요 용어와 메뉴는 영어 그대로 사용하겠습니다.

---

**Page 1**

![Microsoft Logo](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**DP-600: Microsoft Fabric Analytics Engineer**

Copyright Microsoft Corporation. All rights reserved.

---

**Page 2**

![슬라이드 배경 이미지](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**Power BI의 확장성(Scalability) 이해하기**

Copyright Microsoft Corporation. All rights reserved.

> **주석:** 확장성(Scalability)은 데이터 양이 증가하거나 사용자 수가 늘어날 때 시스템(여기서는 Power BI 모델 및 보고서)이 성능 저하 없이 원활하게 처리 능력을 확장할 수 있는 능력을 의미합니다. 효과적인 분석 환경 구축에 매우 중요한 요소입니다.

---

**Page 3**

![학습 목표 아이콘](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**학습 목표 (Learning objectives)**

*   확장 가능한 **semantic models** 구축의 중요성 설명하기
*   Power BI 데이터 모델링 모범 사례(best practices) 구현하기
*   Power BI 대용량 **semantic model** 저장소 형식(**large semantic model storage format**) 사용하기

Copyright Microsoft Corporation. All rights reserved.

---

**Page 4**

![엔터프라이즈 데이터 정의](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**엔터프라이즈 규모 데이터(Enterprise-scale data) 정의**

*   **Massive (대규모):** 데이터 양이 매우 큼
*   **Complex (복잡성):** 데이터 구조나 관계가 복잡함
*   **Diverse (다양성):** 다양한 형태와 소스의 데이터를 포함함
*   **Rapid (신속성):** 데이터 생성 및 변경 속도가 빠름
*   **Valuable (가치성):** 비즈니스 의사결정에 중요한 가치를 지님

Copyright Microsoft Corporation. All rights reserved.

> **주석:** 엔터프라이즈 환경의 데이터는 이러한 특징들을 가지며, Power BI 모델 설계 시 이런 점들을 고려하여 확장성을 확보해야 합니다.

---

**Page 5**

![확장성이란 무엇이며 왜 중요한가?](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**확장성(Scalability)이란 무엇이며 왜 중요한가?**

*   **유연성 (Flexibility)**
    *   모델(Models)은 변화를 수용할 수 있어야 합니다.
    *   > **설명:** 비즈니스 요구사항 변경, 데이터 소스 추가/변경 등에 유연하게 대응할 수 있어야 합니다.
*   **데이터 증가 (Data growth)**
    *   모델은 수용 가능한 **report performance** (보고서 성능)를 유지하면서 데이터 볼륨 증가를 처리할 수 있어야 합니다.
    *   > **설명:** 데이터가 계속 쌓여도 보고서 로딩 속도나 상호작용 속도가 크게 느려지지 않아야 합니다.
*   **복잡성 감소 (Reduced complexity)**
    *   확장성을 염두에 두고 구축된 모델은 덜 복잡하고 관리하기 쉽습니다.
    *   > **설명:** 잘 설계된 모델은 이해하기 쉽고 유지보수가 용이하여 장기적으로 효율적입니다.

Copyright Microsoft Corporation. All rights reserved.

---

**Page 6**

![확장성을 위한 설계 방법](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**확장성(Scalability)을 위해 어떻게 설계해야 하는가?**

*   데이터 모델링 모범 사례(**data modeling best practices**)를 염두에 두고 구축합니다.
*   **Direct Lake / Import / Direct Query** 에 대한 요구 사항을 결정합니다.
    *   > **주석:** 데이터 연결 방식(Import, DirectQuery, Direct Lake)은 성능과 실시간성에 큰 영향을 미치므로, 데이터의 크기, 업데이트 주기, 원본 시스템의 성능 등을 고려하여 최적의 방식을 선택해야 합니다.
*   대용량 **semantic model** 저장소 기능(**large semantic model storage feature**)을 활성화합니다.

Copyright Microsoft Corporation. All rights reserved.

---

**Page 7**

![데이터 모델링 모범 사례 구현](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**데이터 모델링 모범 사례 구현 (Implement data modeling best practices)**

*   **올바른 모델 프레임워크 선택:**
    *   **Import:** 데이터를 Power BI 모델 내부에 복사하여 저장 (빠른 쿼리 성능)
    *   **DirectQuery:** 쿼리 시 실시간으로 원본 데이터에 직접 접근 (대용량 데이터, 실시간 데이터에 적합)
    *   **Direct Lake:** Fabric Lakehouse/Warehouse의 데이터를 Import의 성능과 DirectQuery의 실시간성을 결합한 방식으로 접근 (Fabric 환경에 특화)
*   **데이터 모델링 모범 사례 구현:**
    *   데이터가 Power BI에 도달하기 전에, 가능한 한 **upstream** (상위 단계, 예: 데이터베이스, 데이터 웨어하우스)에서 최대한 많은 데이터 준비 작업을 수행합니다.
    *   > **설명:** 데이터 정제, 변환 등의 작업을 Power Query보다는 원본 데이터 시스템에서 미리 처리하면 Power BI 모델의 새로 고침 시간과 성능을 개선할 수 있습니다.

Copyright Microsoft Corporation. All rights reserved.

---

**Page 8**

![대용량 semantic model 기능 사용](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**대용량 semantic model 기능 사용 (Use the large semantic model feature)**

*   **대용량 데이터셋 저장소 형식 활성화 (Enable large dataset storage format):**
    *   **Semantic model** 크기는 **capacity** 크기 또는 관리자(admin)가 설정한 최대 크기에 의해서만 제한됩니다.
        *   > **주석:** Power BI Premium 용량에서는 기본 10GB 크기 제한을 넘어 훨씬 큰 모델을 호스팅할 수 있게 해주는 기능입니다. 'Capacity'는 Power BI Premium 구독에서 제공하는 전용 리소스 할당 단위를 의미합니다.
    *   모든 **Premium SKU** (Stock Keeping Unit, 제품 단위)에 대해 활성화할 수 있습니다.
        *   > **주석:** P SKU, EM SKU, A SKU 등 다양한 Premium 라이선스 유형에서 이 기능을 사용할 수 있습니다.

*   **(이미지 내용)**
    *   `Large semantic model storage format` 설정 화면
    *   "대부분의 Premium capacity에서 대용량 semantic model 저장소 형식을 사용하면 성능을 향상시킬 수 있습니다. 자세히 알아보기"
    *   `On` (켜짐) 토글
    *   `Apply` (적용), `Discard` (취소) 버튼

Copyright Microsoft Corporation. All rights reserved.

---

**Page 9**

![대용량 Semantic Model 구성](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**대용량 Semantic Models 구성 (Configure Large Semantic Models)**

*   **Workspace** (작업 영역)에 생성된 모든 **semantic models**에 대해 대용량 데이터셋 저장소 형식을 활성화할 수도 있습니다.
    *   **Workspace settings** (작업 영역 설정)에서 **workspace**에 생성된 모든 **semantic models**에 대한 기본 저장소 형식을 설정할 수 있습니다.

*   **(이미지 내용)**
    *   `Default storage format` (기본 저장소 형식) 설정
    *   `Small semantic model storage format` (소용량 semantic model 저장소 형식) 옵션
    *   `Large semantic model storage format` (대용량 semantic model 저장소 형식) 옵션 (선택됨)
    *   "semantic model 저장소 형식에 대해 자세히 알아보기" 링크
    *   `Apply` (적용), `Cancel` (취소) 버튼

Copyright Microsoft Corporation. All rights reserved.

---

**Page 10**

![실습 아이콘](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**실습 (Exercise)**

**Star schema model 생성하기**

**30 분**

Copyright Microsoft Corporation. All rights reserved.

> **주석:** Star Schema는 데이터 웨어하우징에서 널리 사용되는 모델링 방식으로, 하나의 중심 팩트 테이블(Fact Table, 측정값 포함)과 여러 개의 주변 차원 테이블(Dimension Table, 분석 기준 속성 포함)로 구성됩니다. Power BI 모델링의 기본이자 권장 방식입니다.

---

**Page 11**

![지식 점검 아이콘](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**지식 점검 (Knowledge check)**

1.  **확장성(scalability)을 위한 semantic models 설계 시 데이터 변환(data transformation)에 가장 효율적인 접근 방식은 무엇인가요?**
    *   항상 Power BI의 **Power Query**를 사용하여 데이터 변환을 수행해야 합니다.
    *   ✅ 데이터 변환은 Power BI에 도달하기 전에 가능한 한 **source** (원본)에 가깝게 수행해야 합니다.
    *   데이터 변환은 데이터가 모델에 수집(ingested)되고 로드된 후 **DAX**를 사용하여 수행해야 합니다.
2.  **다음 Power BI 데이터 모델링 모범 사례 중 DirectQuery 모델에만 관련된 것은 무엇인가요?**
    *   ✅ 관계에서 **assume referential integrity** (참조 무결성 가정) 속성을 사용하여 무결성을 강제하도록 관계를 설정합니다.
        *   > **주석:** '참조 무결성 가정' 옵션은 DirectQuery 성능 최적화 기법 중 하나입니다. 원본 데이터의 관계 무결성이 보장될 때 사용하면 더 효율적인 쿼리(Inner Join 사용)를 생성할 수 있습니다. Import 모드에서는 이 설정이 직접적인 영향을 주지 않습니다.
    *   넓은 테이블(wide tables) 대신 **star schema**를 사용합니다. (Import 모드에도 중요)
    *   Power BI Desktop에서 **auto date/time** 기능을 비활성화합니다. (Import 모드에도 중요)
3.  **Large semantic model storage format (대용량 semantic model 저장소 형식)이 활성화된 semantic model의 크기 제한은 무엇에 의해 결정되나요?**
    *   Power BI 10 GB 크기 제한.
    *   ✅ **Fabric capacity size** (Fabric 용량 크기) 또는 관리자(administrator)가 설정한 최대 크기.
    *   Large dataset storage format이 활성화되면 크기 제한이 없습니다. (제한은 여전히 존재함)

Copyright Microsoft Corporation. All rights reserved.

---

**Page 12**

![요약 아이콘](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**요약 (Recap)**

**이번 섹션에서 우리는 다음을 배웠습니다:**

*   Power BI에서 **Scalability** (확장성) 및 대용량 데이터 작업은 달성 가능합니다.
*   올바른 **model framework** (모델 프레임워크), 적절한 **data model** (데이터 모델), 그리고 **large dataset storage** (대용량 데이터셋 저장소)와 같은 **Fabric features** (Fabric 기능)를 사용하면 Power BI에서 성능 좋은 엔터프라이즈 보고(enterprise reporting)가 가능합니다.

Copyright Microsoft Corporation. All rights reserved.

---

**Page 13**

![추가 정보 아이콘](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**추가 정보 (Further reading)**

*   **Power BI의 확장성 이해하기 (Understand scalability in Power BI)**
    *   [`https://aka.ms/scalable-models`](https://aka.ms/scalable-models)
*   **Power BI 사용 시나리오: 고급 데이터 모델 관리 (Power BI usage scenarios: advanced data model management)**
    *   [`https://aka.ms/model-management`](https://aka.ms/model-management)

Copyright Microsoft Corporation. All rights reserved.

---

**Page 14**

![슬라이드 배경 이미지](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**모델 관계 만들기 (Create model relationships)**

Copyright Microsoft Corporation. All rights reserved.

---

**Page 15**

![학습 목표 아이콘](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**학습 목표 (Learning objectives)**

*   **Relationships** (관계) 만들기.
*   **DAX relationship functions** (DAX 관계 함수) 사용하기.
*   **Relationship evaluation** (관계 평가) 이해하기.

Copyright Microsoft Corporation. All rights reserved.

---

**Page 16**

![모델 관계 이해](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**모델 관계 이해하기 (Understand model relationships)**

**(이미지: Category, Product, Sales, Year 테이블 간의 관계를 보여주는 다이어그램)**

*   Category (1) -> (*) Product
*   Product (1) -> (*) Sales
*   Year (1) -> (*) Sales

> **주석:** 이 다이어그램은 기본적인 **Star Schema** 구조를 보여줍니다. `Sales` 테이블이 팩트 테이블(Fact Table) 역할을 하고, `Category`, `Product`, `Year` 테이블이 차원 테이블(Dimension Table) 역할을 합니다. 관계는 일반적으로 차원 테이블의 기본 키(1 쪽)에서 팩트 테이블의 외래 키(* 쪽)로 설정됩니다.

Copyright Microsoft Corporation. All rights reserved.

---

**Page 17**

![Star Schema 디자인 원칙 적용](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**Star schema 디자인 원칙 적용하기 (Apply star schema design principles)**

**(이미지: Power BI Model view에서 구현된 Star Schema 예시)**

*   Date, State, Region, Product (Dimension Tables)
*   Sales (Fact Table)

> **설명:** Power BI 모델 뷰에서 테이블들을 배치하고 관계를 설정하여 Star Schema를 시각적으로 구현합니다. 차원 테이블은 분석의 기준(예: 시간, 지역, 제품)을 제공하고, 팩트 테이블은 측정하려는 값(예: 판매량, 금액)을 포함합니다.

© Copyright Microsoft Corporation. All rights reserved.

---

**Page 18**

![관계 설정](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**관계 설정하기 (Set up relationships)**

*   **관계 Cardinality (카디널리티) 및 Cross-filter direction (교차 필터 방향) 설정**

    | Cardinality type (카디널리티 유형)         | Cross filter options (교차 필터 옵션) |
    | :----------------------------------------- | :------------------------------------ |
    | **One-to-many** (일대다) (또는 many-to-one) | Single (단일)                         |
    |                                            | Both (양방향)                         |
    | **One-to-one** (일대일)                    | Both (양방향)                         |
    | **Many-to-many** (다대다)                  | Single (Table1 to Table2)             |
    |                                            | Single (Table2 to Table1)             |
    |                                            | Both (양방향)                         |

*   **(이미지: Sales(*) 와 Product(1) 테이블 간의 일대다 관계 설정 예시)**

> **설명:**
> *   **Cardinality:** 관계를 맺는 두 테이블 간의 데이터 행 연결 방식을 정의합니다 (예: 하나의 제품은 여러 판매 기록을 가질 수 있음 -> 일대다). 대부분의 경우 '일대다'가 권장됩니다.
> *   **Cross-filter direction:** 필터가 어느 테이블에서 다른 테이블로 전파될지 방향을 결정합니다. 'Single'은 주로 차원 테이블에서 팩트 테이블로 필터링할 때 사용하며, 'Both'(양방향)는 특정 시나리오에서 필요하지만 성능 문제를 유발하거나 모델의 복잡성을 높일 수 있어 신중하게 사용해야 합니다.

Copyright Microsoft Corporation. All rights reserved.

---

**Page 19**

![DAX 관계 함수 사용](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**DAX 관계 함수 사용하기 (Use DAX relationship functions)**

모델 관계와 관련된 여러 **DAX** 함수가 있습니다:

*   **RELATED:** 관계의 '일(1)' 쪽 테이블에서 관련 값을 가져옵니다. (계산된 열에서 주로 사용)
*   **RELATEDTABLE:** 관계의 '다(*)' 쪽 테이블에서 관련 행들의 테이블을 가져옵니다.
*   **USERELATIONSHIP:** `CALCULATE` 함수 내에서 특정 계산 동안 비활성 관계를 활성화합니다. (역할 수행 차원에 유용)
*   **CROSSFILTER:** `CALCULATE` 함수 내에서 관계의 교차 필터 방향을 수정합니다.
*   **COMBINEVALUES:** DirectQuery에서 다중 열 관계를 지원하기 위해 두 개 이상의 텍스트 문자열을 결합합니다.
*   **TREATAS:** 테이블 표현식의 결과를 다른 테이블의 열에 필터로 적용하여, 물리적인 관계가 없는 테이블 간에 가상 관계(virtual relationship)를 만듭니다.
*   **Parent and child functions:** 조직 구조나 계정 차트와 같은 부모-자식 계층 구조를 처리하는 함수들입니다 (예: `PATH`, `PATHITEM`).

Copyright Microsoft Corporation. All rights reserved.

---

**Page 20**

![관계 평가 이해](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**관계 평가 이해하기 (Understand relationship evaluation)**

다음을 통해 관계 평가를 이해할 수 있습니다:

*   **Regular relationships (정규 관계):** 양쪽 테이블의 키 열 중 하나 이상이 고유한 값을 가지는 관계 (일대다, 일대일). 성능이 가장 좋습니다.
*   **Limited relationships (제한된 관계):** 양쪽 테이블 모두 키 열에 고유한 값이 없는 관계 (다대다). 특정 상황에서 필요하지만, 성능 및 동작 방식에 제약이 있을 수 있습니다.
*   **Precedence rules (우선 순위 규칙):** 여러 관계 경로가 존재할 때 필터 전파 경로를 결정하는 규칙입니다.
*   **Performance preference (성능 선호도):** Power BI 엔진은 가능한 한 정규 관계를 사용하려고 시도합니다.

> **주석:** Power BI가 쿼리를 처리할 때 관계를 어떻게 해석하고 사용하는지 이해하는 것은 복잡한 모델의 성능 최적화 및 정확한 결과 도출에 중요합니다.

Copyright Microsoft Corporation. All rights reserved.

---

**Page 21**

![실습 아이콘](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**실습 (Exercise)**

**모델 관계 작업하기 (Work with model relationships)**

**45 분**

Copyright Microsoft Corporation. All rights reserved.

---

**Page 22**

![지식 점검 아이콘](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**지식 점검 (Knowledge Check) (1/2)**

1.  Belle은 **Adventure Works**의 **data modeler**이며 **Fabric data warehouse**용 모델을 개발 중입니다. 이 모델에는 제품을 저장하는 테이블과 해당 제품의 판매를 저장하는 다른 테이블이 포함됩니다. Belle은 두 테이블을 연결하기 위해 관계를 추가합니다. 최적의 모델을 달성하기 위해 Belle이 설정해야 하는 **cardinality** 유형은 무엇인가요?
    *   **One-to-one.** (일대일)
    *   ✅ **One-to-many.** (일대다)
    *   **Many-to-many.** (다대다)
    *   > **설명:** 일반적으로 제품 테이블(하나의 제품)과 판매 테이블(해당 제품의 여러 판매 기록) 사이에는 일대다 관계가 가장 적합하고 효율적입니다.
2.  Akira는 **Adventure Works**의 **data modeler**이며 **Fabric data warehouse**용 모델을 개발 중입니다. 이 모델에는 제품을 저장하는 테이블과 제품 카테고리의 월별 목표 판매량을 저장하는 다른 테이블이 포함됩니다. Akira는 두 테이블을 연결하기 위해 관계를 추가합니다. 최적의 모델을 달성하기 위해 Akira가 설정해야 하는 **cardinality** 유형은 무엇인가요?
    *   **One-to-one.** (일대일)
    *   **One-to-many.** (일대다)
    *   ✅ **Many-to-many.** (다대다)
    *   > **설명:** 제품 테이블에는 여러 제품이 있고, 목표 테이블에는 월별 카테고리 목표가 있습니다. 하나의 제품은 하나의 카테고리에 속하고, 하나의 카테고리는 여러 월별 목표를 가질 수 있습니다. 만약 제품 테이블에 'Category' 열이 있고 목표 테이블에도 'Category' 열이 있다면, 'Category'를 기준으로 연결할 때 다대다 관계가 형성될 수 있습니다 (하나의 카테고리에 여러 제품, 하나의 카테고리에 여러 월별 목표). (단, 더 나은 모델링을 위해 별도의 Category 차원 테이블을 사용하는 것이 좋습니다.) 이 질문의 의도는 아마도 제품 테이블과 목표 테이블 간 직접 연결 시 다대다 관계가 필요할 수 있음을 시사하는 것 같습니다.

Copyright Microsoft Corporation. All rights reserved.

---

**Page 23**

![지식 점검 아이콘](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**지식 점검 (Knowledge Check) (2/2)**

3.  Margaret은 **Adventure Works**의 **data modeler**이며 판매 모델에 **measure** (측정값)를 추가하고 있습니다. 이 측정값을 평가할 때, 관련 없는 테이블(unrelated table)에 이미 적용된 필터로 필터링되어야 합니다. Margaret이 **virtual relationship** (가상 관계)을 만들기 위해 사용해야 하는 **DAX function**은 무엇인가요?
    *   `RELATEDTABLE`
    *   ✅ `TREATAS`
    *   `USERELATIONSHIP`
    *   > **설명:** `TREATAS` 함수는 물리적인 관계가 없는 테이블 간에 필터 컨텍스트를 전달하여 가상 관계처럼 동작하게 만듭니다. `USERELATIONSHIP`은 이미 존재하는 비활성 관계를 활성화하는 데 사용됩니다.

Copyright Microsoft Corporation. All rights reserved.

---

**Page 24**

![요약 아이콘](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**요약 (Recap)**

**이번 섹션에서 우리는 다음을 배웠습니다:**

*   가장 일반적인 **model relationship** (모델 관계)은 **one-to-many** (일대다) 관계입니다.
*   모델 관계와 설정 방법을 숙달하면 더 복잡하고 효율적인 모델을 개발할 수 있습니다.
*   모델 디자인 시점에 관계를 추가하는 동안, **model calculations** (모델 계산, 즉 DAX)을 통해 관계를 탐색하고, 관계 동작을 수정하고, 심지어 **virtual relationships** (가상 관계)를 만들 수도 있습니다.

Copyright Microsoft Corporation. All rights reserved.

---

**Page 25**

![추가 정보 아이콘](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**추가 정보 (Further reading)**

*   **모델 관계 만들기 (Create model relationships)**
    *   [`https://aka.ms/model-relationships`](https://aka.ms/model-relationships)
*   **Power BI의 모델 관계 (Model relationships in Power BI)**
    *   [`https://learn.microsoft.com/power-bi/transform-model/desktop-relationships-understand`](https://learn.microsoft.com/power-bi/transform-model/desktop-relationships-understand)
*   **Power BI desktop에서 복합 모델 사용 (Use composite models in Power BI desktop)**
    *   [`https://learn.microsoft.com/power-bi/transform-model/desktop-composite-models`](https://learn.microsoft.com/power-bi/transform-model/desktop-composite-models)
*   **Power BI에서 사용자 정의 집계 만들기 (Create user-defined aggregations in Power BI)**
    *   [`https://learn.microsoft.com/power-bi/transform-model/aggregations-advanced`](https://learn.microsoft.com/power-bi/transform-model/aggregations-advanced)

© Copyright Microsoft Corporation. All rights reserved.

---

**Page 26**

![슬라이드 배경 이미지](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**Power BI 성능 최적화를 위한 도구 사용 (Use tools to optimize Power BI performance)**

Copyright Microsoft Corporation. All rights reserved.

---

**Page 27**

![학습 목표 아이콘](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**학습 목표 (Learning objectives)**

*   **Aggregations** (집계), **dual storage mode** (이중 저장소 모드), **hybrid tables** (하이브리드 테이블)를 사용하여 솔루션 최적화하기.
*   **Performance analyzer** (성능 분석기)를 사용하여 쿼리 최적화하기.
*   **DAX Studio**를 사용하여 **DAX** 성능 문제 해결하기.
*   **Tabular Editor**를 사용하여 데이터 모델 최적화하기.

Copyright Microsoft Corporation. All rights reserved.

---

**Page 28**

![Power BI 솔루션 최적화](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**Power BI 솔루션 최적화 (Optimize Power BI solutions)**

최적화 대상 영역:

*   **Data source(s)** (데이터 원본)
    *   > 원본 시스템 쿼리 성능, 네트워크 지연 시간 등
*   **Data model** (데이터 모델)
    *   > 모델 구조, 관계, DAX 계산 효율성, 데이터 유형 등
*   **Visualizations** (시각화)
    *   > 시각적 개체 수, 각 개체가 생성하는 쿼리 수 및 복잡성 등
*   **Environment** (환경)
    *   > Power BI 용량(Capacity) 성능, 게이트웨이 성능, 사용자 로컬 환경 등

Copyright Microsoft Corporation. All rights reserved.

---

**Page 29**

![Power BI 솔루션 최적화 예시](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**Power BI 솔루션 최적화 (Optimize Power BI solutions)**

*   **Aggregations (집계):**
    *   > **설명:** 미리 계산된 요약 테이블을 만들어 대용량 데이터셋에 대한 쿼리 성능을 향상시키는 기법입니다. Power BI는 쿼리 시 자동으로 상세 데이터 대신 집계 테이블을 사용합니다. (왼쪽 이미지)
*   **Dual storage mode (이중 저장소 모드):**
    *   > **설명:** 테이블 단위로 저장소 모드(Import 또는 DirectQuery)를 혼합하여 사용하는 방식입니다. 자주 사용되는 차원 테이블은 Import로 설정하여 성능을 높이고, 대용량 팩트 테이블은 DirectQuery로 설정하여 실시간성을 확보하거나 모델 크기를 줄일 수 있습니다. (가운데 이미지 - 모델 뷰에서 테이블별 저장소 모드 설정 가능)
*   **Hybrid tables (하이브리드 테이블):**
    *   > **설명:** 하나의 테이블 내에서 파티션(Partition)별로 저장소 모드를 다르게 설정하는 고급 기법입니다. 예를 들어, 최근 데이터 파티션은 DirectQuery로 설정하여 실시간 데이터를 반영하고, 과거 데이터 파티션은 Import로 설정하여 성능을 최적화할 수 있습니다. (오른쪽 이미지: 과거 데이터는 Import(Archived), 최근 데이터는 증분 새로 고침(Incremental refresh)으로 Import, 가장 최신 데이터는 DirectQuery로 구성)

Copyright Microsoft Corporation. All rights reserved.

---

**Page 30**

![도구를 사용한 Power BI 모델 최적화](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**도구를 사용한 Power BI 모델 최적화 (Optimize Power BI models using tools)**

*   **Retroactive (사후 대응적):** 문제가 발생한 후 원인을 분석하고 해결
    *   **Performance Analyzer** (성능 분석기)
    *   **DAX Studio**
*   **Proactive (사전 예방적):** 문제 발생 전에 모범 사례를 적용하고 잠재적 이슈를 미리 식별
    *   **Best Practice Analyzer in Tabular Editor** (Tabular Editor의 모범 사례 분석기)

Copyright Microsoft Corporation. All rights reserved.

---

**Page 31**

![Performance analyzer 사용](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**Performance analyzer 사용하기 (Use Performance analyzer)**

**(왼쪽 이미지: Power BI Desktop 'View' 리본 -> 'Show panes' 그룹 -> 'Performance analyzer' 체크)**
**(아래쪽 이미지: Performance analyzer 창 -> 'Start recording' 버튼)**

**(오른쪽 이미지: Performance analyzer 기록 결과 예시)**
*   `Start recording` (기록 시작) / `Refresh visuals` (시각적 개체 새로 고침) / `Stop` (중지)
*   `Clear` (지우기) / `Export` (내보내기)
*   작업 목록 (예: 페이지 변경, 슬라이서 변경, 교차 강조 표시)
*   각 시각적 개체 및 작업에 소요된 시간 (`Duration (ms)`)
    *   `DAX query` (DAX 쿼리 시간)
    *   `Visual display` (시각적 개체 표시 시간)
    *   `Other` (기타 시간)
*   `Copy query` (쿼리 복사) 기능

> **설명:** Performance Analyzer는 Power BI 보고서 내 각 시각적 개체의 성능을 분석하는 데 사용됩니다. 사용자가 보고서와 상호작용할 때 각 요소(DAX 쿼리, 시각적 렌더링 등)에 소요되는 시간을 기록하여 성능 병목 현상을 식별하는 데 도움을 줍니다. 느린 시각적 개체를 찾아내고 해당 DAX 쿼리를 복사하여 DAX Studio 등에서 추가 분석을 진행할 수 있습니다.

Copyright Microsoft Corporation. All rights reserved.

---

**Page 32**

![DAX Studio 사용](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**DAX Studio 사용하기 (Use DAX Studio)**

*   **VertiPaq 엔진 이해하기**

    **(이미지: Power BI 쿼리 처리 흐름)**
    1.  **Query** (쿼리): 사용자의 시각적 개체 상호작용으로 쿼리 생성
    2.  **Tabular model** (테이블 형식 모델): 쿼리가 모델 구조 참조
    3.  **Formula engine** (수식 엔진): DAX 계산 처리
    4.  **Storage engine** (저장소 엔진): 데이터 검색
        *   **VertiPaq (import):** 메모리에 압축 저장된 데이터 스캔 (Import 모드)
        *   **DirectQuery:** 원본 데이터 소스로 쿼리 전송 (DirectQuery 모드)
    5.  **Data source** (데이터 원본): DirectQuery 시 원본에서 데이터 가져옴

> **설명:** Power BI는 내부적으로 VertiPaq 엔진(Analysis Services 기반)을 사용하여 Import 모드 데이터를 처리합니다. DAX Studio는 이 엔진의 작동 방식을 이해하고 DAX 쿼리 성능을 심층적으로 분석하는 데 유용한 외부 도구입니다. 쿼리가 Formula Engine과 Storage Engine에서 어떻게 처리되는지, 시간이 어디서 많이 소요되는지 등을 파악할 수 있습니다.

Copyright Microsoft Corporation. All rights reserved.

---

**Page 33**

![DAX Studio - VertiPaq Analyzer](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**DAX Studio 사용하기 (Use DAX Studio)**

*   **VertiPaq Analyzer를 사용하여 모델 메트릭 보기**

    **(이미지: DAX Studio의 VertiPaq Analyzer 결과 화면 예시)**
    *   `Tables`, `Columns`, `Relationships`, `Partitions`, `Summary` 탭
    *   테이블/열별 상세 정보:
        *   `Cardinality` (고유 값 개수)
        *   `Table Size` / `Col Size` (테이블/열 크기)
        *   `Data` / `Dictionary` / `Hier Size` (데이터, 사전, 계층 구조의 크기)
        *   `Encoding` (인코딩 방식 - HASH, VALUE)
        *   `Data Type` (데이터 형식)
        *   `% Table` / `% DB` (테이블 내/전체 DB 내 크기 비율)

> **설명:** DAX Studio의 VertiPaq Analyzer 기능은 Power BI 모델(Import 모드)의 내부 저장 구조와 메모리 사용량에 대한 자세한 정보를 제공합니다. 각 테이블과 열의 크기, 카디널리티, 압축 방식 등을 분석하여 모델 크기를 비효율적으로 만드는 요소를 찾아내고 최적화하는 데 도움을 줍니다. 예를 들어, 카디널리티가 매우 높거나 크기가 비정상적으로 큰 열을 식별할 수 있습니다.

Copyright Microsoft Corporation. All rights reserved.

---

**Page 34**

![Best Practice Analyzer를 사용한 데이터 모델 최적화](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**Best Practice Analyzer를 사용하여 데이터 모델 최적화하기 (Optimize data model by using Best Practice Analyzer)**

**(왼쪽 이미지: Power BI Desktop 'External Tools' 리본 -> 'Tabular Editor' 실행)**
**(오른쪽 이미지: Tabular Editor 내 'Best Practice Analyzer' 창 예시)**
*   `Show ignored` (무시된 항목 표시)
*   위반된 모범 사례 규칙 목록 (예: 부동 소수점 데이터 유형 사용 금지, 숫자 열 요약 안 함, 외래 키 열 숨기기, 사용되지 않는 열 제거 등)
*   위반된 개체 및 유형 표시
*   총 위반 개체 및 규칙 수 요약 (예: "20 objects in violation of 7 Best Practice rules.")

*   **The best practice analyzer scans your model for issues whenever a change is made to the model.**
    *   > **번역:** Best Practice Analyzer는 모델이 변경될 때마다 모델을 스캔하여 (잠재적인) 이슈를 찾습니다.

> **설명:** Tabular Editor는 Power BI 모델을 편집하고 관리하는 강력한 외부 도구입니다. 내장된 Best Practice Analyzer(BPA) 기능은 미리 정의된 또는 사용자 정의된 규칙 집합에 따라 모델을 검사하여 성능, 유지 관리성, 디자인 등에 대한 모범 사례 위반 사항을 식별하고 알려줍니다. 이를 통해 모델의 품질을 사전에 개선하고 잠재적인 문제를 예방할 수 있습니다 (Proactive 접근).

© Copyright Microsoft Corporation. All rights reserved.

---

**Page 35**

![실습 아이콘](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**실습 (Exercise)**

**Power BI 성능 최적화를 위한 도구 사용하기 (Use tools to optimize Power BI performance)**

**45 분**

Copyright Microsoft Corporation. All rights reserved.

---

**Page 36**

![지식 점검 아이콘](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**지식 점검 (Knowledge check) (1/2)**

1.  Stephanie는 엔터프라이즈 데이터 분석가이며, 재무 부서의 데이터 분석가들에게 리소스로 일하고 있습니다. 재무 분석가 중 한 명이 슬라이서 선택 후 보고서 시각적 개체가 렌더링되는 데 5초가 걸리는 이유를 해결해 달라고 Stephanie에게 요청했습니다. Stephanie가 문제 해결을 시작하기 위해 가장 먼저 사용해야 하는 도구는 무엇인가요?
    *   **Visual Studio.**
    *   **DAX Studio.**
    *   ✅ **Performance analyzer.**
    *   > **설명:** 보고서 내 특정 시각적 개체의 성능 문제를 진단할 때는 먼저 Power BI Desktop에 내장된 Performance Analyzer를 사용하여 어떤 요소(DAX 쿼리, 시각적 렌더링 등)가 시간을 많이 소모하는지 확인하는 것이 일반적인 첫 단계입니다.
2.  James는 페이지의 다른 시각적 개체보다 눈에 띄게 느리게 렌더링되는 matrix 시각적 개체가 포함된 보고서의 문제를 해결하라는 요청을 받았습니다. **Performance analyzer**를 실행한 후, James는 문제의 원인을 이해하기 위해 **DAX Studio**에서 matrix에 대한 DAX 쿼리를 더 자세히 조사해야 한다는 것을 알게 되었습니다. 동일한 쿼리가 **DAX Studio**에서는 1초 미만으로 실행됩니다. James는 문제를 해결했나요?
    *   예. James는 DAX 쿼리에 더 이상 변경할 필요가 없습니다.
    *   아니요. James는 조사를 계속하기 위해 **Tabular Editor**를 사용해야 합니다.
    *   ✅ 아니요. James는 쿼리 결과가 모델에 캐시되지 않도록 모델 캐시를 지워야 합니다.
    *   > **설명:** Performance Analyzer에서 측정한 시간과 DAX Studio에서 측정한 시간이 크게 다른 경우, Power BI 내부 캐시의 영향일 수 있습니다. DAX Studio에서 'Clear Cache' 옵션을 사용하여 캐시를 지우고 다시 쿼리를 실행하면 더 정확한 성능을 측정할 수 있습니다. DAX 쿼리 자체에는 문제가 없을 수도 있지만, 캐시 때문에 원인을 잘못 판단할 수 있습니다.

Copyright Microsoft Corporation. All rights reserved.

---

**Page 37**

![지식 점검 아이콘](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**지식 점검 (Knowledge check) (2/2)**

3.  Mary-Jo는 전체 조직의 **Power BI assets** (Power BI 자산) 유지 관리 및 배포를 담당합니다. Mary-Jo는 모든 데이터 모델이 **data modeling best practices** (데이터 모델링 모범 사례)를 준수하도록 보장하기 위해 어떤 도구를 사용할 수 있나요?
    *   **DAX Studio.**
    *   ✅ **Best Practice Analyzer in Tabular Editor.**
    *   **Performance analyzer.**
    *   > **설명:** 데이터 모델링 모범 사례를 체계적으로 검사하고 준수 여부를 확인하는 데 가장 적합한 도구는 Tabular Editor의 Best Practice Analyzer입니다. DAX Studio는 주로 DAX 쿼리 성능 분석에, Performance Analyzer는 보고서 시각적 개체 성능 분석에 중점을 둡니다.

Copyright Microsoft Corporation. All rights reserved.

---

**Page 38**

![요약 아이콘](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**요약 (Recap)**

**이번 섹션에서 우리는 다음을 수행했습니다:**

*   **Power BI model relationships** (Power BI 모델 관계) 생성
*   **DAX concepts** (DAX 개념) 검토
*   **Calculation groups** (계산 그룹) 생성
*   **Power BI model security** (Power BI 모델 보안) 적용
*   **Power BI performance** (Power BI 성능) 최적화를 위한 도구 사용

Copyright Microsoft Corporation. All rights reserved.

---

**Page 39**

![추가 정보 아이콘](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**추가 정보 (Further reading)**

*   **Power BI 성능 최적화를 위한 도구 사용 (Use tools to optimize Power BI performance)**
    *   [`https://aka.ms/optimize-power-bi`](https://aka.ms/optimize-power-bi)
*   **Power BI 최적화 가이드 (Optimization guide for Power BI)**
    *   [`https://learn.microsoft.com/power-bi/guidance/power-bi-optimization`](https://learn.microsoft.com/power-bi/guidance/power-bi-optimization)

Copyright Microsoft Corporation. All rights reserved.

---

**Page 40**

![슬라이드 배경 이미지](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**Semantic model 보안 적용하기 (Enforce semantic model security)**

Copyright Microsoft Corporation. All rights reserved.

---

**Page 41**

![학습 목표 아이콘](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**학습 목표 (Learning objectives)**

*   **RLS** (행 수준 보안)를 사용하여 **Power BI model data** (Power BI 모델 데이터)에 대한 접근 제한하기.
*   **OLS** (개체 수준 보안)를 사용하여 **Power BI model objects** (Power BI 모델 개체 - 테이블, 열)에 대한 접근 제한하기.
*   **Power BI model security** (Power BI 모델 보안)를 적용하기 위한 좋은 개발 관행(good development practices) 적용하기.

Copyright Microsoft Corporation. All rights reserved.

---

**Page 42**

![Power BI 모델 데이터 접근 제한](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**Power BI 모델 데이터 접근 제한하기 (Restrict access to Power BI model data)**

**다음 방법으로 Power BI 모델 데이터 접근 제한:**

*   **Apply star schema design principals (Star schema 디자인 원칙 적용):**
    *   **Dimension** (차원) 및 **fact** (팩트) 테이블로 구성된 모델을 생성하기 위해 **star schema** 디자인 원칙을 적용합니다.
    *   > **설명 (RLS 관련):** RLS 규칙은 주로 차원 테이블에 적용하는 것이 성능 및 관리 측면에서 더 효율적입니다.
*   **Define rules (규칙 정의):**
    *   **Static rules** (정적 규칙) 및 **dynamic rules** (동적 규칙)
    *   > **설명:** 정적 규칙은 역할별로 고정된 필터 조건을 정의합니다 (예: 'Region' = "East"). 동적 규칙은 `USERPRINCIPALNAME()`과 같은 DAX 함수를 사용하여 현재 로그인한 사용자에 따라 필터 조건을 동적으로 변경합니다 (예: 'Email' = `USERPRINCIPALNAME()`).
*   **Validate roles (역할 검증):**
    *   역할(roles)을 생성할 때, 올바른 필터가 적용되는지 확인하기 위해 테스트하는 것이 중요합니다.
    *   > **설명:** Power BI Desktop의 'View as' 기능을 사용하여 각 역할이 의도한 대로 데이터를 필터링하는지 반드시 확인해야 합니다.
*   **Set up role mappings (역할 매핑 설정):**
    *   사용자가 **Power BI content** (Power BI 콘텐츠)에 접근하기 전에 **Role mappings** (역할 매핑)이 미리 설정되어야 합니다.
    *   > **설명:** Power BI 서비스에서 어떤 사용자 또는 그룹이 어떤 RLS 역할을 부여받을지 지정하는 작업입니다.
*   **Use single sign-on (SSO) for DirectQuery sources (DirectQuery 원본에 SSO 사용):**
    *   데이터 모델에 **DirectQuery** 테이블이 있고 해당 데이터 원본이 **SSO**를 지원하는 경우, 데이터 원본 자체에서 데이터 권한을 적용할 수 있습니다.
    *   > **설명:** SSO를 사용하면 Power BI 사용자의 자격 증명이 DirectQuery를 통해 원본 데이터베이스로 전달되어, 데이터베이스 수준에서 정의된 보안 규칙(예: 특정 행만 보기)을 적용할 수 있습니다.

Copyright Microsoft Corporation. All rights reserved.

---

**Page 43**

![Power BI 모델 개체 접근 제한](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**Power BI 모델 개체 접근 제한하기 (Restrict Access to Power BI model objects)**

*   **Object-level security (OLS) (개체 수준 보안):**
    *   **Set up OLS (OLS 설정):**
        *   `Create roles` (역할 만들기)
        *   `Add OLS rules to the roles` (역할에 OLS 규칙 추가)
        *   > **설명:** Tabular Editor와 같은 외부 도구를 사용하여 특정 역할에 대해 테이블 또는 열 전체를 보거나 숨길 수 있도록 설정합니다.
    *   **Considerations (고려 사항):**
        *   사용자가 테이블이나 열에 접근할 권한이 없는 경우 오류 메시지를 받게 됩니다.
        *   **(이미지: "Fields that need to be fixed" 오류 메시지 예시 - Salary 필드 접근 불가)**
    *   **Restrictions (제한 사항):**
        *   동일한 역할에 **RLS**와 **OLS**를 혼합하여 사용할 수 없습니다.
        *   > **설명:** 특정 역할에는 RLS 규칙만 적용하거나 OLS 규칙만 적용해야 합니다. 둘 다 필요한 경우 별도의 역할을 만들어야 합니다.

Copyright Microsoft Corporation. All rights reserved.

---

**Page 44**

![좋은 모델링 관행 적용](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**좋은 모델링 관행 적용하기 (Apply good modeling practices)** (보안 관련)

*   잘 설계된 **roles** (역할)을 사용하여 **fewer semantic models** (더 적은 수의 semantic model)을 정의합니다.
    *   > **설명:** 여러 개의 유사한 모델을 만드는 대신, 하나의 모델 내에서 역할을 사용하여 데이터 접근을 제어하는 것이 관리 효율성을 높입니다.
*   **Dynamic rules** (동적 규칙)를 사용하여 **fewer roles** (더 적은 수의 역할)을 만듭니다.
    *   > **설명:** 사용자별로 역할을 만드는 대신, 동적 규칙을 사용하면 단일 역할로 여러 사용자의 데이터 접근을 제어할 수 있습니다.
*   **Fact tables** (팩트 테이블) 대신 **dimension tables** (차원 테이블)을 필터링하는 규칙을 만듭니다.
    *   > **설명:** RLS 필터는 일반적으로 관계의 '1' 쪽인 차원 테이블에 적용하는 것이 성능상 더 유리합니다.
*   **Model design** (모델 디자인), **relationships** (관계) 및 **relationship properties** (관계 속성)가 올바르게 설정되었는지 확인(Validate)합니다.
    *   > **설명:** RLS/OLS가 올바르게 작동하려면 기본 모델 구조와 관계가 견고해야 합니다.
*   `USERNAME` 함수 대신 `USERPRINCIPALNAME` 함수를 사용합니다.
    *   > **설명:** `USERPRINCIPALNAME()`은 사용자의 UPN(이메일 주소 형식)을 반환하며, Power BI 서비스 환경에서 사용자를 식별하는 데 더 안정적입니다. `USERNAME()`은 Windows 사용자 이름을 반환할 수 있어 환경에 따라 값이 달라질 수 있습니다.
*   **Testing all roles** (모든 역할을 테스트)하여 **RLS** 및 **OLS**를 철저히 검증합니다.
*   **Power BI Desktop** 데이터 원본 연결이 **Power BI service** (Power BI 서비스)에서 설정될 때 적용될 것과 **uses the same credentials** (동일한 자격 증명)을 사용하는지 확인합니다.
    *   > **설명:** 특히 DirectQuery와 SSO를 사용할 때, Desktop에서의 테스트와 서비스에서의 실제 동작이 일치하도록 자격 증명 설정을 확인해야 합니다.

Copyright Microsoft Corporation. All rights reserved.

---

**Page 45**

![실습 아이콘](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**실습 (Exercise)**

**모델 보안 적용하기 (Enforce model security)**

**45 분**

Copyright Microsoft Corporation. All rights reserved.

---

**Page 46**

![지식 점검 아이콘](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**지식 점검 (Knowledge check) (1/2)**

1.  Joshua는 **Adventure Works**의 **data modeler**이며 대규모 **data warehouse**용 모델을 개발 중입니다. 모델은 **RLS**를 적용해야 하며, 모델에 연결되는 **Power BI reports**는 가능한 가장 빠른 성능을 제공해야 합니다. Joshua는 어떻게 해야 하나요?
    *   ✅ **Dimension tables** (차원 테이블)에 규칙을 적용합니다.
    *   **Hierarchies** (계층 구조)에 규칙을 적용합니다.
    *   **Fact tables** (팩트 테이블)에 규칙을 적용합니다.
    *   > **설명:** RLS 규칙은 일반적으로 차원 테이블에 적용하는 것이 성능 면에서 더 효율적입니다.
2.  Rupali는 **Adventure Works**의 **data modeler**이며 직원 근무 시간표 데이터를 분석하기 위한 **import model**을 개발 중입니다. 직원 테이블에는 직원의 사회 보장 번호 (**SSN**)가 열에 저장되어 있습니다. 모델은 모든 회사 관리자가 사용할 수 있지만, **Payroll** (급여) 부서의 직원도 사용할 수 있습니다. 그러나 보고서는 급여 담당 직원에게만 직원 SSN을 공개해야 합니다. Rupali는 SSN 열에 대한 접근을 제한하기 위해 어떤 기능을 사용해야 하나요?
    *   **RLS** (행 수준 보안)
    *   **SSO** (Single Sign-On)
    *   ✅ **OLS** (개체 수준 보안)
    *   > **설명:** 특정 열(여기서는 SSN) 자체에 대한 접근을 역할별로 제어해야 하므로, 개체 수준 보안(OLS)이 적합합니다. RLS는 행(Row) 단위의 접근을 제어합니다.

© Copyright Microsoft Corporation. All rights reserved.

---

**Page 47**

![지식 점검 아이콘](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**지식 점검 (Knowledge check) (2/2)**

3.  Kasper는 **Adventure Works**의 **data modeler**이며 **RLS**를 적용해야 하는 모델을 개발 중입니다. 보고서 사용자(**report consumer**)에게 할당된 판매 지역(sales regions)에만 접근을 제한해야 합니다. 원본 데이터베이스에는 직원 사용자 이름과 할당된 지역을 저장하는 테이블이 포함되어 있습니다. Kasper는 어떻게 해야 하나요?
    *   **OLS role** (OLS 역할)을 만들고 **dynamic rule** (동적 규칙)을 사용합니다.
    *   ✅ **RLS role** (RLS 역할)을 만들고 **dynamic rule** (동적 규칙)을 사용합니다.
    *   **RLS role** (RLS 역할)을 만들고 **static rule** (정적 규칙)을 사용합니다.
    *   > **설명:** 사용자별로 볼 수 있는 지역(행 데이터)을 제한해야 하므로 RLS가 필요합니다. 각 사용자의 할당된 지역 정보가 데이터베이스에 있고 이를 기반으로 동적으로 필터링해야 하므로 동적 규칙(예: `USERPRINCIPALNAME()`을 사용하여 사용자 이름을 가져오고, 해당 사용자의 지역만 필터링)이 적합합니다.

Copyright Microsoft Corporation. All rights reserved.

---

**Page 48**

![요약 아이콘](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**요약 (Recap)**

**이번 섹션에서 우리는 다음을 수행했습니다:**

*   **RLS**를 사용하여 **Power BI model data** (Power BI 모델 데이터)에 대한 접근 제한.
*   **OLS**를 사용하여 **Power BI model objects** (Power BI 모델 개체)에 대한 접근 제한.
*   **Power BI model security** (Power BI 모델 보안)를 적용하기 위한 개발 관행 검토.

Copyright Microsoft Corporation. All rights reserved.

---

**Page 49**

![추가 정보 아이콘](https://user-images.githubusercontent.com/11220319/210179120-7e07a44f-298c-4c0e-8618-6a2a8a13e4a4.png) *(슬라이드 이미지 참조)*

**추가 정보 (Further reading)**

*   **Semantic model 보안 적용하기 (Enforce semantic model security)**
    *   [`https://aka.ms/fabric-model-security`](https://aka.ms/fabric-model-security)
*   **Power BI 구현 계획: 보안 (Power BI implementation planning: Security)**
    *   [`https://learn.microsoft.com/power-bi/guidance/powerbi-implementation-planning-security-overview`](https://learn.microsoft.com/power-bi/guidance/powerbi-implementation-planning-security-overview)

Copyright Microsoft Corporation. All rights reserved.

---

여기까지입니다! 슬라이드 내용을 한국어로 번역하고 필요한 부연 설명을 추가했습니다. 학습에 도움이 되셨기를 바랍니다. 😊
