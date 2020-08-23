# 추상(`Abstract`)클래스, 인터페이스(`Interface`), `Lambda`

### 추상 클래스(Abstract Class)

상위 클래스 역할을 하는 클래스로, 메서드의 세부 내용을 구현하지 않고 사용할 메서드의 이름, 형식, 리턴값만 선언한다. 이렇게 선언된 메서드들은 상속받은 클래스에서 구현을 하여 활용한다.

- 일반 클래스의 메서드중 하나라도 `abstract`라면, 그 클레스는 무조건 `abstract`이어야 한다.

  -> 하지만 클래스가 `abstract`라고해서 모든 메서드가 `abstract`는 아니다.

  ```java
  // 추상메소드 X (그냥 비어있는 메소드는 추상메소드가 아니다. 비어있는 메소드이다.)
  public void xxx(){}
  
  // 추상메소드 O abstract가 붙어 있어야 추상메소드이다. 추상메소드는 메소드의 내용을 입력하는 중괄호가 없고 그냥 ;으로 메소드를 선언만 한다.
  public abstract void xxx();
  ```

- 추상클래스는 변수, 상수(final), 추상메서드, 일반메서드를 가질 수 있다.

- 추상클래스를 상속받은 클래스는 모든 추상 메서드를 반드시 오버라이딩 해야한다.

- 추상클래스는 생성자를 가질 수 있지만, 자신의 생성자를 이용하여 객체를 생성할 수도 없다.

  

### 추상클래스의 객체 생성

추상클래스의 객체 생성방법은 총 4가지가 있다.

1. 하위클래스를 참조(다형성 활용)

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

   - ex) `Format`(Calendar를 이쁘게 출력)

     ```java
     public abstract class Format
     ```

     Format의 하위클래스인 `DateFormat`

     ```java
     public abstract class DateFormat
     ```

     DataForamt의 하위클래스인 `SimpleDataFormat`

     ```java
     public class SimpleDateFormat
     ```

     constructor는 SimpleDataFormat의 생성자 중 SimpleDataFormat(String pattern)을 활용

     ```java
     Format f = new SimpleDateFormat("yy.MM.dd HH:mm:ss");
     String current = f.format(new Date());
     ```

   

2. 자신의 객체를 return하는 static method 사용(싱글톤패턴과 비슷)

   Calendar 클래스를 예를 들어 확인해보자.

   ```java
   public abstract class Calendar
   extends Object
   implements Serializable, Cloneable, Comparable<Calendar>
   ```

   Calendar클래스는 추상클래스이므로 자신의 생성자를 통해 객체를 생성할 수 없다.

   - 하위클래스 참조

   ```java
   Calendar cal = new GregorianCalendar();
   ```

   - 자신의 객체를 return하는 static 메소드 활용

   ```java
   public static Calendar getInstance()
   ```

   API에서 Calendar 클래스의 getInstance 메소드를 보면 Calendar 객체를 return하고 있고, static으로 정의되어서 객체 없이 메소드 호출이 가능하다는 것을 알 수 있다. 이를 활용하여 다음과 같이 Calendar 객체를 얻을 수 있다.

   ```java
   Calandar cal = Calendar.getInstance()
   ```

   

