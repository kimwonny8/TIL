[toc]

#  📌Servlet

- 웹 서버의 성능을 향상하기 위해 사용되는 자바 클래스의 일종
- 클라이언트가 어떠한 요청을 하면 그에 대한 결과를 다시 전송해주어야 하는데, 이러한 역할을 하는 자바 프로그램
- WAS내의 서블릿 컨테이너에서 동작
- 요청(Request)을 받으면 요청에 맞는 로직을 실행하고 클라이언트에게 HTTP 형식으로 응답(Response)

## Servlet 동작 방식

> ![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F993A7F335A04179D20)
>
> 1. 사용자(클라이언트)가 URL을 입력하면 HTTP Request가 Servlet Container로 전송합니다.
> 2. 요청을 전송받은 Servlet Container는 HttpServletRequest, HttpServletResponse 객체를 생성합니다.
> 3. web.xml을 기반으로 사용자가 요청한 URL이 어느 서블릿에 대한 요청인지 찾습니다.
> 4. 해당 서블릿에서 service메소드를 호출한 후 클리아언트의 GET, POST여부에 따라 doGet() 또는 doPost()를 호출합니다.
> 5. doGet() or doPost() 메소드는 동적 페이지를 생성한 후 HttpServletResponse객체에 응답을 보냅니다.
> 6. 응답이 끝나면 HttpServletRequest, HttpServletResponse 두 객체를 소멸시킵니다.

[출처](https://mangkyu.tistory.com/14)

## Servlet Container(서블릿 컨테이너)

- 서블릿을 관리해주는 컨테이너
- 서블릿 컨테이너는 클라이언트의 요청(Request)을 받아주고 응답(Response)할 수 있게, 웹서버와 소켓으로 통신 
- 대표적인 예로 톰캣(Tomcat)
  - 실제로 웹 서버와 통신하여 JSP(자바 서버 페이지)와 Servlet이 작동하는 환경을 제공

# 📌 AWS(Amazon Web Service)

- 아마존(Amazon)에서 제공하는 클라우드 서비스
- 네트워킹을 기반으로 가상 컴퓨터와 스토리지, 네트워크 인프라 등 다양한 서비스를 제공

서블릿 기능 지원

클라우드가 서버 작동 관리, 메모리에 유연



## IAM

유저를 관리하고 접근 레벨 및 권한 관리

새로 계정 만들면 루트계정 

- 루트안에서 새로운 유저 만들 수 있음 -> 접근키, 비밀키 제공, 패스워드X
- 매우 세밀한 접근 권한 부여 가능
- 비밀번호를 수시로 변경 가능
- 다중 인증 기능 -> root계정은 이 기능 적극 권장!
- Universal, 지역 설정 필요없음!



1. 그룹
2. 유저
3. 역할
4. 정책
   - 주로 json 형식으로 된 document
   - 그룹, 역할에 추가 가능



IAM정책 시뮬레이터

- 개발환경에서 실제환경으로 빌드하기전 IAM 정책이 잘 작동되는지 테스트
- IAM과 관련된 문제들을 디버깅, 실제 시뮬레이션해봄 