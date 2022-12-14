## SQL과 NoSQL 차이
![](https://velog.velcdn.com/images/cil05265/post/9329c814-4245-4170-a617-1a933f2b4c87/image.png)

### SQL 관계형 데이터베이스
- SQL은 **구조화된 쿼리 언어(Structured Query Language)**의 약자이다. 
- SQL을 사용하면 **관계형 데이터베이스 관리 시스템(RDBMS)**에서 **데이터를 저장, 수정, 삭제 및 검색**할 수 있다.
- 이러한 관계형 데이터베이스의 데이터는 정해진 데이터 스키마를 따라 데이터베이스 테이블에 저장되며, 관계를 통해 연결된 여러 개의 테이블에 데이터가 분산되는 특징이 있다.

### 엄격한 스키마
- 데이터는 테이블(table)에 Rows로 저장되며, 각 테이블에는 명확하게 정의된 구조(structure)가 있습니다. 그리고 구조(structure)는 컬럼의 이름과 데이터 유형으로 정의된다. 관계형 데이터베이스에서 스키마를 준수하지 않는 Row는 추가할 수 없다.

### 관계
- SQL 기반의 데이터베이스의 또 다른 중요한 부분은 관계이다. 데이터들을 여러 개의 테이블에 나눠서 데이터들의 중복을 피할 수 있다. 테이블을 나눠서 데이터를 저장하면, 테이블에서 중복 없이 하나의 데이터만을 관리하기 때문에, 다른 테이블에서 부정확한 데이터를 다룰 위험이 없다는 특징이 있다.

### NoSQL 비 관계형 데이터베이스
- 관계형 데이터베이스도 한계점을 노출하게 되었으며 이로 인해 새로운 데이터베이스인 NoSQL이 탄생하게 되었다. NoSQL은 기본적으로 SQL(관계형)와 반대되는 접근방식을 따르기 때문에 지어진 이름이다.
- NoSQL 데이터베이스의 종류는 키-값 데이터베이스, 도큐먼트 데이터베이스, 그래프 데이터베이스 등으로 나뉜다. 
- 키-값 데이터 베이스는 키와 값으로 구성된 배열구조의 데이터베이스로 NoSQL 데이터베이스 중 가장 단순한 구조이다.
- 도큐먼트 데이터베이스는 필드와 값의 형태로 구성된 데이터를 JSON 포맷으로 관리하는 데이터베이스로 NoSQL 데이터베이스 중 가장 인기가 높다. 
- 그래프 데이터베이스는 노드와 관계로 구성된 데이터베이스로 근접한 객체를 모델링할 목적으로 설계되었다.

- NoSQL 데이터베이스는 각 DBMS별로 차이가 있으나 일반적으로 다음과 같은 특징을 가지고 있다.
1. 유연성 : 스키머 선언 없이 필드의 추가 및 삭제가 자유로운 Schema-less 구조이다.
2. 확장성 : 스케일 아웃에 의한 서버 확장이 용이하다.
3. 고성능 : 대용량 데이터를 처리하는 성능이 뛰어나다.
4. 가용성 : 여러 대의 백업 서버 구성이 가능하여 장애 발생 시에도 무중단 서비스가 가능하다.

### 수직적,  수평적 확장(Scaling)
두 부류의 데이터베이스를 살펴볼 때 어떤 방식으로 확장이 가능한 지 확장(Scaling)에 대한 개념을 살펴봐야 한다.
- 확장은 **수직적 확장**과 **수평적 확장**으로 구별할 수 있다. 
- 수직적 확장이란 단순히 데이터베이스 서버의 성능을 향상시키는 것이다. 예를 들어 CPU를 업그레이드하는 방식으로 말이죠. 
- 반면에 수평적 확장은 더 많은 서버가 추가되고 데이터베이스가 전체적으로 분산됨을 의미한다. 따라서 하나의 데이터베이스에서 작동하지만 여러 호스트에서 작동한다.
- 데이터가 저장되는 방식 때문에 관계형 데이터베이스는 일반적으로 수직적 확장만을 지원한다. 
- 수평적 확장은 NoSQL 데이터베이스에서만 가능하다.
- 관계형 데이터베이스에서는 같은 테이블 스키마를 가진 데이터를 다수의 데이터베이스에 분산하여 저장하는 방식인 '샤딩(Sharding)'을 활용해서 데이터를 저장해서 활용할 수 있지만 구현하기가 대체로 어렵다.
하지만 NoSQL 데이터베이스는 이를 기본적으로 지원하므로 여러 서버에서 데이터베이스를 쉽게 분리할 수 있다.

### SQL과 NoSQL 정리
|항목|NoSQL DB|관계형 DB|
|------|---|---|
|적합업무|오프라인에서 정형 및 비정형 데이터 분석 업무 <br> 초당 동시 처리가 중요한 업무 <br> 로그 및 이력 등의 단순 기록형 업무 | 데이터 무결성 및 일관성이 중요한 트랜잭션 업무 <br> 온라인에서 다양한 집계 및 통계를 분석하는 업무 <br> 복잡한 계산 및 실시간 데이터 정합성이 필요한 업무|
|데이터 모델| 서비스에 맞는 DB 선택이 중요함 <br> 반 정규화에 의한 설계를 기본으로 함 <br> 비정형화 스키마 구조로 미리 스키마를 선언하지 않음 | 엔티티 및 각 엔티티 간 관계를 정의함 <br> 엔티티 정의 시 정규화에 의한 설계가 중요함 <br> 테이블, 칼럼 등 DB요소에 대한 스키마를 엄격히 관리함|
|성능 | 클러스터 크기, 네트워크 및 애플리케이션에 의해 성능이 결정됨 | 성능 향상을 위해서는 성능 최적화 작업이 필요함|
|인터페이스 | 쿼리 외 다양한 API를 통한 데이터 저장 및 검색이 가능함 |SQL을 통해서만 데이터 저장 및 검색이 가능함|
|장점 |데이터 중복에 의해 데이터 일관성이 저하되고 용량이 증가함 |데이터 중복 배제로 데이터 이상 발생 및 용량 증가를 최소화함|
|단점| 데이터 중복에 의해 데이터 일관성이 저하되고 용량이 증가함 |조인이 복잡한 경우 쿼리 프로세싱도 복잡해져 성능이 저하됨|