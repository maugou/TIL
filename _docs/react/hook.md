---
layout: default
title: Hook
parent: React
---

# Hook
{: .no_toc}

React 함수 내 최상위에서만 Hook을 호출해야 한다.

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

---


## useState
useState의 경우 class 컴포넌트에서 작성했던 setState 방식의 단일 객체에 state값을 관리하는 방법을 사용할 수는 있으나, 여러 상태변수로 분할하는 것을 더 권장한다.<br/>
useState는 값을 대체하는 방식이기 때문에 단일 객체일 경우 수동 병합을 필히 진행해야한다.<br/>
상태 변수를 분할하면 사용자 정의 후크로 쉽게 추출 할 수 있습니다.<br/>

즉, 분할을 우선시 하되 연관성에 따라 묶음 필요하다.

```jsx
// 가능o 권장x
const [state, setState] = useState({name: 'john', boolean: true })

const exam = () => {
   setState((preState) => ({...preState, boolean: false})) // 수동 병합
}

// 권장
const [name, setName] = useState('john')
const [boolean, setBoolean] = useState(true)
```

## useEffect
화면의 그리기를 완료한 후 실행한다.<br/>
불필요한 reflow를 방지하기 위해 두번째 인자에 []를 표기할 수 있지만 effect 내에서 의존되는 값이 있으면, 두번째 인자에 표기를 하는 것이 안전하므로 권장된다.

의존성에 함수는 effect 내에서 선언하고 의존성 값을 추가하는 것을 권장한다.
```jsx
// 권장x, 
const Exam = ({name}) => {
   const func = () => {
     console.log(name)
   }

   useEffect(() => {
      fun()
   }, [])
   
   ...
}

// 권장
const Exam = ({name}) => {
   useEffect(() => {
      const func = () => {
         console.log(name)
      }

   }, [name])

   ...
}

```

## useMemo / useCallback
useMemo를 사용하지 않고 동작 구현이 된다면, 성능을 최적화시키는 용도로 추가 사용한다.
기억된 값을 반환한다. 의존성 배열의 값이 변경되었을 때에만 값을 새롭게 계산한다.

useCallback(fn, deps)는 useMemo(()=> fn, deps)와 같다.
즉, useCallback은 기억된 callback을 반환하며, 의존성이 변경되었을 때 callback을 새롭게 나타낸다.