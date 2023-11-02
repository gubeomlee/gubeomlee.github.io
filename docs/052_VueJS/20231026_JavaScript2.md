---
layout: default
title: JavaScript2
parent: VueJS
nav_order: 2
---

# JavaScript

---

## ES6 문법

- spread: 배열로 묶여 있는 값들을 각각의 개별 값으로 풀어준다. 문자열은 각각의 문자로 나눈다.
- rest: 나머지 값들을 모아서 배열로 만든다.
- for/of: for(variable of iterable)

## Promise

#### 콜백함수

- 함수를 매개변수로 전달하여 나중에 실행되도록 하는 것.
- 콜백이 중첩되면 콜백헬이 되어 해석/유지보수가 어려워진다.

#### Promise Object

- 비동기 작업을 마치 동기 작업처럼 값을 반환해서 사용한다.
- 미래의 완료 또는 실패와 그 결과 값을 나타낸다.
- 미래의 어떤 상황에 대한 약속이다.
- resolve(성공 시 사용)
- reject(실패 시 사용)

#### Promise Methods

- .then(callback): Promise 객체를 리턴하고 두 개의 콜백 함수를 인수로 받는다. (이행 했을 때, 거부 했을 때) 콜백 함수는 이전 작업의 성공 결과를 인자로 받는다.
- .catch(callback): .then이 하나라도 실패하면(거부되면) 동작한다. (예외 처리 구문과 유사하다.) 이전 작업의 실패로 인해 생성된 error 객체는 catch 블록 안에서 사용 가능하다.
- .finally(callback): Promise 객체를 반환한다. 결과 상관없이 무조건 실행한다.
- 체이닝 가능

## fetch

#### fetch API

- XMLHttpRequest보다 강력하고 유연한 조작이 가능하다.
- Promise를 지원하므로 콜백 패턴에서 자유롭다.
- ES6 문법은 아니고, BOM(Browser Object Model)객체 중 하나다.
- fetch() 메서드를 사용한다.
- fetch() 메서드는 HTTP 응답을 나타내는 Response 객체를 래핑한 Promise 객체를 반환한다.

#### fetch(resource, options)메서드

- resource: 리소스가 위치한 url 지정
- options: 옵션을 지정한다. method는 HTTP method, headers는 요청 헤더 지정, body는 요청 본문 지정
- fetch 메서드는 Promise 객체를 반환한다.

#### fetch()가 반환하는 Promise 객체

- 성공시 then()을 이용해 처리한다.
- 실패시 catch()를 이용해 처리한다.
