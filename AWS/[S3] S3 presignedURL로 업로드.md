

presignedURL 받는 법



controller

``` java
    private final FileService fileService;

    @PostMapping(value = "/image", consumes = {MediaType.MULTIPART_FORM_DATA_VALUE})
    public String saveImage(@RequestParam("image") MultipartFile img, @RequestParam("menu") String menu) {
        return fileService.saveImageS3(img);
    }
```



service

``` java
 public String saveImageS3(MultipartFile img){
        String preSignedURL = "";

        String uuid = UUID.randomUUID().toString();
        String fileName = uuid + "_" + img.getOriginalFilename();

        Date expiration = new Date();
        long expTimeMillis = expiration.getTime();
        expTimeMillis += 1000 * 60 * 2;
        expiration.setTime(expTimeMillis);

        try {
            GeneratePresignedUrlRequest generatePresignedUrlRequest =
                    new GeneratePresignedUrlRequest(bucket, fileName)
                            .withMethod(HttpMethod.GET)
                            .withExpiration(expiration);
            URL url = amazonS3Client.generatePresignedUrl(generatePresignedUrlRequest);
            preSignedURL = url.toString();
        } catch (Exception e) {
            e.printStackTrace();
        }

        return preSignedURL;
```



vue

```js
    // 이미지 업로드
    async setImage(menu) {
      console.log(menu);
      try {
        let frm = new FormData();
        let imageFile = document.getElementById("image");
        frm.append("image", imageFile.files[0]);
        frm.append("menu", menu);
        frm.append("bookId", this.bookId);
        const res = await axios.post(`/api/tool/image`, frm, {
          headers: {
            'Content-Type': 'multipart/form-data'
          }
        });
        this.isUpload = true;
        console.log(res.data);
        this.uploadImageToS3(res.data, imageFile.files[0]);
      } catch (e) {
        console.log(e);
      }
    },
    async uploadImageToS3(presignedUrl, file) {
      try {
        const res = await fetch(
          new Request(presignedUrl, {
            method: "PUT",
            body: file,
            headers: new Headers({
              "Content-Type": "image/*",
            })
          })
        )
        console.log("업로드 성공", res);
      }
      catch(err){
        console.log(err);
      }
    },
```

