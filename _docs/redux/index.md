---
layout: default
title: Redux
has_children: true
has_toc: false
---

# Redux

---
### 규칙
- state를 직접 변경해서는 안된다.
- reducer는 side effect를 가지면 안된다.
- app당 하나의 store만 존재해야 한다.
- 전체 state 트리는 불변성을 가지는 것을 권장한다.

### 추천
- redux toolkit<br/>
  thunk 및 immer 등의 기능들이 기본적으로 제공되며 redux 사용을 보다 간편하게 해준다.
- immer<br/>
  불변성 관리 라이브러리