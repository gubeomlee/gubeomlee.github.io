---
layout: default
title: Basic Syntax 2
parent: VueJS
nav_order: 5
---

# Basic Syntax 2

---

## Computed Property

#### computed()

- 계산된 속성을 정의하는 함수: 미리 계산된 속성을 사용하여 템플릿에서 표현식을 단순하게 하고 불필요한 반복 연산을 줄인다.
- 아래 계산을 여러 번 사용하는 경우 반복이 발생한다.

```html
<p>{{todos.length > 0 ? '아직 남았다.' : '퇴근'}}</p>
```

```javascript
const todos = ref([{ text: "Vue 실습" }, { text: "자격증 공부" }, { text: "TIL 작성" }]);
```

- 반응성 데이터를 포함하는 복잡한 로직의 경우 computed를 활용하여 미리 값을 계산한다.

```html
<p>{{restOfTodos}}</p>
```

```javascript
const { createApp, ref, computed } = Vue;
const todos = ref([{ text: "Vue 실습" }, { text: "자격증 공부" }, { text: "TIL 작성" }]);
const restOfTodos = computed(() => {
  return todos.value.length > 0 ? "아직 남았다." : "퇴근";
});
```

- 반환되는 값은 computed ref이며 일반 refs와 유사하게 계산된 결과를 .value로 참조할 수 있다.
- 템플릿에서는 .value 생략 가능하다.
- computed 속성은 의존된 반응형 데이터를 자동으로 추적한다.
- 의존하는 데이터가 변경될 때만 재평가한다. 위의 restOfTodos의 계산은 todos에 의존하고 있다. 따라서 todos가 변경될 때만 restOfTodos가 업데이트된다.

#### computed와 method의 차이

- computed 속성 대신 method로도 동일한 기능을 정의할 수 있다.
- computed 속성은 의존된 반응형 데이터를 기반으로 캐시(cached)된다.
- computed 속서은 의존하는 데이터가 변경된 경우에만 재평가한다. 즉 의존된 반응형 데이터가 변경되지 않는 한 이미 계산된 결과에 대한 여러 참조는 다시 평가할 필요 없이 이전에 계산된 결과를 즉시 반환한다.
- 반면 method 호출은 렌더링이 발생할때마다 항상 함수를 실행한다.

#### Cache(캐시)

- 데이터나 결과를 일시적으로 저장해두는 임시 저장소다.
- 이후에 같은 데이터나 결과를 다시 계산하지 않고 빠르게 접근할 수 있도록 한다.
- 웹 페이지 캐시 데이터: 페이지 데이터 일부를 브라우저 캐시에 저장한다. 저장 후 같은 페이지에 요청 시 모든 데이터를 다시 응답 받는 것이 아니라 캐시 된 데이터를 사용하여 더 빠르게 렌더링이 가능하다.

#### computed와 method의 적절한 사용처

- computed: 의존하는 데이터에 따라 결과가 바뀌는 계산된 속성을 만들 때 유용하다. 동일한 의존성을 가진 여러 곳에서 사용할 때 계산 결과를 캐싱하여 중복 게산을 방지한다.
- method: 단순히 특정 동작을 수행하는 함수를 정의할 때 사용한다. 테이터에 의존하는지 여부와 관계없이 항상 동일한 결과를 반환한다.
- computed는 의존된 데이터가 변경되면 자동으로 업데이트 된다. method는 호출해야만 실행된다. 사용목적과 상황에 맞게 computed와 method를 적절히 조합하여 사용한다.

## Conditional Rendering

#### v-if

- 표현식 값의 T/F를 기반으로 요소를 조건부 렌더링한다.
- v-else directive를 사용하여 v-if에 대한 else 블록을 나타낼 수 있다.

```html
<p v-if="isSeen">true일 때 보여요.</p>
<p v-else>false일 때 보여요.</p>
<button @click="isSeen = !isSeen">토글</button>
```

```javascript
const isSeen = ref(true);
```

- v-else-if directive를 사용하여 v-if에 대한 else if 블록을 나타낼 수 있다.

