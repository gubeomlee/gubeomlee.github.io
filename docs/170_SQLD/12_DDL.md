---
layout: default
title: DDL
parent: SQLD
nav_order: 12
---

# DDL

---

## DDL이란

#### DDL(Data Definition Language)

- DDL는 데이터 구조를 정의하는 명령어다.
- 구조를 생성, 병경, 삭제하거나 이름을 바꾸는 명령어다.
- CREATE: 데이터베이스 객체 생성
- ALTER: 생성된 객체의 구조 변경
- DROP: 객체 삭제
- TRUNCATE: 테이블의 모든 행 삭제, 테이블 초기화, 저장공간 반납

#### DDL 대상

- TABLE: 행과 열로 구성된 데이터 저장 기본 단위
- VIEW: 테이블에서 유도된 가상의 테이블
- INDEX: 데이터베이스 검색속도 향상

#### 데이터 유형

- 데이터 유형은 데이터의 저장 용량, 제약, 값의 범위들을 정의한다.
- 사용되는 모든 데이터는 데이터 유형을 갖는다.
- 다른 유형의 데이터가 입력되거나 크기가 초과되면 에러가 발생하거나 묵시적 형변환이 일어난다.
- CHAR(n): 고정길이 문자열 데이터다. 기본 길이는 1바이트고 n바이트의 길이로 정의된다. 할당된 문자열 길이가 n보다 작으면 그 차이는 빈공간으로 채워진다.
- VARCHAR2(n): VARYING CHAR의 약자다. 기본 길이는 1바이트고 n바이트의 길이로 정의된다. 가변길이로 조정되기 때문에 할당된 변수값의 바이트만 사용한다.
- NUMBER(n,m): 정수, 실수 등의 숫자정보다. n길이의 정수 길이고, m은 소수점 길이다. m새량시 정수를 의미한다.
- DATE: 날짜와 시각 정보다. Oracle은 1초 단위, SQL, SERVER은 3.33ms 단위로 관리한다.

## CREATE TABLE

#### CREATE TABLE

```sql
CREATE TABLE 테이블명(
    <컬럼명> <데이터 유형> <제약조건>,
    <컬럼명> <데이터 유형> <제약조건>,
    <컬럼명> <데이터 유형> <제약조건>,
    CONSTRAINT 기본키명 PRIMARY KEY(컬럼명),
    CONSTRAINT 고유키명 UNIQUE KEY(컬럼명),
    CONSTRAINT 외래키명 FOREIGN KEY(컬럼명)
        REFERENCES 참조테이블(참조테이블 기본키명) ON DELETE CASCADE
);
```

#### 예제

```sql
CREATE TABLE EMP(
    EMPNO NUMBER(10),
    ENAME VARCHAR2(10),
    SAL NUMBER(10,2), DEFAULT 0,
    DEPTNO VARCHAR2(10) NOT NULL,
    TODAY DATE DEFAULT SYSDATE,
    CONSTRAINT EMP_PK PRIMARY KEY(EMPNO),
    CONSTRAINT EMP_FK FOREIGN KEY(DEPTNO) REFERENCES DEPT(DEPTNO) ON DELETE CASCADE
);
```

#### 테이블 생성 시 주의사항

- 객체를 의미할 수 있는 적절한 이름(단수)을 사용한다.
- 알파벳 대/소문자를 사용한다.
- 숫자 0~9를 사용한다.
- 특수기호는 \_, $, # 3가지만 사용가능하다.
- 다른 테이블과 동일 테이블명 사용 불가하다.
- 동일 테이블 내에 동일 컬럼명 불가하다.
- 테이블명으로 예약어 사용 불가하다.

#### 제약조건

- 사용자가 원하는 조건의 데이터만 유지하기 위한 방법이다.
- 기본키, 외래키, 기본값, NOT NULl 등은 테이블을 생성할 때 지정할 수 있다.
- 기본키: 열 옆에 PRIMARY KEY를 입력하면 된다. 하나의 테이블에 하나의 기본키만 가능하다. 단일 열, 여러 열로 구성 가능하다. 기본키로 지정된 열은 NULL 불가하다.
- 고유키: 행 데이터를 식별하기 위해 정의한다. NULL은 코유키 제약 대상이 아니다. NULL 값을 갖는 행이 여러개 있더라도 고유키 제약에 위배되지 않는다.
- 외래키: 테이블 간의 관계를 정의하기 위해 다른 테이블의 기본키를 외래키로 사용한다. ON DELETE CASCAD는 참조하고 있는 테이블의 열 데이터가 삭제되면 같은 데이터가 동시에 삭제된다. ON DELETE CASCAD을 사용하면 참조 무결성을 준수할 수 있다.
- CHECK: 입력할 수 있는 값의 범위 등을 제한한다.
- NOT NULL: NULL 사용이 불가하다.

