## 데이터베이스 개론
[데이터 베이스 개론](https://book.naver.com/bookdb/book_detail.nhn?bid=14427495)


## 1. 데이터베이스 기본 개념
- DBMS의 장단점, 발전 과정
- 스키마와 인스턴스
- 3단계 데이터베이스 구조, 데이터 독립성
- 데이터 모델링
- 관계형 모델
- SQL
- DB 설계
- 이상현상, 함수 종속, 정규화
- 트랜잭션
- 회복
- 보안, 권한, 역할
- 응용기술
- 빅데이터



## 2. 데이터베이스 관리 시스템


### DBMS 장점
- 데이터 중복을 통제
- 데이터 독립성 확보: 응용프로그램을 대신해서 데이터베이스에 접근 
- 데이터 동시 공유
- 데이터 보안 향상
- 데이터의 무결성 유지: 새로운 데이터가 입력되거나 변경될 때 유효성 검사가 가능
- 표준화: 모든 접근이 DBMS 를 통해 이루어지기 때문, 접근방식, 데이터 형식 등을 표준화하기 쉽다
- 장애 회복
- 개발 비용이 줄어듦

### DBMS 단점
- 비용이 든다 (파일시스템은 OS와 같이 제공됨)
- 백업 및 회복 방법 복잡(러닝커브)
- 모든 관리가 DBMS 로 집중되어있음



## 3. 데이터베이스 시스템
데이터와 DBMS 를 이용해 조직에 필요한 정보를 제공해주는 전체 시스템


### Schema
데이터 구조와 제약조건을 정의


### 3-Level Database Architecture(feat: 아파트)
목적: Data Independency
- external level: 개별 사용자 관점(집주인 관점)
- conceptual level: 조직 전체 관점(관리인 관점)
- internal level: 물리적인 저장 장치 관점(건설 업체 관점)


### External Level
External schema(sub schema): 외부 단계에서 사용자에게 필요한 데이터베이스 정의
- 사용자마다 업무 내용 및 사용 목적이 다름
- 여러개 존재 가능


### Conceptual Level
Conceptual schema: 모든 사용자에게 필요한 데이터를 통합하여 논리적인 구조를 정의
- 데이터의 종류, 관계, 제약조건, 보안 정책, 접근 권한 등이 포함됨
- 데이터베이스 하나당 개념스키마가 단 하나만 존재
- 사용자는 일부분을 사용함: conceptual schema 를 기초로 external schema 가 만들어짐


### Internal Level
Internal schema: 저장 장치에 실제로 저장되는 방법을 정의
- 레코드의 구조, 필드 크기, 인덱스
- conceptual schema 에 대한 물리적인 저장 구조를 표현하므로 하나만 존재


### Data Independency
하위 스키마를 변경하더라도 상위 스키마가 영향을 받지 않는 특성
- 논리적 데이터 독립성: conceptual schema 가 변경되더라도 external schema 가 영향을 받지 않는 것
- 물리적 데이터 독립성: internal schema 가 변경되더라도 conceptual schema 가 영향을 받지 않는 것



## 4. 데이터 모델링
데이터베이스 설계
- 개념적 모델링: 설계도를 그리는 과정
- 개념적 모델: 설계도를 그릴 때 사용하는 방법이나 도구
- 논리적 모델링: 설계도를 토대로 모델하우스를 만드는 과정
- 논리적 모델: 모델하우스를 만들 때 사용하는 방법이나 도구

### 4.1 Entity-Relationship Diagram
현실 세계를 개체-관계 모델을 이용해 개념적으로 모델링
- Entity: 현실 세계에서 저장할 만한 가치가 있는 것
- Relationship: Entity 간의 의미 있는 연관성
- Attribute: Entity, Relationship 의 속성  


### 4.2 논리적 데이터 모델
Schema: 논리적 데이터 모델로 표현된 데이터베이스의 논리적 구조


