---
layout: default
title: DCL
parent: SQLD
nav_order: 18
---

# DCL

---

## DCL(Data Control Language)의 개념

- DCL은 유저를 생성하고 권한을 제어하는 명령어다.
- 대부분의 데이터베이스는 데이터 보호와 보안을 위해서 유저와 권한을 관리한다.
- Oracle을 설치하면 SYS, SYSTEM, SCOTT유저가 제공된다.
- SYS: DBA ROLE을 부여받은 유저.
- SYSTEM: 데이터베이스의 모든 시스템 권한을 부여받은 DBA유저. Oracle 설치 완료 시 패스워드를 설정한다.

## DCL 명령어

- GRANT: 권한을 부여하는 명령어
- REVOKE: 주어진 권한을 회수하는 명령어

```sql
CREATE USER SQLD IDENTIFIED BY 1234;

GRANT CREATE SESSION TO SQLD;

CONN SQLD/1234;

CREATE USER SQLD1 IDENTIFIED BY 1234;

CONN SQLD_STUDY/1234;

REVOKE CREATE SESSION FROM SQLD;
```

## 오브젝트 권한

#### 권한

- ALTER: 지정된 테이블에 대해서 수정할 수 있는 권한을 부여한다.
- DELETE: 지정된 테이블에 대해서 DELETE권한을 부여한다.
- ALL: 테이블에 대한 모든 권한을 부여한다.
- INDEX: 지정된 테이블에 대해서 인덱스를 생성할 권한을 부여한다.
- INSERT: 지정된 테이블에 대해서 INSERT권한을 부여한다.
- REFERENCES: 지정된 테이블을 참조하는 제약조건을 생성하는 권한을 부여한다.
- SELECT: 지정된 테이블에 대해서 SELECT권한을 부여한다.
- UPDATE: 지정된 테이블에 대해서 UPDATE권한을 부여한다.

#### 오브젝트 권한과 오브젝트와의 관계

|  객체권한  | 테이블 | VIEWS | SEQUENCE | PROCEDURE |
| :--------: | :----: | :---: | :------: | :-------: |
|   ALTER    |   O    |       |    O     |           |
|   DELETE   |   O    |   O   |          |           |
|  EXECUTE   |        |       |          |     O     |
|   INDEX    |   O    |       |          |           |
|   INSERT   |   O    |   O   |          |           |
| REFERENCES |   O    |       |          |           |
|   SELECT   |   O    |   O   |    O     |           |
|   UPDATE   |   O    |   O   |          |           |

## Role을 이용한 권한 부여

#### Role의 개념

- 유저를 생성하면 기본적으로 CREATE SESSION, CREATE TABLE, CREATE PROCEDURE 등 많은 권한을 부여해야 한다.
- ROLE은 많은 데이터베이스에서 유저들과 권한들 사이에서 중개 역할을 한다.
- ROLE은 유저에게 직접 부여될 수도 있고, 다른 ROLE에 포함하여 유저에게 부여될 수도 있다.

#### 예제

```sql
CONN SYSTEM/MANAGER;

REVOKE CREATE SESSION,
CREATE TABLE
FROM SQLD:

CONN SQLD/1234
```

```sql
CONN SYSTEM/MANAGER;

CREATE ROLE LOGIN_TABLE;

GRANT CREATE SESSION, CREATE TABLE TO LOGIN_TABLE;

GRANT LOGIN_TABLE TO SQLD;

CONN SQLD/1234;

CREATE TABLE MENU2(
    MENU_SEQ NUMBER NOT NULL,
    TITLE VARCHAR2(10));
```
