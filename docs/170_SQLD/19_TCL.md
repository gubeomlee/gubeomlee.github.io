---
layout: default
title: TCL
parent: SQLD
nav_order: 19
---

# TCL

---

## 트랜잭션이란

- 트랜잭션은 데이터베이스의 논리적 연산단위다.
- 밀접히 관련되어 분리될 수 없는 한 개 이상의 데이터베이스 조작을 가리킨다.
- 하나의 트랜잭션에는 하나 이상의 SQL문이 포함된다.
- 트랜잭션은 분할 할 수 없는 최소의 단위다. (전부 적용하거나 전부 취소한다.)

#### 트랜잭션의 특성

- 원자성: 트랜잭션에서 정의된 연산들은 모두 성공적으로 실행되던지 아니면 실행되지 않은 상태다.(ALL OR NOTHING)
- 일관성: 트랜잭션이 실행되기 전의 데이터베이스 내용이 잘못 되어 있지 않다면 트랜잭션이 실행된 이후에도 데이터베이스의 내용에 잘못이 있으면 안된다.
- 고립성: 트랜잭션이 실행되는 도중에 다른 트랜잭션이 접근할 수 없다.
- 영속성: 트랜잭션이 성공적으로 수행되면 그 트랜잭션이 갱신한 데이터베이스의 내용은 영구적으로 저장된다.

## TCL(Transaction Control Language)

- TCL은 트랜잭션을 제어하는 명령어다.
- COMMIT: 변경한 데이터를 데이터베이스에 반영시키는 명령어다.
- ROLLBACK: 트랜잭션 시작 이전의 상태로 되돌리는 명령어다.
- SAVEPOINT: 하나의 트랜잭션을 작게 분할하여 저장하는 기능을 수행하는 명령어다.

#### COMMIT

- INSERT, UPDATE, DELETE로 변경한 데이터를 데이터베이스에 반영하는 명령어다.
- COMMIT 이후 복구 불가능하다.
- DDL(CREATE, ALTER, DROP, RENAME, TRUNCATE)은 자동으로 COMMIT이 실행되어 데이터베이스에 저장된다.
- DML(INSERT, UPDATE, DELETE)은 COMMIT을 실행해야만 데이터베이스에 반영된다.

#### ROLLBACK

- 트랜잭션을 취소하는 명령어다.
- ROLLBACK을 싱행하면 마지막 COMMIT 지점으로 돌아간다.

#### SAVEPOINT

- ROLLBACK시 돌아가는 저장포인트를 지정하는 명령어다.
- 하나의 트랜잭션에 여러 개의 SAVEPOINT 지정이 가능하다.

#### SQL Server의 트랜잭션 방법

- SQL Server에서는 기본적으로 AUTO COMMIT 모드이기 때문에 DML 수행 후 COMMIT을 실행할 필요가 없다.
- SQL Server에서의 트랜잭션은 AUTO COMMIT, 암시적 트랜잭션, 명시적 트랜잭션 3가지 방식으로 이루어진다.
- AUTO COMMIT: 기본방식이다. DML, DDL 수행할 때마다 DBMS가 트랜잭션을 컨트롤한다. 명령어가 성공적으로 수행되면 자동으로 COMMIT을 수행한다. 오류가 발생하면 자동으로 ROLLBACK을 수행한다.
- 암시적(묵시적) 트랜잭션: Oracle과 같은 방식으로 처리한다. 트랜잭션의 시작은 DBMS가 처리하고 트랜잭션의 끝은 사용자가 명시적으로 COMMIT 또는 ROLLBACK으로 처리한다. 인스턴스 단위 또는 세션 단위로 설정한다.
- 명시적 트랜잭션: 트랜잭션의 시작과 끝을 모두 사용자가 명시적으로 지정한다. BEGIN TRANSACTION(BEGIN TRAN 구문도 가능)으로 트랜잭션을 시작하고 COMMIT TRANSACTION(TRANSACTION 생략가능) 또는 ROLLBACK TRANSACTION(TRANSACTION 생략가능)으로 트랜잭션을 종료한다. ROLLBACK 구문을 만나면 최초의 BEGIN TRANSACTION 시점까지 모두 ROLLBACK 수행한다.

#### AUTO COMMIT

```sql
CREATE TABLE SQLD1
(
    A NUMBER(10)
);

SELECT * FROM SQLD1;

INSERT INTO SQLD1 VALUES(10);

SELECT * FROM SQLD1;

COMMIT;

SELECT * FROM SQLD1;
```

#### COMMIT, ROLLBACK

```sql
CREATE TABLE SQLD2
(
    A NUMBER(10)
);

SELECT * FROM SQLD2;

INSERT INTO SQLD2 VALUES(10);

ROLLBACK;

SELECT * FROM SQLD2;

INSERT INT SQLD2 VALUES(20);

ALTER TABLE SQLD2 ADD (B NUMBER(10));

SELECT * FROM SQLD2;

ROLLBACK;

SELECT * FROM SQLD2;

INSERT INTO SQLD2 VALUES(30, 30);

SELECT * FROM SQLD2;

SAVEPOINT SV1;

ROLLBACK;

INSERT INTO SQLD2 VALUES(30, 30);

SELECT * FROM SQLD2;

SAVEPOINT SV1;

ROLLBACK TO SV1;

SELECT * FROM SQLD2;

INSERT INT SQLD2 VALUES(40, 40);

SELECT * FROM SQLD2;

SAVEPOINT SV2;

ALTER TABLE SQLD2 DROP;

SELECT * FROM SQLD2;

ROLLBACK TO SV2;
```

#### COMMIT, ROLLBACK, SAVEPOINT

```sql
CREATE TABLE SQLD3
(
    A NUMBER(10)
);

SELECT * FROM SQLD3;

INSERT INTO SQLD3 VALUES(10);

INSERT INTO SQLD3 VALUES(20);

SAVEPOING SV1;

INSERT INTO SQLD3 VALUES(30);

INSERT INTO SQLD4 VALUES(40);

SAVEPOINT SV1;

INSERT INTO SQLD3 VALUES(50);

SELECT * FROM SQLD3;

ROLLBACK TO SV1;

SELECT * FROM SQLD3;
```