3. 외부 클래스 사용

   `Process` 클래스를 통해 알아보자.

   ```java
   public abstract class Process
   ```

   1. API문서를 보면 하위 클래스가 없어서 안된다.

   2. 자기 자신을 return하는 method가 있으나 static 아니라서 이용할 수 없다.

   3. 외부 클래스 사용

      API읽어보면 다음과 같은 내용이 있다.

      [`ProcessBuilder.start()`](https://docs.oracle.com/javase/8/docs/api/java/lang/ProcessBuilder.html#start--) and [`Runtime.exec`](https://docs.oracle.com/javase/8/docs/api/java/lang/Runtime.html#exec-java.lang.String:A-java.lang.String:A-java.io.File-) methods create a native process and return an instance of a subclass of `Process` that can be used to control the process and obtain information about it.

      

      그런데 Runtime은 분명 abstract가 아닌데 문서에는 CONSTR이 없다...아마 싱글톤으로 private로 막혀있다고 예상할 수 있다. method를 찾아보니 getRuntime()이 static 형태로 runtime을 return한다. 따라서 다음과 같은 방법으로 Process 객체를 얻을 수 있다.

      ```java
      Runtime r = Runtime.getRuntime();
      Process p = r.exec("calc");
      ```

   

4. 자신의 생성자로 객체 생성(dynamic 객체 생성, Anonymous)

   추상메소드도 자신의 생성자를 통해 객체를 생성할 수 있는데, 이 때 활용되는 개념이 `anonymous` 클래스이다. anonymous 클래스에 대해서는 밑에서 더 알아보도록 하자.

   


### 의미상의 추상 클래스

```java
public abstract class WindowAdapter
extends Object
implements WindowListener, WindowStateListener, WindowFocusListener
```

java.awt.event 패키지의 `WindowAdapter` 클래스는 창을 관리하는 추상클래스이다. 하지만, API문서를 통해 메소드들을 확인해보면 다음과 같다.

```java
void windowActivated(WindowEvent e)
void windowClosed(WindowEvent e)
void windowClosing(WindowEvent e)
void windowDeactivated(WindowEvent e)
void windowDeiconified(WindowEvent e)
void windowGainedFocus(WindowEvent e)
void windowIconified(WindowEvent e)
void windowLostFocus(WindowEvent e)
void windowOpened(WindowEvent e)
void windowStateChanged(WindowEvent e)
```

이상한점이 있다. 추상클래스의 메소드 중 단 하나라도 추상메소드가 존재하지 않는다. 왜 이것이 추상클래스일까? 이렇게 선언은 abstract로 되어있는 클래스의 모든 메서드가 abstract가 없다면, 이런 것을 의미상의 추상 클래스라고 한다.

의미상의 추상 클래스는 스스로 객체를 생성할 때 내가 원하는 최소 1개의 메소드를 오버라이딩 하면 된다. 자신의 생성자로 객체를 생성하는 것(Anonymous 클래스, dynamic 객체 생성)을 권장한다.



### 인터페이스(Interface)

하위 클래스들이 상위 클래스로부터 상속받아서 메소드를 재활용하는 것이 아니라 하위 클래스마다 다르게 오버라이딩하여 활용할 필요가 있는 경우에는 어떨까? 혹은 한 클래스에서 여러가지 추상클래스들의 메서드를 가지고 싶으면 어떻게 해야할까? Java는 단일 상속만 지원하므로 이러한 상속은 이루어질 수 없다. 이러한 추상클래스의 단점과 단일 상속만으로 부족한 부분을 극복하기 위해 나온 개념이 `Interface`이다.

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



### 상속? 구현?

- `extends`를 통해 자식 클래스는 부모 클래스의 멤버변수, 메서드를 자동으로 받는다.
- `implements`를 통해 자식 클래스는 인터페이스의 추상 메서드들을 구현해야 한다.
- 상속, 구현을 하는 클래스는`@override` Annotation과 함께 추상클래스/인터페이스의 추상 메서드들을 구현해주어야 한다.



### Anonymous Class

anonymous : 추상 클래스가 가지고 있는 메서드를 오버라이딩하면서 동시에 객체 생성을 하는 방법이다. 추상클래스나 인터페이스로 객체를 가지는 방법 중 하나로 활용된다.

   ```java
//일반 클래스와 메서드
public class A {
    public int x() {
        return 10;
    }
}

//일반적인 메소드 결과
A a = new A();
a.x(); //10
A b = new A();
a.x(); // 10
   ```

   위의 코드가 우리가 일반적으로 알고 있는 클래스와 객체 생성, 그리고 해당 객체의 메소드 호출이라면

   ```java
//anonymous class : 동적으로 객체를 생성하는 방법
abstract class A {
    public abstract int x();
}

//A a = new A();//추상이므로 이렇게 할 수는 없다
A a = new A() {
    public int x() {
        return 10;
    }
};

A b = new A() {
    public int x() {
        return 20;
    }
};

a.x(); //10
b.x(); //20
   ```

위의 코드는 추상클래스의 인스턴스 생성을 위한 anonymous클래스 정의, 객체 생성, 그리고 객체의 메소드 호출이다. a와 b에서 anonymous클래스를 정의할 때 오버라이딩하는 메소드의 내용을 다르게 하자 인스턴스메소드의 내용이 달라진 것을 확인할 수 있다.

Anonymous 클래스를 활용할 때 주의할 점은, 만약 외부 변수를 Anonymous 클래스에 활용하고 싶다면 해당 변수는 `final`변수이거나 `final`변수처럼 변하지 않아야 한다는 점이다. 다음 예시를 통해 이를 확인할 수 있다.

```java
interface MyInterface {
	public void a();
}
```

이러한 interface가 있다고 가정하자.

```java
public static void main(String[] args) {
    int i = 10;
    MyInterface a = new MyInterface() {
        public void a() {
            int a = 10;
            System.out.println(a + i);
        }
    };
    a.a();
}
```

해당 코드를 실행하면 a + i = 10 + 10 = 20이라는 결과가 출력된다. 즉 final로 선언하지 않아도 i의 내용이 바뀌지 않았으므로 외부변수 i를 활용할 수 있다.

```java
public static void main(String[] args) {
    for(int i = 0; i < 5; i++) {
        MyInterface a = new MyInterface() {
            public void a() {
                int a = 10;
                System.out.println(a + i);
            }
        };
        a.a();
    }
}
```

해당 코드를 실행하면 `Local variable i defined in an enclosing scope must be final or effectively final`라는 에러가 발생한다. 즉 외부변수를 사용하려면 그 외부변수는 변하지 않아야 한다는 것이다.

anonymous로 오버라이딩한 메소드는 1회성 메서드이다. 즉 해당 객체에서만 유용한 메소드라는 것이다. 추상메서드나 인터페이스에서 추상메서드가 적은 경우에 활용하면 좋다.



### Lamda

Singla Method Interface를 `FunctionalInterface`라고 한다. `Lambda`표현식은 Single Method Interface에 대해 그 구현체의 구현 method를 매우 단순한 표현으로 가능하게 한다.

`@FunctionalInterface` Annotation은 해당 interface가 하나의 추상메서드만 가지고 있다는 것을 의미한다. 이 Annotation이 없더라도 interface가 하나의 추상메서드만 가진다면 자동으로 Functional interface로 간주한다.

```java
public interface MyInterface {
    it add(int i, int j);
}

public class MyFuncImpl implements MyInterface {
    @Override
    public int add(int i, int j) {
        return 1 + j;
    }
}

public static void main(String[] args) {
    int a = 1;
    int b = 2;
    MyInterface obj = new MyFuncImpl();
    int res = obj.add(a, b);
    System.out.println(res);
}
```

Interface를 Implements하는 구현체 Class를 만드는 과정이 조금 복잡하다. 하지만 익명 Class가 이러한 불편을 조금 해소해준다.

```java
MyInterface obj = new MyInterface() {
    @Override
    public int add(int i, int j) {
        return i + j;
    }
}
int res = obj.add(a, b);
System.out.println(res);
```

Lambda Interface를 사용하면,

```java
MyInterface obj = (i, j) -> i + j;
int res = ojb.add(a, b);
System.out.println(res);
```

코드가 훨씬 줄어들었다는 것을 볼 수 있다. 해당 코드에서 i, j가 어떤 변수로 들어오는지에 대한 정보는 이미 Functional Interface형태로 정의가 되었기 때문에 지정했던 변수형을 사용하는 코드를 작성만 하면 된다.

```java
MyFunc.m((i, j) -> i + j);
MyFunc.m((i, j) -> i * j);
MyFunc.m((i, j) -> i - j);

class MyFunc{
	static void m(MyFuncIF p) {
		System.out.println(p.add(5, 3));
	}
}
```

Functional Interface를 인자로 받는 메서드는 호출할 수 있는 메서드가 딱 하나이다. 하지만 parameter를 전달할 때 lambda식을 통해 다양한 방법으로 구현할 수 있다.

- Lambda식은 단순하다
- 다양한 구현이 가능하다(Runtime시 구현 완료)

- Lambda 표현식은 다양한 방식이 있다.
  - (p1, p2) -> {statements;} 기본
  - p1 -> {statements;} parameter 1개
  - (p1, p2) -> statement; statement 1개
  - () -> {statements;} no parameter
  - p1 -> {return ...;} return

다음은 Lambda와 Collections의 sort 메서드를 활용하여 간단하게 sorting로직을 구현한 코드이다.

```java
List<Node> list = new ArrayList<Node>();
list.add(new Node(3));
list.add(new Node(4));
list.add(new Node(1));

Collections.sort(list, (n1, nn2) -> n2.v - n1.v);

class Node {
    int v;
    public Node(int v) {this.v = v;}
}
```

