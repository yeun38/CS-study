## SQL Injection
### 1. SQL Injection이란?
- 응용 프로그램 보안 상의 허점을 의도적으로 이용해, **악의적인 SQL문을 실행되게 함**으로써 **데이터베이스를 비정상적으로 조작하는 공격 기법**
- 웹 애플리케이션이 백엔드에서 구동 중인 데이터베이스에 질의를 하는 과정에 사용되는 **SQL 쿼리를 조작**하여 데이터베이스를 대상으로 공격자가 의도한 악의적인 행위를 할 수 있는 **Injection 기반의 웹 취약점 이용**
- 공격에 성공하게 되면 조직 내부의 민감한 데이터나 개인 정보를 획득할 수 있으며, **심각한 경우에는 조직의 데이터 전체를 장악하거나 완전히 손상시킬 수 있다.**
- 인젝션 공격은 **OWASP Top10 중 첫 번째에 속해 있으며, 공격이 비교적 쉬운 편이다.**


### 2. SQL Injection 공격 기법
##### 1. Error based SQL Injection
##### 2. UNION based SQL Injection
##### 3. Blind SQL Injection (1)
##### 4. Blind SQL Injection (2)
##### 5. Stored Procedure SQL Injection
##### 6. Mass SQL Injection

#### 1. Error based SQL Injection
- **논리적 에러**를 이용함
- 에러가 발생되는 사이트에서는 에러 정보들을 이용하여 **DB 및 쿼리 구조 등의 정보를 추측 가능**

```sql
SELECT * FROM Users WHERE id = 'INPUT1' AND password = 'INPUT2'

SELECT * FROM Users WHERE id = '' OR 1=1 -- ' AND password = 'INPUT2'
```
> 싱글쿼터를 닫아주기 위한 싱글쿼터와, OR 1=1 구문을 이용해 WHERE 절을 모두 참으로 만들고, -- 를 넣어줌으로써 뒤의 구문을 모두 주석 처리 해버림. Users 테이블에 있는 모든 정보를 조회하게 됨으로써 가장 먼저 만들어진 계정 (보통 관리자 계정) 으로 로그인할 수 있게 됨 → 관리자 계정 탈취

#### 2. UNION based SQL Injection
- **UNION** : 두 개의 쿼리문에 대한 결과를 통합해 하나의 테이블로 보여주게 하는 키워드
- 정상적인 쿼리문에 하나의 추가 쿼리를 삽입하여 원하는 정보를 획득함
- Injection이 성공하기 위해서는 두 가지의 조건이 있음
- **UNION하는 두 테이블의 컬럼 수가 같아야 하는 것**과, **UNION하는 두 테이블의 데이터 형이 같아야 하는 것**이 있다.

```sql
SELECT * FROM Board WHERE title LIKE '%INPUT%' OR contents '%INPUT%'

SELECT * FROM Board WHERE title LIKE '% ' UNION SELECT null,id,passwd FROM Users -- INPUT%' OR contents '%INPUT%'
```

> 입력값을 tiltle 과 contents 컬럼의 데이터와 비교한 뒤 비슷한 글자가 있는 게시글을 출력한다. 여기서 입력값으로 UNION 키워드와 함께 컬럼 수를 맞춰서 SELECT 구문을 넣어주게 되면 두 쿼리문이 합쳐져서 하나의 테이블로 보여지게 된다. 사용자의 개인정보가 게시글과 함께 화면에 보여짐.

#### 3. Blind SQL Injection (1)
- Boolean based SQL에 사용되는 SQL 문은 limit, SUBSTR, ASCII..
- 특정한 값이나 데이터를 전달받는 것이 아닌, 쿼리를 통해 나온 참과 거짓의 정보만을 통해 정보를 취득함
- 에러가 발생되지 않는 사이트에서는 논리적 에러를 이용하거나 UNION을 이용할 수가 없기 때문에, Blind를 통해 정상적인 쿼리가 수행되는지, 혹은 쿼리가 수행되지 않아 쿼리 결과가 없는지를 판단함
- 서버가 응답하는 성공과 실패 여부를 이용하여 DB의 테이블 정보 등을 추출해 낼 수 있음


```sql
SELECT * FROM Users WHERE id = 'INPUT1' AND password = 'INPUT2'

SELECT * FROM Users WHERE id = 'abc123' and ASCII(SUBSTR((SELECT name FROM information_schema.tables WHERE table_type='base table' limit 0,1),1,1)) > 100 (로그인이 될 때까지 시도) -- INPUT1' AND password = 'INPUT2'
```

