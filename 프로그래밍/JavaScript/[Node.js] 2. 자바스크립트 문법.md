

## 자바스크립트 문법

## function

- `function` ⇒ `return` 키워드 필수고, hoisting이 발생한다.
- `const` 는 hoisting 발생하지 않으므로 반드시 변수 선언 이후 호출 할 것

``` javascript
function add1(x,y){
    return x+y;
}
const add2 = (x,y) => {
    return x+y;
};
const add3=(x,y) => x+y;
const add4 = (x,y) => (x+y);

// not1 과 not2 도 같은 기능
function not1(x) {
    return !x;
}
const not2 = x => !x;
```

- `function`은 객체임

  - **`function`안에서 `this`는 `function`자기 자신!**

  

``` javascript
var relationship1 = {
    name: 'zero',
    friends: ['nero', 'hero', 'xero'],
    logFriends: function() {
        var that = this; // relationship1
        this.friends.forEach(function(friend) {
            console.log(that.name, friend);
        });
    },
};

const relationship2 = {
    name: 'zero',
    friends: ['nero', 'hero', 'xero'],
    logFriends() {
        this.friends.forEach(friend => {
            console.log(this.name, friend);
        });
    },
};
```

- 화살표함수는 객체이긴하나  this를 가지지않는 객체로 생각하면 됨
- 편하게 생각하면 람다식으로 생각



## 클래스

- ES2015+ 에 Class가 도입되었음

``` javascript
function Info(count) { // 빈 클래스인데 
    this.count = count; // count 라는 프로퍼티 추가해서 return
}

const obj = new Info(1); // obj 안에는 count 라는 프로퍼티가 있는 상태로 만들어짐
```

- 함수 이름이지만 대문자로 만들면 객체를 만들겠다는 의미 ⇒ 생성자 역할
- 모든 객체에 프로퍼티 추가하고싶다면 ⇒ 프로토타입(부모에 있는) 추가



``` javascript
// 기존 프로토타입 이용해서 상속하는 게 불편해서 만든 Class
class Human {
    constructor(type='human'){
        this.type = type
    }
    static isHuman(human) {
        return human instanceof Human;
    }
    breathe() {
        console.log('haha');
    }
}

class Zero extends Human {
    constructor(type, firstName, lastName) {
        super(type);
        this.firstName = firstName;
        this.lastName = lastName;
    }

    sayName() {
        super.breathe();
        console.log(`${this.firstName} ${this.lastName}`);
    }
}

const newZero = new Zero('human', 'Zero', 'Cho');
Human.isHuman(newZero);
```



## 프로미스

- 비동기가 연속으로 있는데 순차적으로 진행되어야할 때 콜백 지옥이 발생한다.

  ex) database 

- 내용이 실행은 되었지만 결과를 아직 반환하지 않은 객체!



### 프로미스 사용법

- promise 객체를 생성
- Resolve(성공리턴값) → then으로 연결
- Reject(실패리턴값) → catch로 연결
- Finally 부분은 무조건 실행됨 (생략가능)

``` javascript
const condition = true;

const promise = new Promise((resolve, reject) => {
    if(condition) {
        resolve('성공');        
    } else {
        reject('실패');
    }
});

promise
    .then((msg) => {
        console.log(msg);
    }) 
    .catch((err) => {
        console.error(err);
    })
    .finally(() => {
        console.log('무조건 실행');
    });
```





두 개 이상 만들고 싶다면 프로미스 체이닝!

### 프로미스 체이닝

- then 안에서 return한 값이 다음 then으로 넘어간다.
- return 값이 프로미스면 resolve 후 넘어간다.
- 하나라도 reject 뜨면 바로 catch로 이동한다.

``` javascript
promise
    .then((msg) => {
        return new Promise((resolve, reject) => {
            resolve(msg);
        });
    })
    .then((msg2) => {
        return new Promise((resolve, reject) => {
            resolve(msg2);
        });
    })
    .then((msg3) => {
        return new Promise((resolve, reject) => {
            resolve(msg3);
        });
    })
    .catch((error) => {
        console.error(error);
    })
```



### 가독성 좋게 만드는 코드

- then 안에 내용이 너무 길때, 함수로 만들어서 넣으면 가독성이 올라간다!

``` javascript
function f1() {
    return new Promise((resolve, reject) => {
        console.log('f1');
        resolve();
    });
}
function f2() {
    return new Promise((resolve, reject) => {
        console.log('f2');
        resolve();
    });
}
function f3() {
    return new Promise((resolve, reject) => {
        console.log('f3');
        resolve();
    });
}

f1()
.then(f2())
.then(f3());
```



## async/await

- `변수 = await 프로미스` → 프로미스가 resolve된 값이 변수에 저장
- `변수 await 값` → 그 값이 변수에 저장
- `catch`가 없기 때문에, `try/catch`로 에러 처리
- 익명함수나 화살표함수도 사용 가능하다.

``` javascript
async function test() {
    try{
        await f1();
        await f2();
        await f3();
    } catch(e){
        console.log(e);
    }
}

test();
```



