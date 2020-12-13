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


## createSlice
내부적으로 createAcion과 createReducer를 사용한다.

**Parameters**<br/>
name: action type의 접두사로 사용된다.<br/>
initialState: 초기 state<br/>
reducers: reducer를 정의하되 해당 key 이름으로 createAction이 된다.

```js
const counter = createSlice({
  name: 'counter'.
  initialState: { value: 0 },
  reducers: {
    increment: (state, action) => {
      state.value++
    }
  }
})

// action 생성자 increment
const { increment } = counter.actions

// reducer
const rootReducer = combineReducers({
  counter: counter.reducer
})
```

extraReducers: 정의된 action type이 아닌 action type에 의해 실행 시킬 수 있다.<br/>
(기본예시) createAsyncThunk와 사용 시 promise의 상태에 따라 reducer 실행 가능

## createAsyncThunk
return 값은 promise 상태 생명주기 중 fulfilled에 응답한다. rejected에 반응하기 위해서는 rejectWithValue를 통해 값을 반환하면 된다.

**Parameters**<br/>
type: action type으로 promise 상태 생명주기가 붙는다.<br/>
(기본예시) type이 "example/request" 일 경우 "example/request/pending" 이 후에 연산 결과에 따라 "example/request/fulfilled" 또는 "example/request/rejected" action type이 보내진다.

```js
const example = createAsyncThunk("example/request", async (arg, thunkAPI) => {
    try {
      const res = await fetch( ... )
    
      return "fulfilled"
    } catch {
      return thunkAPI.rejectWithValue("rejected")
    }
  }
)

const counter = createSlice({
  name: "counter",
  initialState: { value: 0 },
  reducers: {},
  extraReducers: {
    [example.fulfilled]: (state, action) =>{
      console.log(action.payload) // returns "fulfilled"
    },
    [example.rejected]: (state, action) => {
      console.log(action.payload) // returns "rejected"
    }
  }
})
```






--- 
**참조**<br />
[https://redux-toolkit.js.org/](https://redux-toolkit.js.org/)<br/>
[https://github.com/reduxjs/redux-toolkit](https://github.com/reduxjs/redux-toolkit)