> 임의로 가입한 abc123이라는 아이디와 함께 뒤의 구문을 주입한다. 해당 구문은 테이블 명을 조회하는 구문으로 limit 키워드를 통해 하나의 테이블만 조회하고, SUBSTR 함수로 첫 글자만, ASCII를 통해서 ascii 값으로 변환한다. 만약 조회되는 테이블 명이 Users 라면 'U' 자가 ascii 값으로 조회가 될 것이고, 뒤의 100이라는 숫자 값과 비교하게 된다.

> 거짓이면 로그인 실패가 될 것이고, 참이 될 때까지 뒤의 100이라는 숫자를 변경해 가면서 비교하면 된다. 공격자는 이 프로세스를 자동화 스크립트로 만들어 단기간 내에 테이블 명을 알아낼 수 있다.

#### 4. Blind SQL Injection (2)
- Time based SQL에 사용되는 SQL문은  SLEEP, BENCHMARK.. 등이 있다.
- 쿼리 결과를 특정 시간만큼 지연시키는 것
- Blind와 마찬가지로 에러가 발생되지 않는 조건에서 사용하며, 참 혹은 거짓이라는 결과값이 나오지 않으므로 시간을 재는 것
- 궁극적으로는 DB 구조를 파악하기 위함

```sql
SELECT * FROM Users WHERE id = 'INPUT1' AND password = 'INPUT2'

SELECT * FROM Users WHERE id = 'abc123' OR (LENGTH(DATABASE())=1 (SLEEP 할 때까지 시도) AND SLEEP(2)) -- INPUT1' AND password = 'INPUT2'
```

> LENGTH는 문자열의 길이를 반환하고, DATABASE는 DB의 이름을 반환한다 LENGTH(DATABASE()) = 1 이 참이면 SLEEP(2)가 동작하고, 거짓이면 동작하지 않는다. 숫자 1 부분을 조작하여 DB의 길이를 알아낼 수 있다. 단, SLEEP이라는 단어가 치환 처리 되어있는 경우, BENCHMARK나 WAIT 함수를 사용할 수 있다.

#### 5. Stored Procedure SQL Injection
- Stored Procedure (저장 프로시저) : 편의를 위해 일련의 쿼리들을 모아 하나의 함수처럼 모아둔 것
- 대표적으로, MS SQL에서 사용할 수 있는 xp_cmdshell을 통해 윈도우 명령어를 실행할 수 있음 → 자주 악용됨
- 단, 공격자가 시스템 권한을 획득해야 하므로 공격난이도가 높음.
- 공격에 성공한다면 서버에 직접적인 피해를 입힐 수 있음.

#### 6. Mass SQL Injection
- 다량의 SQL Injection 공격
- 한 번의 공격으로 다량의 DB가 조작되어 큰 피해를 입히는 것을 의미
- 보통 MS-SQL을 사용하는 ASP 기반 웹 애플리케이션에서 많이 사용됨
- 쿼리문은 Hex인코딩 방식으로 인코딩하여 공격함
- DB 값을 변조하여 DB에 악성 스크립트를 삽입하고, 사용자들이 변조된 사이트에 접속 시 좀비 PC로 감염되게 함.
- 이렇게 감염된 좀비 PC들은 DDoS 공격에 사용됨.
***- 인코딩 : 문자들의 집합을 부호화하는 방법. 즉, 컴퓨터가 이용할 수 있는 신호로 만드는 것***
***- 인코딩 기준 : ASCII, Hex, URL, Base 64, 유니코드..***
***- Hex : 16을 밑으로 하는 기수법. 0~9와 A~F를 사용하고, 대소문자는 구별하지 않음***
***- DDoS : Distributed Denial of Service. 대량의 패킷 또는 요청을 생성하여 대상 시스템을 마비시키는 공격 기법***

### 3. 방어 방법
#### 1. 입력 값에 대한 검증
- 검증 로직을 추가하여 **미리 설정한 특수문자들이 들어왔을 때 요청을 막아낸다.**

#### 2. Error Message 노출 금지
- 데이터베이스 에러 발생 시 따로 처리를 해주지 않았다면, 에러가 발생한 쿼리문과 함께 에러에 관한 내용을 반환해 준다. 여기서 테이블명, 컬럼명, 쿼리문이 노출이 될 수 있기 때문에, **오류발생 시 사용자에게 보여줄 수 있는 페이지를 따로 제작하거나 메시지박스를 띄우도록 해야한다.**

#### 3. Prepared Statement 구문사용
- 서버의 php파일에 sql 쿼리문이 아래와 같이 고정되어 있고 외부의 입력으로는 이 템플릿을 변경할 수 없다면, ?에 들어가는 데이터는 단순히 문자열로 취급하기 때문에 SQL 인젝션은 발생할 수 없다.

```sql
INSERT INTO MyGuests VALUES(?, ?, ?)
```