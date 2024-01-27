---
layout: default
title: Builder 패턴
parent: 디자인 패턴
---

객체의 생성을 보다 단계적으로 할수 있도록 한다.<br/>
인자 순서를 파악할 필요가 없으니, 잘못된 값을 넣는 실수를 줄일 수 있다.

### 예시

```dart
class Student {
  int? _id;
  String? _name;
  String? _grade;
  int? _age;

  Student(int? id, String? name, String? grade, int? age) {
    _id = id;
    _name = name;
    _grade = grade;
    _age = age;
  }
}
```

```dart
// builder
class StudentBuilder {
  int? _id;
  String? _name;
  String? _grade;
  int? _age;

  StudentBuilder id(int id) {
    _id = id;
    return this;
  }

  StudentBuilder name(String name) {
    _name = name;
    return this;
  }

  StudentBuilder grade(String grade) {
    _grade = grade;
    return this;
  }

  StudentBuilder age(int age) {
    _age = age;
    return this;
  }

  Student build() {
    return Student(_id, _name, _grade, _age);
  }
}
```

각각의 set method는 this를 반환함으로 Chaining하게 호출할 수 있도록 한다
<br />
<br />

아래와 같이 builder 객체 실행

```dart
void main() {
  Student student =
      StudentBuilder().id(1).name("아무개").grade("junior").age(20).build();
}
```
