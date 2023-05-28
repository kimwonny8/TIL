Node - Vue 통신 중

Node에서 받아온 presigned url로 업로드하면 403 에러가 뜸..

```javascript
const { v4: uuidv4 } = require('uuid');
const dotenv = require('dotenv');
const { S3Client, GetObjectCommand } = require('@aws-sdk/client-s3');
const { getSignedUrl } = require('@aws-sdk/s3-request-presigner');

dotenv.config();

const s3 = new S3Client({
  region: 'ap-northeast-2',
  credentials: {
    accessKeyId: process.env.S3_ACCESS_KEY,
    secretAccessKey: process.env.S3_ACCESS_SECRET,
  },
});

exports.getPresignedURL = async (req, res) => {
  try {
    const { filename } = req.query;
    const encodedFileName = `${filename}-${uuidv4()}`;

    const command = new GetObjectCommand({
      Bucket: process.env.S3_BUCKET_NAME,
      Key: 'public/'+encodedFileName,
    });

    const presignedUrl = await getSignedUrl(s3,command, {expiresIn: 3600});

    res.json({ presignedUrl, encodedFileName});
    // console.log(presignedUrl);

  } catch (error) {
    console.error('PresignedURL 생성 오류:', error);
    res.status(500).json({ error: 'PresignedURL 생성 오류' });
  }
};

```



``` javascript
  <input class="form-control" ref="image" accept="image/*" type="file" id="formFile" @change="saveImage()">

async saveImage() {
      try {
        const selectedFile = this.$refs.image.files[0];
        const maxSize = 5 * 1024 * 1024;
        const fileSize = selectedFile.size;
        if (fileSize > maxSize) {
          alert("첨부파일 사이즈는 5MB 이내로 등록 가능합니다.");
          return;
        }
        const filename = selectedFile.name;  
        const type = filename.split('.').pop().toLowerCase();
        
        const res = await axios.get('/api/s3/url', {
          params: { filename },
        });
        const encodedFileName = res.data.encodedFileName;
        const presignedUrl = res.data.presignedUrl;
        
        await axios.put(presignedUrl, selectedFile)
        .then((res) => {
          console.log(res);
        })

        console.log('이미지 업로드 완료');
      } catch (error) {
        console.error('이미지 업로드 오류:', error);
      }
    },

```





버킷의 CORS 정책

```
[
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "HEAD",
            "GET",
            "PUT",
            "POST"
        ],
        "AllowedOrigins": [
            "*"
        ],
        "ExposeHeaders": []
    }
]
```

버킷 정책

```
{
    "Version": "2012-10-17",
    "Id": "Policy1684724565900",
    "Statement": [
        {
            "Sid": "Stmt1684724564059",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::notshovel-union-bucket/*"
        }
    ]
}
```



그냥 내 코드 잘못이였다.. (Put으로 변경)

```
@aws-sdk/client-s3
@aws-sdk/s3-request-presigner
dotenv
uuid
```



```js
  const command = new PutObjectCommand({
      Bucket: process.env.S3_BUCKET_NAME,
      Key: 'public/'+encodedFileName,
    });
```

```js
const { v4: uuidv4 } = require('uuid');
const dotenv = require('dotenv');
const { S3Client, PutObjectCommand } = require('@aws-sdk/client-s3');
const { getSignedUrl } = require('@aws-sdk/s3-request-presigner');

dotenv.config();

const s3 = new S3Client({
  region: 'ap-northeast-2',
  credentials: {
    accessKeyId: process.env.S3_ACCESS_KEY,
    secretAccessKey: process.env.S3_ACCESS_SECRET,
  },
});

exports.getPresignedURL = async (req, res) => {
  try {
    const { filename } = req.query;
    const encodedFileName = `${filename}-${uuidv4()}`;

    const command = new PutObjectCommand({
      Bucket: process.env.S3_BUCKET_NAME,
      Key: 'public/'+encodedFileName,
    });

    const presignedUrl = await getSignedUrl(s3,command, {expiresIn: 3600});

    res.json({ presignedUrl, encodedFileName});
    // console.log(presignedUrl);

  } catch (error) {
    console.error('PresignedURL 생성 오류:', error);
    res.status(500).json({ error: 'PresignedURL 생성 오류' });
  }
};

```

