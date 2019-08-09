# SQL

## 개념

* Structured Query Language
* 데이터베이스에서 사용하는 특수 목적의 프로그래밍 언어
* 자료의 검색과 관리, 데이터베이스 스키마 생성과 수정, 데이터베이스 객체 접근 조정 관리

## 문법

### DDL

* Data Definition Language

* 데이터 정의 언어

* 관계형 데이터베이스 구조(테이블, 스키마)를 정의하기 위한 명령어

* 종류

  * `CREATE` : 테이블을 생성함

    **table명은 복수형으로 지정**

    ```sql
    CREATE TABLE table_name(
    	column_name datatype PRIMARY KEY,
    	column_name datatype,
    	......
    );
    ```

    ```sql
    CREATE TABLE classmates (
        id INTEGER PRIMARY KEY,
        name TEXT,
        age INTEGER,
        address TEXT
    );
    ```

    **기본형**

    ```sql
    CREATE TABLE classmates (
        id INTEGER PRIMARY KEY AUTOINCREMENT, --AUTOINCREMENT : 자동증가
        name TEXT NOT NULL, --NOT NULL 공백 없음
        age INTEGER NOT NULL,
        address TEXT NOT NULL
    );
    ```

  * `DROP` : 특정 table 삭제

    ```sql
    DROP TABLE [삭제하고싶은 table명] : 해당 table을 삭제해줌
    ```

  * `ALTER`

### DML

* Data Manipulation Language

* 데이터 조작 언어

* 데이터를 저장, 수정, 삭제, 조회 등을 하기 위한 언어

* 종류

  * `INSERT` : 특정 table에 새로운 행을 추가하여 데이터를 추가함(**C**)

    **column_name과 column_value의 순서가 동일해야함**

    ```sql
    INSERT INTO [테이블 명] (column_name, column_name, ...) VALUES (column_value, column_value, ...);
    ```

    모든 열에 데이터를 넣을 떄는 column을 명시할 필요 없음

    ```sql
    INSERT INTO [테이블 명] VALUES (column_value, column_value, ...);
    ```

    ```sql
    INSERT INTO classmates VALUES (2, '홍길동', 30, '서울');
    ```

  * `UPDATE` : 특정 table에 특정한 레코드를 수정할 수 있음(**U**)

    ```sql
    UPDATE [테이블 명] SET column_name=column_value, column_name=column_value, ... WHERE condition;
    ```

    ```sql
    UPDATE classmates SET name="홍길동", address="제주도" WHERE id=4;
    ```

  * `DELETE` : 특정 table에 특정한 레코드를 삭제할 수 있음(**D**)

    **기본적으로 id를 기준으로 삭제함**

    * id는 unique하고 테이블 생성시에 `AUTOINCREMENT`지정하면 삭제된 id값은 재사용이 불가함

    ```sql
    DELETE FROM [테이블 명] WHERE condition;
    ```

    ```sql
    DELETE FROM classmates WHERE id=3;
    ```

    

  * `SELECT` : 데이터베이스에서 특정한 테이블 또는 테이블의 특정한 레코드를 반환함(**R**)

    **`*`은 전체를 뜻함**

    ```sql
    SELECT * FROM [테이블 명]; 테이블의 전체 내용을 가져옴
    ```

    ```sql
    SELECT * FROM classmates;
    ```

    특정한 table에서 특정 column만 가져오기

    ```sql
    SELECT column_name, column_name FROM [테이블 명];
    ```

    ```sql
    SELECT id, name FROM classmates;
    ```

    특정한 table에서 특정 column 값 중 특정 개수만 가져오기

    ```sql
    SELECT column_name, column_name FROM [테이블 명] LIMIT num;
    ```

    ```sql
    SELECT id, name FROM classmates LIMIT 2;
    ```

    특정한 table에서 특정 column 값 중 특정 위치에서부터 특정 개수만 가져오기

    ```sql
    SELECT column_name, column_name FROM [테이블 명] LIMIT num OFFSET num;
    ```

    ```sql
    SELECT id, name FROM classmates LIMIT 2 OFFSET 3;
    ```

    특정한 table에서 특정 column 값 중 특정한(원하는) 값만 가져오기

    ```sql
    SELECT column_name, column_name FROM [테이블 명] WHERE column_name=column_value;
    ```

    ```sql
    SELECT id, name FROM classmates WHERE address="서울";
    ```

