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

`final`로 선언된 클래스는 더이상 어떤 클래스에서도 상속받을 수 없다.

상위 클래스에서 `static`으로 선언되는 메소드는 재정의 할 수 없다.

```java
class A{
	static void a() {
		System.out.println("A의 static 메소드");
	}
}
```
```java
class B extends A{
	void a() {} // This instance method cannot override the static method from A
}
```

A에서 static으로 선언된 메소드를 B에서 오버라이딩 할 수 없다는 것을 알 수 있다.

```java
class B extends A{
	static void a() {
		System.out.println("B의 static 메소드");
	}
}

public static void main(String[] args) {
    B b = new B();
    B.a();
}
```

result : B의 static 메소드

위의 경우는 어떻게 작동하는 것일까? class B에서 class A의 메소드 a()를 static으로 오버라이딩 한 것일까? 오버라이딩이 금지되어있지만 static으로는 정의가 가능한 것으로 보아 A에서의 a()는 A에서만 정의되고 B에서 상속까지만 가능하고, B에서 static으로 정의한 a()는 B에서 새롭게 정의된 메소드라고 유추할 수 있다.





### 접근 권한

상위 class에서 선언된 variables의 `Access Modifier`가  `private` 혹은 `default`인 경우, 하위 class(`default` : 다른 패키지의 하위 class)에서 접근할 수 없다. 

이를 해결하기 위해 상위 class에서 `protected`로 선언하거나 상위 class의 public method를 통해 접근해야 한다.

|             | 클래스 자신 | 같은 패키지 | 상속받은 (다른패키지)클래스 | 모든 범위 |
| :-----------: | :-----------: | :-----------: | :---------------------------: | :---------: |
| `private`   | O           | X           | X                           | X         |
| `(default)` | O           | O           | X                           | X         |
| `protected` | O           | O           | O                           | X         |
| `public`    | O           | O           | O                           | O         |



### `this`, `super` 참조변수

`this` : 자기 자신 -> 부모 class 순으로 member variables, method에 접근. 하지만 this로 부모 class에 접근이 가능하다고 해서 그렇게 사용하면 본인 class에 이름이 중복된 member variables/method를 생성할 경우 본인의 memeber variables/method를 가리키게 되면서 member variables/method사용에 혼란을 줄 수 있기 때문이다.

`super` : 부모 객체의 member variables, member method에 접근



### `insatnceof`

Interface, Parent, Child

Interface i = new Child();

--> i instanceof Interface True

--> i instanceof Parent True

--> i instanceof Object True

상속관계에 있어서 if로 기능을 다르게 처리하고 싶다면 insatnece Child --> instanceof Parent --> insatnceof Object 순으로 체크하여야 한다.



### 생성자는 상속이 안된다

하지만 상속을 하였을 때 자식 클래스의 생성자에서 `첫 줄에` 별도의 부모클래스 생성자를 호출하지 않는다면 부모 클래스의 기본 생성자인 `super()`가 자동으로 들어간다. 즉 부모 class의 인스턴스가 생성된 후 자식 클래스의 인스턴스가 생성이 되어야 한다는 것이다.

```java
class A {
	public A() {
		System.out.println("A 기본 생성자");
	}
	
	public A(int a) {
		System.out.println("A 다른 생성자");
	}

}

class B extends A{
	public B() {
		System.out.println("B 기본 생성자");
	}
	public B(int a) {
		System.out.println("B 다른 생성자");
	}
}


public static void main(String[] args) {
    B b = new B();		
    B bb = new B(1);
}
```

Result :

A 기본 생성자

B 기본 생성자

A 기본 생성자

B 다른 생성자

이를 통해 B에서 만든 어떠한 생성자든 A의 기본생성자를 먼저 실행 후 해당 생성자를 실행한다는 것을 알 수 있다. 이는 나중에 Type Polymorphism에서도 필요한 내용으로, 지금같은 상황에서

```java
public static void main(String[] args) {
    A a = new B();
}
```

다음과 같이 다형성을 활용한 객체 생성을 하였다고 가정하자. 이 때, 실행되는 생성자의 순서는 Object(아무것도 상속받지 않는 클래스는 무조건 Object클래스를 생성하도록 되어있다)생성자 -> A생성자 -> B생성자 순이다. 이렇게 상속관계로 실행되는 모든 생성자가 만든 각각의 객체는 서로 이어져있다.  즉 Object객체, A객체, B객체가 모두 생성되어 있고 서로 연결되어있다. 마지막으로 A클래스 변수인 a가 가리키는 객체는 연결되어있는 객체 중 A생성자로 생성된 객체를 가리킨다.

```java
class A {
	public A() {
		System.out.println("A 기본 생성자");
	}
	
	public A(int a) {
		System.out.println("A 다른 생성자");
	}

}

class B extends A{
	public B() {
        super(1);
		System.out.println("B 기본 생성자");
	}
	public B(int a) {
        super(1);
		System.out.println("B 다른 생성자");
	}
}


public static void main(String[] args) {
    B b = new B();		
    B bb = new B(1);
}
```

Result :

A 다른 생성자

B 기본 생성자

A 다른 생성자

B 다른 생성자

위의 코드 결과를 통해 하위 클래스인 B의 생성자에서 상위 클래스 A의 생성자를 따로 호출하는 코드를 넣었을 때, A의 기본생성자가 실행되는 것이 아니라 B의 생성자에서 호출하는 클래스A의 생성자가 실행된다는 것을 알 수 있다.



