---
layout: default
title: Redux Toolkit
parent: Redux
---

# React Toolkit
{: .no_toc}

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

---

## configureStore

기본적으로 redux-thunk를 포함하고, redux devtools extension을 사용할 수 있다.

```ts
import { createStore, applyMiddleware, compose } from "redux";
import thunk from "redux-thunk";
import { rootReducer } from "./reducers";

const composeEnhancers =
  (window as any).__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;

export const store = createStore(
  rootReducer,
  composeEnhancers(applyMiddleware(thunk))
);


// redux-toolkit의 configureStore 적용
import { configureStore } from "@reduxjs/toolkit";
import { rootReducer } from "./reducers";

export const store = configureStore({
  reducer: rootReducer
});
```

기본적으로 포함된 미들웨어는 redux-thunk, immutability, serializability가 있다.

```js
// thunk 미들웨어만 사용하는 경우
const store = configureStore({
  reducer: rootReducer
  middleware: [thunk]
});
```

## createAction
action 타입 선언과 action 생성자 함수를 포함한 것이다.

```ts
const ADD_TODO = "ADD_TODO"

const addTodo = (todo) => ({
  type: ADD_TODO
  todo
})

console.log(addTodo('작성하기'))
// return {type: 'ADD_TODO', todo: '작성하기'}


// createAction 사용
const addTodo = createAction('ADD_TODO')

console.log(addTodo('작성하기'))
// return {type: 'ADD_TODO', payload: '작성하기'}

```

## createReducer
[immer](https://github.com/immerjs/immer)가 내부적으로 사용된다.<br />

```ts
const INCREMENT = "INCREMENT"
// immer produce 사용 시 reducer
const counter = produce((draft, action) => {
  switch (action.type) {
    case INCREMENT:
      draft.value++
    }
  },
  { value: 0 }
);


// createReducer 사용
const increment = createAction('INCREMENT')

const counter = createReducer({ value: 0 }, (builder) => {
  builder
    .addCase(increment, (state, action) => {
      state.value++
    })
})
```
builder methods는 addCase, addMatcher, addDefaultCase가 있다.




--- 
**참조**<br />
[https://redux-toolkit.js.org/](https://redux-toolkit.js.org/)<br/>
[https://github.com/reduxjs/redux-toolkit](https://github.com/reduxjs/redux-toolkit)
