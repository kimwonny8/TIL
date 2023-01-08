[toc]

***

# 📌 스프링부트(Spring Boot) 란?

- 자바의 웹 프레임워크

- 기존 스프링(Spring) 프레임워크에 톰캣 서버를 내장하고 여러 편의 기능들을 추가한 프레임워크

  == 스프링부트는 자바로 만들어진 웹 프레임워크다!



**프레임워크?**

- 여러 기능을 가진 [클래스](https://namu.wiki/w/객체 지향 프로그래밍)와 [라이브러리](https://namu.wiki/w/라이브러리)가 '특정 결과물을 구현하고자' 합쳐진 형태

**웹 프레임워크?**

- 웹 서비스 개발을 위한 프레임워크 
- 즉, 웹 프로그램을 만들기 위한 스타터 키트



- 스프링부는 보안에 강함(스프링 시큐리티)

  > 스프링 시큐리티는 스프링 기반의 어플리케이션의 보안(인증과 권한)을 담당하는 프레임워크이다. 만약 스프링시큐리티를 사용하지 않았다면, 자체적으로 세션을 체크하고 redirect 등을 해야할 것이다. 스프링 시큐리티는 보안과 관련해서 체계적으로 많은 옵션들로 이를 지원해준다. spring security는 filter 기반으로 동작하기 때문에 spring MVC 와 분리되어 관리 및 동작한다. 참고로 security 3.2부터는 XML로 설정하지 않고도 자바 bean 설정으로 간단하게 설정할 수 있도록 지원한다.
  >
  > [출처](https://sjh836.tistory.com/165)
  
  

# 📌 프로젝트 생성하기

 **인텔리제이(IntelliJ)** 사용

### File - new - New Project - Spring Initializr 클릭

- Name : 프로젝트 이름 (backend로 진행)

- Language : Java

- Type : Gradle - Groovy

- Spring Boot : 3.0.1 버전

- Dependencies : 추후에 추가 가능

  - 기본적으로 Spring Web를 선택합니다.

  

### 🤔 Gradle 과 Maven 의 차이?

1. Maven ? 
   - 아파치 메이븐은 자바용 프로젝트 관리 도구
   - 아파치 Ant의 대안
   - 아파치 라이센스로 배포되는 오픈 소스 소프트웨어
2. Gradle ?
   - 빌드, 프로젝트 구성/관리, 테스트, 배포 도구
   - 안드로이드 앱의 공식 빌드 시스템
   - 빌드 속도가 Maven에 비해 10~100배 가량 빠름
   - JAVA, C/C++M Python 등을 지원
   - 빌트툴인 Ant Builder와 Groovy 스크립트 기반으로 만들어져 기존 Ant의 역할과 배포 스크립트의 기능을 모두 사용 가능
3. 차이
   - 스크립트 길이와 가독성 면에서 gradle이 우세
   - 빌드와 테스트 실행 결과 gradle이 더 빠름
   - 의존성이 늘어날 수록 성능과 스크립트 품질의 차이가 심해질 것

[참고 블로그](https://dev-coco.tistory.com/65#Gradle%EC%-D%B-%EB%-E%--%-F)

# 📌 스프링부트 도구 설치하기

## Spring Boot Devtool

- 스프링부트 개발시 도움을 주는 도구

- 서버 재시작 없이도 클래스 변경시 서버가 자동으로 재기동

- `build.gradle` 파일 - `dependencies` 에 추가

- ```
  developmentOnly 'org.springframework.boot:spring-boot-devtools'
  ```

- 이 후  [Gradle - Refresh Gradle Project] 클릭해 라이브러리를 다운로드



## Lombok

- 자바 클래스에 Getter, Setter, 생성자 등을 자동으로 만들어 주는 도구

- https://projectlombok.org/download 

- 다운로드, 설치 후 `build.gradle` 파일 - `dependencies` 에 추가

- ```
  compileOnly 'org.projectlombok:lombok'
  annotationProcessor 'org.projectlombok:lombok'
  ```

