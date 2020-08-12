# 다형성(Polymorphism)

> `다형성(Ploymorphism)`은, OOP의 가장 큰 특징중 하나로, 하나의 이름으로 여러 형태를 구성하는 것이다.

다형성은 크게 method 다형성, Type 다형성으로 나눌 수 있다.

### Method 다형성

- `Overloading`

  method 이름이 같아도, parameter가 다르면 별개의 method로 간주한다(return 타입, 접근 제어자는 무관).

  클래스에서 기본 생성자와 여러 parameter를 받은 생성자를 같은 이름으로 정의하는 것도 같은 원리이다.

  JVM은 오버로딩된 메소드를 호출할 때 매개 변수의 타입을 보고 메서드를 선택한다. 따라서

  - <변수명>(int a, long b)
  - <변수명>(long a, int b)

  다음과 같이 헷갈리게 함수를 작성한다면 <변수명>(1, 1)과 같이 호출할 경우 어떤 매개 변수로 들어갈 것인지 헷갈리기 때문에 오류가 발생할 수 있다. 따라서 같은 매개변수들의 다른 순서로 오버로딩 하지 않도록 주의하자.

  Java에서는 연산자 오버로딩을 지원하지 않는다.

- `Overriding`

  상속 관계에서 부모 Class의 method를 자식 Class에서 재정의할 수 있다. 자식 Class에서 해당 method 호출 시, 재정의한 method가 호출된다.

  overriding에서는 부모 클래스의 메서드와 이름, 매개변수, 리턴 타입이 모두 동일해야 한다.

  메서드 overriding시, 부모 Class에서 정의된 접근 제어자보다 자식 Class에서 정의되는 접근 제어자는 같거나 더 넓은 범위어야 한다.

  부모 클래스에서 static으로 정의된 메서드는 자식 클래스에서 같은 이름/매개변수/리턴타입으로 정의한다고 해도 이는 Overriding이 아니라 같은 이름의 새로운 메서드를 정의한 것 뿐이다.

  

### `@Override` Annotation

메서드를 재정의할 때, @Override Annotation을 사용하면 컴파일러에 상속 관계에서 오버라이딩 했다는 사실을 명시할 수 있다. 이 때 부모 Class에 해당 메서드가 없는 경우, 즉 의도한 overriding이 이루어질 수 없는 경우 오류를 발생시킨다.

- `Annotation` : 주석의 일종으로, 역할을 가지고 있는 주석이다.



### Type 다형성

- 부모 Type으로 자식 Type의 객체를 Reference할 수 있다. 즉, 한 개의 Type으로 여러 하위 Type의 객체를 할당받을 수 있다.

  ex) 상속 관계가 Object <- Parent <- Phild일 때

  Object o = new Object(); 가능

  Parent p = new Parent(); 가능

  Child c = new Child(); 가능

  Object o = new Parent(); 가능

  Object o = new Child(); 가능

  Parent p = new Child(); 가능

  Parent p = new Object(); 불가능

  Child c = new Parent(); 불가능

  Child c = new Object(); 불가능

- 부모 Type으로 자식 Type 객체를 Reference하였을 때

  Parent p = new Child()

  - 가장 마지막에 오버라이딩된 메서드가 호출된다.
  - 부모클래스에는 없고 자식 클래스에서 정의한 메서드는 호출할 수 없다
  - 부모 클래스에서 정의한 메서드, 이를 자식 클래스에서 오버라이딩한 메서드만 호출 가능하다.

