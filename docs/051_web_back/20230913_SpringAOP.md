---
layout: default
title: SpringAOP
parent: Java Basic
nav_order: 6
---

# SpringAOP

---

## AOP(Aspect Oriented Programming)

#### AOP

- OOP에서 모듈화의 핵심 단위는 클래스인 반면, AOP에서 모듈화의 단위는 Aspect다.
- Aspect는 여러 타입과 객체에 거쳐서 사용되는 기능(Cross Cutting, 트랜잭션 관리 등)을 모듈화한 것이다.
- Spring framework의 필수요소는 아니지만, AOP프레임워크는 Spring IoC를 보완한다.

#### AOP 용어

- Aspect: 여러 클래스에 공통적으로 구형되는 괌심사(Concern)을 모듈화한 것이다.
- Join Point: 메서드 실행이나 예외처리와 같은 프로그램 실행중의 특정 지점이다. Spring에서는 메서드 실행을 의미한다.
- Pointcut: Join Point에 Aspect를 적용하기 위한 조건을 서술한다. Aspect는 지정한 pointcut에 일치하는 모든 join point에서 실행된다.
- Advice: 특정 조인포인트(Join Point)에서 Aspect에 의해서 취해진 행동이다. Around, Before, After 등의 Advice 타입이 존재한다.
- Target 객체: 하나이상의 advice가 적용될 객체다. Spring AOP는 Running Proxy를 사용하여 구현되므로 객체는 항상 Proxy 객체가 된다.
- AOP Proxy: AOP를 구현하기 위해 AOP 프레임워크에 의해 생성된 객체다. Spring Framework에서 AOP 프록시는 JDK dynamic proxy 또는 CGLIB proxy이다.
- Weaving: Aspect를 다른 객체와 연결하여 Advice 객체를 생성한다. 런타임 또는 로딩 시 수행할 수 있지만 Spring AOP는 런타임에 위빙을 수행한다.

#### Spring AOP Proxy

- 실제 기능이 구현된 Target 객체를 호출하면, target이 호출되는 것이 아니라 advice가 적용된 Proxy 객체가 호출된다.
- Spring AOP는 기본값으로 표준 JDK dynamic proxy를 사용한다.
- 인터페이스를 구현한 클래스가 아닌 경우 CGLIB 프록시를 사용한다.

#### Spring AOP

- @AspectJ: 일반 Java 클래스를 Aspect로 선언하는 스타일이다. AspectJ 프로젝트에 소개되었다.
- Spring AOP에서는 pointcut 구문 분석, 매핑을 위해서 AspectJ 라이브러리를 사용한다.
- 하지만 AOP runtime은 순수 Spring AOP이며, AspectJ에 대한 종속성은 없다.

#### Advice Type

- before - target: 메서드 호출 이전
- after - target: 메서드 호출 이후, Java exception 문장의 finally와 같이 작동한다.
- after returning - target: 메서드 정상 동작 후 작동한다.
- after throwing - target: 메서드 에러 발생 후 작동한다.
- around - target: 메서드의 실행 시기, 방법, 실행 여부를 결정한다.
