

## npm

- `i` : install
- `-g` : 전역 설치, `package.json`엔 설정안되기 때문에 팀으로 할 땐 주의해야함
- `-D` : 개발할 때만 쓸 것, 배포할 때는 빼줘 ex) nodemon

```
npm init

npm i express

npm i morgan cookie-parser express-session
```

```
npm i -D nodemon
```



### package.json 버전

- ^1.1.1: 패키지 업데이트 시 minor 버전까지만 업데이트 됨(2.0.0버전은 안 됨) 
- ~1.1.1: 패키지 업데이트 시 patch버전까지만 업데이트 됨(1.2.0버전은 안 됨)