#### SELECT 문장으로 테이블 생성

- CTAS: CREATE TABLE ~ AS SELECT
- 예제: CREATE TABLE EMP_CATS AS SELECT \* FROM EMP;
- 기존 테이블을 이용한 CTAS 방벙을 사용하면 컬럼별로 데이터 유형을 다시 정의하지 않아도 된다.

## ALTER

- ADD: 테이블에 새로운 컬럼을 추가하거나 제약조건을 추가한다.

```sql
ALTER TABLE <테이블명> ADD (<컬럼명> <자료형> <제약조건>);
// 예제 ALTER TABLE EXAMPLE1 ADD (DATE1 NUMBER(10) NOT NULL);
ALTER TABLE <테이블명> ADD (<제약조건명> <제약조건> <컬럼명>);
```

- MODIFY: 컬럼의 데이터 타입을 변경한다.

```sql
ALTER TABLE <테이블명> MODIFY (<컬럼명> <자료형> <제약조건>);
// 예제 ALTER TABLE EXAMPLE1 MODIFY(DATE1 VARCHAR2(10));
```

- RENAME: 테이블 명 또는 컬럼 명을 변경한다.

```sql
ALTER TABLE <테이블명> RENAME TO <변경 후 테이블명>;
// 예제 ALTER TABLE EXAMPLE1 TO EXAMPLE2;
ALTER TABLE <테이블명> RENAME COLUMN <변경 전 컬럼명> TO <변경 후 컬럼명>;
// 예제 ALTER TABLE EXAMPLE2 RENAME COLUMN DATE1 TO DATE2;
```

- DROP: 테이블의 컬럼 또는 제약조건을 삭제한다.

```sql
ALTER TABLE <테이블명> DROP COLUMN <컬럼명>;
// 예제 ALTER TABLE EXAMPLE2 DROP COLUMN DATE2;
ALTER TABLE <테이블명> DROP CONSTRAINT <제약조건명>;
```

## DROP

- 테이블을 삭제하는 명령어다.
- 테이블의 모든 데이터 및 구조를 삭제한다.

```sql
DROP TABLE <테이블명> [CASCADE CONSTRAINT];
// 예제 DROP TABLE EXAMPLE;
```

## TRUNCATE

- 테이블은 삭제되지 않지만, 해당 테이블에 들어어있던 모든 행들을 제거한다.
- 저장공간 재사용을 가능하도록 해제한다.

```sql
TRUNCATE TABLE <테이블명>;
// 예제 TRUNCATE TABLE EXAMPLE;
```

## VIEW

- 테이블은 데이터를 가지고 있지만 뷰는 데이터를 가지고 있지 않다.
- 뷰는 정의만 가지고 있다.
- 참조한 테이블이 변경되면 뷰도 변경된다.
- 질의에서 뷰가 사용되면 뷰 정의를 참조해서 DBMS내부적으로 질의를 재작성해 질의를 수행한다.

#### 장단점

- 장점: 숨기고 싶은 정보와 같은 특정 컬럼을 제외한 나머지 일부 컬럼만으로 정의할 수 있어 보안기능이 있다. 복잡한 질의를 단순하게 작성할 수 있다. 테이블 구조가 변경되어도 뷰를 사용하는 응용 프로그램은 변경하지 않아도 된다. 하나의 테이블로 열러 개의 뷰를 만들 수 있다.
- 단점: 삽입, 수정, 삭제 연산이 제한적이다. 데이터 구조를 변경할 수 없다. 독자적인 인덱스를 만들 수 없다.

#### 뷰 생성

```sql
CREATE VIEW <뷰명> AS SELECT <컬럼명>, <컬럼명> FROM <테이블명>;
// 예제
CREATE VIEW EMP_VIEW1 AS SELECT EMPNO, ENAME, DEPTNO FROM EMP:
CREATE VIEW EMP_VIEW2 AS SELECT A.EMPNO, A.ENAME, A.DEPTNO, B.LOC FROM EMP A, DEPT B WHERE A.DEPTNO = B.DEPTNO;
```

#### 뷰 조회

```sql
SELECT EMPNO, ENAME FROM EMP_VIEW1;
SELECT EMPNO, ENAME, LOC FROM EMP_VIEW2;
```
