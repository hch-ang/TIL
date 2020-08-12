# Java Throwable(Error + Exception)

### `Throwable`

Java의 Throwable class는 Java에서 발생하는 모든 Error와 Exception class들의 상위클래스이다. Throwable의 모든 object들은 JVM이나 JavaThrowStatement에 의해 처리될 수 있다.



### Error

...에러



### Exception

```java
public class Exception extends Throwable
```

Throwable 클래스를 상속받은 클래스로, Java에서의 Exception은 크게 RuntimeException과 일반 Exception으로 나눌 수 있다.



### Exception e

Java의 Error를 제외한 모든 종류의 Exception들은 `Exception`클래스의 하위 클래스로 정의되어 있다. 따라서, 어떤 Exception이던 처리를 하고자 할때 `Type Polymorphism`을 이용하여 Exception e로 reference한 상태로 처리가 가능하다.

```java
Exception e;

// Prints this throwable and its backtrace to the standard error stream.
// 에러메시지의 발생 근원지를 찾아서 단계별 에러를 출력한다.
e.printStackTrace();

// Returns a short description of this throwable.
// 발생한 에러의 종류와 발생한 원인을 간단하게 String형태로 반환한다.
e.toString()
    
// Returns the detail message string of this throwable.
// 에러가 발생한 원인을 간단하게 String형태로 반환한다.
e.getMessage();
```

API 문서의 내용과 직관적으로 맞지는 않지만, printStackTrace _. toString -> getmessage()순으로 간단하게 에러를 출력해준다. 하지만 이렇게 Exception 인스턴스 메소드를 통해 에러를 출력하는 방법은 디버깅용으로는 좋지만 결국 에러를 발생시켜 프로그램을 종료시키기 때문에 실시간으로 실행되어야 하는 프로그램에는 적합하지 않다.



### Runtime Exception

문법상으로는 맞는 코드이지만, Runtime중에 오류가 발생할 수 있다. 이러한 Exception들을 Runtime Exception이라고 한다. 문법상으로는 오류가 없기 때문에 정상적으로 컴파일 되지만 실행 과정중 오류를 발생시키기 때문에 `Runtime Error`가 발생한다.

- IndexOutOfBoundsException
- NumberFormatException
- NullPointerException

Java API 문서를 보면, Runtime Exception은 모두 상위클래스로 `RuntimeException`을 가지고 있다.

- Runtime Exception은 try-catch로 해결하는 것이 아니라 제대로된 로직을 통해 해결해야 한다. 보통 if문으로 상황을 나누어 Runtime Exception을 해결하는 경우가 많다.



### Exceptiom(non-runtime exception, catched exception)

컴파일시 오류가 생기는 exception. 컴파일 오류가 발생하므로 코드가 실행되지 않는다.

- IOException
- SQLException

throw 또는 try-catch구문을 통해 해결을 해야만 실행할 수 있다.

- `throws` : 예외를 처리하지 않고 떠넘기다

  ```java
  class A {
      void a() throws <exception name>{
          line 1
          /*
          의심되는 코드
          */
          line2
      }
  }
  
  class B {
      A a = new A();
      void b() throws <exception name> {
          a.a();
      }
  }
  ```

  - class B에서 A의 객체 a를 만들어서 a.a()메서드를 호출하였다.

  - class A에 정의되어있는 a()메서드를 실행 중, 의심되는 코드부분에서 결과는 달라진다.

    1. 의심되는 코드 부분에서 예외가 발생한 경우

       - 메서드 a()는 발생한 예외를 a()를 호출한 b()에게 throw한다.

       - 메서드 b()는 a()에게 받은 예외를 b()를 호출한 상위 메서드에게 throw한다.
       - 결과적으로 main() 메서드는 받은 예외를 VM에게 throw하여 MV이 오류를 처리하거나 보통 프로그램을 종료시킨다.

    2. 의심되는 코드 부분에서 예외가 발생하지 않는 경우

       이후의 코드(line2)를 정상적으로 실행시킨다.

- `try`-`catch` -`finally`: 예외에 대처하는 코드를 작성하다

  ```java
  class A {
      void a() {
          int x = 1;
          int y = 2;
          try {
              line 1
              /*
              의심되는 코드
              */
              line 2
          }
          catch (exception e){
              //exception발생 시 대처할 수 있는 코드
          }
          finally {
              //try, catch에 관계없이 마무리하는 코드
          }
      }
  }
  
  class B {
      A a = new A();
      void b() {
          a.x();
      }
  }
  ```

  - class B에서 A의 객체 a를 만들어서 a.a()메서드를 호출하였다.

  - class A에 정의되어있는 a()메서드를 실행 중, 의심되는 코드 부분을 try{}구문으로 묶어준다.

    1. try{}구문에서 예외가 발생하지 않는 경우, 이후의 코드(line2)를 정상적으로 실행한다.
    2. try{}구문에서 예외가 발생하는 경우, try코드를 중단시키고 catch{}구문을 실행한다.

  - `finally`구문은 예외가 발생하거나 발생하지 않거나에 상관없이 항상 실행되는 구문이다.

  - `catch`는 if-else if 구문처럼 다양한 error에 대해 처리를 할 수 있다. 이때, catch구문을 위에서부터 순서대로 체크하여 먼저 부합하는 구문만을 실행하기 때문에 상위 클래스의 Exception을 가장 아래에 넣어주어야 한다.

    ```java
    try {
        ...
    }
    catch (ArrayIndexOutOfBoundsException e) {
        
    }
    catch (Exception e) {
        
    }
    ```

    위의 코드를 보면, ArrayIndexOutOfBoundsException e를 Exception e보다 먼저 체크하고 있다. 만약 순서가 바뀌어 Exception e가 먼저 오게되면, ArrayIndexOutOfBoundsException은 Exception 클래스의 하위 클래스이기 때문에 Exception e에서 catch되어버린다. 이런 경우 세부적인 에러 케이스에 대한 대처가 힘들어진다.



### Throws vs try-catch-finally

Throws와 try-catch-finally 모두 catched error를 해결하기 위한 방법이다. 우리는 어떤 상황에서 각각의 기법을 처리해야할까?

- Throws : 해당 메서드에서 exception을 Throw하기 때문에 다른 사후 처리를 할 필요가 없어진다. 하지만 이 경우, 해당 메서드를 호출한 모든 상위 메서드들에서 exception을 처리해야 하기 때문에 번거로움이 생긴다.
- try-catch-finally : 해당 메서드에서 exception을 처리해버린다. 이러면 해당 메서드를 호출한 다른 메서드들에서 exception을 처리할 필요가 없어진다. 하지만 exception이 발생한 메서드를 호출한 다양한 메서드들이 다양한 처리를 원하는 경우에는 이 방법은 적합하지 않다. 따라서 이 경우에는 exception을 throw한 후 호출한 다양한 메서드들이 입맛에 맞게 처리해야 한다.

Throws와 try-catch-finally는 각자 장단점이 있다. 따라서 상황에 따라 적절하게 활용하여야 한다.



### 트랜젝션

Error가 발생하는 과정에서 트랜젝션의 흐름도 알고 있어야 한다. (나중에 추가)