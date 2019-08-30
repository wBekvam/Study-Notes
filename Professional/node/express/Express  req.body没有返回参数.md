## Express  req.body没有返回参数

##### 因为express将body-parser分离了出来，所以你需要手动添加进下面的内容即可

```
$ npm install body-parser
```

#### 引入

```javascript
const bodyParser = require('body-parser')

app.use(bodyParser.urlencoded({ extended: false }))

app.use(bodyParser.json())
```

### 巨坑！巨坑！巨坑！巨坑！

##### 在使用 body-paeer 时，一定要先引用在模块后在引入路由接口

```javascript
const bodyParser = require('body-parser')

// 引入处理post数据的模块

app.use(bodyParser.urlencoded({ extended: false }))

app.use(bodyParser.json())

const api = require('./api') // 引入编写好的api

app.use(api)
```

