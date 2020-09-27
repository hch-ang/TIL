# `JSP(Java Server Page)`

HTML 내에 자바 코드를 삽입. 실행시 자바 Servlet코드로 변환된 후 실행된다(코딩 a.jsp -> 실행 a.class). 이러한 코드의 변환은 WAS(Web Application Server)에서 자동으로 해준다.

### JSP vs Servlet

- 최초 jsp 요청 시, 혹은 jsp file 변경 시 jsp 파일이 Servlet java 파일로 변환이 되고 다시 class 파일로 컴파일되는 작업이 들어가야 하므로 컴파일 시간이 소요된다.
- 객관적으로 보면 이러한 변환작업이 없으므로 Servlet이 더 빠르다. 하지만 최초의 변환작업을 제외하고 난 후에는 Servlet과 속도차이가 없다고 볼 수 있다.
- jsp파일이 Servlet 클래스로 변환 될 때 항상 HttpServlet을 상속받는 것이 아니다. 추상클래스를 상속받거나 인터페이스를 구현하는지는 WAS에 따라 달라진다. 해당 WAS에 최적화된 클래스/인터페이스를 상속 혹은 구현한다.
- JSP는 Servlet의 연장선으로 생각할 수 있고, 코딩하는 영역이 조금 다를 뿐이다.
- workspace > .metadata > .plugins > org.eclipse.wst.server.core > tmp0 > work > Catalina > `<domain명>` > `<context root명>` > org > apache > jsp > `<해당 dircetory>` > ... > aaa_jsp.java파일을 열어보면 jsp가 바뀐 Servlet code를 볼 수 있다.



### JSP 스크립팅 요소

1. 선언

   멤버변수, 메소드를 선언하는 영역

   ```jsp
   <%! // 멤버변수, 메소드 %>
   ```

   여기서는 request, response와 관련된 동작을 할 수 없다.

2. 스크립트릿(처리문)

   Client 요청 시 매번 호출되는 영역(Servlet에서 `service()`느낌)

   ```jsp
   <% // java code %>
   ```

   여기서 `request`, `response`와 관련된 동작을 정의한다.

3. 표현식(Expression)

   데이터를 브라우저에(from 서버 to 클라이언트) 출력할 때 사용

   ```jsp
   <%= 문자열 %>
   <%-- servlet 코드를 응용하여 <% out.print(문자열); %>과 같이 동작한다. --%>
   ```

- html 주석

  ```html
  <!-- 주석할 내용 -->
  ```

- jsp 주석

  ```jsp
  <%-- 주석 할 내용 --%>
  ```

- html 주석 : client에서 실행, jsp : 서버에서 실행
  - html 주석 : server에서 client까지 주석 내용은 전달되나 화면에 보이지는 않는다.
  - jsp 주석 : server에서 client에 보낼 때부터 내용을 전달하지 않는다.
- JSP코드에서 오류가 있는 내용에 html주석을 써버리면 서버에서 에러가 발생(500 error)하게된다. 따라서 html 주석이 아니라 jsp 주석을 사용해야 한다.

- 순수 HTML를 JSP로 만들어도 된다. JSP는 HTML의 동급 이상이다.



### JSP 지시자(Directive)

