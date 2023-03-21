https://mrw0119.tistory.com/136



https://inpa.tistory.com/entry/ODM-%F0%9F%93%9A-%EB%AA%BD%EA%B5%AC%EC%8A%A4-%EC%82%AC%EC%9A%A9%EB%B2%95-%EC%A0%95%EB%A6%AC#%EB%AA%BD%EA%B5%AC%EC%8A%A4_%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8_%EA%B5%AC%EC%84%B1



문법 바뀜 주의 ㅜ

https://stackoverflow.com/questions/75586474/mongoose-stopped-accepting-callbacks-for-some-of-its-functions

``` js
 
// 몽구스 연결 함수 - 구버전
const connect = () => {
  mongoose.connect(dbUrl, {
    dbName: 'test',
  }, (error) => {
    if (error) {
      console.log('몽고디비 연결 에러', error);
    } else {
      console.log('몽고디비 연결 성공');
    }
  });
};

// 몽구스 연결 함수 - 현 버전
const connect = () => {
  mongoose.connect(dbUrl)
  .then(() => console.log('mongodb 연결 성공'))
  .catch(e => console.error(e));
};

```

