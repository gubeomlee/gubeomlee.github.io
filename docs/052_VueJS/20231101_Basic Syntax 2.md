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

#### HTML \<template> element

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

## Watchers

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

#### Computed, Watchers 비교

- 공통점: 데이터의 변화를 감지하고 처리한다.

|           |                   Computed                   |                      Watchers                       |
| :-------: | :------------------------------------------: | :-------------------------------------------------: |
|   동작    | 의존하는 데이터 속성의 계산된 값을 반환한다. | 특정 데이터 속성의 변화를 감시하고 작업을 수행한다. |
| 사용 목적 |     템플릿 내에서 사용되는 데이터 연산용     |         데이터 변경에 따른 특정 작업 처리용         |
| 사용 예시 |      연산된 길이, 필터링된 목록 계산 등      |      비동기 API 요청, 연관 데이터 업데이트 등       |

## LIfecycle Hooks

- Vue 인스턴스의 생애주기 동안 특정 시점에 실행되는 함수로 개발자가 특정 단계에서 의도하는 로직이 실행될 수 있도록 한다.
- Vue 컴포넌트 인스턴스가 초기 렌더링 및 DOM 요소 생성이 완료된 후 특정 로직을 수행하기

```javascript
const { createApp, ref, onMounted } = Vue;
setup() {
  onMounted(() => {
    consol.log('mounted')
  })
}
```

- 반응형 데이터의 변경으로 인해 컴포넌트의 DOM이 업데이트된 후 특정 로직 수행하기

```html
<button @click="count++">Add 1</button>
<p>Count: {{count}}</p>
<p>{{message}}</p>
```

```javascript
const { createApp, ref, onMounted, onUpdated } = Vue;

const count = ref(0);
const message = ref(null);

onUpdated(() => {
  message.value = "updated!";
});
```

- Vue는 Lifecycle Hooks에 등록된 콜백 함수들을 인스턴스와 자동으로 연결한다.
- 이렇게 동작하려면 Hooks 함수들은 반드시 동기적으로 작성되어야 한다.
- 인스턴스 생에 주기의 여러 단계에서 호출되는 다른 Hooks도 있으며, 가장 일반적으로 사용되는 것은 onMounted, onUpdated, onUnmounted

## Vue style Guide

- Vue의 스타일 가이드 규칙은 우선순위에 따라 4가지 범주로 나눈다.
- 우선순위 A 필수(Essential): 오류를 방지하는 데 도움이 되므로 어떤 경우에도 규칙을 학습하고 준수한다.
- 우선순위 B 적극 권장(Strongly Recommended): 가독성 및 개발자 경험을 향상시킨다. 규칙을 어겨도 코드는 여전히 실행되지만, 정당한 사유가 있어야 규칙을 위반할 수 있다.
- 우선순위 C 권장(Recommended): 일관성을 보장하도록 임의의 선택을 할 수 있다.
- 우선순위 D 주의 필요(Use with Caution): 잠재적 위험 특성을 고려한다.

#### computed의 반환 값은 변경하지 말 것

- computed의 반환 값은 의존하는 데이터의 파생된 값이다.
- 일종의 snapshot이며 의존하는 데이터가 변경될 때 마다 새 snapshot이 생성된다.
- snapshot을 변경하는 것은 의미가 없으므로 계산된 반환 값은 읽기 전용으로 취급되어야 하며 변경되어서는 안된다.
- 대신 새 값을 얻기 위해서 의존하는 데이터를 업데이트 해야 한다.

#### computed 사용 시 원본 배열 변경하지 말 것

- computed에서 reverse() 및 sort() 사용 시 원본 배열을 변경하기 때문에 복사본을 만들어서 진행 해야한다.

```javascript
return [...numbers].revers();
```

#### 배열의 인덱스를 v-for의 key로 사용하지 말 것

- 인덱스는 식별자가 아닌 배열의 항목 위치만 나타내기 때문에 Vue가 DOM을 변경할 때 (끝이 아닌 위치에 새 항목이 배열에 삽입되면) 여러 컴포넌트간 데이터 공유 시 문제가 발생한다.
- 직접 고유한 값을 만들어 내는 메서드를 만들거나 외부 라이브러리 등을 황요하는 등 식별자 역할을 할 수 있는 값을 만들어 사용한다.

#### v-for와 배열: 배열 변경 감지

- 수정 메서드(원본 배열 수정): Vue는 반응형 배열의 변경 메서드가 호출되는 것을 감지하여, 필요한 업데이트를 발생시킨다. push(), pop(), shift(), unshift(), splice(), sort(), reverse()
- 배열 교체: 원본 배열을 수정하지 않고 항상 새 배열을 반환한다. filter(), concat(), slice()

#### v-for와 배열: 필터링/정렬 결과 표시

- 원본 데이터를 수정하거나 교체하지 않고 필터링 되거나 정렬된 결과를 표시한다.
- computed 활용

```html
<li v-for="num in evenNumbers">{{num}}</li>
```

```javascript
const numbers = ref([1, 2, 3, 4, 5]);

const evenNumbers = computed(() => {
  return numbers.value.filter((n) => n % 2 === 0);
});
```

- method 활용(computed가 불가능한 중첩된 v-for에 있는 경우)

```html
<ul v-for="numbers in numberSets">
  <li v-for="num in evenNumbers(numbers)">{{num}}</li>
</ul>
```

```javascript
const numberSets ref([
  [1,2,3,4,5],
  [6,7,8,9,10]
])

const evenNumbers = (numbers) => {
  return numbers.filter((n) => n % 2 === 0)
}
```
