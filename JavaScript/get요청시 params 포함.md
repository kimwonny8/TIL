```js
// 요청  
const ToDoListHandler = () => {
    axios.get('https://localhost:4000/sendlist/todo', 
      {params: {userId: userId}}, 
      { withCredentials: true }
    )
    .then((res)=> {
      setToDoList(res.data.data)
    })
  }

```

- axios로 통신시 `{ withCredentials: true }`



```js

// 서버
module.exports = async (req, res) => {

    const {userId} = req.query;

    const listInfo = await ToDoList.findAll({
        where: {
            userId
        }
    })

    if(!listInfo) {
        res.status(400).send('Bad Request')
    } else {
        res.status(200).json({message: 'ok', data: listInfo})
    }

}
```