### DCL

* Data Control Language
* 데이터 제어 언어
* 데이터베이스 사용자의 권한 제어를 위해 사용되는 언어
* 종류
  * `GRANT`
  * `REVOKE`
  * `COMMIT`
  * `ROLLBACk`

### 추가

#### table 생성시

* `PRIMARY KEY` : 기본키, table당 하나의 column값에만 지정할 수 있으며, 일반적으로 id에 부여함

* `AUTOINCREMENT` : 자동 증가, id와 같이 unique한 값을 부여할 때 사용하며, INTEGER에만 사용이 가능함

* `NOT NULL` : 공백으로 비워두면 안되는 경우에 사용하여 공백을 만들지 못하도록함

  ```sql
  CREATE TABLE table_name(
  	column data_type NOT NULL,
  	column data_type NOT NULL
  );
  ```

  ```sql
  sqlite> INSERT INTO classmates (name, age) VALUES ('정찬미', 26);
  Error: NOT NULL constraint failed: classmates.address
  ```

  

#### WHERE

* 특정 조건을 지정할때 사용함

  **string은 연산이 오래걸리기 때문에 int형으로 찾아주는 것이 좋음**

  * users에서 age가 30이상인 사람만 가져오기

    ```sql
    SELECT * FROM users WHERE age >= 30;
    ```

  * users에서 age가 30이상인 사람의 이름만 가져오기

    ```sql
    SELECT first_name FROM users WHERE age >= 30;
    ```

  * users에서 age가 30이상이고 성이 김인 사람의 성과 나이만 가져오기

    ```sql
    SELECT age, last_name FROM users WHERE age >= 30 AND last_name="김";
    ```

  * classmates에서 id가 6인 사람의 주소를 부산으로 바꾸기

    ```sql
    sqlite> SELECT * FROM classmates;
    id          name        age         address   
    ----------  ----------  ----------  ----------
    6           홍길동   25          서울    
    7           고길동   24          창원    
    8           홍창민   29          구미    
    9           홍하린   22          김포    
    10          하수정   23          서울    
    sqlite> UPDATE classmates SET address = '부산' WHERE id = 6;
    sqlite> SELECT* FROM classmates;
    id          name        age         address   
    ----------  ----------  ----------  ----------
    6           홍길동   25          부산    
    7           고길동   24          창원    
    8           홍창민   29          구미    
    9           홍하린   22          김포    
    10          하수정   23          서울    
    ```

* 등호 사용시 3가지 방법 모두 가능함

  ```sql
  sqlite> SELECT * FROM classmates WHERE id = 8;
  id          name        age         address   
  ----------  ----------  ----------  ----------
  8           홍창민   29          구미    
  sqlite> SELECT * FROM classmates WHERE id == 8;
  id          name        age         address   
  ----------  ----------  ----------  ----------
  8           홍창민   29          구미    
  sqlite> SELECT * FROM classmates WHERE id IS 8;
  id          name        age         address   
  ----------  ----------  ----------  ----------
  8           홍창민   29          구미    
  ```

#### LIKE

