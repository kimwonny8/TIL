```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

```
implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
```



```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1 th:text="${name}">Name</h1>
</body>
</html>
```

## 1) 설정

###   xmlns:th=" "

```html
<html lang="en" xmlns:th="http://www.thymeleaf.org">
```

- 타임리프의 th속성을 사용하기 위해 선언된 네임스테이스이다.
- 순수 HTML로만 이루어진 페이지인 경우 선언하지 않아도 된다.





## 2) 기본 기능

###   th:text="${}"

```html
<div th:text="${data}"></div>
```

- JSP의 EL 표현식인 ${}와 마찬가지로 ${} 표현식을 사용해서 컨트롤러에서 전달받은 데이터에 접근할 수 있다.

###   th:href="@{}"

```html
<body>
  <a th:hrf="@{/boardListPage?currentPageNum={page}}"></a>
</body>
```

- **`<a>`** 태그의 href 속성과 동일하다.
- 괄호안에 클릭시 이동하고자하는 url를 입력하면 된다.

###   th:with="${}"

```html
<div th:with="userId=${number}" th:text="${usesrId}">
```

- 변수형태의 값을 재정의하는 속성이다. 즉, **`th:with`**를 이용하여 새로운 변수값을 생성할 수 있다.

###   th:value="${}"

```html
<input type="text" id="userId" th:value="${userId} + '의 이름은 ${userName}"/>
```

- input의 value에 값을 삽입할 때 사용한다.
- 여러개의 값을 넣을땐 + 기호를 사용한다.





## 3) Layout

###   xmlns:layout="", layout:decorator=""

👉 **`예시`**

```html
<html lang="ko" xmlns:th="http://www.thymeleaf.org" xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout" layout:decorate="~{board/layout/basic}">
```

👉 **`의존성`**

```java
implementation 'nz.net.ultraq.thymeleaf:thymeleaf-layout-dialect'
```

- 타임리프의 layout 기능을 사용하기 위해서는 의존성을 추가해줘야 한다.
- **`xmlns:layout`**은 타임리프의 레이아웃 기능을 사용하기 위한 선언이다. 레이아웃을 적용시킬 HTML 파일에 해당 선언을한다. 그리고 해당 페이지에 **`th:fagment`**로 조각한 공통 영역을 가져와서 삽입해준다.

###   th:block

```html
<html lagn="ko" 
      xmlns:th="http://www.thymeleaf.org"
      xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout">
<body>
  	//전체 레이아웃
	<th:block th:fragment="footerFragment">
 	</th:block>
</body>  
  
</html>
```

- block은 타임리프 표현을 어느 곳에서든 사용할 수 있도록 하는 구문이다.
- 해당 기능은 동적인 처리가 필요할 때 사용된다. 주로 layout기능이나 switch에 사용이 많이 된다.

###   th:fragment=""

```html
<body>

<footer th:fragment="footerFragment">
  <p>안녕하세요</p>
</footer>

</body>
```

- 웹페이지에 메뉴 탭이나 네비게이션바와 같이 공통으로 반복되는 영역이 존재한다. 이 공통의 역영들을 매 페이지마다 HTML코드를 반복해서 쓰면 굉장히 지저분 해지는데 **`fragment가 바로 공통 영역을 정의`**하여 코드를 정리해준다.
- 특히, header와 footer에 삽입하여 조각화 한다. 이렇게 만들어진 조각을 삽입하고자 하는 대상 HTML 파일에서 **`th:replace"[파일경로 :: 조각 이름]"`**을 통해 삽입한다.

### th:replace="~{파일경로 :: 조각이름}"

```html
<body>
  <div th:replace="~{/common/footer :: footerFragment}"></div>  
</body>
```

- JSP의 **`<include>`** 태그와 유사한 속성이다.
- fragment로 조각화한 공통 영역을 HTML에 삽입하는 역할을 한다.
- **`::`**을 기준으로 앞에는 조각이 있는 경로를 뒤에는 조각의 이름을 넣어준다.
- **`insert`88와 다르게 완전하게 대체한다.

### th:insert="~{파일경로 :: 조각이름}"

```html
<body>
  <div th:insert="~{/common/footer :: footerFragment}"></div>  
