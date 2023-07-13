## 사이트 접속시 오류

<img src="./assets/image-20230712215009203.png" alt="image-20230712215009203" style="zoom:67%;" />

```
Failed to load API definition

Errors : Fetch error
undefined http://localhost:8080/v3/api-docs
```



SecurityConfig 에 설정 추가

```
.antMatchers("/swagger-ui/**", "/swagger-ui.html", "/swagger-resources/**", "/v3/api-docs", "/v3/api-docs/**").permitAll()
```

