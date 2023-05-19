```properties
spring.servlet.multipart.max-file-size=5MB
cloud.aws.stack.auto=false
cloud.aws.region.static=ap-northeast-2
cloud.aws.credentials.access-key=${AWS_ACCESS_KEY}
cloud.aws.credentials.secret-key=${AWS_SECRET_KEY}
cloud.aws.s3.bucket=${AWS_S3_BUCKET}
```



```java
@Configuration
public class S3Config {
    @Value("${cloud.aws.credentials.access-key}")
    private String accessKey;
    @Value("${cloud.aws.credentials.secret-key}")
    private String secretKey;
    @Value("${cloud.aws.region.static}")
    private String region;

    @Bean
    public AmazonS3Client amazonS3Client() {
        BasicAWSCredentials awsCreds = new BasicAWSCredentials(accessKey,secretKey);
        return (AmazonS3Client) AmazonS3ClientBuilder.standard()
                .withRegion(region)
                .withCredentials(new AWSStaticCredentialsProvider(awsCreds))
                .build();
    }

}
```



``` java
@Service
@RequiredArgsConstructor
public class S3Service {

    private final S3Config s3Config;

    @Value("${cloud.aws.s3.bucket}")
    private String bucket;


    public Map<String, Serializable> getPreSignedUrl(String fileName) {
        String encodedFileName = fileName + "_" + LocalDateTime.now();
        String objectKey = "test/" + encodedFileName;

        Date expiration = new Date();
        long expTimeMillis = expiration.getTime();
        expTimeMillis += (3 * 60 * 1000); // 3 minutes
        expiration.setTime(expTimeMillis); // Set URL expiration time

        GeneratePresignedUrlRequest generatePresignedUrlRequest = new GeneratePresignedUrlRequest(bucket, objectKey)
                .withMethod(HttpMethod.PUT)
                .withExpiration(expiration);

        Map<String, Serializable> result = new HashMap<>();
        result.put("preSignedUrl", s3Config.amazonS3Client().generatePresignedUrl(generatePresignedUrlRequest));
        result.put("encodedFileName", encodedFileName);

        return result;
    }
}
```



``` java
@RestController
@RequestMapping("/api/tool")
@RequiredArgsConstructor
public class ToolController {
    private final S3Service s3Service;

    @GetMapping("/s3/image")
    public Map<String, Serializable> s3saveImage(@RequestParam("fileName") String fileName){
        return s3Service.getPreSignedUrl(fileName);
    }
```



## Vue

``` vue
<template>
	<input type="file" @change="uploadFile" ref="file">
</template>
<script>
    data: {
        return() {
          file: null,
          preSignedUrl: null,
          encodedFileName: null,
          uploadedUrl: null
        }
    }
    methods: {
        // S3 presigned url 받아오기
        async uploadFile() {
          this.file = this.$refs.file.files[0];
          await axios.get("/api/tool/s3/image", {params: {fileName: this.file.name}},)
          .then((res) => {
            console.log(res.data);
            this.preSignedUrl = res.data.preSignedUrl
            this.encodedFileName = res.data.encodedFileName
            this.uploadImageToS3(this.preSignedUrl, this.file)
          })
        },
        // S3 업로드
        uploadImageToS3(preSignedUrl, file) {
          axios.put(preSignedUrl, file)
          .then(() => {
            this.uploadedUrl = `${process.env.VUE_APP_S3_PATH} + this.encodedFileName`
          });
        },
    }
</script> 
```



### uploadImageToS3 과정에서 CORS 발생

- S3 CORS 정책 수정

```
[
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "GET",
            "POST",
            "PUT",
            "DELETE"
        ],
        "AllowedOrigins": [
            "http://localhost:8200"
        ],
        "ExposeHeaders": [
            "x-amz-server-side-encryption",
            "x-amz-request-id",
            "x-amz-id-2"
        ],
        "MaxAgeSeconds": 3000
    }
]
```

