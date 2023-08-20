---
layout: default
title: DML
parent: SQLD
nav_order: 13
---

# DML

---

## DML이란

- DML(Data Manipulation Language): 데이터베이스에 저장된 데이터를 조회, 입력, 수정, 삭제하는 명령어다.
- SELECT: 테이블에서 전체 또는 조건을 만족하는 레코드를 조회한다.
- INSERT: 테이블에 새로운 레코드를 입력한다.
- UPDATE: 특정 레코드의 내용을 변경한다.
- DELETE: 특정 레코드를 삭제한다.

## SELECT

```sql
SELECT * FROM 테이블명;
SELECT <컬럼명1> AS 별명1, <컬럼명2> AS 별명2 FROM 테이블명;
// 예제
SELECT * FROM EMP;
SELECT EMPNO, ENAME, FROM EMP;
SELECT EMPNO AS A, ENAME AS B FROM EMP;
SELECT EMPNO A, ENAME B FROM EMP;
// 소문자도 같은 결과 조회된다.
select empno a, ename b from emp;

```

## INSERT

```sql
INSERT INTO 테이블명 VALUES(입력값1, 입력값2);
INSERT INTO 테이블명(<컬럼명1>, <컬럼명2>) VALUES(입력값1, 입력값2);
// 예제
INSERT INTO EMP VALUES(7934, 'PAUL', 'CLERK', 7782, TO_DATE('25-05-1982', 'DD-MM-YYYY'), 1300, NULL, 10);
INSERT INTO EMP(EMPNO, ENAME, DEPTNO) VALUES(7934, 'PAUL', 10);
```

## UPDATE

```sql
UPDATE 테이블명 SET 컬럼명=입력값;
UPDATE 테이블명 SET 컬럼명=입력값 WHERE 조건절;
// 예제
UPDATE TEST1 SET C=888;
UPDATE TEST1 SET C=999 WHERE A=111;
UPDATE TEST1 SET B='DDD' WHERE A=444 AND C=888;
UPDATE TEST1 SET B='FFF' WHERE A=111 OR C=888;
```

## DELETE

```sql
DELETE FROM 테이블명;
DELETE FROM 테이블명 WHERE 조건절;
// 예제
DELETE FROM TEST1;
DELETE FROM TEST1 WHERE C=333;
DELETE FROM TEST1 WHERE B='CCC';
DELETE FROM TEST1 WHERE A=111 AND C=333;
DELETE FROM TEST1 WHERE B=NULL;
DELETE FROM TEST1 WHERE B IS NULL;
DELETE FROM TEST1 WHERE C IS NOT NULL;
```

## DELETE, DROP, TRUNCATE

|    구분     |          DELETE          |        DROP         |      TRUNCATE       |
| :---------: | :----------------------: | :-----------------: | :-----------------: |
| 명령어 분류 |      데이터만 삭제       |     테이블 삭제     |    테이블 초기화    |
|    삭제     |       로그기록존재       |    로그기록삭제     |    로그기록삭제     |
|  로그기록   |       로그기록존재       |    로그기록삭제     |    로그기록삭제     |
|   디스크    | 디스크사용량 초기화 안됨 | 디스크사용량 초기화 | 디스크사용량 초기화 |

## Nologging

- 데이터베이스에 데이터를 입력하면 Redo Log가 쌓인다.
- Redo Log가 쌓이면 I/O가 발생하게 되어 속도가 느려진다.
- Nologging은 Log 기록을 최소화시켜 입력 시 성능을 향상시키는 기법이다.

```sql
ALTER TABLE 테이블명 NOLOGGING;
ALTER TABLE 테이블명 LOGGING;
//예제
ALTER TABLE EMP NOLOGGING;
ALTER TABLE EMP LOGGING;
```
