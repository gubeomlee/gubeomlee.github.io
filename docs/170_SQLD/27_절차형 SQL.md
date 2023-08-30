---
layout: default
title: 절차형 SQL
parent: SQLD
nav_order: 27
---

# 절차형 SQL

---

## 절차형 SQL

- PL(Prodedural Language), SQL(Oracle), SQL/PL(DB2), T-SQL(SQL Server) 등의 절차형 SQL이 있다.
- 절차형 SQL에서는 SQL문의 연속적 실행이나 조건에 따른 분기처리를 이용하여 특정 기능을 수행하는 저장 모듈을 생성할 수 있다.

## PL/SQL

- Oracle의 PL/SQL은 블록구조다.
- 블록 내에 DML 문장, QUERY 문장, 절차형 언어(IF, LOOP) 등을 사용할 수 있다.
- 절차적 프로그래밍을 가능하게 하는 트랜잭션 언어다.
- PL/SQL 문장을 데이터베이스 서버에 저장하여 사용자와 애플리케이션 사이에 공유할 수 있도록 만든 일종의 SQL 컴포넌트 프로그램이다.
- Oracle 저장모듈로 Procedure, User Defined Function, Trigger가 있다.

#### PL/SQL 특징

- PL/SQL은 블록구조로 각 기능별 모듈화가 가능하다.
- 변수, 상수 등을 선언하여 SQL 문장 간 값 교환이 가능하다.
- IF, LOOF 등의 절차형 언어를 사용하여 절차적인 프로그램이 가능하다.
- DBMS 정의 에러나 사용자 정의 에러를 정의하여 사용할 수 있다.
- PL/SQL은 Oracle에 내장되어 있어 Oracle과 PL/SQL을 지원하는 서버로 프로그램을 옮길 수 있다.
- PL/SQL은 응용 프로그램의 성능을 향상시킨다.
- PL/SQL은 여러 SQL 문장을 블록으로 묶고 한 번에 블록 전부를 서버로 보내기 때문에 통신량을 줄일 수 있다.

#### PL/SQL 블록 구조

- DECLARE: BEGIN ~ END 절에서 사용될 변수와 인수에 대한 정의 및 데이터 타입을 선언하는 선언부다.
- BEGIN ~ END: 개발자가 처리하고자 하는 SQL문과 여러 가지 비교문, 제어문을 이용하여 필요한 로직을 처리하는 실행부다.
- EXCEPTION: BEGIN ~ END 절에서 실행되는 SQL문이 실행될 때 에러가 발생하면 그 에러를 어떻게 처리할 것인지를 정의하는 예외 처리부다.

## T-SQL

- T-SQL은 근본적으로 SQL Server를 제어하기 위한 언어다.
- ANSI/ISO 표준의 SQL에 약간의 기능을 더 추가해 보완적으로 만든 것이다.
- T-SQL을 이용하여 다양한 저장 모듈을 개발할 수 있다.

#### T-SQL의 프로그래밍 기능

- 변수 선언 기능 @@이라는 전역변수(시스템 함수)와 @이라는 지역변수가 있다.
- 지역변수는 사용자가 자신의 연결 시간 동안만 사용하기 위해 만들어진 변수이며 전역변수는 이미 SQL서버에 내장된 값이다.
- 데이터 유형(int, float, varchar)을 제공한다.
- 산술연산자, 비교연산자, 논리연산자 사용이 가능하다.
- 흐름 제어 기능 IF-ELSE와 WHILE, CASE-THEN 사용이 가능하다.

#### T-SQL 구조

- PL/SQL과 유사하다.
- DECLARE: BEGIN ~ END 절에서 사용될 변수와 인수에 대한 정의 및 데이터 타입을 선언하는 선언부다.
- BEGIN ~ END: 개발자가 처리하고자 하는 SQL문과 여러가지 비교문, 제어문을 이용하여 필요한 로직을 처리하는 실행부다. T-SQL에서는 BEGIN, END 문을 반드시 사용해야 하는 것은 아니지만 블록 단위로 처리하고자 할 때는 반드시 작성해야 한다.
- ERROR 처리: BEGIN - END 절에서 실행되는 SQL문이 실행될 때 에러가 발생하면 그 에러를 어떻게 처리할 것인지를 정의하는 예외 처리부다.

## Trigger

- Trigger는 특정 테이블에 INSERT, UPDATE, DELETE와 같은 DML문이 수행되었을 때 데이터베이스에서 자동으로 동작하도록 작성된 프로그램이다.
- 사용자가 직접 호출하여 사용하는 것이 아니고 데이터베이스에서 자동적으로 수행하게 된다.
- Trigger는 테이블 뷰, 데이터베이스 작업을 대상으로 정의할 수 있으며, 전체 트랜잭션 작업에 대해 발생되는 Trigger와 각 행에 대해서 발생되는 Trigger가 있다.

## 프로시저와 트리거의 차이점

- 프로시저는 BEGIN ~ END 절 내에 COMMIT, ROLLBACK과 같은 트랜잭션 종료 명령어를 사용할 수 있지만 데이터베이스 트리거는 BEGIN ~ END 절 내에 사용할 수 없다.
  | 프로시저 | 트리거 |
  | :----: | :---: |
  | CREATE Procedure 문법 사용 | CREATE Trigger 문법 사용 |
  | EXECUTE 명령어로 실행 | 생성 후 자동으로 실행 |
  | COMMIT, ROLLBACK 실행 가능 | COMMIT, ROLLBACK 실행 안됨 |
