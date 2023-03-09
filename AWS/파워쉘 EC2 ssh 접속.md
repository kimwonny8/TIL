```
cd ~/.ssh

ssh myEC2
```

[참고 블로그](https://velog.io/@leesomyoung/%ED%8C%8C%EC%9B%8C%EC%89%98%EB%A1%9C-EC2%EC%97%90-ssh%EB%A1%9C-%EC%A0%91%EC%86%8D%ED%95%98%EA%B8%B0)

```
Host myEC2
	HostName (ip)
	User ubuntu
	IdentityFile ~/.ssh/키명.pem
```



자바 접속

```
java -jar jar파일명
```



마리아 DB 접속

```
# 클라이언트가 잘 설치되었는지 확인
mysql --version 
 
# 접속 커맨드 
mysql -u admin -p -h {DB의 엔드포인트}
```



Springboot에서 바꿀 설정

```
spring.jpa.hibernate.ddl-auto=none

spring.datasource.url=jdbc:mariadb://rds주소(엔드포인트):포트명(기본은 3306)/database명
spring.datasource.username=db계정
spring.datasource.password=db계정 비밀번호
spring.datasource.driver-class-name=org.mariadb.jdbc.Driver
```



## vue

 vue 빌드 후 node_modules 제외 EC2서버로 이

```
sudo apt update
sudo apt install nodejs
sudo apt install npm
```




