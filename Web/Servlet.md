# Servlet

### persistence(영속성)

어떤 웹사이트에서 회원이 회원가입 정보를 입력하였다. 이 정보를 어떻게 저장해야 할까??

- File
  - id, name, pw, 등 회원에 대한 정보를 file에 저장
  - 회원의 수가 매우 많은 웹사이트의 경우, 이러한 파일이 회원 수만큼 만들어진다.
  - 이러한 파일을 I/O를 통해 접근하여 각 회원의 정보 중에서 ID, PW를 일일이 대조하는 작업은 매우 불편하고 오래 걸린다.
- DB
  - Database(내부적으로는 같은 File System)를 만들어서 SQL을 활용해서 Select, Insert, Update, Delete작업을 수행한다.

  - Java에서 JDBC에 접근할 때 : `jdbc`:.....mysql.../id/fw.... --> 여기서 jdbc는 프로토콜이다. 공통이다.

  - HTTP(Hyper Text Transfer Protocol)를 처리하기 위한 Web Browser와

    Database를 관리하는 RDBMS를 연결하기 위해서 중간 단계로 Java라는 환경이 필요하다(Java는 RDBMS에 접근할 수 있는 JDBC가 존재한다).

  - 클라이언트가 html, css, js를 통해 데이터를 발생시켜서 원하는 서비스를 요청한다. 해당 요청을 받기 위해서 서버단에서는 `http server(Web Server)`가 필요하다. http server는 데이터를 처리하는 역할이 아니라 client의 접속을 도와주는 역할을 한다.

  - client의 (http + css + JS) 데이터를 처리하기 위해서는 `Application Server`가 필요하다. 서버에서 Java를 처리하는 부분은 Application Server이다.
  - Tomcat은 Web Server와 Application Server의 역할을 둘 다 수행한다.

- Application Server가 하는 일
  - Presentation : html을 만들어서 보내주는 역할(html을 보여주는 역할은 Web Browser가 한다)
  - Business Logic : data를 받아서 로직 처리(로그인), 페이징 처리(계산)
  - Persistence Logic : DB의 table들과 연동하는 역할

- Application Server는 Java + Web을 동시에 실행할 수 있어야 한다. 이를 servlet(+JSP)이라고 한다.



### 프로젝트 만들 때

- Tatget Runtime

   프로젝트가 어떤걸 통해서 실행 될것인지?

  - 백엔드에서는 Java의 Main Method가 없어도 실행된다.
  - Servlet은 Runtime Server에 의해서 실행될 것이다(ex : tomcat).

- Dynamic web module version

  - 3.0 이후부터는 아직까지 큰 차이는 없다.
  - 하지만 3.0 이후와 3.0 이전은 많이 다르다.

- Context root : 
  - localhost:8080/`context root`/aaa.html
  - abc.com/`context_root`/...
- context root를 비워도 되지만 프로젝트를 만들다 보면 구분을 해야 할 상황이 생긴다. 
  
- Content directory
  
  - 자바 외 모든 소스들(HTML, CSS, JS, JSP, Image)
- deployment descriptor
  - 3.0 이후 --> annotation 사용 : @WebServlet("/URL")
  - 2점대 : annotation이 없다 --> xml(web.xml)을 만들어야 한다.

- 프로젝트를 통째로 import하면 환경이 다를 때 error가 날 수 있다.
  - File System으로 들어간 후 프로젝트로 From directory를 해당 workspace로 설정한다.
  - sec(java), Webcontent(나머지 소스들)만 체크한다음 Finish한다.



### servlet만들 때

- servlet은 java file이므로 `Class`파일로 만들어도 되지만 servlet을 implements하고 복잡한 과정이 필요하므로 `Servlet`파일로 만들어주는게 편하다. servlet파일로 만들어도 `.java`파일로 되어있다.
- URL mappings : @SebServlet("/URL")을 자동으로 만들어 준다. 이를 aaa.do로 바꿀 수 있다.
- HttpServlet을 extends 했지만 엄밀히 말하자면 `Servlet` Interface를 Implement한 것이다.



### Servlet

- `doGet`, `doPost` 메소드 : client의 전송방식에 따라 자동으로 실행되는 method들
  
  - HttpServletResponse : 응답과 관련된 변수
  - HttpServletRequest : 요청과 관련된 변수
  
- page 이동 방식
  1. link(href) : GET방식
  2. 주소 입력 : GET방식
  3. button(form -> submit) : <form `method:"GET"` action="url"> : GET방식
  4. button(form -> submit) : <form `method:"POST"` action="url"> : POST방식

