# CSE4110
서강대학교 데이터베이스시스템(CSE4110) 수업의 과제

# Project 1
## 1. ERD (Entity-Relationship Diagram)

![ERD](20210091_이동형.png)

## 2. Entity and Relationship Justification (엔터티 및 관계)

[cite_start]이번 과제에서는 Store, Product, Vendor, Inventory, Customer, Sales Transaction, Transaction_item을 포함합니다. [cite: 4]
[cite_start]선택적 엔터티(Customer, Sales Transaction, Transaction_item)은 ERD에서 다른 entity와 색깔에서 차이를 두면서 구별하였습니다. [cite: 38]

### Store
* [cite_start]**설명**: 편의점 매장을 나타냅니다. [cite: 6]
* [cite_start]**속성**: `store_id` (기본 키), 점포명, 주소, 영업시간, 소유형태(프랜차이즈/직영점) [cite: 6]
* [cite_start]**Relationship (Records)**: 하나의 매장은 여러 개의 판매(SalesTransaction)가 일어날 수 있습니다 (1:N 관계). [cite: 8]

### Product
* [cite_start]**설명**: 제품을 나타냅니다. [cite: 10]
* [cite_start]**속성**: `UPC` (기본 키), 브랜드, 포장 종류, 크기, 가격 [cite: 10]
* [cite_start]**Relationship (Is_part_of)**: 하나의 제품은 여러 거래(Transaction_item)에 따라 다양한 개수로 거래가 됩니다 (1:N 관계). [cite: 13, 14]

### Vendor
* [cite_start]**설명**: 제품을 공급하는 업체를 나타냅니다. [cite: 16]
* [cite_start]**속성**: 고유 `id` (기본 키), 이름, 연락처 [cite: 16]
* [cite_start]**Relationship (Supply)**: 여러 개의 업체는 여러 개의 상품(Product)을 제공할 수 있습니다 (M:N 관계). [cite: 18]

### Inventory
* [cite_start]**설명**: 특정 점포가 보유한 재고를 나타냅니다. [cite: 20]
* [cite_start]**속성**: `(store_id, UPC)` (기본 키, 각각 Foreign Key), 제품의 수량, 재주문 임계값, 최근 주문일, 재입고 필요 여부 [cite: 20, 21]
* [cite_start]**Relationship (Has_inventory)**: 하나의 매장(Store)은 여러 제품의 재고를 가질 수 있습니다 (N:1 관계). [cite: 23]

### Customer
* **설명**: 고객을 나타냅니다. (선택적 entity) [cite_start][cite: 25]
* [cite_start]**속성**: `customer_id` (기본 키), 이름, 전화번호, 이메일, 로열티 프로그램 참여 여부 [cite: 25]
* [cite_start]**Relationship (Transaction)**: 한 명의 고객은 여러 횟수의 거래(SalesTransaction)를 진행할 수 있습니다 (1:N 관계). [cite: 27]

### Sales Transaction
* [cite_start]**설명**: 판매 내역을 기록합니다. [cite: 28]
* [cite_start]**속성**: `transaction_id` (기본 키), `store_id` (FK), `customer_id` (FK), 거래 일시, 결제 방식, 총액 [cite: 28, 29]
* [cite_start]**Relationship (Contains)**: Transaction_item들을 이용하여 하나의 Sales Transaction이 일어납니다 (1:N 관계). [cite: 31, 32, 33]

### Transaction_item
* **설명**: 거래 내의 개별 제품 항목을 나타냅니다. (선택적 entity) [cite_start][cite: 35]
* [cite_start]**속성**: `(transaction_id, UPC)` (기본 키, 각각 Foreign Key), 제품 수량 [cite: 35, 36]

# Project 2

## 개요

`main.cpp`는 MySQL C API를 사용하여 편의점 유통 데이터베이스에 연결하고, 쿼리를 실행합니다.

## ⚙️ 실행 환경

- **실행 환경**: Windows MinGW
- **DB**: MySQL 8.0

## 컴파일 방법

ctrl+ shift+ b

## ▶실행 방법

./main.exe

## DB 접속 정보 설정

`connectDB()`에서서 본인의 환경에 맞게 정보를 입력한다:

const char *server = "localhost";
const char *user = "root";
const char *password = "YourPassword";
const char *database = "convenience_store";

## 지원 쿼리 목록

프로그램은 아래의 7개 쿼리를 지원한다

1. 특정 제품의 매장별 재고 조회 -> `TYPE1()`
2. 전체 제품 중 판매량 상위 5개 조회 -> `TYPE2()`
3. 매장별 총 매출 조회 -> `TYPE3()`
4. 공급자별 판매 제품 총량 조회 -> `TYPE4()`
5. 재고가 임계치 이하인 항목 목록 -> `TYPE5()`
6. 로열티 고객들이 가장 많이 구매한 제품 Top3 -> `TYPE6()`
7. 프랜차이즈 vs 직영점 상품 다양성 비교 -> `TYPE7()`

## 프로그램 실행

프로그램 실행 시 아래와 같은 메뉴가 출력되며, 번호를 입력하면 원하는 쿼리를 실행된다.

=== Select Query Types ===
1.TYPE 1
2.TYPE 2
3.TYPE 3
4.TYPE 4
5.TYPE 5
6.TYPE 6
7.TYPE 7

## 예외 처리 및 주의 사항

- SSL 인증 비활성화를 위해 `mysql_options()`에 `SSL_MODE_DISABLED`를 명시한다.
- 연결 실패 시 오류 메시지가 출력된다.
- SSL 인증 비활성화를 위해 `mysql_options()`에 `SSL_MODE_DISABLED`를 명시한다.
- 연결 실패 시 오류 메시지가 출력된다.
