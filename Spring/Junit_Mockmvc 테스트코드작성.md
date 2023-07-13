# 테스트 코드 작성

## 의존성 추가

### build.gradle

```
dependencies {
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    testImplementation 'org.springframework.security:spring-security-test'

    runtimeOnly 'mysql:mysql-connector-java'
    runtimeOnly 'com.h2database:h2'
}

tasks.named('test') {
    useJUnitPlatform()
}
```

- `org.springframework.boot:spring-boot-starter-test` 에는 `Junit5`이 포함되어 있다.
- `org.springframework.security:spring-security-test` 는 시큐리티 테스트
- DB를 분리해서 사용할 거라 `h2database`도 추가해주었다.

### 테스트 코드 DB 분리

- 메인DB - MySQL
- 테스트DB - H2

 

# SpringBootTest 와 WebMvcTest

> - **WebMvcTest**
>
>   WebMvcTest는 Controller(API) Layer만을 테스트하기 적합한 테스트 어노테이션으로 전체 애플리케이션을 실행하는 것이 아닌 Controller만을 로드하여 테스트를 진행할 수 있는 아주 일목요연한 테스트 방법
>
>   
>
> - **SpringBootTest**
>
>   SpringBootTest는 실제 애플리케이션을 자신의 로컬 위에 올려서 포트 주소가 Listening 되어지고, 실제 Database와 커넥션이 붙어지는 상태에서 진행되는 Live 테스트 방법
>
> 언뜻보면 두 테스트간의 차이는 전체 애플리케이션을 띄운다는 점과 일부 Controller만을 띄운다는 점으로 보여지는데, 목적으로만 본다면 이 점이 가장 핵심이며 애플리케이션의 규모가 커지게 되는 경우 테스트 시간이 그만큼 길어지기 때문에 신규 기능이나 버그 패치 등 일부 기능만을 테스트하고자 할 떄는 WebMvcTest가 적당하다고 볼 수 있습니다.
>
> [공식문서](https://spring.io/guides/gs/testing-web/)

```java
@WebMvcTest
public class WebLayerTest {

	@Autowired
	private MockMvc mockMvc;

	@Test
	public void shouldReturnDefaultMessage() throws Exception {
		this.mockMvc.perform(get("/")).andDo(print()).andExpect(status().isOk())
				.andExpect(content().string(containsString("Hello, World")));
	}
}
```



## 1. WebMvcTest

### `IndexController` 테스트 코드

```java
package com.crossfit.server.controller;

import com.crossfit.server.config.SecurityConfig;
import com.crossfit.server.jwt.JwtAccessDeniedHandler;
import com.crossfit.server.jwt.JwtAuthenticationEntryPoint;
import com.crossfit.server.jwt.JwtTokenProvider;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.context.annotation.Import;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.request.MockMvcRequestBuilders;

import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@WebMvcTest(IndexController.class)
@Import(SecurityConfig.class)
public class IndexControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private JwtTokenProvider jwtTokenProvider;

    @MockBean
    private JwtAuthenticationEntryPoint jwtAuthenticationEntryPoint;

    @MockBean
    private JwtAccessDeniedHandler jwtAccessDeniedHandler;

    @Test
    public void index() throws Exception {
        mockMvc.perform(MockMvcRequestBuilders
                        .get("/"))
                .andDo(print())
                .andExpect(status().isOk())
                .andExpect(content().string("Welcome Crossfit Page!")); 
    }
}
```

- SecurityConfig 오류가 날 수 있기 때문에 `SecurityConfig`를 `@Import`하여 테스트 컨텍스트에 포함시키고, 
- 필요한 시큐리티 빈들을 `@MockBean`으로 주입하여 목 객체로 대체하면 테스트를 진행



## 2. SpringBootTest

### AbstractControllerTest

``` java
package com.crossfit.server.controller;

import org.junit.jupiter.api.BeforeEach;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.jdbc.EmbeddedDatabaseConnection;
import org.springframework.boot.test.autoconfigure.jdbc.AutoConfigureTestDatabase;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;
import org.springframework.web.filter.CharacterEncodingFilter;

import java.nio.charset.StandardCharsets;

import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;

@SpringBootTest
@AutoConfigureMockMvc
@AutoConfigureTestDatabase(connection = EmbeddedDatabaseConnection.H2)
public abstract class AbstractControllerTest {

    @Autowired
    protected MockMvc mockMvc;

    abstract protected Object controller();

    @BeforeEach
    private void setup() {
        mockMvc = MockMvcBuilders.standaloneSetup(controller())
                .addFilter(new CharacterEncodingFilter(StandardCharsets.UTF_8.name(), true))
                .alwaysDo(print())
                .build();
    }
}
```

- 추상 클래스를 생성해서 사용하는 컨트롤러에서 받아서 사용하는 방식으로 구현

- `@AutoConfigureTestDatabase(connection = EmbeddedDatabaseConnection.H2)` 

  - H2 데이터베이스를 사용하여 테스트용 데이터베이스를 자동으로 구성

- `@BeforeEach`

  - Junit5
  - 각각의 테스트 메소드가 실행되기 이전에 실행되어야 하는 설정 또는 초기화 로직을 포함하는 메소드를 지정하는 데 사용

- `@AutoConfigureMockMvc` 

  - 테스트를 위해 실제 객체와 비슷한 객체를 만드는 것을 모킹(Mocking)이라고 하고, 

    테스트 하려는 객체가 복잡한 의존성을 가지고 있을 때, 모킹한 객체를 이용하면 의존성을 단절시킬 수 있어 쉽게 테스트 가능하다.

|            | @WebMvcTest                                                  | **@AutoConfigureMockMvc**                                    |
| :--------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| **차이점** | - 웹에서 테스트하기 힘든 컨트롤러를 테스트 하는데 적합  <br />- 웹 상에서 요청과 응답에 대해 테스트할 수 있을 뿐만 아니라 시큐리티 혹은 필터까지 자동으로 테스트하여 수동으로 추가/삭제 가능  <br />- 일반적으로 @MockBean 또는 @Import와 함께 사용되어 @Controller 빈에 필요한 협력자를 생성한다. <br />- 여러 스프링 테스트 어노테이션 중 Web에 집중할 수 있는 어노테이션  <br />- 선언할 경우 @Controller,@ControllerAdvice등을 사용 가능  <br />- 단, @Service, @Component, @Repository 등은 사용 불가 | - @WebMvcTest와 비슷하게 사용할 수 있는 어노테이션이다.  <br />- @WebMvcTest와 가장 큰 차이점은 컨트롤러 뿐만 아니라 테스트 대상이 아닌 @Service나 @Repository사 붙은 객체들도 모두 메모리에 올린다.  <br />- 간단하게 테스트 하기 위해서는 @AutoConfigureMockMvc가 아닌 @WebMvcTest를 사용해야 한다.  <br />- MockMVC를 보다 세밀하게 제어하기 위해 사용하며,<br />전체 애플리케이션 구성을 로드하고 MockMVC를 사용하려는 경우 **@WebMvcTest**보다 **@AutoConfigureMockMvc**와 결합된 @SpringBootTest를 고려해야 한다. |

- 공통점 :  웹 애플리케이션에서 컨트롤러를 테스트 할 때, 서블릿 컨테이너를 모킹하기 위해서 사용한다.
- @WebMvcTest는 @SpringBootTest와 같이 사용될 수 없다. 
  - 각자 서로의 MockMvc를 모킹하기 때문에 충돌이 발생하기 때문



### AuthControllerTest

```java
package com.crossfit.server.controller;

import com.crossfit.server.dto.member.LoginRequestDto;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.MediaType;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

public class AuthControllerTest extends AbstractControllerTest {

    @Autowired
    private AuthController authController;
    
    @Override
    protected Object controller() {
        return authController;
    }

    private ObjectMapper objectMapper = new ObjectMapper();
    
    @Test
    public void register() throws Exception {
        LoginRequestDto dto = new LoginRequestDto();
        dto.setEmail("test");
        dto.setPassword("test");

        mockMvc.perform(
                post("/api/v1/auth")
                        .contentType(MediaType.APPLICATION_JSON)
                        .content(objectMapper.writeValueAsString(dto))
                )
                .andExpect(status().isOk())
                .andDo(print());
    }
    //...
}
```

-  AbstractControllerTest 를 상속 받고 사용