- Client에서 GET(POST)방식으로 호출하였는데 `doGet(doPost)`을 정의하지 않은 경우 : `405 Error` 발생

- doGet, doPost로 응답을 만들 것이다. 클라이언트가 읽을 수 있는 대표적인 응답은 `HTML`이다.

- server에 있는 servlet은 Java파일인데 어떻게 java파일에서 client에게 전해줄 HTML파일을 포함시킬 수 있을까???

- Java는 Client에게 (I/O를 통해서)HTML파일을 출력시켜야 한다. 이 떄 사용하는 출력객체는 `PrintWriter`이다.

  - PrinwWriter out = response.getWriter();

  - 모든 html 내용을 out.println()형태로 등록시킨다.

    ```java
    PrintWriter out = response.getWriter();
    out.println("<html>");
    out.println("<body>");
    out.println("Hello World!!!!!");
    out.println("</body>");
    out.println("</html>");
    ```

  - HTML에서의 모든 white space(엔터, 띄어쓰기, 탭)은 하나의 띄어쓰기로 적용된다. 따라서 개행을 위해서는 println말고 `<br>`태그를 활용하여 주자.

  - Tomcat이 클라이언트한테 HTML을 넘겨줄 때 encoding을 사용하려면 따로 미리 설정해야 한다.

    ```java
    response.setCharacterEncoding("UTF-8");
    response.setContentType("text/html");
    ```

    혹은

    ```java
    response.setContentType("text/html;charset=UTF-8");
    ```

    를 사용하여 한글 인코딩을 설정해준다. 이 때, 이 설정은 무조건 `PrintWriter`를 만들기 전에 해야한다. response.getWriter()을 통해 stream을 연결하기 전에 encoding 방식을 설정해야 하기 때문이다.

    `text/html` : text형식으로 보내지만 html로 인식하라는 뜻

- html 코드 중 println에서 "str" + 변수 + "str"을 통해 변수값을 출력해줄 수도 있다.

- java에서 변수의 초기화하는 코드를 생성자 안에 넣음으로써 해당 객체가 생성될 때 초기화 시켜주는 방법을 사용할 수 있었다 --> 일반적으로 웹과 관련된 변수의 초기화는 기존의 java에서 생성자와 비슷한 개념인 servlet의 init()메소드 안에 설정해두면 좋다.

  ```java
  public void init() {
      name = "이호창";
  }
  
  protected void doGet(HttpServletRequest request, HttpservletResponse response) {
      response.setCharacterEncoding("UTF-8");
  	response.setContentType("text/html");
      PrintWriter out = response.getWriter();
      out.println("<html>");
      out.println("<body>");
      out.println("Hello" + name + "!!!!!");
      out.println("</body>");
      out.println("</html>");
  }
  ```

- 이렇게 긴 HTML을 servlet을 통해 라인별로 println해주는 것은 매우 불편하다. 이를 해결하기 위해 나온것이 `jsp`이다.

- `Servlet` = html in java

- `JSP(Java Server Pages)` = java in html

  

- `.do` : url token같은거다(보통 자사 사이트의 domain과 의미통한다.`.nhn`, `.cafe`, `.blog`이런식으로 사용된다)

  servlet을 생성할 대 URL mappings:라고 나와있다. 이 때` /<servletname>`으로 적어두면 form에서 `action = "<servletname>"`으로 접근할 수 있고, `/<url.do>`으로 적어두면 form `action = "url.do"`로 접근할 수 있다.



- Servlet 동작 흐름

  1. client(Web Browser)에서 data가 생성된다.

  2. `WAS(Web Application Server)`가 Request(데이터)를 받는다(Tomcat은 WAS중 하나)

     2-1 GET(주소에 Query String으로 받는다)

     2-2 POST(주소에 넘어가지는 않지만 HTML Body에 포함된다)

  3. WAS가 해당되는 Servlet(aaa.do, 등)을 찾아서 일처리를 한다.

     3-1 Data를 받는다

     3-2 Business Logic, DataBase Logic 처리를 한다.

     3-3 Response Page를 만든다



### Servlet API

- 원래는 나만의 Servlet파일을 만들기 위해서는 `Servlet`이라는 interface를 직접 구현해야 한다.

  - Servlet 인터페이스의 `destroy()`, `getServletConfig()`, `getServletInfo()`, `init()`, `service()` 메소드를 모두 오버라이딩 해야하므로 할 일이 많아진다.