* 정확한 값에 대한 비교가 아닌, 패턴을 확인하여 해당하는 값을 반환함

  ```sql
  SELECT * FROM [table 명] WHERE column LIKE '';
  ```

  | %    | 2%      | 2로 시작하는 값                              |
  | ---- | ------- | -------------------------------------------- |
  |      | %2      | 2로 끝나는 값                                |
  |      | %2%     | 2가 들어가는 값                              |
  | _    | _2%     | 아무값이나 들어가고 두번째가 2로 시작하는 값 |
  |      | 1___    | 1로 시작하고 4자리인 값                      |
  |      | 2\_%\_% | 2로 시작하고 적어도 3자리인 값               |

  * users에서 20대인 사람의 테이블 가져오기

    ```sql
    SELECT * FROM users WHERE age LIKE '2%';
    ```

#### ORDER BY

* 특정 조건을 기준으로 값을 정렬하여 반환함

  * `ASC` : 오름차순
  * `DESC` : 내림차순

  ```sql
  SELECT columns FROM [table 명] ORDER BY column_name, column_name [ASC|DESC];
  ```

  * users에서 나이순으로 오름차순 정렬하여 상위 10개만 뽑기

    ```sql
    SELECT * FROM users ORDER BY age ASC LIMIT 10;
    ```

  * users에서 나이순, 성 순으로 오름차순 정렬하여 상위 10개만 뽑기

    ```sql
    SELECT * FROM users ORDER BY age, last_name ASC LIMIT 10;
    ```

  * users에서 계좌잔액순으로 내림차순 정렬하여 해당하는 사람이름 10개만 뽑기

    ```sql
    SELECT first_name, last_name FROM users ORDER BY balance DESC LIMIT 10;
    ```

#### Expression

* `COUNT(column)` : 레코드의 갯수를 반환함

  * users의 총 갯수

    ```sql
    sqlite> SELECT COUNT(*) FROM users;
    COUNT(*)
    1000
    ```

* `AVG(column)` : column값의 평균을 반환하며, INTEGER일 때만 가능함

  * users에서 30살 이상인 사람의 계좌 평균 잔액을 가져오기

    ```sql
    SELECT AVG(balance) FROM users WHERE age >= 30;
    ```

* `SUM(column)` : column값의 총 합을 반환하며, INTEGER일 때만 가능함

* `MIN(column)` : column값들 중 최소값을 반환하며, INTEGER일 때만 가능함

* `MAX(column)` : column값들 중 최대값을 반환하며, INTEGER일 때만 가능함

  * users에서 계좌 잔액(balance)이 가장 높은 사람과 액수를 가져오기

    ```sql
    sqlite> SELECT last_name, first_name, MAX(balance) FROM users;
    last_name   first_name  MAX(balance)
    ----------  ----------  ------------
    김         선영      990000
    ```

#### LIMIT & OFFSET

* `LIMIT` : 지정한 개수만큼 반환함

* `OFFSET` : 앞에서부터 특정 개수를 제외하고 LIMIT 개수만큼 출력함, LIMIT와 함께 사용함

  ```sql
  SELECT id, name, age FROM classmates;
  id          name        age       
  ----------  ----------  ----------
  6           홍길동   25        
  7           고길동   24        
  8           홍창민   29        
  9           홍하린   22        
  sqlite> SELECT id, name, age FROM classmates LIMIT 2;
  id          name        age       
  ----------  ----------  ----------
  6           홍길동   25        
  7           고길동   24        
  sqlite> SELECT id, name, age FROM classmates OFFSET 1; --LIMIT없으면 ERROR
  Error: near "1": syntax error 
  sqlite> SELECT id, name, age FROM classmates LIMIT 2 OFFSET 1;
  id          name        age       
  ----------  ----------  ----------
  7           고길동   24        
  8           홍창민   29
  ```

## Datatype

* SQLite는 동적 데이터 타입으로, 기본적으로 affinity에 맞게 들어감

