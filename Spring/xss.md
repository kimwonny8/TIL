## XSS

- Cross Site Scriptin

- 웹 사이트의 어드민(관리자)이 아닌 악의적인 목적을 가진 제 3자가 악성 스크립트를 삽입하여 의도하지 않은 명령을 실행시키거나 세션 등을 탈취할 수 있는 취약점입니다.

  

### **위험성**

**1. 쿠키 및 세션정보 탈취**

- XSS에 취약한 웹 게시판 등에 쿠키나 세션 정보를 탈취하는 스크립트를 삽입하여 해당 게시글을 열람하는 유저들의 쿠키 및 세션 정보를 탈취할 수 있으며 이는 공격자가 탈취한 정보를 바탕으로 인증을 회피하거나 특정 정보를 열람할 수 있는 권한을 가지게 해 줍니다.

**2. 악성 프로그램 다운 유도**

- XSS 자체는 악성 프로그램을 다운로드 시킬 수 없지만 스크립트를 통해 악성 프로그램을 다운받는 사이트로 리다이렉트 시켜서 악성 프로그램을 다운받도록 유도할 수 있습니다.

**3. 의도하지 않은 페이지 노출**

- XSS를 이용해 <img>태그 등을 삽입하여 원본 페이지와는 전혀 관련 없는 페이지를 노출시키거나 페이지 자체에 수정을 가해 노출된 정보를 악의적으로 편집할 수 있습니다.



### **공격 종류**

**1. Reflected XSS**

<img src="https://t1.daumcdn.net/cfile/tistory/2272044B58C4E00504" alt="img" style="zoom:67%;" />

- 공격자가 **악성 스크립트를 클라이언트에게 직접 전달**하여 공격하는 방식입니다.
- 특히 URL에 스크립트를 포함시켜 공격하는 경우가 대표적이며 URL이 길면 클라이언트가 의구심을 가질 수 있기 때문에 Shorten URL(URL 단축)을 이용해 짧은 URL로 만들어 공격하기도 합니다.
- 서버에 스크립트를 저장하지 않기 때문에 서버에서 이루어지는 필터링을 피할 수 있는 공격 방식입니다.

 **2. Stored XSS**

<img src="https://t1.daumcdn.net/cfile/tistory/22662A4B58C4E00705" alt="img" style="zoom:67%;" />

- 공격자가 **악성 스크립트를 서버에 저장시킨 다음 클라이언트의 요청/응답 과정을 통해 공격**하는 방식입니다.
- 여기서 스크립트를 서버에 저장하는 행위는 게시글 쓰기 등의 행동을 지칭합니다.
- 보통 서버에서 필터링을 하기 때문에 공격을 우회하기 어렵지만 한 번 성공하면 관리자 입장에서는 눈치채기 힘들고 광범위한 피해를 줄 수 있다는 것이 특징입니다.

**3. DOM Based XSS**

- 피해자의 **브라우저가 html 페이지를 분석하여 DOM을 생성할 때 악성 스크립트가 DOM의 일부로 구성되어 생성되는 공격**입니다.
- 서버의 응답 내에는 악성 스크립트가 포함되지 않지만 브라우저의 응답 페이지에 정상적인 스크립트가 실행되면서 악성 스크립트가 추가되서 실행되는 특징이 있습니다.



### XSS 방지법

**1. script 문자 필터링**

- XSS 공격은 입력값에 대한 검증이 제대로 이루어지지 않아 발생하는 취약점이다.
- 때문에 사용자의 모든 입력값에 대하여 서버측에서 필터링을 해 주어야 한다.
- 스크립트를 실행하기 위한 특수문자를 필터링 하며, <, >, ", ' 등의 문자가 있다.



**2. htmlentities 사용**

- htmlentities는 모든 특수문자를 HTML 엔티티(entity)로 변환한다.
- HTML 엔티티는 "&약어;" 및 "&#숫자;"의 형태를 표현하는 것을 의미한다. 예를 들어 <는 &lt;로, >는 &gt;로 표현한다.



### java 

```java
public static String xssFilter(String str) {
	String result = "";
	
	result = str;
	result = result.replaceAll("[<]", "&lt;");
	result = result.replaceAll("[>]", "&gt;");
	
	return result;
} 
```

```java
public static String xssDecoding(String str) {
	String result = "";
	
	result = str;
	result = result.replaceAll("&lt;", "[<]");
	result = result.replaceAll("&gt;", "[>]");
	
	return result;
}
```



### spring - xss filter

```java
import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;

public class CrossScriptingFilter implements Filter {
	public FilterConfig filterConfig;

	public void init(FilterConfig filterConfig) throws ServletException {
	       this.filterConfig = filterConfig;
	   }
	
	   public void destroy() {
	       this.filterConfig = null;
	   }
	
	   public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
	       throws IOException, ServletException {
	
	       chain.doFilter(new RequestWrapper((HttpServletRequest) request), response);
     }
}
```

- [FilterConfig 참고](https://docs.oracle.com/javaee%2F6%2Fapi%2F%2F/javax/servlet/FilterConfig.html)



```java
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletRequestWrapper;
 
public final class RequestWrapper extends HttpServletRequestWrapper {
 
    public RequestWrapper(HttpServletRequest servletRequest) {
        super(servletRequest);
    }
 
    public String[] getParameterValues(String parameter) {
 
      String[] values = super.getParameterValues(parameter);
      if (values==null)  {
                  return null;
          }
      int count = values.length;
      String[] encodedValues = new String[count];
      for (int i = 0; i < count; i++) {
                 encodedValues[i] = cleanXSS(values[i]);
       }
      return encodedValues;
    }
 
    public String getParameter(String parameter) {
          String value = super.getParameter(parameter);
          if (value == null) {
                 return null;
                  }
          return cleanXSS(value);
    }
 
    public String getHeader(String name) {
        String value = super.getHeader(name);
        if (value == null)
            return null;
        return cleanXSS(value);
 
    }
 
    private String cleanXSS(String value) {
    	System.out.println("XSS Filter before : " + value);
        value = value.replaceAll("<", "& lt;").replaceAll(">", "& gt;");
        value = value.replaceAll("\\(", "& #40;").replaceAll("\\)", "& #41;");
        value = value.replaceAll("'", "& #39;");
        value = value.replaceAll("eval\\((.*)\\)", "");
        value = value.replaceAll("[\\\"\\\'][\\s]*javascript:(.*)[\\\"\\\']", "\"\"");
        value = value.replaceAll("script", "");
        System.out.println("XSS Filter after : " + value);
        return value;
    }
}
```

```xml
<filter>
	<filter-name>XSS</filter-name>
    <filter-class>com.mycom.myapp.filter.CrossScriptingFilter</filter-class> // 자신의 CrossScriptingFilter 적용 위치
</filter>
<filter-mapping>
    <filter-name>XSS</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>

```







[참고](https://4rgos.tistory.com/1)

[스프링코드참고](https://junhokims.tistory.com/28)