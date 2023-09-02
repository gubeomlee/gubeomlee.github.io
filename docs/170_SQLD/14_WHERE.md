---
layout: default
title: WHERE
parent: SQLD
nav_order: 14
---

# WHERE

---

## WHERE이란

- 사용자가 원하는 자료만 검색하여 출력하기 위한 조건절이다.
- FROM 다음에 위치한다.

```sql
SELECT * FROM 테이블병 WHERE 조건;
// 예제
SELECT EMPNO, DEPTNO FROM EMP WHERE DEPTNO=10;
```

## 연산자 종류

#### 비교연산자

- =, >, >=, <, <=

```sql
SELECT EMPNO, SAL FROM EMP WHERE SAL>=2000;
```

#### 부정비교연산자

- 같지 않은 것을 조회: !=, ^=, <>, NOT 컴럼명 =
- 크지 않은 것을 조회: NOT 컬럼명 >

```sql
SELECT * FROM EMP WHERE JOB != 'MANAGER';
```

#### 논리연산자

- AND, OR, NOT

```sql
SELECT ENAME, SAL FROM EMP WHERE SAL>=2000;
```

#### SQL연산자

- LIKE'비교문자열': 비교문자열과 형태가 일치(%, \_ 사용)
- BETWEEN A AND B: A와 B값 사이의 값을 조회한다.(A, B값 포함)
- IN(list): list 값 중에서 하나라도 일치하면 조회한다.
- IS NULL: NULL 값을 조회한다.

```sql
SELECT EMPNO, ENAME, JOB, FROM EMP WHERE JOB IN ('CLERK', 'SALESMAN');
SELECT EMPNO, SAL FROM EMP WHERE SAL BETWEEN 1000 AND 2000;
```

#### 부정 SQL연산자

- NOT BETWEEN A AND B: A와 B값 사이에 있지 않은 값을 조회한다. (A, B값 미포함)
- NOT IN(list): list 값 중에서 하나라도 일치하면 조회하지 않는다.
- IS NOT NULL: NULL이 아닌 값을 조회한다.

```sql
SELECT EMPNO, ENAME, JOB, FROM EMP WHERE JOB NOT IN ('CLERK', 'SALESMAN');
SELECT EMPNO, SAL FROM EMP WHERE SAL NOT BETWEEN 1000 AND 2000;
```

#### LIKE 연산자

- 와일드카드를 사용해서 조회한다.
- %: 0개 이상의 문자
- \_: 1개인 단일 문자

```sql
SELECT ENAME, EMPNO FROM EMP WHERE ENAME LIKE '%S';
SELECT ENAME, EMPNO FROM EMP WHERE ENAME LIKE '%A%';
SELECT ENAME, EMPNO FROM EMP WHERE ENAME LIKE '_A%';
SELECT ENAME, EMPNO FROM EMP WHERE ENAME LIKE '____';
```

#### NULL 조회

- 존재하지 않는 것으로 확정되지 않은 값을 표현할 때 사용한다.
- NULL값과 산술연산은 NULL값을 반환한다.
- NULL 값과 비교연산은 알 수 없음(FALSE)를 반환한다.
- NULL 값을 조회할 경우 IS NULL을 사용한다.
- NULL 값이 아닌 것을 조회할 경우 IS NOT NULL을 사용한다.

```sql
SELECT EMPNO, ENAME, MGR FROM EMP WHERE MGR IS NULL;
SELECT EMPNO, ENAME, SAL, COMM, FROM EMP WHERE COMM IS NOT NULL;
```

#### ROWNUM

- ROWNUM은 SQL 처리 결과 집합의 각 행에 임시로 부여되는 일련번호다.
- 테이블이나 집합에서 원하는 만큼의 행만 가져오고 싶을 때 WHERE 절에서 행위 개수를 제한하는 목적으로 사용한다.

```sql
// 한건의 행만 가져오는 방법
SELECT * FROM EMP WHERE ROWNUM=1;
SELECT * FROM EMP WHERE ROWNUM<=1;
SELECT * FROM EMP WHERE ROWNUM<2;
// 2건 이상의 행을 가져오는 방법
SELECT * FROM EMP WHERE ROWNUM<=N;
SELECT * FROM EMP WHERE ROWNUM<N+1;
```

- ROWNUM=1이 포함되어야 한다. 아래 코드 실행 결과 오류가 발생한다.

```sql
SELECT * FROM EMP WHERE ROWNUM=2;
```

#### 연산자 우선순위

1. 괄호()
2. NOT 연산자
3. 비교 연산자, SQL 비교 연산자
4. AND
5. OR
