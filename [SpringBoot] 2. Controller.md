[toc]

# 📌 프로젝트 구조

## Repository

- Entity를 통해 데이터 테이블이 생성이 되면, 받아온 정보를 데이터베이스에 저장하고 조회하는 기능
- 실제 데이터베이스에 쿼리문 실행

## Service

- Repository에서 얻어온 정보를 자바로 가공 후 다시 Controller에게 정보를 보냄

## Controller

- 클라이언트 측의 요청이 가장 먼저 서버 측과 맞닿는 부분
- 데이터를 클라이언트에게 보낼 때도 컨트롤러를 통해 보냄



![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbyW031%2FbtqGvdxw1uy%2FfUvpO1u7geOUk5exDaoNT1%2Fimg.png)

# 📌 Controller

- src - main - java - com.example.backend(프로젝트명) 안에 
  - controller 패키지 - WebController 클래스 생성

``` java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class MainController {

    @GetMapping("/hello")
    @ResponseBody
    public void index() {
    	return "hello";
    }
}
```

## @Controller

- 이 WebController 클래스는 스프링부트의 컨트롤러가 됨

## @GetMapping

- 요청된 URL과의 매핑을 담당

## @ResponseBody

- URL요청에 대한 응답으로 문자열 리턴하라는 의미
- 이 이노테이션 생략하면 hello라는 템플릿 파일을 찾게되는 것

- ex) http://localhost:8080/hello 입력시 hello가 나오는 것을 볼 수 있음

  들어가서 확인해보자! 