</body>
```

- **`insert`**는 태그 내로 조각을 삽입하는 방법이다. **`replace`**는 완전하게 **`대체`**하기 때문에 replace 태그 가 입력된 **`<div>`**가 사라지고 **`fragment`**로 조각화한 코드가 완전히 대체된다.
- 하지만 **`insert`**는 **`insert`**가 입력된 **`<div>`** 안에 **`fragment`**를 삽입하는 개념이기 때문에 **`<div>`**안에 조각화한 코드가 삽입된다.
- 형식은 replace와 동일하다.





## 4) Form

👉 **`대표예시`**

```html
<body>
  <form th:action="@{/join}" th:object="${joinForm}" method="post">
    <input type="text" id="userId" th:field="*{userId}" >
    <input type="password" id="userPw" th:field="*{userPw}" >
  </form>
</body>
```

###   th:action="@{}"

- **`<form>`** 태그 사용시, 해당 경로로 요청을 보낼 때 사용한다.

###   th:object="${}"

- **`<form>`** 태그에서 데이터를 보내기 위해 Submit을 할 때 데이터가 th:object 속성을 통해 object에 지정한 객체에 값을 담아 넘긴다. 이때 값을 **`th:field`**속성과 함꼐 사용하여 넘긴다.
- Controller와 View 사이의 DTO클래스 객체라고 생각하면 된다.

###   th:field="*{}"

- **`th:object`**속성과 함께 **`th:field`**를 이용해서 HTML 태그에 멤버 변수를 매핑할 수 있다.
- **`th:field`**을 이용한 사용자 입력 필드는 id, name, value 속성 값이 자동으로 매핑된다.
- th:object와 th:field는 Controller에서 특정 클래스의 객체를 전달 받은 경우에만 사용 가능하다.





## 5) 조건문과 반복문

###   **`th:if="${}", th:unless="${}"`**

```html
<span th:if="${userNum} == 1"></span> 
<span th:unless="${userNum} == 2"></span>
```

- JAVA의 조건문에 해당하는 속성이다. 각각 **`if`**와 **`else`**를 뜻한다.
- **`th:unless`**는 일반적인 언어의 else 문과는 달리 **`th:if`**에 들어가는 조건과 동일한 조건을 지정해야 한다.

###   th:each="변수 : ${list}"

```html
<body>
  <li th:each="pageButton" : ${#numbers.sequece(paging.firstPage, paging.lastPage)}></li>
</body>
```

- JSP의 JSTL에서 **`<c:foreach>`** 그리고 JAVA의 반복문 중 **`for문`**을 뜻한다.
- ${list}로 값을 받아온 것을 변수로 하나씩 가져온다는 뜻으로, 변수는 이름을 마음대로 지정할 수 있다.





###   th:switch, th:case

```html
<th:block th:switch="${userNum}"> 
  <span th:case="1">권한1</span> 
  <span th:case="2">권한2</span> 
</th:block>
```

- JAVA의 **`switch-case`**문과 동일하다.
- switch case문으로 제어할 태그를 **`th:block`**으로 설정하고 그 안에 코드를 작성한다.
- userNum라는 변수의 값이 1이거나 2일때 동작하는 예제이다.





## 4) ETC

###  numbers.sequest(start, end, step)

- 기본적으로 타임리프에서는 **`#numbers`**라는 숫자 포맷 메소드를 지원한다. **`#numbers`**에는 다양한 메소드들이 존재하는데 가장 많이 사용하는 것이 **`#numbers.sequece`**이다.
- **`#numbers.sequece`** 메소드는 start, end, step을 이용하여 원하는 범위에 대해 시퀀스를 생성해준다.



https://velog.io/@alicesykim95/Thymeleaf