## Base64

-  Base64는 0과 1로 이루어진 이진 데이터(바이너리)를 인코딩하여 텍스트 형식으로 변환하는 것

> base64로 변환해주면 우리가 직접 소스 코드단에 이미지 데이터 자체를 저장할 수 있게 된다. 우리가 변수에 문자열이나 숫자를 저장하는 것 처럼, **변수에 이미지를 저장**할 수 있는 것이다.
>
> base64는 이미지 데이터 값을 변환하는게 아닌 그냥 출력 형식을 변환하는 것이기 때문에 이미지 자체가 바뀌는 것이 아니니 가능한 것이다. 즉, 이미지 데이터 정보는 이미 base64 텍스트 자체에 포함되어 있기때문에, 결과적으로 링크 url을 통해 **서버에 요청하지 않고도 직접 이미지를 사용**할 수 있게 된다.



## Blob

- Binary Large OBject의 약자로 주로 이미지, 오디오, 비디오와 같은 멀티미디어 파일 바이너리를 객체 형태로 저장한 것을 의미



## ArrayBuffer

> 일정 구획만큼의 데이터를 쪼개서 전달되는 stream을 저장한 후 일정 크기가 도달하면 출력장치나 동영상 플레이어로 전달해주는 중개자 역할을 하는 객체라고 보면 된다. (버퍼링이 바로 이 개념이다)
>
> 따라서 자바스크립트 용도가 다양해지면서 이처럼 오디오나 비디오와 같은 binary data들 역시 다룰 필요성이 생기게 되자, 필요한 메모리 공간을 적절하게 할당해서 사용할 수 있는 유연성이 필요해 ArrayBuffer를 만들게 되었다고 이해하면 된다.



## Base64 -> arrayBuffer -> Blob

base64 데이터를 blob으로 바꾸기 위해 중간 과정으로 arrayBuffer가 쓰인다.

ArrayBuffer를 Blob으로 변환하는 방법은 최종 결과는 동일하지만, arrayBuffer를 사용하는 이유는 호환성과 성능 측면에서 유리하며, 추가적인 데이터 처리 기능을 제공하기 때문

Base64는 텍스트 형식으로 인코딩된 데이터이기 때문에 일부 메모리 오버헤드가 발생합니다. 그러나 ArrayBuffer는 이진 데이터를 직접 표현하기 때문에 메모리 오버헤드가 적어서, 큰 데이터를 처리할 때 메모리 효율성과 성능 면에서 arrayBuffer를 사용하는 것이 유리할 수 있다고함.

```js

function b64toBlob(b64Data, contentType = '', sliceSize = 512) {
   const image_data = atob(b64Data.split(',')[1]); // data:image/gif;base64 필요없으니 떼주고, base64 인코딩을 풀어줌
   const arraybuffer = new ArrayBuffer(image_data.length);
   const view = new Uint8Array(arraybuffer);

   for (let i = 0; i < image_data.length; i++) {
      view[i] = image_data.charCodeAt(i) & 0xff;
      // charCodeAt() 메서드는 주어진 인덱스에 대한 UTF-16 코드를 나타내는 0부터 65535 사이의 정수를 반환
      // 비트연산자 & 와 0xff(255) 값은 숫자를 양수로 표현하기 위한 설정
   }

   return new Blob([arraybuffer], { type: contentType });
}

const contentType = 'image/png';
const b64Data = 'data:image/gif;base64,R0lGODlhAAEAAcQAALe9v9ve3/b393mDiJScoO3u74KMkMnNz4uUm......';

const blob = b64toBlob(b64Data, contentType); // base64 -> blob
const blobUrl = URL.createObjectURL(blob); // object url 생성

const img = document.createElement('img');
img.src = blobUrl;
document.body.appendChild(img);
```



### 정리

- base64는 바이너리 데이터를 다루기 위해 **텍스트(문자열)** 형태로 저장한 포맷
- blob이란 바이너리 데이터를 다루기 위해 **객체(Object)** 저장하는 것



https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-Base64-Blob-ArrayBuffer-File-%EB%8B%A4%EB%A3%A8%EA%B8%B0-%EC%A0%95%EB%A7%90-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-%EC%89%BD%EA%B2%8C-%EC%84%A4%EB%AA%85