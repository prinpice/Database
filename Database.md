# Database

## 데이터베이스(DB)

* 체계화된 데이터의 모임

  cf) 파일은 체계화되어있지 않아 수정, 삭제가 어려움

* CRUD(Create Read Update Delete) operation

### RDBMS(관계형 데이터베이스 관리 시스템)

* 관계형 데이터베이스 : 행과 열로 이룸(excel)
* 오픈소스
  * MySQL
  * SQLite
  * PostgreSQL

#### SQLite

- 서버가 아닌 응용 프로그램에 넣어 사용하는 비교적 가벼운 데이터베이스

- 파일 형태로 되어 있음

- 구글 안드로이드, 임베디드 소프트웨어(냉장고 등)등에 활용됨

- 로컬에서 간단한 DB구성 가능

- 용어

  - 말을 걸 때 말 앞에 (.)을 찍어 보냄

  - dot(.)으로 시작하면 문장끝에 세미콜론(;)을 찍지 않음

    ```git
    ~/workspace $ sqlite3
    SQLite version 3.8.2 2013-12-06 14:53:30
    Enter ".help" for instructions
    Enter SQL statements terminated with a ";"
    sqlite> .exit
    ```

### Scheme(스키마)

* 데이터베이스의 구조와 제약 조건에 관련한 전반적인 명세를 기술한 것
* 설계도
* 데이터베이스에서 자료의 구조, 표현방법, 관계 등을 정의한 구조
* 각 column이 어떤것이 있고 어떤 자료형 가지고 있는지 정의
* 테이블(table), 열(column), 레코드/행(row), PK(Primary Key, 기본키)
  * 열(Column) 
    * 각 열에는 고유한 데이터 형식이 지정됨
    * INTEGER TEXT NULL 등
  * 행(row), 레코드
    * 테이블의 데이터는 행에 저장됨
  * PK(기본키)
    * 각 행(레코드)의 고유값으로 Primary Key로 불림
    * 반드시 설정해야하며, 데이터베이스 관리 및 관계 설정시 주요하게 활용됨