```html
<div v-if="name === 'Alice'">Alice입니다.</div>
<div v-else-if="name === 'Bella'">Bella입니다.</div>
<div v-else-if="name === 'Cathy'">Cathy입니다.</div>
<div v-else>아무도 아닙니다.</div>
```

```javascript
const name = ref("Cathy");
```

#### HTML <template> element

- 여러요소에 대한 v-if를 적용기 위해서 template 요소에 v-if를 사용한다.

```html
<template v-if="name === 'Cathy'">
  <div>Cathy입니다.</div>
  <div>나이는 30살 입니다.</div>
</template>
```

- 페이지가 로드될 때 렌더링 되지 않지만 JavaScript를 사용하여 나중에 문서에서 사용할 수 있도록 하는 HTML을 보유하기 위한 메커니즘이다.
- 보이지 않는 wrapper 역할을 한다.

#### v-show

- 표현식 값의 T/F를 기반으로 요소의 가시성을 전환한다.
- 항상 렌더링 되어 DOM에 남아 있다. CSS display 속성만 전환한다.

#### v-if, v-shoe 비교

- v-if(cheap initial load, expensive toggle): 초기 조건이 false인 경우 아무 작업도 수행하지 않는다. 토글 비용이 높다.
- v-shoe(expensive initial load, cheap toggle): 초기 조건에 관계 없이 항상 렌더링 한다. 초기 렌더링 비용이 높다.
- 무언가를 매우 자주 전환해야 하는 경우에는 v-show를, 실행 중에 조건이 변경되지 않는 경우에는 v-if를 권장한다.

## List Rendering

#### v-for

- 소스 데이터를 기반으로 요소 또는 템플릿 블록을 여러번 렌더링한다.
- v-for는 alias in expression 형식의 특수 구문을 사용하여 반복되는 현재 요소에 대한 별칭(alias)을 제공한다.
- 인덱스(객체에서 키)에 대한 별칭을 지정할 수 있다.

```html
<div v-for="item in items">{{item.text}}</div>
<div v-for="(item, index) in items"></div>
<div v-for="value in object"></div>
<div v-for="(value, key) in object"></div>
<div v-for="(value, key, index) in object"></div>
```

#### v-for with key

- v-for와 key를 함께 사용한다. key는 반드시 각 요소에 대한 고유한 값을 나타낼 수 있는 식별자를 사용한다.
- 내부 컴포넌트의 상태를 일관되게 유지한다.
- 데이터의 예측 가능한 해동을 유지한다.(Vue 내부 동작 관련)

#### v-for with v-if

- 동일 요소에 v-for와 v-if를 함께 사용하지 않는다.
- 동일한 요소에서 v-if가 v-for보다 우선순위가 더 높아 v-if 조건은 v-for 범위의 변수에 접근할 수 없다.

#### 해결방법1: computed를 활용해 필터링 된 목록을 반환하여 반복한다.

```html
<ul>
  <li v-for="todo in completeTodos" :key="todo.id">{{todo.name}}</li>
</ul>
```

```javascript
const completeTodos = computed(() => {
  return todos.value.filter((el) => el.isComplete);
});
```

#### 해결방법2: v-for와 template 요소를 사용하여 v-if 이동

```html
<ul>
  <template v-for="todo in todos" :key="todo.id">
    <li v-if="todo.isComplete">{{todo.name}}</li>
  </template>
</ul>
```

## Waechers

#### watch()

- 반응형 데이터를 감시하고, 감시하는 데이터가 변경되면 콜백 함수를 호출한다.

```javascript
watch(variable, (newValue, oldValue) => {});
```

- variable: 감시하는 변수
- newValue: 감시하는 변수가 변화된 값. 콜백함수의 첫번재 인자.
- oldValue: 콜백함수의 함수의 두번째 인자.

```html
<input v-model="message" />
<p>Message length: {{messageLength}}</p>
```

```javascript
const message = ref("");
const messageLength = ref(0);

const messageWatch = watch(message, (newValue, oldValue) => {
  messageLength.value = newValue.length;
});
```

## LIfecycle Hooks

```html

```

```javascript

```

## Vue sytle Guilde

```html

```

```javascript

```
