# Firebase란?

[https://firebase.google.com/docs/projects/learn-more?hl=ko]: 	"Firebase 공식 문서"

 **구글(Google)이 소유하고 있는 모바일 애플리케이션 개발 플랫폼 **

> 파이어베이스는 “앱을 개발하고, 개선하고, 키워갈 수 있는” 도구 모음(toolset)이며, 이러한 도구가 없다면 개발자들은 일반적으로 서비스의 상당 부분을 직접 만들어내야만 합니다. 그런데 개발자들은 앱의 사용자 경험(UX)에 집중을 해야 하기 때문에, 그런 세세한 부분들까지 전부 만드는 걸 좋아하지 않습니다. 그런 부분들로는 분석, 인증, 데이터베이스, 구성 설정, 파일 저장, 푸시(push) 메시지 등, 열거하자면 끝이 없습니다. 파이어베이스로 만든 이러한 서비스들이 클라우드에 호스팅 되면, 개발자의 입장에서는 거의 아무런 노력을 들이지 않고도 앱의 규모를 확장할 수 있습니다.



<img src="https://blog.wishket.com/wp-content/uploads/2021/03/5-3-1024x537.png" style="zoom: 67%;" />

## Firebase 프로젝트의 계층 구조

<img src="https://firebase.google.com/static/docs/projects/images/firebase-projects-hierarchy_projects-apps-resources.png?hl=ko" style="zoom: 50%;" />

## Firebase 프로젝트 식별자

### 프로젝트 이름

- 프로젝트를 생성할 때 프로젝트 이름을 제공함. 
- 이 식별자는 [Firebase 콘솔](https://firebase.google.com/docs/projects/learn-more?hl=ko#manage-console) , [Google Cloud Console](https://cloud.google.com/docs/overview/?hl=ko#google-cloud-console) 및 [Firebase CLI](https://firebase.google.com/docs/projects/learn-more?hl=ko#manage-cli) 에 있는 프로젝트의 내부 전용 이름

### 프로젝트 번호

- 프로젝트에 대해 Google에서 할당한 전역 고유 표준 식별자
- 통합을 구성하거나 Firebase, Google 또는 타사 서비스에 API를 호출할 때 이 식별자를 사용

### 프로젝트 ID

- Firebase 및 Google Cloud 전체에서 프로젝트에 대한 사용자 정의 고유 식별자

  

## 데이터 모델

### Firestore는 Collection과 Document 

- Firestore는 NoSQL 문서 중심의 데이터베이스

- SQL Database와 달리 테이블이나 행이 없으며, *컬렉션*으로 정리되는 문서에 데이터를 저장

- 각 문서에는 키-값 쌍이 있음

- 모든 문서는 컬렉션에 저장되어야 함

- 문서는 하위 컬렉션과 중첩 개체를 포함할 수 있음

- 둘 다 모두 문자열과 같은 기본형 필드나 목록과 같은 복합 객체를 포함할 수 있음

  

### 문서

- Firestore의 스토리지 단위는 문서

  - 예로 사용자 wonny 의 문서

    ```
    first : "wonny"
    last : "kim"
    born : 1997
    ```

  - 사용자 이름을 맵(문서의 복잡한 중첩 개체)으로 구조화하기

    ```
    name :
        first : "wonny"
        last : "kim"
    born : 1997
    ```

    ➡️ 문서가 JSON과 매우 비슷해졌는데, 사실은 기본적으로 JSON이 사용됨. 문서가 추가적인 데이터 유형을 지원하고 크기가 1MB로 제한되는 등의 몇 가지 차이점이 있기는 하지만, 일반적으로는 문서를 간단한 JSON 레코드로 취급해도 무방

### 컬렉션

- 문서는 컬렉션에 저장되며, 컬렉션은 단순히 문서의 컨테이너

- 오로지 문서만 포함! (원시 필드, 다른 컬렉션 포함할 수 없음)

- 컬렉션 내의 문서 이름은 고유함

- 컬렉션에 첫 번째 문서를 만들면 컬렉션이 생성

- 컬렉션의 모든 문서를 삭제하면 컬렉션도 삭제

  <img src="https://cloud.google.com/static/firestore/docs/images/structure-data.png?hl=ko" style="zoom: 50%;" />