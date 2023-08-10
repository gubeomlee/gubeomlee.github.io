---
layout: default
title: JavaScript
parent: JavaScript Basic
nav_order: 3
---

# JavaScript

---

## JavaScript

- 프로토타입 기반 객체 생성을 지원하는 동적 스크립트 언어다.
- 웹 브라우저에서 주로 사용하고, Node.js를 이용하여 콘솔 환경에서 사용한다.
- js는 인터프리티드 언어다.
- 웹 브라우저의 UI를 제어하기 위해 만들어진 프로그래밍 언어다.
- C언어의 기본 구문을 바탕으로 한다.

## 기본문법

#### 자바스크립트 사용

- \<script> 태그를 이용하여 사용한다. 문서 내의 위치 제약이 없다.
- .js 확장자를 가진 파일을 생성하고, \<script src = "외부파일의 위치">(실행되지 않는 부분)\</script>

#### 변수

- var, let, const가 있는데, ES6이후 var사용을 지양하고, let 사용이 권장된다.
- 동적타입: 대입되는 값에 따라 용도가 달라진다.
- var: 재선언, 재할당 가능하다. 함수 스코프다. 호이스팅 특성이 있다. 변수로 사용한다.
- let: 재선언은 불가능하지만 재할당은 가능하다. 블록 스코프다. 변수로 사용한다.
- const: 재선언, 재할당 불가하다. 블록 스코프다. 선언 시 값을 할당해야 한다. 상수로 사용한다. 대문자 SNAKE_CASE를 사용한다.
- '함수 스코프다.' -> '블록 스코프다.'
- undefined: 변수에 아무 값도 할당되지 않아 타입을 알 수 없는 경우다.

#### 데이터 타입

- 원시형(primitive Type): String, Number, Boolean, null, undefined
- 참조형(객체): 원시형 빼고 전부 다 객체다.
- Symbol: 변경 불가능한 기본타입이다.

```javascript
// 기본적인 방법으로 Symbol 생성
const mySymbol = Symbol();

// 설명을 추가하여 Symbol 생성
const myNamedSymbol = Symbol("description");

// 객체에 Symbol 속성 추가
const user = {
  name: "John",
  age: 30,
};

const symbolKey = Symbol("id");
user[symbolKey] = 12345;

console.log(user[symbolKey]); // 출력: 12345
```

- 숫자는 정수와 실수를 구분하지 않고 부동소수점 형식이다.
- false로 인식되는 값: null, undefined, 0, ''(빈 문자열), NaN
- null의 데이터 타입은 object다. 이는 설계실수다.
- 배열: 크기는 동적이다. 여러가지 데이터 타입을하나의 배열에 입력할 수 있다.

#### 문자열 연산

- 문자열과 숫자타입의 +연산은 문자열 연산이다.
- 문자열과 숫자타입의 +연산 이외의 연산은 숫자 연산이다.

#### 일치연산자

- ==: 타입을 무시하고 값만 비교하기 때문에 예측하기 어렵다.
- ===: 값과 티입이 모두 일치하는비교한다.

#### 호이스팅

- 변수 호이스팅: 변수 선언은 해당 스코프 상단으로 끌어올려진다. 하지만 변수에 할당된 값은 초기화 되기 전까지 'undefined'로 간주된다. 변수 선언은 실제 코드보다 먼저 수행되지만 값이 할당되는 시점은 코드의 순서대로다.
- 함수 호이스팅: 함수 선언은 해당 스코프 상단으로 끌어올려진다. 함수 표현식은 호이스팅되지 않기때문에 호출 전에 효현식이 정의되어야 한다.

## 객체

- 객체는 문자열로 이름을 붙인 값들의 집합체이다. (key : value)
- 객체에 저장하는 값을 프로퍼티(Property)라고 한다.
- 객체는 prototype이라는 프로퍼티를 가지고 있다.
- 객체 값은 .key를 통해 접근한다. 하지만 키 값이 숫자형이거나, 공백을 포함한 문자열인 경우에는 []를 사용하여 접근한다.
- 객체 프로퍼티를 생성할 때는 두가지 방법 모두 가능하다. 숫자형이나, 공백을 포함한 문자열인 경우에는 []를 사용해서 생성한다.
- 객체 프로퍼티는 주소가 저장되어 공유되는 것으로 얕은 복사한 객체는 프로퍼티를 변경했을 때 모두 바뀌게 된다.

```javascript
let student = {
  name: "김코딩",
  age: 20,
  10: 15,
  hobby: ["공부", "숙면"],
  "favorite singer": "아이유",
};
// 주석 처리된 코드는 오류가 발생한다.
console.log(student.name);
// console.log(student[name]);
console.log(student.age);
// console.log(student[age]);
console.log(student.hobby);
// console.log(student[hobby]);

// console.log(student.10);
console.log(student[10]);
// console.log(student.favorite singer);
console.log(student["favorite singer"]);
```

## 함수

- 함수 안에서 this는 함수를 호출한 객체다.
- 화살표 함수는 this를 갖지 않고 외부 스코프의 this를 그대로 사용한다. 따라서 화살표 함수에서 this는 전역 객체를 나타낸다.

```javascript
let m1 = { name: "홍길동" };

function msg() {
  console.log(this);
  console.log(this.name);
}

m1.msg = msg; // 객체 프로퍼티 추가
m1.msg();

// 결과값
{ name: '홍길동', msg: [Function: msg] }
홍길동
```

- JavaScript의 함수는 일급 객체(First-class-citizen)에 해당하여 변수할당, 매개변수로 전달, 반환값으로 사용하가능하다.
- 함수는 객체타입으로 값처럼 사용 가능하다.
- 배열의 요소에 넣거나 객체의 프로퍼티로 설정이 가능하다.
- arguments라는 함수 내부 프로퍼티를 이용하여 매개변수 처리가 가능하다.
- 함수를 변수에 대입하거나 매개변수로 넘길 수 있다.(함수 표현식이 가능하다.)
- 매개변수의 개수가 일치하지 않아도 호출이 가능하다.(호출 시 매개변수의 영향을 받지 않는다.)

```javascript
function func() {
  console.log(arguments.length);
  for (let i = 0; i < arguments.length; i++) {
    console.log(arguments[i]);
  }
}

func(1); // 1 1

func(1, 10, 100); // 3 1 10 100
```

- 함수가 특정값을 리턴하지 않으면 undefined가 반환된다.
- 함수는 오버로딩 안되고 덮어쓰기 된다.
- 함수 선언식: 호이스팅 된다.
- 함수 표현식: 익명함수로 정의 가능하다. 호이스팅 안된다.

```javascript
//함수 선언식
function func() {
  consol.log("hello world");
}
//함수 표현식
let func = function () {
  consol.log("hello world");
};
```
