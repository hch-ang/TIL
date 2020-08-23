# Java 이것저것

### `JVM(Java Virtual Machine)`

Java의 가장 큰 장점을 만들어주는 머신이다. 한번 작성한 Java코드는 `javac` compiler에 의해 `.class`라는 형식의 `java byte code`로 complie 되는데, 이 java byte code는 모든 OS에서 작동되지 못한다. 하지만 각 OS마다 JVM을 가지고 있어서 이 java byte code를 해당 OS에 맞게 해석하여 실행시킨다. 기존의 C같은 native 언어들의 작동 방식은 프로그램 -> OS -> HW의 구조를 가지고 있어서 OS에 `메모리`를 할당받아 사용하였다면, Java의 작동 방식은 프로그램 -> JVM -> OS -> HW의 구조를 가지고 있어서 JVM이 OS에 `메모리 사용 권한`을 할당받아서 사용하였다. 즉 프로그램이 OS로부터 독립적이게 된 것이다.

JVM은 OS로부터 메모리를 할당받아서 프로그램 실행 시 여러 개의 메모리 영역으로 나누는데, 이를 `JVM메모리(Runtime Data Area)`라고 한다. 런타임 메모리는 크게 `Class Area`, `Heap Area`, `Stack Area`, `PC Register`, `Native Method Stack` 이렇게 5개의 영역으로 구분이 되고 그 중에서 Class Area, Heap Area, Stack Area를 구분하는 것이 중요하다.

- Class Area(Method Area, Static Area)

  `.java`파일을 자바 컴파일러(`Javac.exe`)가 컴파일하면 java파일에 작성된 클래스들은 모두 클래스파일(`.class`)로 변환이 된다. 변환이 된 클래스파일은 `Class Loader`에 의해 JVM메모리 구역 중 `Class Area`에 적재된다. Class 정보, Static 변수, Method의 내용이 들어 있다.

  - 상수 풀(Constant Pool)에 대한 정보도 들어있다.

- Heap Area(가변적인 공간)

  Reference Type, Object들이 들어있는 공간이다 Class Area에 적재된 Class의 객체만 생성이 가능하고, `new` 키워드를 통해 생성된 객체 및 배열이 생성되는 공간이다. 여기서 동적으로 생성된 객체/배열은 `Garbage Collector`를 통해 참조되지 않을 시 할당해제된다.

- Stack Area

  지역 변수, 파라미터, 리턴 값, 연산에 사용되는 임시 값 등 메소드가 생성되고 실행되면서 만들어지는 로컬변수에 대한 내용이 들어있다. 메소드가 종료되는 시점에서 해당 메소드에서 생성된 로컬변수들은 사라진다.

이의 3가지 외 나머지 2가지 영역은 다음과 같다.

- Native method stack area

  자바 외 다른 언어로 작성되어 제공되는 메소드, 즉 네이티브 코드가 저장되는 영역이다. 

- PC Register

  Thread가 생성될 때마다 만들어지는 영역으로, 쓰레드가 실행되는 부분의 주소와 명령을 저장하는 `Program Counter`가 저장되는 영역이다.



### `Garbage Collection`(in JVM)

Heap에 만들어진 Reference Type 객체들 중 더이상 참조되지 않은 객체들을 JVM이 자동으로 할당해제를 하기 위해 Garbage Collection 작업을 수행한다.



### Inner class(local)

class 안에서 다시 정의되는 class를 inner class라고 하며, 바깥 class의 일부처럼 사용한다. 별도의 객체가 만들어지며, 다른 클래스와는 무관하지만 바깥 클래스에서만 의미있게 활용할 용도로 사용된다.

### Inner class(anonymous)

클래스 안에서 이름없이 만들어지는 inner class이다. 이름이 없으므로 재사용되지 않고, 객체를 생성하는 코드에 바로 class의 내용을 전달한다. `Event Handling`처럼 interface에 정의된 method의 구현부를 객체 생성 시점에 전달한다.

### Inner class(static)

별도의 객체를 만들지 않고 사용하 수 있는 클래스. 다른 Class에서 사용하지 않고 바깥 클래스에서만 의미가 있는 경우 많이 사용된다.



### `Member variable` vs `Local varialbe`

- Member variable(멤버 변수)

  클래스 내부에서 선언된 변수로, 일반적으로 선언된 변수는 `인스턴스 변수`로 설정되어 인스턴스가 생성될 때 가지는 변수이고, `static`으로 선언된 변수는 `클래스 변수`로 모든 클래스 혹은 인스턴스에서 접근 가능한 전역변수의 역할을 한다. 일반적으로 클래스 변수는 인스턴스로 접근하기 보다는 클래스를 통해 접근하는 것이 정상적이다.

  멤버변수는 초기화를 하지 않아도 기본값으로 자동으로 초기화가 되는 특성이 있다.

  ```java
  class A {
  	static int a;
  	int b;
  }
  
  public static void main(String[] args) {
      System.out.println(A.a); // 0, 자동으로 초기화된 값
      System.out.println(A.b); // Exception 발생, b는 인스턴스변수이므로 클래스명을 통해 접근할 수 없다.
      A a = new A();
      System.out.println(a.a); // 0, 인스턴스를 통해 클래스 변수에 접근할 수 있다.
      System.out.println(a.b); // 0, 자동으로 초기화된 값
  }
  ```

  

- Local variable(지역 변수)

  메소드 혹은 생성자 내부에서 선언된 변수로, 메소드(생성자)가 실행되어 선언될 때 생성되어 메소드(생성자)가 종료되는 시점에 사라지는 변수이다. 지역 변수는 클래스를 통해 접근할 수 없고, 인스턴스르 생성하여 인스턴스로 접근하는 방법을 사용해야 한다.

  ```java
  class A {
  	void test() {
  		int a;
  		System.out.println(a);
  	}
  }
  
  public static void main(String[] args) {
      A.test(); // exception, test 메소드는 인스턴스 메소드이므로 클래스를 통해 호출할 수 없다.
      A a = new A();
      a.test(); // exception, test 메소드에서 지역변수로 선언된 a의 값이 초기화되지 않았다.
  }
  ```

  ```java
  class A {
  	static void test() {
  		int a;
  		System.out.println(a);
  	}
  }
  
  public static void main(String[] args) {
      A.test(); //exception, test 메소드가 static을 통해 클래스 메소드로 선언되었으므로 클래스를 통해 접근할 수 있으나 지역변수로 선언된 a의 값이 초기화되지 않았다.
  }
  ```

  ```java
  class A {
  	void test() {
  		int a = 10;
  		System.out.println(a);
  	}
  }
  
  public static void main(String[] args) {
      A a = new A();
      a.test(); // 10, 정상적으로 초기화된 a의 값 출력
  }
  ```

  
