### 🤔 Base64 -> Blob으로 변환하는 이유

Base64 를 Blob으로 변환하여 저장하는 방법은 데이터 전송, 데이터 저장, 웹에서의 이미지 표시 등에 유용하다.

-   데이터 전송: Base64는 이진 데이터를 텍스트로 변환하여 전송할 수 있으므로, 텍스트 기반 시스템에서 이진 데이터를 처리할 수 있게 한다.
-   데이터 저장: 일부 시스템은 텍스트 형식으로 데이터를 저장하는 것을 선호하며, Base64를 사용하여 이진 데이터를 텍스트로 변환한 후 Blob으로 저장할 수 있다.
-   웹에서 이미지 표시: 웹 페이지에서 이미지를 효율적으로 표시하기 위해 이미지 파일을 Base64로 변환하여 Blob으로 저장할 수 있다.

아래는 자바스크립트로 Base64를 Blob으로 바꾸는 코드!



## Base64 -> Blob

```js
function base64ToBlob(base64Data) {
      const binaryString = window.atob(base64Data);
      const bytes = new Uint8Array(binaryString.length);

      for (let i = 0; i < binaryString.length; i++) {
        bytes[i] = binaryString.charCodeAt(i);
      }
      return new Blob([bytes], { type: 'image/png' });
},
```



## Base64 -> ArrayBuffer -> Blob

-   이 과정을 하는 이유는 최종 결과는 동일하지만, 호환성과 성능 측면에서 유리하며, 추가적인 데이터 처리 기능을 제공하기 때문.
-   Base64는 텍스트 형식으로 인코딩된 데이터이기 때문에 일부 메모리 오버헤드가 발생하는데, ArrayBuffer는 이진 데이터를 직접 표현하기 때문에 메모리 오버헤드가 적어서 큰 데이터를 처리할 때 메모리 효율성과 성능 면에서 arrayBuffer를 사용하는 것이 유리할 수 있다고 한다.

```js
const contentType = 'image/png';
const base64Data = 'data:image/gif;base64,R0lGODlhAAEAAcQAALe9v9ve3/b393mDiJScoO3u74KMkMnNz4uUm......';

const blob = base64ToBlob(b64Data, contentType); 
const blobUrl = URL.createObjectURL(blob); 

const img = document.createElement('img');
img.src = blobUrl;
document.body.appendChild(img);

function base64ToBlob(base64Data, contentType = '', sliceSize = 512) {
   const image_data = atob(base64Data.split(',')[1]); 
   const arraybuffer = new ArrayBuffer(image_data.length);
   const view = new Uint8Array(arraybuffer);

   for (let i = 0; i < image_data.length; i++) {
      view[i] = image_data.charCodeAt(i) & 0xff;
      // charCodeAt() 메서드는 주어진 인덱스에 대한 UTF-16 코드를 나타내는 0부터 65535 사이의 정수를 반환
      // 비트연산자 & 와 0xff(255) 값은 숫자를 양수로 표현하기 위한 설정
   }

   return new Blob([arraybuffer], { type: contentType });
}
```

j

### blob 바꾼 파일 s3 업로드

``` js
async saveThumbnail() {
      for (let i = 0; i < this.pageList.length; i++) {
        const dataUrl = this.pageList[i].thumbnail;
        const base64Data = dataUrl.split(',')[1];
        const fileName = `${this.bookId}_${i}_thumbnail.png`;

        try {
          const res = await axios.get('/api/v1/tool/s3/image', {
            params: { fileName: fileName }
          });

          const preSignedUrl = res.data.preSignedUrl;
          const encodedFileName = res.data.encodedFileName;

          const blob = this.base64ToBlob(base64Data);

          this.s3.preSignedUrl = preSignedUrl;
          this.s3.encodedFileName = encodedFileName;

          try {
            await axios.put(this.s3.preSignedUrl, blob);
            this.s3.uploadedUrl = `${process.env.VUE_APP_S3_PATH}/${this.s3.encodedFileName}`;
            this.pageList[i].thumbnail = this.s3.uploadedUrl;
          } catch (error) {
            console.error(error);
          }
          console.log(`Thumbnail ${i} 처리 완료`);
        }
        catch (err) {
          console.error(`Thumbnail ${i} 처리 실패:`, err);
          alert('서버 문제로 파일 처리에 실패하였습니다. 잠시 후 다시 시도해주세요🙇‍♀️');
        }
      }
    },

```