- B가 A를 상속받았다고 가정하자.

  ```java
  Class A {
      int a = 1;
  }
  Class B extends A {
      int b = 2;
  }
  ```

  ```java
  A a = new B();
  System.out.println(a.a);
  System.out.println(a.b); // exception 발생
  ```

  객체가 생성자를 통해 생성되려면 무조건 상위클래스의 기본생성자가 먼저 실행 후 해당 클래스의 생성자가 실행되는 과정을 거친다. 즉 객체는 Object() -> A() -> B() 순으로 생성자가 실행되어 완성이 되고, a는 그 중에서 A()로 생성된 객체를 가리킨다. 실제로 메모리(Heap)에는 Object(), A(), B() 객체가 다 생성되있고 모두 연결되어 있다.

  이 때, a.a는 크래스 A가 가지고 있던 변수이므로 접근이 가능하지만 a.b는 클래스 b만 가지고 있는 변수이므로 A클래스의 인스턴스로 선언한 a에서는 접근할 수 없다. 따라서 exception이 발생한다.

  ```java
  B b = a; //exception 발생
  ```

  A의 인스턴스였던 a를 클래스 B의 b변수로 reference하려고 시도하는 경우, 오류가 발생한다. 이는 다형성을 거꾸로 활용하여 틀린 것이다. 상위 클래스의 변수로 하위 클래스의 변수는 reference할 수 있지만, 하위 클래스의 변수로 상위 클래스의 변수를 reference할 수 없다.

  ```java
  B b = (B)a;
  System.out.println(a.a);
  System.out.println(a.b); // exception 발생
  ```

  위의 코드를 보면 A의 인스턴스였던 a를 B로 형변환하여 B클래스의 변수 b에 reference하였다. 이렇게 explicit casting을 통해야만 하위클래스가 상위클래스의 변수를 reference할 수 있다. 하지만 조건이 있다. 다음의 코드를 보자.

  ```java
  A a = new A();
  B b = (B)a; //exception 발생
  ```

  이번 코드는 A클래스의 인스턴스를 a로 가리키고, 이를 다시 B클래스의 변수 b로 형변환하여 가리키려고 하는 상황이다. 이 상황에서는 왜 exception이 발생할까?

  처음의 객체 생성 과정을 살펴보자. 객체가 생성자를 통해 생성될 때 무조건 상위 클래스의 기본생성자가 먼저 생성된 후 생성된다고하였다. 즉 A a = new A(); 과정에서 만들어진 객체는 A보다 먼저 만들어지는 Object 객체와 A객체 뿐이다. B객체는 가지고있지 않기 때문에 B로 강제형변환을 할 수 없다.

  이렇게 만들어진 a를 b로 가리키려면 형변환이 필요하다. 따라서 B b = a에서 exception이 발생한다.

  ```java
  A a = new B();
  B b = (B)a; //형변환을 통해 제대로 작동
  System.out.println(a.a);
  System.out.println(a.b); // exception 발생
  ```

  위의 상황은 우선적으로 생성된 객체가 A클래스의 객체이기 때문에 아무리 B로 형변환을 하였어도 B클래스만 가지고 있던 변수인 b에는 접근할 수 없다는 것을 말해준다.

  ```java
  B temp = new B();
  System.out.println(temp.a);
  System.out.println(temp.b);
  A a = temp;
  System.out.println(a.a);
  // B b = a; // exception 발생
  B b = (B)a;
  System.out.println(a.a);
  System.out.println(a.b); // exception 발생
  ```

  위의 코드를 보면 아무리 B 생성자로 객체를 먼저 만들었다고 해도 A로 형번환 후 다시 B로 자동형변환을 할 수 없다는 것이다. 또 강제형변환을 하더라도 B가 가지고 있던 변수 b에 접근시 오류가 발생한다는 것을 알 수 있다. 이것을 통해 상위클래스로 형변환을 하면 하위클래스만 가지고 있던 변수나 메서드는 더이상 이용할 수 없다는 것을 짐작할 수 있다.

- C와 B가 각각 A를 상속받았다고 가정하자.

  ```java
  A a = new B();
  B b = (B)a;
  A aa = b;
  C c = (C)aa;
  ```

  해당 코드는 문법적으로는 맞다. 따라서 컴파일러가 오류를 띄우지는 않는다. 하지만 메모리상으로 C의 객체가 없으므로 Runtime Error가 뜬다.

