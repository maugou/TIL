---
layout: default
title: MVVM
parent: 패턴
---

# MVVM 아키텍처 패턴

---

Model, View, View Model 세 가지 요소로 구분된다.

![](../../../images/pattern/mvvm.png)

- View : 사용자에게 보여지는 화면(UI)
- ViewModel : UI에서 사용되는 상태와 동작을 관리
- Model : 애플리케이션 data(서버로부터 수신 등)와 비즈니스 로직

Model은 View와 ViewModel에서 필요한 data를 가져가거나 동작할 수 있도록 독립적으로 설계해야한다.<br/>
ViewModel 또한 View와 직접적인 참조가 되지않게 한다면, 재사용성이 증가한다.<br/>
각 요소의 역할을 분리하여 개발하게되면 유지보수하기가 쉬워진다.
