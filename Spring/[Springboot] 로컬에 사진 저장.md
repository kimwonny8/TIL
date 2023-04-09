## Springboot

```java
@PostMapping(value = "/image", consumes = {MediaType.MULTIPART_FORM_DATA_VALUE})
    public @ResponseBody String addPhoto(@RequestParam("image") MultipartFile img) {

        UUID uuid = UUID.randomUUID();
        String imageFileName = uuid+"_"+img.getOriginalFilename();

        String path = "D:/project/cnu-taleteller/backend/src/main/resources/static/image/";
        Path imagePath = Paths.get(path+imageFileName);

        try{
            Files.write(imagePath, img.getBytes());
        }
        catch(Exception e){
            System.out.println(e);
        }
        System.out.println(img.getOriginalFilename());

        return imageFileName;
    }
```



## vue

```vue
<template>
    <div class="menu">
        <input type="file" @change="setImage()" accept="image/*" id="image">
    </div>
</template>
<script>
import axios from 'axios';
export default {
    data(){
        return{
            image: null
        }
    },
    methods: {
        setImage(){
            let frm = new FormData();
            let imageFile = document.getElementById("image");
            frm.append("image", imageFile.files[0]);
            
            axios.post("/api/users/image", frm, {
                headers: {
                    'Content-Type': 'multipart/form-data'
                }
            })
            .then(()=>{
                console.log("전송 성공");
            })
            .catch((e)=> {
                console.log(e);
            })
        },
 }
}

```

