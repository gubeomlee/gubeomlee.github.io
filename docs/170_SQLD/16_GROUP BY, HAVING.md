---
layout: default
title: GROUP BY, HAVING
parent: SQLD
nav_order: 16
---

# GROUP BY, HAVING

---

## 집계함수(Aggregate Function)

- 여러 행들의 그룹이 모여서 그룹당 단 하나의 결과를 돌려주는 함수다.
- GROUP BY 절은 행들을 소그룹화한다.
- SELECT, HAVING, ORDER BY에 사용 가능하다.
- 집계함수명([DISTINCT | ALL] 컬럼 | 표현식) 구조다.
- DISTINCT: 같은 값을 하나의 데이터로 간주하여 하나만 조회한다.
- ALL: 기본 옵션으로 생략가능하다.

#### 집계함수 종류

- COUNT(\*): NULL 값을 포함한 행의 수를 출력한다.
- COUNT(컬럼 | 표현식): 컬럼이나 표현식의 값이 NULL 값인 것을 제외한 행의 수를 출력한다.
- SUM([DISTINCT | ALL] 컬럼 | 표현식): 컬럼이나 표현식의 NULL값을 제외한 합계를 출력한다.
- AVG([DISTINCT | ALL] 컬럼 | 표현식): 컬럼이나 표현식의 NULL값을 제외한 평균을 출력한다.
- MAX([DISTINCT | ALL] 컬럼 | 표현식): 컬럼이나 표현식의 최댓값을 출력한다. 문자, 날짜 타입도 가능하다.
- MIN([DISTINCT | ALL] 컬럼 | 표현식): 컬럼이나 표현식의 최솟값을 출력한다. 문자, 날짜 타입도 가능하다.
- STDDEV([DISTINCT | ALL] 컬럼 | 표현식): 컬럼이나 표현식의 표준편차를 출력한다.
- VARIAN([DISTINCT | ALL] 컬럼 | 표현식): 컬럼이나 표현식의 분산을 출력한다.
- 기타 통계 함수: 벤더별로 다양한 통계식을 제공한다.

```sql
// 예제
SELECT
    COUNT(*) 전체함수,
    COUNT(COMM) COMM건수,
    SUM(SAL) 급여합계,
    ROUND(AVG(SAL)) 급여평균,
    MAX(SAL) 최대급여,
    MIN(SAL) 최소급여,
FROM EMP;
```

## GROUP BY

- GROUP BY는 SQL 문에서 FROM과 WHERE 뒤체 위치한다.
- 데이터들을 작은 그룹으로 분류하여 소그룹에 대한 항목별 통계 정보를 얻을 때 추가로 사용한다.

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
```

## HAVING

- HAVING은 GROUP BY의 조건절이다.
- HAVING은 GROUP BY 뒤에 위치한다.
- WHERE은 집계함수를 사용할 수 없지만 HAVING은 사용할 수 있다.

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
HAVING AVG(SAL)>2000;
```

## GROUP BY, HAVING 특징

- GROUP BY를 통해 소그룹별 기준을 정하고 SELECT에서 집계함수를 사용한다.
- 집계함수를 통해 NULL값을 갖는 행을 제외하고 수행한다.
- GROUP BY에는 SELECT와 달리 ALIAS명을 사용할 수 없다.
- 집계함수는 WHERE에 올 수 없다. (GROUP BY보다 WHERE가 먼저 수행된다.)
- HAVING은 GROUP BY의 기준 항목이나 소그룹의 집계함수를 이용한 조건을 표시할 수 있다.
- GROUP BY에 의해 소그룹별로 만들어진 집계 데이터 중 HAVING의 제한조건을 만족하는 내용만 출력된다.
- HAVING은 일반적으로 GROUP BY뒤에 위치한다.
- GROUP BY에 사용되지 않은 일반 컬럼은 SELECT나 ORDER BY에 사용할 수 없다.

## 집계함수와 NULL

- 테이블의 빈칸을 NULL이 아닌 0으로 표현하기 위해 NVL(Oracle)/ISNULL(SQL Server) 함수를 사용하는 경우가 많다. 다중 행 함수를 사용하는 경우 오히려 불필요한 부하가 발생하므로 NVL함수를 다중 행 함수 안에서 사용할 이유가 없다.
- 다중 행 함수는 입력 값으로 전체 건수가 NULL값인 경우만 함수의 결과가 NULL이 나오고 전체 건수 중 일부만 NULL인 경우는 NULL인 행을 다중 행 함수의 대상에서 제외한다.
