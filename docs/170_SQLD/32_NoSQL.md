---
layout: default
title: NoSQL
parent: SQLD
nav_order: 32
---

# NoSQL

---

## MongoDB

- MongoDB는 대표적인 NoSQL 문서형 데이터베이스다.
- JSON과 유사 형식으로 문서화한다.
- 각각의 문서는 필드-값의 형태로 가지고 있고, 컬렉션이라고 하는 그룹으로 묶어서 관리한다.

## NoSQL 데이터베이스 사용이 적합한 경우

- 비구조적인 대용량의 데이터를 저장하는 경우
- 클라우드 컴퓨팅 및 저장공간을 최대한 활용하는 경우: 수평적 확장이 용이하여 서버 분리가 쉽다.
- 빠르게 서비스를 구축하고 데이터 구조를 자주 업데이트 하는 경우: 스키마 준비가 필요 없어 빠른 개발이 가능하다. 데이터 구조를 자주 변경해야 하는 경우 NoSQL이 적합하다.

## 용어정리

- 레플리카 세트: 동일한 데이터를 저장하는 소수의 연결된 머신을 말한다. 하나에 문제가 생기더라도 데이터를 유지할 수 있다.
- 인스턴스: 특정 소프트웨어를 실행하는 단일 머신으로 MongoDB에서는 데이터베이스를 뜻한다.
- 클러스터: 데이터를 저장하는 서버그룹으로 여러 컴퓨터를 연결하여 하나의 컴퓨터처럼 동작하게 한다.
- 도큐먼트: 필드-값 쌍으로 저장되는 데이터
- 필드: 데이터 포인트를 위한 고유 식별자
- 값: 주어진 식별자와 연결된 데이터
- 컬렉션: MongoDB의 도큐먼트로 구성된 저장소로 일반적으로 도큐먼트 간의 공통 필드가 있다. 데이터베이스 당 많은 컬렉션이 있고, 컬렉션 당 많은 도큐먼트가 있을 수 있다.

## JSON vs BSON

- JSON 형식은 읽기 쉽지만 파싱이 느리고 메모리 사용이 비효율 적이다.
- BSON 형식은 이진법에 기반을 둔 표현법이다. 메모리 사용이 빠르고 유연하다는 장점이 있다.
- MongoDB에서는 JSON 형식으로 작성된 것을 무엇이든 추가하고 조회할 수 있다. 다만 내부에서는 BSON 형식으로 저장, 사용하고 있다.