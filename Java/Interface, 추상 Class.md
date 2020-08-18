# 추상(`Abstract`)클래스, 인터페이스(`Interface`)

### 추상 클래스(Abstract Class)

상위 클래스 역할을 하는 클래스로, 메서드의 세부 내용을 구현하지 않고 사용할 메서드의 이름, 형식, 리턴값만 선언한다. 이렇게 선언된 메서드들은 상속받은 클래스에서 구현을 하여 활용한다.

- 일반 클래스의 메서드중 하나라도 `abstract`라면, 그 클레스는 무조건 `abstract`이어야 한다.

  -> 하지만 클래스가 `abstract`라고해서 모든 메서드가 `abstract`는 아니다.

- 추상클래스는 변수, 상수(final), 추상메서드, 일반메서드를 가질 수 있다.

- 추상클래스를 상속받은 클래스는 모든 추상 메서드를 반드시 오버라이딩 해야한다.

- 추상클래스는 객체를 생성할 수 없다.

```java
abstract public class AbstractA {
	abstract void a();
}

class A extends AbstractA{
	void a() {
		System.out.println("추상클래스 메소드 구현");
	}
}

public class MyTest {

	public static void main(String[] args) {
		A a = new A();
		a.a(); // 추상클래스 메소드 구현
		System.out.println(a instanceof A); // true
		System.out.println(a instanceof AbstractA); // true
		System.out.println(a instanceof Object); //true
        
        AbstractA aa = new A();
		aa.a(); // 추상클래스 메소드 구현
		System.out.println(aa instanceof A); // true
		System.out.println(aa instanceof AbstractA); // true
		System.out.println(aa instanceof Object); // true
	}
}
```

위의 결과로 보아, 추상클래스(AbstractA)를 상속받아서 구현한 A클래스의 생성자로 생성된 객체는 A의 변수로도, 또 추상클래스인 AbstractA의 변수로도 reference가 가능하다는 것을 알 수 있다. 또 이렇게 생성된 객체는 A의 instance이면서  AbstractA의 instance이다. 

하위 클래스들이 상위 클래스로부터 상속받아서 활용할 공통적인 메서드도 있고 하위 클래스마다 다르게 활용할 필요가 있는 메서드도 있는 경우에는 어떨까? 혹은 한 클래스에서 여러가지 추상클래스들의 메서드를 가지고 싶으면 어떻게 해야할까? Java는 단일 상속만 지원하므로 이러한 상속은 이루어질 수 없다. 이러한 추상클래스의 단점과 단일 상속만으로 부족한 부분을 극복하기 위해 나온 개념이 `Interface`이다.



### 인터페이스(Interface)

메서드의 추상화를 class와 독립적인 개념으로 `Interface`라고 이름 짓고, 한 클래스가 여러 개의 interface를 `implements`할 수 있다.

- 상속 : 재사용 vs 인터페이스 : 규정, 약속

- 인터페이스는 껍데기만 있는 형태이므로 객체를 생성할 수가 없다.

- 인터페이스 안의 모든 메서드는 굳이 지정하지 않아도 모두 추상메서드이다. 인터페이스는 추상메서드만 가질 수 있다.

- 인터페이스에서 지정한 모든 변수는 (`final`)이 붙어 상수로 취급된다.

- 인터페이스는 추상메서드, 상수를 가질 수 있다(Java 8부터는 default 키워드를 이용해 interface에서도 일반 메소드 구현이 가능하다).

- Type의 다형성은 `Interface`에서도 가능하다. 즉 Interface Type으로 선언된 변수로 해당 객체를 Reference할 수 있다. 이는 캡슐화 개념에서 가장 중요한 역할을 한다,

- 인터페이스는 객체를 생성할 수 없다.

  ```java
  MyClass c = new MyClass();
  //MyClass가 MyInterface를 implements 했을 경우
  MyInterface I = new MyClass();
  ```

  MyInterface에서 MyClass의 method를 필요로 하나 내부적인 구현이 궁금하지 않을 때,

  혹은 MyClass에서 MyInterface에 제공해주는 method의 내용을 숨기고 싶을 때 활용할 수 있다.

  이는 나중에 `API`에서 구현 코드를 숨기고 기능만 제공하고 싶을 때 API Document를 통해 선언하는 변수 Type과 메서드들의 명세서만 제공하는것과 연관이 있다.

```java
interface InterfaceB {
	public void b();
}

class B implements InterfaceB{
	public void b() {
		System.out.println("인터페이스 메소드 구현");
	}
}

public class MyTest {

	public static void main(String[] args) {
		B b = new B();
		b.b(); // 인터페이스 메소드 구현
		System.out.println(b instanceof B); // true
		System.out.println(b instanceof InterfaceB); // true
		System.out.println(b instanceof Object); // true
		
		InterfaceB bb = new B();
		bb.b(); // 인터페이스 메소드 구현
		System.out.println(bb instanceof B); // true
		System.out.println(bb instanceof InterfaceB); // true
		System.out.println(bb instanceof Object); // true
	}
}
```

위의 결과로 보아, 추상클래스와 마찬가지로 인터페이스(InterfaceB)를 구현한 B클래스의 생성자로 생성된 객체는 B의 변수로도, 또 추상클래스인 InterfaceB의 변수로도 reference가 가능하다는 것을 알 수 있다. 또 이렇게 생성된 객체는 B의 instance이면서  InterfaceB의 instance이다.



### 상속? 인터페이스?

- `extends`를 통해 자식 클래스는 부모 클래스의 멤버변수, 메서드를 자동으로 받는다.
- `implements`를 통해 자식 클래스는 인터페이스의 추상 메서드들을 구현해야 한다.
- 상속, 구현을 하는 클래스는`@override` Annotation과 함께 추상클래스/인터페이스의 추상 메서드들을 구현해주어야 한다.
