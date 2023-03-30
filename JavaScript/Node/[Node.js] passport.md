

```
npm i passport passport-local
```

https://inpa.tistory.com/entry/NODE-%F0%9F%93%9A-Passport-%EB%AA%A8%EB%93%88-%EA%B7%B8%EB%A6%BC%EC%9C%BC%EB%A1%9C-%EC%B2%98%EB%A6%AC%EA%B3%BC%EC%A0%95-%F0%9F%92%AF-%EC%9D%B4%ED%95%B4%ED%95%98%EC%9E%90



## 로그인 과정

1. 로그인 요청이 들어옴 

2. passport.authenticate 메서드 호출 

3. 로그인 전략 수행(전략은 뒤에 알아봄)

4. 로그인 성공 시 사용자 정보 객체와 함께 req.login 호출 

5. req.login 메서드가 passport.serializeUser 호출 

6. req.session에 사용자 아이디만 저장 

   

## 로그인 완료 로그인 이후 과정 

1. 모든 요청에 passport.session() 미들웨어가 passport.deserializeUser 메서드 호출 
1. req.session에 저장된 아이디로 데이터베이스에서 사용자 조회
1. 조회된 사용자 정보를 req.user에 저장
1. 라우터에서 req.user 객체 사용 가능

