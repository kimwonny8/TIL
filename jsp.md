- JSP, 서블릿, MVC 학습 및 실습 보고서

  # 3장

  ​    

  ## HTML(HyperText Markup Language)

  \- 모든 웹 콘텐츠는 HTML로 이루어져 있다.

  \- 웹 브라우저는 서버로부터 전달받은 HTML 문서의 구조를 해석해 화면을 구성함 

  \- 클라이언트인 웹 브라우저가 서버로부터 수신하는 데이터의 구조는 HTML임

  ​    

  ### HyperText

  \- 다른 문서로 이동할 수 있는(링크)

  \- 다른 정보와 연결된 텍스트를 의미하며 사용자가 관련 문서를 링크를 통해 이동하며 정보를 얻을 수 있음 

  ​    

  ### Markup Language

  \- 어떤 문서의 구조를 나타내는 데 사용

  \- 어디서 어디까지가 리스트인지, 제목인지 등을 알 수 있게 하는 것

  ​    

  ### element

  \- 시작부터 끝을 지정

  ​    

  ### Attribute

  \- element의 추가 정보

  \- Attribute 앞에는 반드시 띄어쓰기 한 칸 필요, 따옴표로 묶여야

  ​    

  ### id

  \- element의 식별자

  \- html 파일 안에서 유일

  ​    

  ###  class

  \- 분류

  \- 여러 개 가능(공백으로 구분)

  ​    

  ### name

  \- form에서 사용

  \- 서버에서 받을 때 변수 이름으로 사용

  ​    

  ​    

  ## Html 선언 방법

  ### HTML5

  ``` html
  <!DOCTYPE html>
  ```
  
  

  ​    

  ### HTML4

  ```
  <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
  ```
  
  

  ## CSS

  선택자와 선언부로 구성되며 h2{ color: red; }면 h2가 선택자고, 중괄호가 선언부

  ​    

  ## 자바스크립트(Javascript)

  정적인 html을 동적으로 만들어 줌

  객체(Object) 기반의 스크립트 언어로 기본적으로는 웹 브라우저에서 해석되는 인터프리터 언어

  \- 객체(object) 기반의 스크립트 언어

  \- `ECMA - 262`의 규칙을 준수하는 언어

  \- `ECMA 2015(ECMA6)` : V8 Engine의 기본 버전, 현재 버전

  \- 브라우저 별 지원 문법이 다름

  ​    

  ### ECMAScript?

  \- European Computer Manufacturer's Association

  \- 정보와 통신 시스템을 위한 국제적 표준화 기구

  \- JavaScript와 같은 스크립트 언어의 표준

  ​    

  ## 자바스크립트 기본 문법

  \- 파일 확장자 js

  \- 파일의 첫 줄부터 실행

  \- 문장 끝에 세미콜론을 생략하면 자동으로 삽입(ASI, Automatic Semicolon Insertion)

    \- 특정한 경우에 추가되므로 적어주는 게 좋음

  ​    

  ### 변수

  선언과 할당은 분리해서 기억할 것

  1. 원시타입(불변 타입)

  \-   let a="hello"; 일 때 불변 타입은 "hello"

  ​    \-   a="world"; 가 되면 hello가 사라지는 게 아니라 a가 다른 걸 만들어서 가르키는 것

  ​    \-   " "로 만드는 스트링은 무조건 메모리에 잡힘

  \-   boolean, null, undefined(선언은 되었으나 값이 없을 때), number, bigint, string, Symbol

  \-   자바스크립트의 숫자는 기본적으로 소수임

  ​    

  2. 변수 선언과 할당

  `var`

  \- 중복 선언 가능

  \- 자기를 감싸고있는 함수가 Scope라서 주의해야 함! => 0 1 2 3 출력

  \- function test() { for(var i=0; i<3; i++){ console.log(i); } console.log(i); }

  \- Hoisting 의 문제

    \- 인터프리터가 변수와 함수의 메모리 공간을 선언 전에 미리 할당하는 것

    \- var 로 선언한 변수의 경우 호이스팅 시 undefined로 변수를 초기화

  

  `let, const`

  \- let : 값을 여러 번 할당 가능 / const : 상수(선언 할당 동시에)

  \- 같은 범위 내 중복 선언 불가능

  \- 중괄호를 범위로 가짐

  \- Hoisting X

  

  ​    

  # 4장

   

  ## 서블릿

  - 자바 기반의 웹 프로그램 개발을 위해 만들어진 기술

  - 자바로 작성된 프로그램을 실행할 수 있는 서버 소프트웨어(톰캣)를 통해 관리됨 

  - 서블릿을 실행하기 위해서는 톰캣과 같은 서블릿 컨테이너가 필요함

  -  서버에서 돌아가는 자바클래스

  ​    

  ## jsp

  - html에서 자바 코드를 사용할 수 있는 구조

  - 태그형식으로 돌아가기 편하게 만드는 것

  ​    

  - 서블릿과 jsp 둘다 서블릿 형태로 돌아감 

  ​    

  ## JSTL/EL

  - jsp의 구조가 복잡하고 가독성이 떨어지기 때문에 나온 게 JSTL/EL

    ![그림입니다. 원본 그림의 이름: CLP000037fc0002.bmp 원본 그림의 크기: 가로 656pixel, 세로 210pixel](./assets/EMB00001b242b3c.bmp)  

  ​    

  ## REST?

  - Representational State Transfer

  - 네트워크상에서 클라이언트와 서버 사이의 통신을 구현하는 방법 중 하나

  - url에 요청되는 게 바로처리되는 구조가 Rest한 아키텍처

  - 서버 응답을 다양한 형태로 전달하는 개념

  REST의 주요 이점 중 하나는 단순성과 확장성

  ​    

  ## REST API?

  Representational State Transfer Application Programming Interface

  REST의 원칙을 따르는 웹 API을 구축하기 위한 아키텍처 스타일

  기존 get방식으로 보내던 방식과 다르게 url에 내용을 보냄

  ex) ~.com/current/city/seoul

  ​    

  ### REST API의 기본 특징

  리소스 개념을 기반

  표준화된 HTTP 메서드(예: GET, POST, PUT 및 DELETE)를 사용하여 리소스와 상호 작용함

  각 요청에는 서버에 저장된 정보에 의존하지 않고 요청을 완료하는 데 필요한 모든 정보가 포함

  표현(예: JSON 또는 XML)을 사용하여 클라이언트와 서버 간에 데이터를 통신

  ​    

  ​    

  ### 자바 웹 개발에서 REST API를 구현하는 방법

  1. JAX-RS
  2. 스프링에서 @RestController 사용
  
  ​    

  여기서 @는 어노테이션이고, 컴파일할 때 내용과 맞게 자동으로 매칭된다. (메타 데이터)

  ​    

  ​    

  ## 스프링 프레임워크

  - 자바 기반의 오픈 소스 프레임워크

  - Java EE(Enterprise Edition, 기업 버전의 자바 솔루션을 만드는 것) 을 Java EE 사용하지 않고 구현하려고 시작

  ​    

  ### 스프링 특징

  1. 경량 컨테이너 

  - 가볍진 않지만, 자바 EE에 비해 경량하다는 의미

  ​    

  **2.** **제어의 역행****(Inversion of Control, IoC)****지원**

  - 메서드나 객체의 호출 제어권이 사용자가 아닌 프레임워크에 있어 필요에 따라 스프링에서 사용자의 코드를 호출한다.

  - 스프링 자체가 IoC 컨테이너를 갖고 있고, 알아서 인스턴스를 만들고 만들어진 인스턴스에 다양한 클래스 조합들을 끝에서 부터 하나씩 만들어서 내 목적에 맞게 만들어주는 걸 spring이 한다는 것

  ​    

  **3.** **의존성 주입****(Dependency Injection, DI)****지원**

  - 각 계층이나 서비스 간에 의존성이 존재할 경우 프레임워크가 서로 연결해줌

  - A라는 객체를 운영하다가 B가 필요한데, 둘다 엮으면 너무 밀접관계가 되기때문에 해당 부분에서만 가져와서 연결시켜주는 작업을 스프링이 함

  - 필요에 따라 스프링이 가져와서 연결시켜준다.

  ​    

  **4.** **관점 지향 프로그래밍****(Aspect-Oriented Programming, AOP)****지원**
   - 트랜잭션이나 로깅, 보안과 같이 여러 모듈에서 공통적으로 사용하는 기능의 경우 해당 기능을 분리하여 관리할 수 있음
  
  - 이미 만들어진 프로그램 코드(ex. 클래스)가 돌아갈 때, 언제 실행되는 지 남기는 게 로깅

  - 코드 앞 뒤에 뭐가 붙어도 원래 있던 애랑 똑같은 걸로 느낌

  - 프록시 패턴과 연관

  ​    

  스프링은 서로 간의 연관성을 느슨하게 만들겠다는 목적을 갖고 있음 

  (결합도를 낮춰서 유연하게, 즉 변화를 쉽게 적용할 수 있도록 만드는 것)

  ​    

  ## 스프링 부트(SpringBoot)

  - 스프링 프레임워크 프로젝트를 손쉽게 시작할 수 있도록 하며 개발과 관련한 스프링 구성요소를 편하게 관리할 수 있도록 한다.

  ​    

  1. 서블릿 스택

     - 동기 방식의 블로킹 구조

  2. 리액티브 스택 

     - 비동기 논블로킹 구조

     - 대규모, 동접자가 많을 때

  

  블로킹과 논블로킹

  - 블로킹 : 작업이 완료될 때까지 프로그램을 일시 중지(계속 waiting)

  - 논블로킹 : 프로그램이 작업이 완료되기를 기다리는 동안 다른 작업을 계속 실행

  ​    

  ​    

  ​    

  # 5장

  ## 서블릿 동작 과정

    ![그림입니다. 원본 그림의 이름: CLP000037fc0003.bmp 원본 그림의 크기: 가로 736pixel, 세로 284pixel](./assets/EMB00001b242b3e.bmp)  

  ​    

  ​    

  1. MyServlet(자바로 만들어진 클래스) load
  2. init() 메소드 먼저 실행 - HttpServlet 상속 받은 애는 init 메소드 무조건 구현
  3. service() 실행 
  4. doGet() or doPost()
  5. destroy()
  6. unload
  
  ​    

  ​    

  ### 서블릿 클래스 구조

  - javax.servlet.Servlet 인터페이스를 구현한 추상 클래스인 GenericServlet 클래스와 HttpServlet 클래스 중 하나를 상속해 구현한다.

  - HttpServlet을 상속받아 doGet( ), doPost( ) 메서드를 오버라이딩한 구조

     

  ### 서블릿 정보 등록

  - 서블릿 2.0

    - web.xml 이나 애너테이션으로 서블릿임을 선언해야 한다.

  - 서블릿 3.0

    - 애너테이션을 이용해 등록한다.(권장)

  ​    

  ### 쿠키(Cookie)

  클라이언트에 저장되는 작은 정보

  ​    

  ### 세션(Session)

  클라이언트가 웹 애플리케이션 서버에 접속할 때 서버 쪽에 생성되는 공간으로 내부적으로는 세션 아이디를 통해 참조됨

  브라우저 : 서버에 접속할 때 발급받은 세션 아이디를 기억함

  서버 : 해당 세션 아이디로 할당된 영역에 접근함

  ​    

  ### Scope Object

    ![그림입니다. 원본 그림의 이름: CLP000037fc0004.bmp 원본 그림의 크기: 가로 708pixel, 세로 250pixel](./assets/EMB00001b242b40.bmp)  

  ​    

  컨테이너에서 서블릿 관리를 위해 자동으로 생성한 객체 중 속성 관리 기능을 제공하며 특정 범위 동안 유지되는 객체를 의미

  각각의 객체는 관리 목적에 따라 별도의 메서드로 구현된 기능을 가지고 있고 공통적으로 ‘키-값’ 형태의 맵(Map) 자료구조를 가짐

  - 주의할 점) 저장하고자 하는 데이터가 Object 형태임

  - Object 타입은 데이터를 가지고 올 때 리턴된 Object 타입을 원래 저장된 데이터 타입으로 변환해야 함

  ​    

  

  # 6장

  ## JSP 동작 과정

    ![그림입니다. 원본 그림의 이름: CLP0000288816be.bmp 원본 그림의 크기: 가로 640pixel, 세로 473pixel](./assets/EMB00001b242b4b.bmp)  

  jsp 파일이 들어오면 class 로 컴파일 하고 서블릿을 실행한다.

  ​    

  ### JSP의 장점

  HTML 파일에 자바 기술을 사용할 수 있어 비교적 쉽게 프로그래밍 가능

  확장 태그 구조를 사용할 수 있음

  서블릿으로 변환되어 실행되므로 서블릿 기술의 장점을 다 가짐

  ​    

  ### JSP의 단점

  JSP → 자바 → 클래스 → 서블릿 실행과정을 거치므로 ui 변경에 시간이 소요됨

  ​    

  ​    

  ### 지시어

  - JSP 파일의 속성을 기술하는 요소

  - JSP 컨테이너에 해당 페이지를 어떻게 처리해야 하는지를 전달하는 내용을 담음

  지시어의 기본 형식은 <% 지시어 속성=“값” %>

  ​    

  1. page

  - 현재 JSP 페이지를 컨테이너에서 처리(서블릿으로 변환)하는 데 필요한 각종 속성을 기술 

  ​    

  2. include

  - 다른 파일을 포함

  <% include file = “파일 위치” %>

  header, footer 넣을 때 주로 사용

  ​    

  3. taglib

  - JSP의 태그 확장 메커니즘인 커스텀 태그를 사용하기 위한 지시어 

  - uri: 태그 라이브러리 위치로 태그를 정의하고 있는 .tld 파일 경로를 나타냄

  - tagdir: 태그 파일로 태그를 구현한 경우 태그 파일 경로를 나타냄

  - prefix: 해당 태그를 구분해서 사용하기 위한 접두어

  ​    

  ### 템플릿 데이터

  JSP의 화면 구성요소

  ​    

  <%! %> : 선언(Declaration) 태그 

  <%= %> : 표현(Expression) 태그

  <% %> : 스크립트릿(Scriptlet) 태그

  ​    

  

  ​    

  # 7장

  ​    

  vue에 jsp에는 자바 코드를 집어넣지 않는 것이 적극적으로 권장하는 방법임.

  그래서 태그를 사용하는 것이 바람직하다.

  ​    

  - 자바 코드를 넣지 않고 태그의 형태로 쓸 수 있는 방법

  ​    

  ## 액션 태그

  - 표준 액션이라고도 불리며 커스텀 태그 기반이지만 별도의 taglib 지시어 사용 없이 jsp 접두어를 사용함

  - forward, include, useBean, setProperty, getProperty, param

  ​    

  ## 자바 빈

  - 자바의 재활용 가능한 컴포넌트 모델

  - 웹 개발에만 국한된 개념이 아니고, POJO 구조

  - 멤버 변수, 기본 생성자, getter, setter 라고 묶여있는 형태가 빈!!! → VO, DTO

  ex) public void setId(int id) { this.id = id; ｝ /  public int getId() { return id; }

  ​    

  - POJO(Plain Old Java Object) : 특정 기술이나 프레임워크에 종속하지 않고 기본 생성자와 멤버 변수에 대한 getter/setter 메서드를 제공하고 직렬화할 수 있는 자바 클래스를 의미함

  ​    

  ## useBean 액션

  - JSP에서 자바 빈 객체를 생성하거나 참조하기 위한 액션 

    → vue 역할만 시킬거면 필요 없음

  - HTML 폼에서 입력한 값을 자바 객체로 연동할 때 주로 활용

  ​    

  - id: 자바 빈을 특정 scope에 저장하거나 가져올 때 사용하는 이름

  - scope: 해당 클래스 타입의 객체를 저장하거나 가지고 오는 범위로 내장객체의 일부

  - class: 패키지 명, 클래스명

  - type: 특정 타입의 클래스를 명시

  - beanName: type과 beanName 사용을 통해 class 속성을 대체할 수 있음

  ​    

  ex) <jsp:useBean id=“m” class=“com.my.Member”>

   <jsp:setProperty name=“m” property=“*”>

  ​    

  → BeanUtils 라이브러리 사용하면 setter로 하나씩 데이터 안 넣어줘도 됨

   ex) BeanUtils.populate(m, request.getParameterMap());

  ​    

  ### include 액션

  - include 지시어와 include 액션은 다름

  - include 지시어 : 하나의 파일로 컴파일한 다음 처리

  - include 액션 : 따로 컴파일하고 합쳐서 결과 보여줌

  ​    

  ### forward 액션

  - 리디렉션(response.sendRedirect()) : a → b → c 일 때, a → b / b → c 로 클라이언트가 새로 요청해야 함. 즉 응답이 되면 응답을 받은 클라이언트가 새로운 페이지로 접속 (새로운 리퀘스트)

      → 기존 데이터가 다 날아가기 때문에 단순 페이지 이동일 때 사용!

  ​    

  - forward : a → b 로 넘겨주면 클라이언트로 가지 않고 a에서 b로 바로 제어권 넘어감

  ​    

  ### 커스텀 태그

  - 외형적인 형태는 XML(HTML) 태그 구조이지만 서블릿 형태로 변환될 때 자바 코드로 변경되어 통합되는 방식

  - 즉, 사용자가 정의해서 쓰는 태그

  - jsp에서 쓰는 템플릿 JSTL : 커스텀 태그 기술로 만들어짐

  ​    

  ex) <% taglib tagdir=“/WEB-INF/tags” prefix=“m” %>

  → WEB-INF/tags/ 파일에서 가지고 옴, 접두어는 ‘m’

  ​    

  ### 표현 언어 (Expressiond Language, EL)

  - 간단한 구문으로 손쉽게 변수/객체를 참조할 수 있음

  - ${m.name}처럼 사용할 수 있음, 아닐 땐 <%= m.name %> 으로 사용해야 함

  - 기본적인 연산 가능 `${ 10+20 }`, `${ true && false }` 등

  - 리스트, 맵 사용 가능

  - Scope Object 접근 가능

  ​    

  ## JSTL(JSP Standard Tag Library)

  - jsp로 만들어진 표준화된 라이브러리

  - HTML 형식을 유지하면서 조건문, 반복문, 간단한 연산과 같은 기능을 손쉽게 사용할 수 있도록 지원하기 위해 만들어진 표준 커스텀 태그 라이브러리

  - 개발하면서 ui 확인 해야 할 때 서버를 통해야 하는 문제, 디자이너와의 협업 불편 등의 문제가 있음

  - Impl 과 Spec를 다운해서 사용

  

  ### 사용법

    ![그림입니다. 원본 그림의 이름: CLP00001b242a6e.bmp 원본 그림의 크기: 가로 722pixel, 세로 54pixel](./assets/EMB00001b242b4e.bmp)  

  

  ###  core 라이브러리

  - 변수 처리, 흐름 제어, URL 관리, 출력 등 가장 기본적인 기능을 구현해둔 라이브러리

  

  <c:if> : else는 지원하지 않음

    ![그림입니다. 원본 그림의 이름: CLP00001b240001.bmp 원본 그림의 크기: 가로 656pixel, 세로 97pixel](./assets/EMB00001b242b50.bmp)  

  <c:forEach> : index, count 제공

    ![그림입니다. 원본 그림의 이름: CLP00001b240002.bmp 원본 그림의 크기: 가로 656pixel, 세로 185pixel](./assets/EMB00001b242b52.bmp)  

  <c:set> : 변수 설정

  <c:out> : 출력

  <c:choose>, <c:when>, <c:otherwise> : switch문과 유사

  <c:forTokens> : 자바의 StringTokenizer와 유사

  ​    

  ## 자바 빌드 도구

  - Ant: 가장 오래된 자바 빌드 도구

  - Maven: 2004년에 아파치 프로젝트로 Maven이 새롭게 나옴.

  - Gradle: 2012년 Gradle이 나옴. 안드로이드 앱 기발의 기본 빌드 도구

  - 현재 Maven과 Gradle이 가장 대표적인 빌드 도구

  - 스프링은 Maven, Gradle 중 선택가능함

  - 빌드 도구를 사용하는 이유? 자동 설치가 되기 때문에 신경 쓸 필요가 없음

  ​    

  ​    

  ### Maven 과 Gradle

  - Maven : ‘pom.xml’ 파일에 빌드 설정, xml 구조라서 가독성 떨어짐

  - Gradle : Groovy를 써서 간결하게 작성 가능

  ​    

  리포지터리

  라이브러리 저장소

  ​    