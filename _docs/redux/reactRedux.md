---
layout: default
title: React Redux
parent: Redux
---

# React Redux
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

## Hooks
connect()를 통해 접근하지 않아도 store 접근 및 dispatch 할 수 있게 한다. v7.1.0 에서 추가되었다.

### useSelector
- store에 action이 전달될 때마다 실행된다.
  useSelector()가 사용된 함수컴포넌트의 re-render는 출력 값이 다를 경우에만 발생한다. **비교방식은 '===' 참조 비교이다.**<br/>
  즉, 출력 값이 단일 값이라면 해당 값의 변경에 따라 re-render가 발생한다.

```jsx
const Example1 = () => {
  const dispatch = useDispatch();
  return (
    <button onClick={() => dispatch(type: "CHANGE_NAME")}>
      CHANGE_NAME
    </button>
  )
}

const Example2 = () => {
  const name = useSelector(store => store.user.name);
  ...
}

const Example3 = () => {
  const phone = useSelector(store => stroe.user.phone);
  ...
}
```
Example1 컴포넌트에서 store의 name값을 변경하는 action을 dispatch 한다면, Example2 컴포넌트는 re-render가 발생하지만, Example3 컴포넌트는 re-render가 발생하지 않는다.

- 다른 실행주기는 함수 컴포넌트가 렌더링 될 때마다 실행된다. 출력 값의 변경이 없는 경우 useSelector를 실행하지 않고 캐시된 값을 반환한다. 

위 두 가지 실행주기에 따라 객체 형태를 useSelector()의 결과값으로 반환하려면 아래방법을 따른다. (객체형태는 '===' 참조비교 시 동일한 형태라도 false로 인식하기 때문에 불필요한 re-render가 발생할 수 있다.)

1. useSelector()를 여러번 호출하여 각각의 단일값으로 반환한다.
2. [reselect](https://github.com/reduxjs/reselect) 같은 모듈을 이용하여 하나의 Object에서 여러 값을 반환하는 memoized selector를 만든다. 이 경우, Object 내 값 변경이 있을 경우에만 새 Object를 반환한다.
3. shallowEqual()를 사용한다.




