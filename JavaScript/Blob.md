# Blob? Binary Large Object

JavaScript에서 Blob(Binary Large Object, 블랍)은 이미지, 사운드, 비디오와 같은 멀티미디어 데이터를 다룰 때 사용할 수 있습니다.
대개 데이터의 크기(Byte) 및 MIME 타입을 알아내거나, 데이터를 송수신을 위한 작은 Blob 객체로 나누는 등의 작업에 사용합니다.
이 글에서는 Blob의 생성과 읽고 쓰는 방법들에 대해서 알아보겠습니다.

> [File 객체](https://developer.mozilla.org/ko/docs/Web/API/File)도 `name`과 `lastModifiedDate` 속성이 존재하는 Blob 객체입니다.

https://heropy.blog/2019/02/28/blob/