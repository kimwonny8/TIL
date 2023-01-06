```
dependencies {
	// springboot 기본 설정
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
	implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	implementation 'org.springframework.boot:spring-boot-devtools'

	implementation 'org.springframework.boot:spring-boot-starter-validation'

	// jpa 로그
	implementation 'com.github.gavlyukovskiy:p6spy-spring-boot-starter:1.8.1'

	//mybatis
	implementation 'org.mybatis.spring.boot:mybatis-spring-boot-starter:3.0.1'    

	implementation 'junit:junit:4.13.2'
	runtimeOnly 'com.h2database:h2'

	// mariadb 연동
	runtimeOnly 'org.mariadb.jdbc:mariadb-java-client'

	// lombok 사용
	compileOnly 'org.projectlombok:lombok'
	annotationProcessor 'org.projectlombok:lombok'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```



application.properties

```
server.port = 9960

spring.datasource.driverClassName=org.mariadb.jdbc.Driver
spring.datasource.url = jdbc:mariadb://localhost:3306/mydays
spring.datasource.username = root
spring.datasource.password = root

spring.h2.console.enabled= true

jpa.hibernate.ddl-auto = create
jpa.properties.hibernate.format_sql = true
jpa.properties.hibernate.default_batch_fetch_size= 100
jpa.properties.generate-ddl = true

logging.level.org.hibernate.SQL = debug
logging.level.org.hibernate.type = trace

# JPA
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.H2Dialect
spring.jpa.hibernate.ddl-auto=update
```



```
const path = require("path");

module.exports = {
  devServer: {
    proxy : 'http://localhost:9959'
  },
  indexPath: '../../templates/vue/index.html',
  publicPath: '/vue',
  outputDir: path.resolve(__dirname, "../backend/src/main/resources/static/vue")
}
```



DTO 생성

Mybatis?

https://khj93.tistory.com/entry/MyBatis-MyBatis%EB%9E%80-%EA%B0%9C%EB%85%90-%EB%B0%8F-%ED%95%B5%EC%8B%AC-%EC%A0%95%EB%A6%AC

