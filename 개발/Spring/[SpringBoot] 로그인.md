

- AuthenticationManager는 스프링 시큐리티의 인증을 담당한다. AuthenticationManager 빈 생성시 스프링의 내부 동작으로 인해 위에서 작성한 UserSecurityService와 PasswordEncoder가 자동으로 설정된다.