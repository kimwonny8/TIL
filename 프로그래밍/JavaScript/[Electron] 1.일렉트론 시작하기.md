일렉트론을 활용한 프로젝트를 만들기 전, 일렉트론에 대해 알아보자.



## 일렉트론(Electron)

- OpenJS Foundation에서 개발하고 GitHub에서 유지보수하고 있는 앱 프레임워크
- 일렉트론은 여러 저명한 오픈 소스 프로젝트를 뒷받침하는 주요 GUI 프레임워크

즉, 일렉트론은 Javascript 만으로 여러 플랫폼에서 동작할 수 있는 데스크톱 어플리케이션을 만들 수 있게 해 주는 프레임워크이다.

- 오픈소스 웹브라우저 **크로미움(Chromium)**, 브라우저 밖에서 자바스크립트 코드를 실행하는 런타임 환경인 **노드JS**, 오픈소스 자바스크립트 엔진 **V8**, 이 세 가지 핵심 요소로 구성되어 있다.




크로미움은 처음 들은 용어라 크로미움에 대해 알아보자면,

## 크로미움(크로미엄, 크로뮴) ?

크로미움은 오픈 소스 웹 브라우저 프로젝트이며 구글 크롬은 크로미엄 코드를 사용하여 개발된다.

즉 크로미움은 브라우저의 이름이고, 크롬과 엣지 등에서 사용하는 소스코드를 생성하는 오픈소스 프로젝트의 이름이기도 하다.

![](https://t1.daumcdn.net/cfile/tistory/993C3B505C35C5E71E)



### 크로미움과 크롬과 차이점

크롬에는 있지만 크로미엄에는 없는 것

- 사용자 식별 인증키 설정 요구사항
- 자동 업데이트
- 사용 정보 및 오류 보고서를 구글로 보내기 선택하기
- 사용 정보 추적(구글 웹사이트에서 구글 크롬을 다운로드하지 않을 경우에 이 코드가 포함되어 언제 어디서 구글 크롬을 다운로드했는지 등의 정보를 전송)
- 구글 크롬과는 다른 별도의 오픈소스 내장 PDF 뷰어
- 구글 크롬과는 다른 별도의 내장 인쇄 미리보기와 인쇄 시스템
- 내장 어도비 플래시 플레이어
- 구글 계정 로그인

[참고 나무위키](https://ko.wikipedia.org/wiki/%ED%81%AC%EB%A1%9C%EB%AF%B8%EC%97%84_(%EC%9B%B9_%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80))



## 일렉트론으로 만든 애플리케이션

VSCode, Slack, Figma, Github Desktop, Atom, Discord 등

https://www.electronjs.org/apps 이 주소에 나와있으니 참고하시면 됩니다!



***



## 👉 일렉트론 시작하기

먼저 프로젝트 폴더를 생성하고, 그 폴더에서 프로젝트를 생성합니다.

```
mkdir electron-app
cd electron-app

npm init
```



Electron package 추가

```
npm install --save-dev electron
```



`package.json`에 script 추가 => `start`로 스크립트 실행 가능

```
"scripts": { 
     "start": "electron ." 
 }
```



`index.html` 생성 후 코드 작성

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self'">
    <meta http-equiv="X-Content-Security-Policy" content="default-src 'self'; script-src 'self'">
    <title>Hello World!</title>
  </head>
  <body>
    <h1>Hello World!</h1>
    <p>일렉트론 테스트 중입니다.</p>
  </body>
</html>
```



`index.js` 생성 후 코드 작성

``` javascript
const { app, BrowserWindow } = require('electron')
// include the Node.js 'path' module at the top of your file
const path = require('path')


// modify your existing createWindow() function
function createWindow () {
  const win = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      preload: path.join(__dirname, 'preload.js')
    }
  })
  win.loadFile('index.html')
}
app.whenReady().then(() => {
  createWindow()
})
app.on('window-all-closed', function () {
  if (process.platform !== 'darwin') app.quit()
})
```



`preload.js` 생성 후 코드 입력

``` javascript
window.addEventListener('DOMContentLoaded', () => {
  const replaceText = (selector, text) => {
    const element = document.getElementById(selector)
    if (element) element.innerText = text
  }

  for (const dependency of ['chrome', 'node', 'electron']) {
    replaceText(`${dependency}-version`, process.versions[dependency])
  }
})
```



이 후 프로젝트 실행!! `npm start`