# Java Thread

- 프로세스 : CPU에서 처리할 수 있는 작업의 단위. process = 컴퓨터에서 돌아가고 있는 프로그램

- Thread : 하나의 프로세서 안에서 동시에 여러 작업을 처리할 수 있는 단위

  Java에서는 main메소드를 실행하는 스레드가 기본적으로 하나 있다.

- Thread를 사용하는 방법

  1. extends Thread
  2. implements Runnable

- Thread의 상태

  new(`start()`) --> runnable

  runnable --> running

  running --> `run()` --> terminate

  ​				--> waiting

  ​				--> `yield()` --> runnable

  waiting --> runnable

  - 생성

    - Thread t = new ThreadExt();  Thread를 상속받은 클래스의 생성자로 생성
  
    ```java
      public class ThreadExtend extends Thread {
        @Override
          public void run() {
              //스레드를 활용하여 처리할 코드
          }
      }
      
    public static void main(String[] args) {
          Thread t1 = new ThreadExtend();
        Thread t2 = new ThreadExtend();
          t1.start();
        t2.start();
      }
    ```
  
    
  
    - Thread t = new Thread(new RunnableImpl()); Runnable을 구현한 클래스의 생성자로 생성
  
      ```java
      public class RunnableImpl implements Runnable {
          @Override
        public void run() {
              //스레드를 활용하여 처리할 코드
        }
      }
    
      public static void main(String[] args) {
        Thread t1 = new Thread(new RunnableImpl());
          Thread t2 = new Thread(new RunnableImpl());
          t1.start();
          t2.start();
      }
      ```
  
  - 실행 : `.run()`을 overriding한 후 `.start()`를 활용하여 실행
  
    우리는 Thread 클래스의 run()메소드를 직접 호출할 수 있다. 하지만 스레드를 생성 후 run() 메소드를 직접 호출하면 우리가 실행한 순서대로 시작->종료->다음시작->종료->다음시작->종료 이렇게 실행이 된다.
    
    스레드가 동시에 실행되기 위해서는 생성 -> 준비 -> 실행 단계 이렇게 중간에 준비단계가 필요하다. 준비는 `.start()`메소드를 통해 실행된다. 준비단게에서는 `Thread Scheduler`에 의해 임의로 thread의 호출 순서를 정하여 자동으로 run() 메소드를 실행시킨다.Thread가 running상태로 들어간다고 해서 run()메소드가 끝까지 완료된다는 보장은 없다. `sleep()`, `wait()`, `join()`, `I/O블로킹`등의 원인으로 `waiting`상태로 들어갔다가 다시 `runnable`상태로 들어갈 수 있다.
    
  - 대기(`.sleep()`)
  
    Thread.sleep()을 통해 실행 run()메소드를 실행한 쓰레드를 실행 상태에서 대기상태로 전환 -> 시간이 지나면(Time Out) 자동으로 대기상태에서 실행상태로 전환.
  
    ```java
    @Override
    public void run() {
        Thread.sleep(10);
    }
    ```
  
  - 중지(`.stop()`)
  
    가능하면 쓰면 안되는 메소드
  
  - 종료
  
    - 강제 종료
    - Run() method의 종료로 인한 자동종료

### `join()`

특정 Thread 객체가 종료될 때까지 기다렸다가 해당 Thread 종료 후 실행

### `interrupt()`

스레드가 `.sleep()`메소드에 의해 일시정지 상태애 있을 때, 해당 스레드를 정상종료시키는 메소드

### `synchronized`

공유하는 자원에 대한 관리에 용이. 가장 먼저 접근하는 Thread가 공유되는 자원에 Lock을 걸어 다른 Thread들의 접근을 제한한다.

```java
synchronized(<자원>) {
    //원래 내용
}
```

### `DeadLock`

Thread A가 자원 a에 lock을 걸고 코드를 실행 중 자원 b가 필요하다

Thread B가 자원 b에 lock을 걸고 코드를 실행 중 자원 a가 필요하다

A와 B모두 서로를 기다리면서 상황이 진행되지 않는다.

Deadlock을 해결하기 위해 `wait()`-`notify()`-`notifyAll()`메소드 활용한다.