---
layout: default
title: Interceptor
parent: Web_Back
nav_order: 8
---

# Interceptor

---

## Listener & Filter 

#### Listener란
- 프로그래밍에서 Listener란 특정 이벤트가 발생하기를 기다리다가 실행되는 객체다. 
- 이벤트란 특정한 사건 발생을 말한다. 
- 이벤트 소스란 이벤트 발생한 근원지(객체)를 말한다. 

#### Filter
- 요청과 응답데이터를 필터링하여 제어, 변경하는 역할을 수행한다. 
- 사용자의 요청이 Servlet에 전달되기전에 Filter를 거친다. 
- Servlet으로부터 응당이 사용자에게 전달되기 전에 Filter를 거친다. 
- FilterChian을 통해 연쇄적으로 동작할 수 있다. 

## Interceptor 
 
#### Interceptor란
- HandlerInterceptor를 구현 하거나 HandlerInterceptorAdapter를 상속한것이다. 
- 요청(requests)을 처리하는 과정에서 요청을 가로채서 처리한다. 
- 접근제어(Auth), 로그(Log) 등 비즈니스 로직과 구분되는 반복적이고 부수적인 로직 처리한다. 
- HandlerIntercepter의 주요 메서드로 preHandle(), postHandle(), afterCompletion()이 있다. 
- preHandle(): Controller(핸들러) 실행 이전에 호출한다. false를 반환하면 요청을 종료한다. 
- postHandle(): Controller(핸들러) 실행 후에 호출한다. 정상 실행 후 추가 기능 구현 시 사용한다. Controller에서 예외 발생 시 해당 메서드는 실행되지 않는다. 
- afterCompletion(): 뷰가 클라이엍느에게 응답을 전송한 뒤 실행한다. Controller에서 예외 발생 시 네번째 파라미터로 전달된다. 기본은 null이다. 따라서 Controller에서 발생한 예외 혹은 실행 기간 같은 것들을 기록하는 등 후처리 시 주로 사용한다. 