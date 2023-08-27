---
layout: default
title: JOIN
parent: SQLD
nav_order: 20
---

# JOIN

---

## JOIN의 개념

- 두 개 이상의 테이블들을 연결/결합하여 데이터를 추출하는 것을 JOIN이라고 한다.
- 일반적으로 사용되는 SQL문장의 상당수가 JOIN이다.
- 관계형 데이터베이스의 장점이자 핵심기능이다.
- 일반적인 경우 행들은 PRIMARY KEY나 FOREIGN KEY값의 연관에 의해 JOIN이 성립한다.

## EQUI JOIN(등가조인)

- 등가조인은 두 개의 테이블 간에 컬럼 값들이 서로 정확하게 일차하는 경우 사용되는 방법이다.
- 일반적으로 테이블 설계 시에 나타난 PK, FK의 관계를 이용하는 것이지 PK, FK만 사용하는 것은 아니다.
- JOIN 조건은 WHERE에 작성하고 "="연산자를 사용해 나타낸다.

```sql
SELECT *
FROM 테이블1, 테이블2
WHERE 테이블1.컬럼명1 = 테이블2.컬럼명2;
// 예제
SELECT EMP.EMPNO, EMP.ENAME, DEPT.DNAME
FROM EMP, DEPT
WHERE EMP.DEPTNO = DEPT.DEPTNO;
```

## NON-EQUI JOIN(비등가조인)

- 비등가조인은 두개의 테이블 간에 컬럼 값이 서로 정확하게 일치하지 않는 경우 사용된다.
- 비등가종인은 "=" 연산자가 아니라 "Between, >. >=, <, <=" 등의 연산자를 사용한다.

```sql
SELECT *
FROM 테이블1, 테이블2
WHERE 테이블1.컬럼명1 BETWEEN 테이블2.컬럼명1 AND 테이블2.컬럼명2;
// 예제
SELECT EMP.EMPNO, EMP.ENAME, EMP.XAL, SALGRADE.GRADE
FROM EMP, SALGRADE
WHERE EMP.SAL BETWEEN SALGRADE.LOSAL AND SALGRAGE.HISAL;
```

## 3개 이상 TABLE JOIN

```sql
SELECT EMP.EMPNO, EMP.ENAME, DEPT.DNAME, EMP.SAL, SALGRADE.GRADE
FROM EMP, DEPT, SALGRADE
WHERE EMP.DEPTNO = DEPT.DEPTNO AND EMP.SAL BETWEEN SALGRADE.LOSAL AND SALGRADE.HISAL;
```