* **BOOLEAN은 정수 0,1로 저장됨**

  | Affinity |                                                              |
  | :------: | :----------------------------------------------------------: |
  | INTEGER  | TINYINT(1byte), SMALLINT(2bytes), MEDUIMINT(3bytes), INT(4bytes), BIGINT(8bytes), UNSIGNED BIG INT |
  |   TEXT   |              CHARACTER(20), VARCHAR(255), TEXT               |
  |   REAL   |                     REAL, DOUBLE, FLOAT                      |
  | NUMERIC  |               NUMERIC, DECIMAL, DATE, DATETIME               |
  |   BLOB   |                    no datatype specified                     |

  

## console 용어

* `sqlite3` : sqlite 콘솔창에 들어감

  ```git
  $ sqlite3 tutorial.sqlite3
  ```

**dot(.)은 console창에서 사용함**

* `.mode csv` : csv를 활용하는 모드로 바꿈

* `.import [임폴트할 파일 명] [새로 만들 db의 table 이름];`

  * 사용하고자 하는 파일을 사용하는 곳으로 가져옴
  * 정보를 가져올 때 text(string)으로 불려옴

  ```sql
  sqlite> .import users.csv users
  ```

* `.database` : 데이터베이스 생성

* `.tables` : table 목록 조회, 어떤 table이 있는지 확인할 수 있음

  * table : 데이터 sheet
  * 데이터 : 전체 excel 파일

* `.read [파일 명]` : sql파일의 코드를 한번에 읽어와서 작성해줌

  * 콘솔창에서 일일이 작성하는 불편함을 없애줌

  ```sql
  'create_table.sql'
  
  CREATE TABLE classmate (
      id INTEGER PRIMARY KEY,
      name TEXT,
      age INTEGER,
      address TEXT
  );
  
  
  'console창'
  sqlite> .read create_table.sql 
  ```

* `.schema [테이블 명]` : 특정 테이블 스키마 조회

  * Scheme(스키마)
    * 설계도
    * 데이터베이스에서 자료의 구조, 표현방법, 관계등을 정의한 구조
    * 각 column이 어떤것이 있고 어떤 자료형 가지고 있는지 정의
    * 테이블(table), 열(column), 레코드/행(row), PK(Primary Key, 기본키) 

* `.mode column` : 열 처럼 출력함(이 선언 전에는 그냥 나열해서 출력됨)

* `.headers on` : 각 열의 title도 같이 출력됨

* `.help` : 도움말

* `.exit`  또는 `ctrl + d`: sqlite에서 나가기

  ```sql
SQLite version 3.8.2 2013-12-06 14:53:30
  Enter ".help" for instructions
Enter SQL statements terminated with a ";"
  sqlite> .exit
piie:~/workspace $ sqlite3
  SQLite version 3.8.2 2013-12-06 14:53:30
Enter ".help" for instructions
  Enter SQL statements terminated with a ";"
sqlite> .mode csv
  sqlite> .import hellodb.csv hellodb
sqlite> .tables
  hellodb
sqlite> SELECT * FROM hellodb;
  1,"길동","홍",600,"충청도",010-2424-1232
  sqlite> .mode column
  sqlite> SELECT last_name, first_name, age FROM hellodb;
  홍         길동      600       
  sqlite> .headers on
  sqlite> SELECT last_name, first_name, age FROM hellodb;
  last_name   first_name  age       
  ----------  ----------  ----------
  홍         길동      600       
  
  ```
  
  ```sql
  piie:~/workspace $ sqlite3 tutorial.sqlite3
  SQLite version 3.8.2 2013-12-06 14:53:30
  Enter ".help" for instructions
  Enter SQL statements terminated with a ";"
  sqlite> .databases
  seq  name             file                                                      
  ---  ---------------  ----------------------------------------------------------
  0    main             /home/ubuntu/workspace/tutorial.sqlite3                   
  sqlite> CREATE TABLE classmates (
     ...> id INTEGER PRIMARY KEY,
     ...> name TEXT
     ...> );
sqlite> .tables
  classmates
  sqlite> .schema classmates
  CREATE TABLE classmates (
  id INTEGER PRIMARY KEY,
  name TEXT
  );
  ```
  
  

