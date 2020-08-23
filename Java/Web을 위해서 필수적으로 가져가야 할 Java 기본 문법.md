# Web을 위해서 필수적으로 가져가야 할 Java 기본 문법

- 변수

  - `접근제어자`

    public, protected, (default), private

  - `Primitive Type`

    논리형(boolean), 문자형(char), 숫자형(정수형 + 실수형)

- 연산자

  - 산술연산자
  - 논리연산자
  - 증감연산자
  - 비트연산자
  - 삼항연산자
  - 비교연산자

- 조건문(`if`, `if-else`, `if-else if-else`, `switch`)

- 반복문(`while`, `do-while`, `for`)

- `메소드(접근 제어자 + 이름 + Argument + return type)`

- 클래스(여러 가지 메소드를 묶기 위해)

  - 클래스 = 변수 + 메소드
  - 변수 : 생성 + 사용
  - Reference data : 선언 + 생성 + 사용

  - final 클래스(상속을 받을 수 없다) - `String`

- 생성자

  - 객체를 생성해 준다

  - 클래스와 같은 이름
  - `오버로딩`을 통해 여러가지를 만들 수 있다.
  - 오버로딩 : 인자값의 갯수나 Type이 달라야 한다.
  - `this`, `super`

- 상속

  - 상위클래스의 기능을 그대로 사용할 수 있다.
  - 혹은 `오버라이딩`을 통해 기능을 재정의할 수 있다.
  - `equals()`, `toString()` 등을 재정의해서 활용
  - 단일 상속만 가능하다

- 인터페이스

  - 상속 개념이 아닌 `구현(Implements)`의 개념. 명세서의 개념
  - 추상클래스가 원조이다
  - 단일 상속의 단점을 보완하기 위해 나온 개념
  - 상수, 추상메소드만 가질 수 있다.

- 추상 클래스

  - 변수, 상수, 생성자, 일반메소드, 추상메소드 다 가질 수 있다.
  - 하위클래스에게 반드시 오버라이딩해야 하는 메소드를 지정해 줌으로써 제어할 수 있다.

- 패키지

  기능별로 폴더를 만들어서 묶어놓은 것

  - 사용자 정의 패키지

  - 시스템 패키지(`java.awt`, `java.lang`, ...)

    1. 기본 패키지

       `java.XXX` - `lang`, `util(Collection, List, Set, Hast, StringTokenizer)`, `io`, `sql(JDBC)`, `text(Format)`

    2. 확장 패키지(`javax`(?))

- Exception

  - Runtime

    예외처리가 아니라 `Logic`으로 예외가 발생하지 않도록 막아야 한다

  - non-Runtime

    예외처리를 하지 않으면 Complie시 오류가 된다.

    1. `throws` : 예외가 발생한 메소드를 호출한 상위 메소드에 던진다

    2. `try`-`catch`-`finally` : 예외가 발생한 메소드에서 처리를 한다

       ```java
       try {
           //에외 발생 코드
       } catch(Exception1 e1) {
           // e1이 e2보다 하위 예외여야 한다
       } catch(Exception2 e1) {
           //
       } finally {
           //반드시  처리해야 하는 코드
       }
       ```

  - 사용자 정의 Exception

    사용자가 원하는 상황(비밀번호가 틀렸을 때, 원하는 숫자 범위가 아닐 때, 등)

    ```java
    class MyException extends Exception {
        public MyException() { }
        public MyException(String message) {
                super(message);
        }
    }
    ```

  - throws : Error를 던질 것이다, throw : Error를 던져