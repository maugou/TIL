---
layout: default
title: Redux
---

# **Redux**

---

**Actions**<br>
액션은 JS 객체 형태이다. 반드시 type 속성을 가지고 있으며, 값은 문자열 형태가 일반적이다. <br>
액션은 dispatch(Action) 를 통해 스토어에 전달된다. (상태 변경하기 위한 유일한 방법)

**Reducers**<br>
기존의 상태와 액션을 받아서 상태를 변경시킨 후 반환시킨다. 다만, 인수는 변경하지 않는 순수함수여야 한다.

**Store**<br>
스토어는 상태를 저장한다.

