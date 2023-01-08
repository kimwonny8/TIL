[toc]

# 📌JPA

JPA를 알기 전에 ORM을 먼저 알아야한다.

## ORM?

- ORM(Object-Relational Mapping)
- 객체와 관계형 데이터베이스의 데이터를 자동으로 매핑(연결)해주는 도구
- ORM을 이용하면 데이터베이스 종류에 상관 없이 일관된 코드를 유지할 수 있어서 프로그램을 유지·보수하기가 편리하다.
- 내부에서 안전한 SQL 쿼리를 자동으로 생성해 주므로 개발자가 달라도 통일된 쿼리를 작성할 수 있고 오류 발생률도 줄일 수 있다.

## JPA?

- 스프링부트는 JPA(Java Persistence API)를 사용하여 데이터베이스를 처리
- 자바에서 ORM의 기술 표준으로 사용하는 인터페이스의 모음
- 자바 어플리케이션과 JDBC 사이에서 동작



## JPA 동작 과정

> <img src="https://gmlwjd9405.github.io/images/inflearn-jpa/jpa-insert-structure.png" alt="img"  />
>
> JPA는 애플리케이션과 JDBC 사이에서 동작한다. JPA 내부에서 JDBC API를 사용하여 SQL을 호출하여 DB와 통신한다.
>
> 개발자가 ORM 프레임워크에 저장하면 적절한 INSERT SQL을 생성해 데이터베이스에 저장해주고, 검색을 하면 적절한 SELECT SQL을 생성해 결과를 객체에 매핑하고 전달해 준다.
>
> [출처](https://girawhale.tistory.com/119) 김영한, 『자바 ORM 표준 JPA 프로그래밍』, 에이콘출판(2015)



## JPA를 사용해야하는 이유

1. 생산성 증가
   - DDL문 자동 생성
   - SQL을 작성하고 JDBC API 를 사용하는 반복적인 일을 대신 처리
2. 유지보수의 편리함
   - 기존 : 필드 변경시 모든 SQL 수정
   - JPA : 필드만 추가
3. Object와 RDB간의 패러다임 불일치 해결



### JPA 추가하는 방법

- build.gradle - dependencies 추가

```
 implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
```



# 📌 Hibernate



# 📌 Spring Data JPA



[참고블로그](https://suhwan.dev/2019/02/24/jpa-vs-hibernate-vs-spring-data-jpa/)
