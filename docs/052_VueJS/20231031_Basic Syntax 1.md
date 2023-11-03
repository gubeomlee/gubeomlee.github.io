---
layout: default
title: Basic Syntax 1
parent: VueJS
nav_order: 4
---

# Basic Syntax 1

---

## Template Systax

- DOM을 기본 구성 요소 인스턴스 데이터에 선언적으로 바인딩할(Vue Instance와 DOM을 연결) 수 있는 HTML 기반 템플릿 구문을(확장된 문법 제공) 사용한다.

#### Template Systax 종류

- Text Interpolation
- Raw HTML
- Attribute Bindings
- JavaScript Expressions

#### Text Interpolation

```html
<p>Message: {{msg}}</p>
```

- 데이터 바인딩의 가장 기본적인 형태
- 이중 중괄호 구문(콧수염 구문)을 사용
- 콧수염 구문은 해당 구성 요소 인스턴스의 msg 속성 값으로 대체
- msg 속성이 변경될 때마다 업데이트 된다.

#### Raw HTML

```html
<div v-html="rawHtml"></div>
```

```javascript
cont rawHtml = ref('<span style="color:red">This should be red.</span>')
```

- 콧수염 구문은 데이터를 일반 텍스트로 해석하기 때문에 실제 HTML을 출력하기 위해서는 v-html을 사용해야 한다.

#### Attribute Bindings

```html
<div v-bind:id="dynamicId"></div>
```

```javascript
const dynamicId = ref("my-id");
```

- 콧수염 구문은 HTML 속성 내에서 사용할 수 없기 때문에 v-bind를 사용한다.
- HTML의 id 속성 값을 vue의 dynamicId 속성과 동기화 되도록 한다.
- 바인딩 값이 null이나 undefined인 경우 렌더링 요소가 제거된다.

#### JavaScript Expressions

```html
{{number + 1}} {{ok ? 'yes' : 'no'}} {{message.split('').reverse().join('')}}
<div :id="`list-${id}`"></div>
```

- Vue는 모든 데이터 바인딩 내에서 javaScript 표현식의 모든 기능을 지원한다.
- Vue 템플릿에서 JavaScript 표현식을 사용할 수 있는 위치: 콧수염 구문 내부, 모든 directive의 속성 값('v-'로 시작하는 특수 속성)

#### Expression 주의 사항

- 각 바인딩에는 하나의 단일 표현식만 포함될 수 있다.
- 표현식은 값으로 평가할 수 있는 코드조각(return 뒤에 사용할 수 있는 코드여야 한다.)

## Directive

- 'v-'접두사가 있는 특수 속성
- Directive의 속성 값은 단일 JavaScript 표현식이어야 한다. (v-for, v-on 제외)
- 표현식 값이 변경될 때 DOM에 반응적으로 업데이트를 적용한다.
- Directive Arguments: 일부 directive는 directive 뒤에 콜론(:)으로 표시되는 인자를 사용할 수 있다: v-bind:href, v-on:click
- Directive Modifiers: .(dot)로 표시되는 특수 접미사로 directive가 특별한 방식으로 바인딩되어야 함을 나타낸다. .prevent는 발생한 이벤트에서 event.preventDefault()를 호출하도록 v-on에 지시하는 modifier다.

## Dynamically data binding

- 하나 이상의 속성 또는 컴포넌트 데이터를 표현식에 동적으로 바인딩한다.

#### Attribute Bindings

- v-bind는 ':'으로 줄여 쓸 수 있다.
- v-bind:src='imageSrc' 와 :src="imageSrc"는 같다.
- Dynamic attribute name(동적 인자 이름): 대괄호로 감싸서 directive argument에 JavaScript 표현식을 사용할 수 있다. JavaScript 표현식에 따라 동적으로 평가된 값이 최종 argument 값으로 사용된다.

```html
<button :[key]="myValue"></button>
```

#### Class and Style Bindings

- 클래스와 스타일은 모두 속성이므로 v-bind를 사용하여 다른 속성과 마찬가지로 동적으로 문자열 값을 할당할 수 있다.
- 그러나 단순히 문자열 연결을 사용하여 이러한 값을 생성하는 것은 번거럽고 오류가 발생하기 쉽다.
- Vue는 클래스 및 스타일과 함께 v-bind를 사용할 때 객체 또는 배열을 활용한 개선사항을 제공한다.

#### Binding HTML Classes: Binding to Objects

- 객체를 클래스에 전달하여 클래스를 동적으로 전환한다.
- 클래스 객체에 직접 바인딩 가능하다.

```html
<div :class="{active: isActive, 'text-primary': hasInfo}">Text</div>
<div class="static" :class="classObj"></div>
```

```javascript
const isActive = ref(false);
const hasInfo = ref(true);
const classOnj = ref({
  active: isActive,
  "text-primary": hasInfo,
});
```

#### Binding HTML Classes: Binding to Arrays

- :class를 배열에 바인딩하여 클래스 목록을 적용할 수 있다.

```html
<div :class="[activeClass, infoClass]">Text</div>
<div :class="[{active: isActive}, infoClass]">Text</div>
```

```javascript
const activeClass = ref("active");
const infoClass = ref("text-primary");
const isActive = ref(false);
```

#### Binding Inline Styles: Binding to Objects

- :style은 JavaScript 객체 값에 대한 바인딩을 지원한다.
- 실제 CSS에서 사용하는 것처럼 :style은 kebab-cased 키 문자열도 지원한다.(단 camelCase 작성을 권장한다.)
- 스타일 객체에 직접 바인딩 가능하다.

```html
<div :style="{color: activeColor, fontSize: fontSize + 'px'}">Text</div>
<div :style="{color: activeColor, 'font-size': fontSize + 'px'}">Text</div>
<div :style="syleObj"></div>
```

