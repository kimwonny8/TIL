## Vue , Firebase 생성 후 연동

1. vue 설치 및 프로젝트 생성

   - vue2로 진행

   ```
   npm install -g @vue/cli 
   
   vue init webpack 프로젝트명
   ```

2. [https://console.firebase.google.com/ ]: 	"Firebase 사이트"

   - Firebase에서 로그인 후 프로젝트 생성

3. </> 버튼 눌러서 웹 앱에 Firebase 추가

   [https://firebase.google.com/docs/web/setup?hl=ko]: 	"앱 추가 공식 문서"

   - 앱 닉네임 정하고 

   - Firebase SDK 추가 - 나는 npm 사용

   - 스크립트에서 const firebaseConfig 부분 저장

     ```js
     // Import the functions you need from the SDKs you need
     import { initializeApp } from "firebase/app";
     import { getAnalytics } from "firebase/analytics";
     // TODO: Add SDKs for Firebase products that you want to use
     // https://firebase.google.com/docs/web/setup#available-libraries
     
     // Your web app's Firebase configuration
     // For Firebase JS SDK v7.20.0 and later, measurementId is optional
     const firebaseConfig = {
       apiKey: "~",
       authDomain: "프로젝트명.firebaseapp.com",
       projectId: "프로젝트명",
       storageBucket: "프로젝트명.appspot.com",
       messagingSenderId: "~",
       appId: "~",
       measurementId: "~"
     };
     
     // Initialize Firebase
     const app = initializeApp(firebaseConfig);
     const analytics = getAnalytics(app);
     ```

4. firebase 설치, 로그인, 초기화 작업

   - 구글링하면 대부분 나오는 글들이 8버전이라 8버전으로 설치했음
   - firebase init 에서 사용할 기능들 선택 시, 선택한 기능이 내 firebase 프로젝트에서 생성된 상태여야 함

   ```
   npm install firebase@8.6.5
   
   firebase login
   
   firebase init
   ```

5. main.js 파일에 적용

   [https://firebase.google.com/docs/auth/web/start]: 	"공식문서"

   ```js
   // The Vue build version to load with the `import` command
   // (runtime-only or standalone) has been set in webpack.base.conf with an alias.
   import Vue from 'vue'
   import App from './App'
   import router from './router'
   
   import firebase from "firebase/app";
   import "firebase/firestore";
   
   // 아래는 개인 프로젝트와 개인 API KEY 내용
   const firebaseConfig = {
     apiKey: "",
     authDomain: "",
     projectId: "",
     storageBucket: "",
     messagingSenderId: "",
     appId: "",
     measurementId: ""
   };
   
   //Initialize Firebase
   firebase.initializeApp(firebaseConfig);
   export const db = firebase.firestore();
   
   Vue.config.productionTip = false
   
   /* eslint-disable no-new */
   new Vue({
     el: '#app',
     router,
     components: { App },
     template: '<App/>'
   })
   
   ```

6. 이 후 사용 시

   ```js
   import {db} from '../main.js'
   import "firebase/firestore"; 
   ```



## 간단한 로그인 예제로 연동 확인해보기

1. Firebase 프로젝트에서 Authentication 
2. Sign-in method 에 '이메일/비밀번호' 활성화 시키기
3. user에 가서 테스트할 이메일과 비밀번호 추가
4. vue 파일에서 아래와 같이 입력 후 로그인 테스트

```vue
<template>
    <div class="loginForm">
      <div class="loginInput">
        <p>이메일:</p>
        <input type="email" v-model="email" class="input" />
      </div>
      <div class="loginInput">
        <p>비밀번호:</p>
        <input type="password" v-model="password" class="input"  />
      </div>
      <div>
        <button @click="login" class="submitBtn">로그인</button>
      </div>
    </div>

  </template>
  
  <script>
import db from '../main'
import "firebase/firestore";
  
  export default {
    name: "Login",
    data() {
      return {
        email: '',
        password: '',
        goSignUp: false,
      };
    },
    methods: {
      login() {
        firebase.auth().signInWithEmailAndPassword(this.email, this.password).then(
          function(user) {
            alert('로그인 완료!');
          },
          function(err) {
            alert('에러 : ' + err.message)
          }
      );
    }
    }
  };
  </script>
```



오늘은 Vue 와 Firebase 연동을 해보았다.

처음에 구글링 + 공식문서로 계속 시도했는데 버전이 안맞는지 오류가 많이 났다.

오랜만에 머리 지끈지끈 서터레스 받았지만 버전 맞추고 다른 블로그를 보고 결국 성공했당 🥲

이제 진짜 시작해봐야겠다!!