- 이러한 일을 피하기 위해 추상클래스인 `GenericServelet`이 `Servlet`을 미리 implement하였다.

  ```java
  public abstract class GenericServlet extends Object
  implements Servlet, ServletConfig, Serializable
  ```

  - `init()`, `destory()`, `getServletConfig()`, `getServletInfo()`는 이미 implement되어있다.

  - 우리가 할용할만한 `service()`메소드만 abstract으로 설정되어 있다.

    ```java
    abstract void service(ServletRequest req, ServletResponse res)
    ```

  - GET방식과 POST방식일 때 넘어온 data를 처리하는 과정에 다소 차이가 있다. 이를 구분하기 위해 if문을 사용해야하고, 이 또한 귀찮은 일이다.

- 이를 해결하기 위해 `HttpServlet`이 `GenericServlet`을 상속받았다.

  ```java
  public abstract class HttpServlet extends GenericServlet
  ```

  API를 보면 HttpServlet의 메소드 중 추상메소드가 하나도 없다는 것을 알 수 있다. 즉 이 메소드 중에서 최소 1개의 메소드만 오버라이딩하여 사용할 수 있다는 것이다.

  - `doGet()`, `doPost()`메소드를 통해 각 상황에 따른 반응을 오버라이딩 할 수 있다.

- 결과적으로 우리가 사용할 Servlet 클래스는 `HttpServlet`을 상속받아 메소드중 최소 하나만 구현하여 활용하면 된다.

  ```java
  public class MyServlet extends HttpServlet {
      ...
  }
  ```

- Servlet파일 메소드 호출 순서

  ```java
  // 1. 생성자
  // 2. init() 메소드
  // 3. doGet() 혹은 doPost() 메소드
  ```

  - 생성자와 init() 메소드는 딱 한번만 실행된다. 즉 servlet 객체는 하나만 만들어진다.
  - `doGet()`, `doPost()`는 서비스 메소드라고 하며, 클라이언트가 실행될 때마다 실행이 되는 메소드이다.

  - 많은 client가 접속한다면 스레드가 생성되어 인스턴스 풀(스레드 풀)에 의해 조정된다. 동기화 처리를 위해서는 스레드관리를 직접 해야하지만 그런 경우가 아니라면 스레드 처리는 WAS가 알아서 해준다.

  - servlet 객체가 메모리에서 제거되는 순간(서버가 종료되거나 재시작되는 상황)에 `destroy()`메소드가 실행이 된다.

- 서블릿이 어떻게 연결되어있는지 궁금할 때 Deployment Descriptor > Servlet Mapping에 보면 알 수 있다.
- 서블릿은 항상 직접실행하는 것이 아니라 HTML에서 클릭을 통해 실행시키는 것이다. 서블릿 java파일을 직접 실행시킬 일은 없다고 보면 된다.
  - href = "/`<root context>`/path" 를 통해 서블릿을 실행시킬 수 있다.
  - /`<root context>` 가 프로젝트의 `WebContent`까지의 경로를 가리킨다고 보면 된다.

- 엘리먼트의 `id`는 보통 클라이언트쪽에서 CSS나 JS를 적용하기 위해 사용되었다.
- 엘리먼트의 `name`, `value`가 GET방식에서 Query String의 parameter로 전달되는 내용들이다.



### sevlet을 사용하는 과정

1. Data get

   ```java
   request.setCharacterEncoding("utf-8");
   // Type <parameter> = request.getParameter("<paraname>")
   String userId = request.getParameter("userid");
   String userName = request.getParameter("username");
   ```

   - request의 `getParameter("<paraname>")`메소드를 통해 따로 I/O를 만들 필요 없이 data를 전달받을 수 있다. 여기서 `<paraname>`에 들어가는 이름이 엘리먼트의 `name = "paraname"` 부분이다.
- `response.setContentType()` 메소드는 `response.getWriter()`로 출력하는 부분의 인코딩을 담당하는 부분으로 getWriter메소드 전에 setContentType메소드를 통해 인코딩 방식을 설정해야 했다.
   - request 또한 받아올 때 인코딩을 설정해주어야 한다. GET방식은 주소창을 통해 query string이 넘어오기 때문에 문제가 되지 않을수도 있지만 POST방식은 인코딩이 필수이다. 즉 `request.getParameter()`메소드로 데이터를 얻기 전에 `request.setCharacterEncoding("utf-8")`메소드를 통해 인코딩 설정을 해주어야 한다.

   ```java
String[] getParameterValues(String name)
   ```

   `<input type="checkbox">`같은 경우,  같은 name에 해당하는 value가 여러 개 들어올 수 있다. 이러한 경우 getParameter로 모든 value를 다 받을 수 없고, 대신에 getParameterValues메소드를 활용할 수 있다.

2. Logic 실행

   2-1 Business Logic

   2-2 Database Logic

3. Response page 생성(HTML 생성)

