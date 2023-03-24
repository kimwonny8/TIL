## Open ai 사용법

[공식문서](https://platform.openai.com/docs/api-reference/chat)

### gpt 사용시 요청

```
POST https://api.openai.com/v1/chat/completions
```

### Headers

```json
{
   "Authorization": "Bearer {your key}",
   "Content-Type": "application/json"
}
```

### Parameters

```jsx
{
  "model": "gpt-3.5-turbo",
  "messages": [{"role": "user", "content": "Hello!"}]
}
```

### Response

⇒ 접근시 `response.data.choices[0].message.content`;

```jsx
{
  "id": "chatcmpl-123",
  "object": "chat.completion",
  "created": 1677652288,
  "choices": [{
    "index": 0,
    "message": {
      "role": "assistant",
      "content": "\\n\\nHello there, how may I assist you today?",
    },
    "finish_reason": "stop"
  }],
  "usage": {
    "prompt_tokens": 9,
    "completion_tokens": 12,
    "total_tokens": 21
  }
}
```



## vue2 에서 사용

```vue
<template>
  <div>
    <div class="gpt">
      <input v-model="prompt" type="text">
      <button @click="submit()">확인</button>
      <p v-text="this.generatedText"></p>
    </div>
</div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      prompt: '',
      generatedText: null,
  },
  methods: {
    // gpt 테스트
    async submit() {
      try {
        const response = await axios.post('https://api.openai.com/v1/chat/completions', {
          model: "gpt-3.5-turbo",
          messages: [{"role": "user", "content": this.prompt}]
        }, {
          headers: {
            'Authorization': 'Bearer {key}',
            'Content-Type': 'application/json'
          }
        });
        this.generatedText = response.data.choices[0].message.content;
        console.log(this.generatedText);
      } catch (error) {
        console.error(error);
      }
    },

  },
};
</script>

```

