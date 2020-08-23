# Java Encapsulation(캡슐화)

### `Encapsulation`

캡슐화는 자바의 특징중 하나인 OOP의 꽃이라고 부를 수 있다. 외부에서 객체의 변수나 메소드에 직접 접근할 수 없게 만들어 내부 코드구현/프로그램을 숨길 수 있고, 외부에 허용된 변수/메소드에만 접근할 수 있도록 하는 것이 바로 Encapsupation(캡슐화)이다.

캡슐화를 이해하기 위해서는 자바의 접근제어자에 대해 알아야 한다. 접근제어자는 `Java 상속.md`에 정리되어 있다. 캡슐화를 원하는 변수/메소드를 private처리하고, 외부에 제공할 메소드를 public으로 만들어 외부에서 직접적인 접근을 막는 것이 기본 원리이다.

```java
class EncapsuledClass {
    private int a; // 외부에서 접근할 수 없는 변수
	private String b; // 외부에서 접근할 수 없는 변수
	private List<Integer> list; // 외부에서 접근할 수 없는 변수

    // 외부에서 변수 a의 값을 가질 수 있게 하는 메소드
	public int getA() {
		return a;
	}

    // 외부에서 변수 a의 값을 바꿀 수 있게 하는 메소드
    public void setA(int a) {
		this.a = a;
	}

    // 외부에서 변수 b의 값을 가질 수 있게 하는 메소드
    public String getB() {
		return b;
	}

    // 외부에서 변수 b의 값을 바꿀 수 있게 하는 메소드
    public void setB(String b) {
		this.b = b;
	}

    // 외부에서 변수 list의 값을 가질 수 있게 하는 메소드
    public List<Integer> getList() {
		return list;
	}

    // 외부에서 변수 list의 값을 바꿀 수 있게 하는 메소드
    public void setList(List<Integer> list) {
		this.list = list;
	}
}
```

위의 예에서 나온 것처럼 primitive 형태나 referenced형태 모두 privite 으로 선언하여 외부 접근을 막은 후, `getter` 메소드인 `getXXX()`, `setter`메소드인 `setXXX()`를 통해 해당 변수들에 대한 간접적인 접근을 허용한다.



### `Singleton Pattern`

자바의 캡슐화를 활용한 디자인 패턴으로, 외부에서 해당 클래스의 객체생성을 하지 못하도록 클래스의 생성자를 `private`로 만든다. 이후 public으로 정의하는 메소드를 통해 외부에 클래스의 객체를 제한적으로(보통 1개만) 제공하는 방법이다. 싱글턴 패턴을 구현하는 방법은 다양하지만 그 중에서 하나를 소개하자면 다음과 같다.

1. private으로 생성자 만들기

   ```java
   public class SingletonClass {
   	private SingletonClass() {};	
   }
   ```

   이렇게 생성자를 private로 지정함으로써 외부에서 해당 클래스의 객체를 생성할 수 없게 해준다.

2. private static으로 자기 객체 만들기

   ```java
   public class SingletonClass {
   	private SingletonClass() {};
       
       private static SingletonClass singletonClass;
   }
   ```

   외부에 전달하기 위한 자기 객체를 만들어 준다. 이 때, 객체를 private로 지정함으로써 외부에 해당 객체에 직접적으로 접근하지 못하도록 막는다. 또한 static으로 지정함으로써 외부에서 해당 클래스의 객체를 만들지 않아도 사용할 수 있게 해준다.

3. public static으로 자기 자신 return하는 메소드 만들기

   ```java
   public class SingletonClass {
   	private static SingletonClass singletonClass;
       
   	private SingletonClass() {};
       
   	public static SingletonClass getInstance() {
   		if(singletonClass == null)
   			singletonClass = new SingletonClass();
   		return singletonClass;
   	}
   }
   ```

   외부에서 접근할 수 있는 public으로 메소드를 선언함으로써 해당 클래스의 객체를 반환받을 수 있도록 해준다. 또 해당 메소드는 static메소드로 지정해야 하는데, 외부에서는 객체를 얻기 위해 해당 메소드를 사용하는 것이기 때문에 해당 메소드를 가지고 있지 않다는 것과 같다. 따라서 클래스메소드로 정의해야 외부에서 해당 메소드에 접근할 수 있다. 메소드의 `==null`조건을 통해 제공하는 객체를 하나로 제한하는 것을 알 수 있다.

4. 외부에서 해당 클래스 객체 얻기

   ```java
   SingletonClass singletonClass = SingletonClass.getInstance();
   ```

   외부에서 해당 클래스의 객체를 얻는 방법은 간단하다. 생성자를 활용하는 것도 아니고 해당 클래스의 클래스메소드로 정의된 `getInstance()` 메소드를 통해 객체를 얻을 수 있다.

