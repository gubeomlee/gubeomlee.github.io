---
layout: default
title: ORDER BY
parent: SQLD
nav_order: 17
---

# ORDER BY

---

## ORDER BY

- ORDER BY는 SQL문장으로 조회된 데이터들을 다양한 목적에 맞게 특정 칼럼을 기준으로 정렬하여 출력한다.
- ORDER BY에 컬럼명 대신 SELECT에서 사용하는 ALIAS 명이나 컬럼 순서를 나타내는 정수도 사용가능하고 ALIAS명과 정수를 혼용하여 사용할 수 있다.
- 오름차순 정렬이 기본이다.
- 오름차순 정령에서 숫자는 작은 값부터, 날짜는 과거부터 출력한다.
- Oracle은 NULL을 가장 큰 값으로 간주하고, SQL Server는 NULL을 가장 작은 값으로 간주한다.
- SQL 문장의 가장 마지막에 위치한다.

```sql
SELECT 칼럼명 [ALIAS]
FROM 테이블명
WHERE 조건식
GROUP BY 컬럼|표현식
HAVING 그룹조건식
ORDER BY 컬럼|표현식 [ASC|DESC];
```

```sql
// 예제
SELECT
    DEPTNO,
    COUNT(*) 전체행수,
    COUNT(COMM) COMM건수,
    SUM(SAL) 급여합계,
    ROUND(AVG(SAL)) 급여평균,
    MAX(SAL) 최대급여,
    MIN(SAL) 최소급여,
FROM EMP
GROUP BY DEPTNO;
HAVING AVG(SAL)>2000
ORDER BY 급여평균;
```

## SELECT 문장 실행 순서

- GROUP BY, ORDER BY가 같이 쓰일 때 SELECT문은 6개절로 구성된다.
- FROM에서 정의하지 않은 컬럼을 사용하면 에러가 발생한다.
- ORDER BY에서는 SELECT에 나타나지 않은 문자형 항목을 사용할 수 있다.
- SELECT DISTINCT를 지정하거나 SQL문장에 GROUP BY 절이 있거나 SELECT 문에 UINON 연산자가 있으면 열 정의가 SELECT 목록에 표시되어야 한다.
- 관계형 데이터베이스가 데이터를 메모리에 올리 때 행단위 모든 칼럼을 가져온다. SELECT절에서 일부 컬럼만 선택하더라도 ORDER BY에서 메모리에 올라와 있는 다른 컬럼의 데이터를 사용할 수 있다.
  | | 작성순서 | 실행순서 | 설명 |
  | :------: | :------: | :------: | ------------------------------------------ |
  | SELECT | 1 | 5 | 데이터 값을 출력, 계산한다. |
  | FROM | 2 | 1 | 조회 대상 테이블을 참조한다. |
  | WHERE | 3 | 2 | 조회 대상 데이터가 아닌 것을 제거한다. |
  | GROUP BY | 4 | 3 | 행들을 소그룹화한다. |
  | HAVING | 5 | 4 | 소그룹화된 값의 조건에 맞는 것만 출력한다. |
  | ORDER BY | 6 | 6 | 데이터를 정렬한다. |

```sql
// 예제1
SELECT EMPNO, ENAME
FROM EMP
ORDER BY MGR;
// 예제2
SELECT EMPNO, ENAME
FROM (
  SELECT EMPNO, ENAME
  FROM EMP
  ORDER BY MGR
);
// 예제3
SELECT JOB
FROM EMP
GROUP BY JOB
HAVING COUNT(*) > 0
ORDER BY MAX(EMPNO), MAX(MGR), SUM(SAL), COUNT(DEPTNO), MAG(HIREDATE);
```
