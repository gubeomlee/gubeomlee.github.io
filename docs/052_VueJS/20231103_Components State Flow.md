---
layout: default
title: Components State Flow
parent: VueJS
nav_order: 7
---

# Components State Flow

---

## Passing Props

- 부모는 자식에게 데이터를 전달(Pass Props)하며, 자식은 자신에게 일어난 일을 부모에게 알린다.(Emit event)

#### Props

- 부모 컴포넌트 -> 자식 컴포넌트 데이터를 전달하는데 사용되는 속성이다.
- 모든 props는 자식 속성과 부모 속성 사이에 하향식 단반향 바인딩을 형성한다.
- 부모 속성이 업데이트 되면 자식으로 흐르지만 그 반되는 안된다. 즉, 자식 컴포넌트 내부에서 props를 변경하려고 시도해서는 안되며 불가능하다. 또한 부모 컴포넌트가 업데이트 될 때마다 자식 컴포넌트의 모든 props가 최신 값으로 업데이트 된다.
- 부모 컴포넌트에서만 변경하고 이를 내려 받은 자식 컴포넌트는 자연스럽게 갱신한다.
- 단방향인 이유: 하위 컴포넌트가 실수로 상위 컴포넌트의 상태를 변경하여 앱에서 데이터 흐름을 이해하기 어렵게 만드는 것을 방지하기 위해서다.

#### Props 선언

- 부모 컴포넌트에서 보낸 props를 사용하기 위해서는 자식 컴포넌트에서 명시적인 props 선언이 필요하다.
- 부모 컴포넌트에서 자식 컴포넌트로 보낼 props를 작성한다.

```html
<template>
  <div>
    <ParentChild my-msg="message" />
  </div>
</template>
```

- my-msg: prop 이름
- "message": prop 값

- 문자열 배열을 사용한 선언

```html
<!-- ParentChild.vue -->

<script setup>
  defineProps(["myMsg"]);
</script>
```

- 객체를 사용한 선언: 객체 선언 문법의 각 객체 속성의 키는 props의 이름이 되며, 객체 속성의 값은 값이 될 데이터의 타입에 해당하는 생성자 함수(Number, String, ...)이어야 한다. 객체 선언 문법 사용이 권장된다.

```html
<!-- ParentChild.vue -->

<script setup>
  defineProps({
    myMsg: String,
  });
</script>
```

#### props 세부사항: Props Name Casing(Props 이름 컨벤션)

- 선언 및 템플릿 참조 시 -> camelCase
- 자식 컴포넌트로 전달 시 -> kebab-case (기술적으로 camelCase도 가능하지만 HTML 속성 표기법과 동일하게 kebab-case로 표기할 것을 권장한다.)

#### props 세부사항: Static & Dynamic Props

- v-bind를 사용하여 동적으로 할당된 props를 사용할 수 있다.

```html
<!-- Parent.vue -->
<ParentChild my-msg="message" : dynamic-props="name" />
```

```javascript
// Parent.vue
import { ref } from "vue";
const name = ref("Alice");
```

```html
<!-- ParentChild.vue -->
<p>{{dynamicProps}}</p>
```

```javascript
// ParentChild.vue
defineProps({
  myMsg: String,
  dynamicProps: String,
});
```

## Component Events

- 부모는 자식에게 데이터를 전달(pass Props)하며, 자식은 자신에게 일어난 일을 부모에게 알린다. (Emit event)
- 부모가 prop 데이터를 변경하도록 소리쳐야 한다.

#### emit()

- 자식 컴포넌트가 이벤트를 발생시켜 부모 컴포넌트로 데이터를 전달하는 역할의 메서드다.
- '$' 표기는 Vue인스턴스나 컴포넌트 내에서 제공되는 전역 속성이나 메서드를 식별하기 위한 접두어다.

```
$emit(event, ...args)
```

- event: 커스텀 이벤트 이름
- args: 추가 인자

#### 이벤트 발신 및 수신(Emitting and Listening to Events)

- $emit을 사용하여 템플릿 표현식에서 직접 사용자 정의 이벤트를 발신한다.

```html
<button @click="$emit('someEvent')">클릭</button>
```

- 부모는 v-on을 사용하여 수신한다. 수신 후 처리할 로직 및 콜백함수를 호출한다.

```html
<ParentComp @some-event="someCallback" />
```

#### emit 이벤트 선언

- defineEmits()를 사용하여 밍시적으로 발신할 이벤트를 선언할 수 있다.
- script에서 $emit 메서드를 접근할 수 없기 때문에 defineEmits()는 $emit 대신 사용할 수 있는 동등한 함수를 반환한다.

```html
<!-- ParentChild.vue -->
<script setup>
  const emit = defineEmits(["someEvent", "myFocus"]);

  const buttonClick = () => {
    emit("someEvent");
  };
</script>
<!-- ParentChild.vue -->
<button @click="buttonClick">클릭</button>
```

#### 이벤트 인자

- 이벤트 발신 시 추가 인자를 전달하여 값을 제공할 수 있다.