```javascript
const activeColor = ref("crimson");
const fontSize = ref(50);
const styleObj = ref({
  color: activeColor,
  fontSize: fontSize.value + "px",
});
```

#### Binding Inline Styles: Binding to Arrays

- 여러 스타일 객체의 배열에 :style을 바인딩할 수 있다.
- 작성한객체는 병합되어 동일한 요소에 적용된다.

```html
<div :style="[StyleObj1, styleObj2]">Text</div>
```

```javascript
const activeColor = ref("crimson");
const fontSize = ref(50);
const styleObj1 = ref({
  color: activeColor,
  fontSize: fontSize.value + "px",
});

const styleObj = ref({
  color: "blue",
  border: "1px solid black",
});
```

## Event Handling

- DOM 요소에 이벤트 리스너를 연결 및 수신
- v-on을 @으로 줄일 수 있다. v-on:event="handler"와 @evetn="handler"는 같다.
- Inline handlers: 이벤트가 트리거 될 때 실행 될 JavaScript 코드다.
- Method handlers: 컴포넌트에 정의된 메서드 이름이다.

#### Inline Handlers

- 주로 간단한 상황에 사용한다.

```html
<button @click="cont++">Add 1</button>
<p>Count: {{count}}</p>
```

```javascript
const count = ref(0);
```

- 메서드 이름에 직접 바인딩하는 대신 Inline Handlers에서 메서드를 직접 호출할 수 있다. 이렇게 하면 기본 이벤트 대신 사용자 지정 인자를 전달할 수 있다.

```html
<button @click="greeting('hello')">Say hello</button>
<br />
<button @click="greeting('bye')">Say bye</button>
```

```javascript
const greeting = (mes) => {
  consol.log(msg);
};
```

- Inline Handlers에서 원래 DOM이벤트에 접근할 때는 $event 변수를 사용한다.

```html
<button @click="warning('경고입니다.', $event)">Submit</button>
```

```javascript
const warning = (mes, event) => {
  consol.log(msg);
  consol.log(event);
};
```

#### Method Handlers

- Inline Handlers로는 불가능한 대부분의 상황에 사용한다.
- Method Handlers는 이를 트리거 하는 기본 DOM event 객체를 자동으로 수신한다.

```html
<button @click="myFunc"></button>
```

```javascript
const name = ref("Alice");
const myFunc = (event) => {
  consol.log(event);
  consol.log(event.currentTarget);
  consol.log(`Hello ${name.value}!`);
};
```

#### Event Modifiers

- Vue는 v-on에 대한 Event Modifiers를 제공해 event.preventDefault()와 같은 구문을 메서드에서 작성하지 않도록한다.
- stop, prevent, self 등 다양한 modifiers를 제공한다.
- 메서드는 DOM 이벤트에 대한 처리보다는 데이터에 관한 논리를 작성한느 것에 집중한다.
- modifiers는 chained되게 작성할 수 있다. 이때 작성된 순서로 실행되기 때문에 작성 순서에 유의한다.

```html
<form @submit.prevent="onSubmit"></form>
<a @click.stop.prevent="onLink"></a>
```

#### key Modifiers

- Vue는 키보드 이벤트를 수신할 때 특정 키에 관한 별도 modifiers를 사용할 수 있다.

## From Input Bindings

- 양방향 바인딩: form을 처리할 때 사용자가 input에 입력하는 값을 실시간으로 JavaScript 상태에 동기화 해야하는 경우
- v-bind, v-on을 함께 사용하거나 v-model을 사용한다.

#### v-bind, v-on을 함께 사용

- v-bind를 사용하여 input 요소의 value 속성 값을 입력 값으로 사용한다.
- v-on을 사용하여 input 이벤트가 발생할 때마다 input 요소의 value 값을 별도 반응형 변수에 저장하는 핸들러를 호출한다.

```html
<input :value="inputText1" @input="onInput" />
<p>{{inputText1}}</p>
```

```javascript
const inputText1 = ref("");
const onInput = (event) => {
  inputText1.value = event.currentTarget.value;
};
```

#### v-model

- form input 요소 또는 컴포넌트에서 양방향 바인딩을 만든다.
- IME(입력기)가 필요한 언어(한국어, 중국어, 일본어)의 경우 v-model이 제대로 업데이트 되지 않는다.
- 해당 언어에 대해 올바르게 응답하려면 v-bind, v-on 방법을 사용해야 한다.

```html
<input v-model="inputText2" />
<p>{{inputText2}}</p>
```

```javascript
const inputText2 = ref("");
```

- v-model은 단순 text input뿐만 아니라 Checkbox, Radio, Select 등 다양한 타입의 사용자 입력 방식과 함께 사용 가능하다.
- 단일 체크박스는 불리언 값을 활용한다.

```html
<input type="checkedbox" id="checkbox" v-model="checked" />
<br />
<label for="checkbox">{{checked}}</label>
```

```javascript
const checked = ref(false);
```

- 여러 체크박스와 배열에서는 현재 선택된 체크박스의 값이 포함된다.

```html
<input type="checkedbox" id="alice" value="Alice" v-model="checkedNames" />
<br />
<label for="alice">Alice</label>
<input type="checkedbox" id="bella" value="Bella" v-model="checkedNames" />
<br />
<label for="bella">Bella</label>
```

```javascript
const checkedNames = ref([]);
```

- select에서 v-model 표현식의 초기값이 어떤 option 과도 일치하지 않는 경우 select 요소는 선택되지 않은(unselected) 상태로 렌더링 된다.

```html
<select v-model="selected">
  <option disabled value="">Please select one</option>
  <option>Alice</option>
  <option>Bella</option>
  <option>Cathy</option>
</select>
```

```javascript
const selected = ref("");
```
