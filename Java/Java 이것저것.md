# Java 이것저것

### `JVM(Java Virtual Machine)`

Java의 가장 큰 장점을 만들어주는 머신이다. 한번 작성한 Java코드는 `javac` compiler에 의해 `.class`라는 형식의 `java byte code`로 complie 되는데, 이 java byte code는 모든 OS에서 작동되지 못한다. 하지만 각 OS마다 JVM을 가지고 있어서 이 java byte code를 해당 OS에 맞게 해석하여 실행시킨다.



### Static 공간

JVM안에는 Stack, Heap말고도 Static이라는 공간을 가지고 있다. 



### Inner class(local)

class 안에서 다시 정의되는 class를 inner class라고 하며, 바깥 class의 일부처럼 사용한다. 별도의 객체가 만들어지며, 다른 클래스와는 무관하지만 바깥 클래스에서만 의미있게 활용할 용도로 사용된다.

### Inner class(anonymous)

클래스 안에서 이름없이 만들어지는 inner class이다. 이름이 없으므로 재사용되지 않고, 객체를 생성하는 코드에 바로 class의 내용을 전달한다. `Event Handling`처럼 interface에 정의된 method의 구현부를 객체 생성 시점에 전달한다.

### Inner class(static)

별도의 객체를 만들지 않고 사용하 수 있는 클래스. 다른 Class에서 사용하지 않고 바깥 클래스에서만 의미가 있는 경우 많이 사용된다.

