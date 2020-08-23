# Java I/O

### Java IO

- 파일을 읽고 쓰는 과정 : File로부터 JVM에 읽어들인 후에 다시 FIle로 쓰는 과정.

- Java는 이러한 일련의 과정을 Stream으로 이해하고 표현한다. 즉 Java에서 Stream -> IO

- Java Program 中

  - 입력 장치 : java프로그램에서 사용하는 데이터를 발생시키는 장치

    키보드, 마우스, 마이크, 파일, 네트워크, ...

  - 출력 장치 : java프로그램에서 만들어낸 데이터를 내보내는 장치

    콘솔, 모니터, 스피커, 프린터, 파일, 네트워크, ...

이러한 입출력 장치와 Java 언어는 서로 독립적이다. 이러한 입출력장치와 소통하기 위해 java는 `Stream`이라는 관을 뚫어놓는다. 즉 Java로 들어오는 입력은 `InputStream`, Java에서 나가는 출력은 `OutputStream`이라고 한다. 이러한 Stream들은 단방향으로 되어있다.

Stream은 가장 마지막 단에 있으므로 함수를 호출할 때 Stream이 가장 마지막에 온다. 함수 호출관계를 확인하면서 method가 byte를 받는지 char를 받는지 확인해보면 구조를 이해하기 좋다.

- `Stream` : 연속된 바이트 단위의 흐름. Keyboard의 입력, Monitor에 출력 할 때 처리하는 형태 역시 Stream
- 데이터의 시작과 끝이 있고 그 사이에서 데이터의 흐름(Stream)이 발생하는데, 이러한 데이터의 시작과 끝을  Node라고 부른다.

### Java I/O의 분류

java의 I/O 클래스를 분류하는 방법은 크게 2가지가 있다.

- 다루는 데이텨 형식에 따른 분류 : Byte(Binary) <-> Character

- 기능(이동 통로)에 따른 분류 : 1차 Stream : `File`, `Piped`, `String`, 등

  ​											<-> 2차 Stream(Filtered Stream) : `Filter`, `Buffered`, `Object`, `Print`, 등

- `Stream` = `Character`(문자) + `Binary`(비문자)

- ByteStream : 모든 data가 1byte 단위로 전송(Node Stream)

  Binary를 처리하는 Stream --> ...Stream으로 구성

- CharacterStream : 모든 data가 2byte 단위로 전송(Filter Stream)

  Character를 처리하는 Stream --> ...Reader, ...Writer로 구성

- Node Stream의 시작과 끝 사이에서 다른 기능을 하는 Stream들이 있다(2차 Stream).
  - Buffered(+ Input/OutputStream, + Reader/Writer) : Buffer를 활용해서 속도 향상. 어느 정도 담은 다음에 한번에 처리
  - Object(+ Input/OutputStream) : character말고 binary에 대한 지원. 메모리의 객체 상태를 파일로 저장/복원. 객체를 file로 저장
- IO관련 Class, Interface들은 대부분이 IOException 또는 그 하위 Exception을 throws한다.

ByteStream보다 CharacterStream이 더 느리다. 하지만 사용자가 사용하기에 더 편하다.

Scanner(System.in)에서 Scanner는 filter가 아니다. 내부적으로는 filter가 되겠지만 처리는 했겠지만 util이다.

```java
int x = System.in.read();
```

문자 하나(1byte)만 읽을 수 있다. 한글을 읽을 수 없다.

```python
byte[] b = new byte[1024];
int x = System.in.read(b);
```

x는 읽어들인 바이트 수를 가지고 있다. 엔터를 포함한 글자수를 출력한다. 윈도우에서는 엔터가 2byte로 표현된다. b에는 입력이 바이트코드 단위로 저장되어 잇다.

b를 찍어보면 읽은 글자들의 아스키코드들이 b배열에 들어가있다는 것을 알 수 있다.

```java
for(int i = 0; i < x; i++)
    System.out.print((char)b[i]);
```

하지만 여전히 한글을 입력하면 제대로 나오지 않는다. 한글 한 글자를 표현하는데 utf-8방식에서는 3byte를 필요로 하기 때문이다.

```java
//String st = new String(b, x-2); //윈도우에서 엔터가 2byte이므로
String st = new String(b);
System.out.println(st);
```

위의 코드를 통해 입력받은 바이트배열을 다시 string형태로 바꿀 수 있다.



`File Class`

java.io에 속한 클래스. file, folder의 구분 없이 모두 file Class로 표현한다.

OS마다 다른 경로 구분자를 표현하기 위해 File.Separator(statice변수)를 제공하여 OS 독립적인 코드를 구성할 수 있게한다. Java의 OS independent한 성질을 유지하게 해준다.

- File Class 객체를 만드는 3가지 방법

  - 인자로 폴더경로를 입력

    ```java
    String dir = "c:" + File.separator + "folder1" + File.separator + "folder2";
    File f1 = new File(dir);
    f1.mkdir();
    ```

  - 인자로 파일 경로 및 파일명을 입력

    ```java
    String dir = "c:" + File.separator + "folder1" + File.separator + "folder2";
    String folderName = "test.txt";
    File f2 = new File(dir, folderName);
    f2.createNewFile();
    ```

  - URI형태의 파일 객체 생성

    ```java
    String url = "fie:///C:/folder1/folder2/test.txt";
    File f3 = new File(url);
    f3.createNewFile();
    ```

    

```java
FileInputStream fi = new FileInputStream(srcDir);
FileOutputStream fo = new FileOutputStream(targetDir);
```

Node Stream인 File[Input/Output]Stream을 활용하는 것보다

```java
BufferedInputStream bi = new BufferedInputStream(fi);
BufferedOutputStream bo = new BufferedOutputStream(fo);
```

보조 Stream인 Buffered[Input/Output]Stream을 활용하는 것이 훨씬 빠르다.



`.csv`

excel을 활용할 때 `.csv`확장자를 활용한다. <fileName>.csv

```java
BufferedWriter writer = 
    new BufferedWriter(
		new OutputStreamWriter(
        	new FileOutputStream(filePath+File.separator+fileName), "MS949"))
```

MS949 : 인코딩방식으로 한글이 깨지지 않게

csv방식에서는 구분자로 `^`를 사용하는것이 편하다.

실행을 더블클릭으로하면 `^`를 구분 안할 수도 있다. 따라서 excel -> 데이터 -> 텍스트를 눌러 해당 파일을 찾아간 후 [구분 기호로 구분됨], 기터(`^`), 텍스트 서식 -> 마침을 하면 각 셀이 구분되어 나타나진다.



### 객체 직렬화

ObjectOutputStream, ObjectInputStream을 사용, Object를 binary형태로 파일로 생성하여 저장해두었다가 필요시 다시 꺼내서 사용하는 것이다.

객체 직렬화를 위한 `Serializable Interface` : 추상 method가 없다. 즉 객체 직렬화 대상인지 아닌지만 확인하는 serial number같은 느낌이다.

`serialVersionUID` : 객체 직렬화 과정에서 사용되는 고유의 id. 보통 중요하지 않으면 1L로 설정

`transient` : (보안 등의 이유로) 객체 직렬화에서 제외되는 대상



### Stream API

Data를 복사의 개념으로 읽어들이면서 적절한 필요에 따라 데이터의 흐름을 이용해서 데이터에 다양한 기능을 수행할 수 있도록 도와주는 기능(Filter, Map, 등). 분석에 많이 활용된다.
