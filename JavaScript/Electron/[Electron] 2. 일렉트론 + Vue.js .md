프로젝트를 vue로 진행할 예정이라, vue 프로젝트에 Electron Builder를 추가해 실행하는 법을 알아보겠습니다.



터미널에서 아래와 같은 순서로 진행해주세요.



## 1. Vue프로젝트 생성

```
npm i -g @vue/cli 

vue create test
```

- vue 버전 선택 후 진행 (필자는 vue3버전으로 진행)



## 2. 프로젝트에 Electron Builder 추가

```
vue add electron-builder
```

- Electron 버전 선택 (13.0.0)

- 설치가 완료되면 `package.json` 에 스크립트가 추가됨

  ``` json
  "scripts": {
      "serve": "vue-cli-service serve",
      "build": "vue-cli-service build",
      "lint": "vue-cli-service lint",
      "electron:build": "vue-cli-service electron:build",
      "electron:serve": "vue-cli-service electron:serve",
      "postinstall": "electron-builder install-app-deps",
      "postuninstall": "electron-builder install-app-deps"
    },
  ```



## 3. 실행

```
npm run electron:serve
```

이 작업에서 `Error: error:0308010C:digital envelope routines::unsupported` 라는 오류가 난다면, 노드 버전때문!

Node.js 버전 17 이상부터 더 이상 지원되지 않는 MD4 알고리즘을 빌드 프로세스에 사용했기 때문에 나타납니다.



### 오류 발생시 해결 방법

- Webpack5 업그레이드

```
npm install webpack@latest
```

- NODE_OPTIONS 환경 변수를 --openssl-legacy-provider로 설정하거나 --openssl-legacy-provider 플래그를 webpack에 전달합니다.

```javascript
{
  "scripts": {
    "serve": "vue-cli-service serve --openssl-legacy-provider"
  },
}
```

- 설치된 node.js 버전을 16 이하로 다운그레이드

```
nvm install 16
nvm alias default 16
nvm use 16

# Check node version:
node -v
```



## 4. 빌드

```
npm run electron:build
```

- 해당 프로젝트 폴더 경로의 `dist_electron` 에 exe 파일이 생성된 것을 볼 수 있습니다.





***



Reference

https://velog.io/@abc2752/error-0308010C-digital-envelope-routines-unsupported

https://sebhastian.com/error-0308010c-digital-envelope-routines-unsupported/