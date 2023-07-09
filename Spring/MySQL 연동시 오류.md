## Springboot - MySQL 연동 시 오류

```
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```

부분에서 오류가 난다면



**SELECT VERSION();** 

자신의  MySQL 버전 확인 후



build.gradle 에 버전 명시 해줄 것!

```
dependencies {
    implementation 'mysql:mysql-connector-java:8.0.33'
}
```

