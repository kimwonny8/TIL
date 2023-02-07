형식 안맞춰주면 `request method 'get' not supported 405` 에러 발생

# @PostMapping

- axios 통신시 get방식으로 받을 때 PostMapping사용
- @RequestBody으로 받을 것



# @GetMapping

- axios 통신시 get방식으로 받을 때 GetMapping 사용
- @RequestParam으로 받을 것



## vue에서 axios 로 보낼 때

``` javascript
axios.get("/api/diary/list", { params: { email: sessionStorage.getItem("email") }} )
            .then((res) => {
                        console.log(res);
             })
```



``` java
@RestController
@RequiredArgsConstructor
@RequestMapping(value="/api/diary")
public class DiaryController {

    private final DiaryRepository diaryRepository;
    private final DiaryService diaryService;

    @PostMapping("/post")
    public String post(@RequestBody DiaryForm diaryForm){
        return diaryService.post(diaryForm.toEntity());
    }

    @GetMapping("/list")
    public List<Diary> allList(@RequestParam Map<String, String> params){
         List<Diary> diaryList = diaryService.diaryList(params.get("email"));
        System.out.println(diaryList);
        return diaryList;
    }
}

```



[참고블로그](https://imucoding.tistory.com/217)

[참고블로그2](https://susblog.tistory.com/110)