# 상속

### 상속이란?

> 상속이란, OOP(Object Oriented Programming)의 대표적인 특징이다.

어떤 Class(A)가 다른 Class(B)의 `member variables`와 `method`를 그대로 받으면 A는 B를 상속받는 것이다.

- 아무런 상속을 받지 않은 모든 class는 컴파일러에 의해 `java.lang`패키지의  `Object` class를 상속받는다.

- 결과적으로 모든 class는 `Object` class의 하위 class이다.



### `extends` keyword

class A가 class B를 상속받고 싶을 때, `extends`예약어를 통해 상속을 표현한다. Java에서는 단일 상속만 가능하다. 다중 상속 대신에 `interface` keywrod를 활용하여 해결할 수 있다.

```java
Public class B {
    
}
```

```java
Public class A extends B {
    ...
}
```



### 상속

A extends B를 통해 B의 `member variables`, `methods`를 상속받은 자식 class A는 자신만의 `member variables`, `methods`또한 가질 수 있다.



### 접근 권한

상위 class에서 선언된 variables의 `Access Modifier`가  `private` 혹은 `default`인 경우, 하위 class(`default` : 다른 패키지의 하위 class)에서 접근할 수 없다. 

이를 해결하기 위해 상위 class에서 `protected`로 선언하거나 상위 class의 public method를 통해 접근해야 한다.

- `private` : 상속 X, 같은 package 안에서 X

- `default`  :상속 X, 같은 package 안에서 O
- `protected` : 상속 O, 같은 package 안에서 O



### `this`, `super` 참조변수

`this` : 자기 자신 -> 부모 class 순으로 member variables에 접근. 하지만 this로 부모 class에 접근이 가능하다고 해서 그렇게 사용하면 본인 class에 이름이 중복된 member variables를 생성할 경우 본인의 variables를 가리킨다.

`super` : 부모 객체의 member variables에 접근



### `insatnceof`

Interface, Parent, Child

Interface i = new Child();

--> i instanceof Interface True

--> i instanceof Parent True

--> i instanceof Object True

상속관계에 있어서 if로 기능을 다르게 처리하고 싶다면 insatnece Child --> instanceof Parent --> insatnceof Object 순으로 체크하여야 한다.



### 생성자는 상속이 안된다

하지만 상속을 하였을 때 자식 클래스의 생성자의 첫 줄에는 부모 클래스의 기본 생성자인 `super()`가 자동으로 들어간다. 즉 부모 class의 인스턴스가 생성된 후 자식 클래스의 인스턴스가 생성이 되어야 한다는 것이다.