- page Dircetive

  현재 페이지에 지시한다. 처음 HTML코드랑 다 똑같지만 맨 위에 있는 내용들이다.

  ```jsp
  <%@ page language="java" contentType="text/html; charset=UTF-8"
      pageEncoding="UTF-8"%>
  ```

  `속성 = "값"` 형태로 지정해주어야 한다.

  - language : JSP에서 사용하는 언어를 말한다(default: java)
  - info : 간단한 정보교환용(document주석같은 느낌. html에서는 보이지 않는다.)
  - `contentType` : (default : ISO-8859-1 서유럽꺼,,,)
  - pageEncoding : 현재 JSP 페이지 문자집합 지정(contentType에서의 `charset=UTF-8`과 같은 역할이다. 둘 중 하나만 해도 된다.)
  - `import` : 현재 JSP 페이지에서 사용할 Java 패키지나 클래스를 지정
  - `session` : 세션의 사용 유무 결정(default : true)
  - errorPage : 에러가 발생했을 때 직접 만든 JSP 페이지 지정
  - isErrorPage : 현재 JSP 페이지가 에러 핸들링하는 페이지인지 지정(default: false)
  - buffer : 버퍼의 크기(default: 8KB)
  - autoflush : 버퍼의 내용을 자동으로 브라우저로 보낼 지에 대한 설정(default: true)
  - isThreadsafe : 멀티스레드로 동작해도 괜찮은지에 대한 여부. false이면 싱글스레드로 서비스가 된다.(dafault : true)

- include Directive : 중복되는 디자인에 해당하는 코드를 줄이기 위해 include해서 사용

- taglib Directive : MVC 에서 V(View)는 JSP가 담당한다. JSP는 최대한 HTML에 가까울 수록 좋은 코드이다. 즉 java코드가 없을 수록 좋다. 이 때 java코드를 tag로 만들어서 사용한다.



### JSP 기본 객체

Java와 다르게 따로 만들지 않아도 JSP에서 사용할 수 있는 객체를 말한다.

- `request` : 요청에 대한 처리
- `response` : 응답에 대한 처리
- `out` : jsp에서는 기본적으로 사용할 수 있다.(servlet에서 `response.getWriter()`같은 역할)



### 기본 객체의 영역(scope)

- pageContext : 해당 JSP 페이지에서만 사용할 수 있다
- request : 다음으로 넘어가는 JSP까지 사용할 수 있다
- session : 프로젝트 안의 모든 JSP(session : false으로 설정된 JSP만 제외하고)에서 사용할 수 있다
- application : 프로젝트 안의 모든 JSP에서 사용할 수 있다



### Page 이동방법

- sendRedirect
  - 이동 범위 : 동일 서버를 포함하여 타 URL까지 이동 가능(프로젝트 안의 다른 JSP, 다른 서버, 등)
  - 웹 브라우저에게 다른 페이지로 이동하라는 명령을 내리기 때문에 이동하는 URL로 주소가 바뀐다.
  - 기존의 request, response는 버리고 새로운 request, response를 생성한다. 유지가 되지 않는다.
  - forward에 비해 비교적 느리다.
  - session이나 cookie를 이용하여 데이터를 전달한다(parameter로도 가능).
  - `request.setAttribute("name", value)`를 통해 정보를 담아도 `request.sendRedirect("url")`를 실행하면 담았던 정보를 버리고 새로운 request를 만들어서 이동한다.
  - 요청을 보낸 후에는 기존의 request정보가 유효하지 않기 때문에 시스템(session, DB)에 변화를 줄 수 있는 요쳥, 즉 한번만 시행되어야 하는 요청(수정, 등록, 등)에 사용한다.

- forward

  - 동일 서버(프로젝트)내의 경로만 이동 가능

  - 이동을 하여도 주소는 바뀌지 않는다.

  - 웹 브라우저는 다른 페이지로 이동하였는지 알 수 없고, 최초 호출한 URL만 표시된다.

  - forward에 의해 호출된 페이지와 request, response를 그대로 공유한다.

  - 비교적 빠르다

  - `request.setAttribute("name", value)`를 통해 데이터를 전달한다.
  
    ```jsp
    RequestDispatcher dp = request.getRequestDispatcher("url");
  dp.forward(request, response)
    ```
  
    위의 코드를 통해 담은 정보와 함께 페이지를 이동한다.
    
  - 요청을 보내도 기존의 요청정보가 그대로 유지되기 때문에 시스템에 변화를 주지 않는 요청, 즉 여러번 시행되어도 무관한 요청(조회)에 사용한다.