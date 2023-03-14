``` javascript
var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res, next) {
  res.render('index', { title: 'Express' });
});

router.get('/api/getTest', async (req, res) => {
  try {
    res.send({
      msg: 'This my response : get'
    })
  } catch (err) {
    console.log(err);
    res.send({
      error: 'Can"t read api data',
    });
  }
});

router.post('/api/postTest', async (req, res) => {
  try {
    console.log(req.body)
    res.send({
      msg: 'Hi!' + req.body.name
    })
  } catch (err) {
    console.log(err);
    res.send({
      error: 'Can"t read api data',
    });
  }
});

module.exports = router;
```

```
{
  "dependencies": {
    "@octokit/core": "^4.2.0",
    "dotenv": "^16.0.3",
    "express": "^4.18.2",
    "https": "^1.0.0",
    "marked": "^4.2.12",
    "node-fetch": "^3.3.1",
    "nodemon": "^2.0.21",
    "prismjs": "^1.29.0"
  },
  "name": "backend",
  "version": "1.0.0",
  "main": "./routes/index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "nodemon app"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "description": ""
}

```

