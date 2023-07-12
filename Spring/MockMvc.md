# [MockMvc](https://velog.io/@jkijki12/Spring-MockMvc)



> MockMvc는 웹 어플리케이션을 애플리케이션 서버에 배포하지 않고 테스트용 MVC환경을 만들어 요청 및 전송, 응답기능을 제공해주는 유틸리티 클래스다.

## 예제

``` java
class ControllerTest{
	
    @Autowired
    private MockMvc mockMvc; // mockMvc 생성
    
    @Test
    public void testController() throws Exception{
    	
        String jjson = "{\"name\": \"부대찌개\"}";
        
        //mockMvc에게 컨트롤러에 대한 정보를 입력하자.
        mockMvc.perform(
        get("test?query=food") //해당 url로 요청을 한다.
        .contentType(MediaType.APPLICATION_JSON) // Json 타입으로 지정
        .content(jjson) // jjson으로 내용 등록
        .andExpect(status().isOk()) // 응답 status를 ok로 테스트
        .andDo(print()); // 응답값 print
        )  
   }
}
```

1. MockMvc를 생성한다.
2. MockMvc에게 요청에 대한 정보를 입력한다.
3. 요청에 대한 응답값을 Expect를 이용하여 테스트한다.
4. Expect가 모두 통과하면 테스트 통과
5. Expect가 1개라도 실패하면 테스트 실패



## MockMvc 요청 설정 메소드

- param / params : 쿼리 스트링 설정
- cookie : 쿠키 설정
- requestAttr : 요청 스코프 객체 설정
- sessionAttr : 세션 스코프 객체 설정
- content : 요청 본문 설정
- header / headers : 요청 헤더 설정
- contentType : 본문 타입 설정



## 검증 메소드

- status : 상태 코드 검증
- header : 응답 header 검증
- content : 응답 본문 검증
- cookie : 쿠키 상태 검증
- view : 컨트롤러가 반환한 뷰 이름 검증
- redirectedUrl(Pattern) : 리다이렉트 대상의 경로 검증
- model : 스프링 MVC 모델 상태 검증
- request : 세션 스코프, 비동기 처리, 요청 스코프 상태 검증
- forwardedUrl : 이동대상의 경로 검증



## 응답 생태코드

- isOk() : 상태 코드가 200인지 확인
- isNotFount() : 404인지 확인
- isMethodNotAllowed() : 405인지 확인
- isInternalServerError() : 500인지 확인
- is(int status) : 임의로 지정한 상태 코드인지 확인

