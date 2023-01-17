https://wikidocs.net/162141

``` java
// SiteUser.java
@Getter
@Setter
@Entity
public class SiteUser {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(unique = true)
    private String username;

    private String password;

    @Column(unique = true)
    private String email;
}
```



``` java
// UserRepository.java
public interface UserRepository extends JpaRepository<SiteUser, Long> {
}
```



```java
// UserService.java
@RequiredArgsConstructor
@Service
public class UserService {

    private final UserRepository userRepository;

    public SiteUser create(String username, String email, String password) {
        SiteUser user = new SiteUser();
        user.setUsername(username);
        user.setEmail(email);
        BCryptPasswordEncoder passwordEncoder = new BCryptPasswordEncoder();
        user.setPassword(passwordEncoder.encode(password));
        this.userRepository.save(user);
        return user;
    }
}
```

- BCryptPasswordEncoder는 BCrypt 해싱 함수(BCrypt hashing function)를 사용해서 비밀번호를 암호화

-> 암호화 방식을 변경하면 일일이 찾아서 수정해야하므로 PasswordEncoder 빈(bean)으로 등록해서 사용하는 것이 좋다.

PasswordEncoder 빈(bean)을 만드는 가장 쉬운 방법은 @Configuration이 적용된 SecurityConfig에 @Bean 메서드를 생성하는 것이다

``` java
// SecurityConfig.java 파일에 추가

 @Bean
    PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
```

``` java
// UserService.java 변경
@RequiredArgsConstructor
@Service
public class UserService {

    private final UserRepository userRepository;
    private final PasswordEncoder passwordEncoder;

    public SiteUser create(String username, String email, String password) {
        SiteUser user = new SiteUser();
        user.setUsername(username);
        user.setEmail(email);
        user.setPassword(passwordEncoder.encode(password));
        this.userRepository.save(user);
        return user;
    }
}

```

