---
layout: default
title: Sate Management
parent: VueJS
nav_order: 9
---

# Sate Management

---

## State Management

#### State Management(상태관리)

- Vue 컴포넌트는 이미 반응형 상태(데이터)를 관리하고 있다.
- 상태(State): 앱 구동에 필요한 기본 데이터
- 뷰(View): 상태를 선언적으로 매핑하여 시각화
- 기능(Action): 뷰에서 사용자 입력에 대해 반응적으로 상태를 변경할 수 있게 정의된 동작

#### 상태관리의 단순성이 무너지는 시점

- 여러 뷰가 동일한 상태에 종속되는 경우: 공유 상태를 공통 조상 컴포넌트로 '끌러올린' 다음 props로 전달하는 경우 계층 구조가 깊어지면 효율성이 떨어지고 관리가 어려워진다.
- 서로 다른 뷰의 기능이 동일한 상태를 변경시켜야 하는 경우: 발신(emit)된 이벤트를 통해 상태의 여러 복사본을 변경 및 동기화 하는 경우 관리 패턴이 깨지기 쉽고 유지관리가 어려운 코드가 된다.
- 해결방법: 각 컴포넌트의 공유 상태를 추출하여, 전역에서 참조할 수 있는 저장소에서 관리한다. 컴포넌트 트리는 하나의 큰 '뷰'가 되고 컴포넌트 트리 계층 구조에 관계없이 상태에 접근하거나 기능을 사용할 수 있다.
- Pinia: Vue의 공식 상태관리 라이브러리다.

## Pinia

- Pinia는 store, state, getters, actions, plugin으로 구성된다.
- Pinia는 store라는 저장소를 가지고 store는 state, getters, actions으로 이루어지며 각각 ref(), computed(), function()과 동일하다.
- store: 중앙 저장소. 모든 컴포넌트가 공유하는 상태, 기능 등이 작성된다.
- state: 반응형 데이터(상태). ref()
- getters: 계산된 값. computed()
- actions: 메서드. function()
- plugin: 어플리케이션의 상태관리에 필요한 추가 기능을 제공하거나 확장하는 도구/모듈이다. 어플리케이션의 상태관리를 더욱 간편하고 유연하게 패키지 매니저로 설치 이후 별도 설정을 통해 추가한다.

#### State

- store 인스턴스로 state에 접근하여 직접 읽고 쓸 수 있다.
- 만약 store에 state를 정의하지 않았다면 컴포넌트에서 새로 추가할 수 없다.

```javascript
import { useCounterStore } from "@/stores/counter";

const store = userCounterStore();

console.log(store.count);
const newNumber = store.count + 1;
```

#### Getters

- store의 모든 getters를 state처럼 직접 접근할 수 있다.

```html
<template>
  <div>
    <p>getters : {{store.doubleCount}}</p>
  </div>
</template>
```

#### Actions

- store의 모든 actions를 직접 접근 및 호출 할 수 있다.
- getters와 달리 state 조작, 비동기, API 호출이나 다른 로직을 진행할 수 있다.

```html
<template>
  <div>
    <button @click="store.increment()">+++</button>
  </div>
</template>
```

```javascript
store.increment();
```

## Pinia 실습

```javascript
// stores/counter.js

import { ref, computed } from "vue";
import { defineStore } from "pinia";

export const useCounterStore = defineStore("counter", () => {
  let id = 0;
  const todos = ref([
    { id: id++, text: "할 일 1", isDone: false },
    { id: id++, text: "할 일 2", isDone: false },
  ]);

  const addTodo = (todoText) => {
    todos.value.push({
      id: id++,
      text: todoText,
      inDone: false,
    });
  };

  const deleteTodo = (todoId) => {
    const index = todos.value.findIndex((el) => el.id === todoId);
    todos.value.splice(index, 1);
  };

  const updateTodo = (todoId) => {
    todo.value = todo.value.map((el) => {
      if (el.id === todoId) {
        todo.isDone = !todo.isDone;
      }
      return todo;
    });
  };

  const doneTodoCount = computed(() => {
    return todos.value.filter((el) => el.isDone);
  });

  return { todos, addTodo, deleteTodo, updateTodo, doneTodoCount };
});
```

- store의 todos 상태를 참조한다.
- 하위 컴포넌트인 TodoListItem을 반복 하면서 개별 todo를 props로 전달한다.

```javascript
// TodoList.vue

import { useCounterStore } from "@/stores/counter";
import TodoListItem from "@/components/TodoListItem.vue";

const store = useCounterStore();
```

```html
<!-- TodoList.vue -->
<template>
  <div>
    <TodoListItem v-for="todo in store.todos" :key="todo.id" :todo="todo"></TodoListItem>
  </div>
</template>
```

- TodoFrom에서ㅓ 실시간으로 입려되는 사용자 데이터를 양방향 바인딩하여 변수로 할당한다.

```html
<!-- TodoFrom.vue -->
<template>
  <div>
    <form @submit.prevent="createTodo(todoText)" ref="formElem">
      <input type="text" v-model="todoText" />
      <input type="submit" />
    </form>
  </div>
</template>
```

```javascript
// TodoFrom.vue

import { ref } from "vue";
import { useCounterStore } from "@/stores/counter";

const todoText = ref("");
const formElem = ref(null);

const store = useCounterStore();
const createTodo = (todoText) {
	store.addTodo(todoText);
	formElem.value.reset();
}
```

- 각 todo에 삭제 버튼을 작성한다. 버튼을 클릭하면 선택된 todo의 id를 인자로 전달해 deleteTodo 메서드를 호출한다.

```html
<!-- TodoListItem.vue -->
<template>
  <div>
    <span @click="store.updateTodo(todo.id)">{{ todo.text }}</span>
    <button @click="store.delteTodo(todo.id)">Delete</button>
  </div>
</template>
```

```javascript
// TodoListItem.vue
import { useCounterStore } from "@/stores/counter";

defineProps({
  todo: Object,
});

const store = useCounterStore();
```

## Local Storage

- 브라우저 내에 key-value 쌍을 저장하는 웹 스토리지 객체
- 페이지를 새로 고침하고 브라우저를 다시 실행해도 데이터가 유지된다.
- 쿠키와 다르게 네트워크 요청 시 전송되지 않는다.
- 여러 탭이나 창 간에 데이터를 공유할 수 있다.
- 웹 어플리케이션에서 사용자 설정, 상태정보, 캐시 데이터 등을 클라이언트 측에서 보관하여 웹사이트 성능을 향상시키고 사용자 경험을 개선하기 위해 사용하다.

## 언제 store를 사용해야 하는가?

- Pinia를 사용한다고 해서 모든 데이터를 state에 넣어야 하는 것은 아니다.
- 필요한 경우 pass props, emit event를 사용하여 상태를 관리할 수 있다.
- Pinia는 공유된 상태를 관리하는데 유용하지만, 개념에 대한 이해와 시작하는 비용이 크다.
- 어플리케이션이 단순하다면 Pinia가 없는 것이 더 효율적일 수 있다.
- 그러나 중대형 규모의 SPA를 구축하는 경우 Pinia는 자연스럽게 선택할 수 있는 단계가 오게 된다.
- 결과적으로 역할에 적절한 상황에서 활용 했을 때 Pinia 효용을 극대화 할 수 있다.
