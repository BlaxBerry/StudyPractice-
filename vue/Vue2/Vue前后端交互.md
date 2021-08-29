# Vue前后端交互

## 接口调用方式

- **原生Ajax**

- **jQuery的Ajax**

- **fetch**

- **axios**



## Ajax

onload方法

```js
var xhr = new XMLHttpRequest();
xhr.open('get','URL');
xhr.send();
xhr.onload = function(){
  console.log(xhr.responeseText)
})
```

onreadystatechange方法

```js
var xhr = new XMLHttpRequest();
xhr.open('get','URL');
xhr.onreadystatechange = function(){
  if(xhr.readyState == 4){
    console.log(xhr.responeseText)
  }else{
    console.log('有错误')
  }
}
xhr.send();
```





## jQuery - Ajax

```js
$.ajax({
  url:'http://localhost:3000/data',
  success:function(data){
    console.log(data)
  }
})
```



## fetch

可看为传统Ajax的升级版，更简单

**基于Promise语法**

```js
fetch('URL').then(function(data){
  return data.text();
}).then(function(result){
  console.log(result)
})
```

其中的 `text()`方法是fetch的API，返回的是个Promise实例对象

然后用Promise的 `then()`方法返回结果



### fetch请求参数

method ：HTTP请求方法默认是GET

body ： HTTP请求参数

headers ：HTTP请求头

---

### GET请求（传统方式）

一般的写法是：

```js
fetch("URL?key=value&key=value").then(data=>{
  return data.text()
}).then(result=>{
  console.log(result)
})
```

---

如下：

```js
fetch('http://localhost:3000/get?name=andy&age=28').then(function(data) {
    return data.text()
}).then(function(result) {
    console.log(result);
});
```

```js
// 服务端路由
app.get('/get', (req, res) => {
    res.send(req.query)
})
```



### GET请求（restful）

如下：

模拟get请求获取图书的ID

```js
fetch('http://localhost:3000/books/10000', {
    method: 'get'
}).then(function(data) {
    return data.text()
}).then(function(result) {
    console.log(result);
    console.log(parseInt(result));
})
```

```js
// 服务端路由
app.get('/books/:id', (req, res) => {
    res.send(req.parmas.num)      //{id:1000}
})
```

请求地址是 

> localhost:3000/books/1000





### DELETE请求

如下：

模拟delete请求获取图书的ID

```js
fetch('http://localhost:3000/books/10000', {
    method: 'delete'
}).then(function(data) {
    return data.text()
}).then(function(result) {
    console.log(result);
})
```

```js
// 服务端路由
app.delete('/books/:id', (req, res) => {
    res.send(req.parmas.num)
})
```

请求地址是 

> localhost:3000/books/1000





### POST请求

除了method属性设置请求方式

还要用body设置请求体和headers设置请求头

headers支持两种数据格式类型：

#### application/json 格式的数据

若POST提交的参数是JSON对象格式

如下：

```js
fetch('http://localhost:3000/books', {
    method: 'post',
    body: JSON.stringify({
        name: 'andy',
        age: 28
    }),
    headers: {
        'Content-Type': 'application/json'
    }
}).then(function(data) {
    return data.text()
}).then(function(result) {
    console.log(JSON.parse(result));
  	console.log(JSON.parse(result).name);
})
```

```js
// 服务端路由
const bodyParser = require('body-parser');

app.use(bodyParser.json());

app.post('/books', (req, res) => {
    res.send(req.body)
})
```

#### application/x-www-form-urlencoded 格式的数据

若POST提交的参数是键值对格式

如下：

```js
fetch('http://localhost:3000/books', {
    method: 'post',
    body:'name=andy&age=28',
    headers: {
        'Content-Type': 'application/x-www-form-urlencoded'
    }
}).then(function(data) {
    return data.text()
}).then(function(result) {
    console.log(result);
})
```

```js
// 服务端路由
const bodyParser = require('body-parser');

app.use(bodyParser.urlencoded());

app.post('/books', (req, res) => {
    res.send(req.body)
})
```



### PUT请求

若PUT提交的参数是JSON对象格式

如下：

```js
fetch('http://localhost:3000/books', {
    method: 'put',
    body: JSON.stringify({
        name: 'andy',
        age: 28
    }),
    headers: {
        'Content-Type': 'application/json'
    }
}).then(function(data) {
    return data.text()
}).then(function(result) {
    console.log(JSON.parse(result));
  	console.log(JSON.parse(result).name);
})
```

```js
// 服务端路由
const bodyParser = require('body-parser');

app.use(bodyParser.json());

app.put('/books', (req, res) => {
    res.send(req.body)
})
```



### 响应结果

text() ：将返回体处理成字符串类型

json() ：将返回体处理成JSON类型，和JSON.parse(responseText)一样

```js
fetch('URL').then(function(data){
  return data.json();
}).then(function(result){
  console.log(result)
})
```

```js
// 服务端路由
app.get('/books', (req, res) => {
    res.json({
      name:'andy',
      age:'28'
    })
})
```

---

如下：

```js
fetch('http://localhost:3000/data').then(function(data) {
    return data.text()
}).then(function(result) {
    console.log(result);    //字符串类型
});

fetch('http://localhost:3000/data').then(function(data) {
    return data.json()
}).then(function(result) {
    console.log(result);    //JSON对象类型
});

fetch('http://localhost:3000/data').then(function(data) {
    return data.text()
}).then(function(result) {
    console.log(JSON.parse(result));    
});
```

```js
// 服务端路由
app.get('/data', (req, res) => {
    res.send({ 'name': 'Andy' })
});
```





## axios

是**专门用来调用后台接口**的JS库

是基于Promise语法的HTTP客户端

比fetch更强大

- 可支持浏览器和node.js

- 支持Promise语法
- 拦截器，可拦截请求和响应
- 自动转换JSON数据



### 导入

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```



### 基本用法

参数函数的参数是个对象，响应的结果是在参数的data属性中

```js
axios.get('URL').then(function(res){
  // 成功时的结果
  console.log(res.data);
},function(err){
  // 错误时的结果
  console.log(res.data);
})
```

如下：

用axios自带的 `get()` 方法发送get请求

```js
axios.get('http://localhost:3000/axios').then(function(res) {
    console.log(res.data);
}, function(err) {
    console.log(err);
})
```

```js
// 服务端路由地址
app.get('/axios', (req, res) => {
    res.send('hello Axios')
})
```





### GET请求参数

通过拼接URL请求地址传递参数

```js
axios.get('http://localhost:3000/get?name=andy&age=28')
  .then(function(res) {
    console.log(res.data);
})
```

```js
// 服务端路由地址
app.get('/axios', (req, res) => {
    res.send(req.query)
})
```



### GET请求参数（params）

通过axios的params选项传递参数（restful）

**推荐**

```js
axios.get('http://localhost:3000/get', {
    params: {
        name: 'andy',
        age: 28
    }
}).then(function(res) {
    console.log(res.data);   // {name: "andy", age: "28"}
})
```

```js
// 服务端路由地址
app.get('/axios', (req, res) => {
    res.send(req.query)
})
```



### GET请求参数（restful）

通过URL地上 / 拼接路径传参

```js
axios.get('http://localhost:3000/axios/1000')
  .then(function(res) {
    console.log(res.data);   //{id:1000}
})
```

```js
//服务端路由
app.get('/axios/:id', (req, res) => {
    res.send(req.params)
})
```





### DELELT请求参数

三种方式，和get相同

```js
axios.delete('http://localhost:3000/get', {
    params: {
        name: 'andy',
        age: 28
    }
}).then(function(res) {
    console.log(res.data);
})
```

```js
// 服务端路由地址
app.delete('/axios', (req, res) => {
    res.send(req.query)    // {name: "andy", age: "28"}
})
```





### POST请求（JSON）

通过选项传递参数

默认是传递json格式的数据给后台

```js
axios.post('http://localhost:3000/post', {
    name: 'andy',
    pwd: 1212
}).then(function(res) {
    console.log(res.data);   // {name: "andy", pwd: 1212}
})
```

```js
// 服务端路由地址
const bodyParser = require('body-parser');
app.use(bodyParser.json())

app.post('/post', (req, res) => {
    res.send(req.body)
})
```



### POST请求（表单格式）

通过 **`URLSearchParams`** 传递参数

```js
var params = new URLSearchParams();
params.append('键'，'值');
params.append('键'，'值');
params.append('键'，'值');
axios.post('URL',params).then(function(result){
  console.log(result.data)
})
```

如下：

```js
var params = new URLSearchParams();
params.append('name', 'andy');
params.append('pwd', '1313');
axios.post('http://localhost:3000/post', params).then(function(result) {
    console.log(result.data);   //{name: "andy", pwd: 1212}
})
```

```js
//服务器路由地址
const bodyParser = require('body-parser');
app.use(bodyParser.urlencoded())

app.post('/post', (req, res) => {
    res.send(req.body)
})
```





### PUT请求（JSON）

类似POST请求

推荐

```js
axios.put('http://localhost:3000/put', {
    name: 'andy',
    pwd: 1212
}).then(function(res) {
    console.log(res.data);
});
```

```js
//服务器路由地址
const bodyParser = require('body-parser');
app.use(bodyParser.json())

app.put('/put', (req, res) => {
    res.send(req.body)
})
```



### PUT请求（表单格式）

通过 **`URLSearchParams`** 传递参数

```js
var params = new URLSearchParams();
params.append('键'，'值');
params.append('键'，'值');
params.append('键'，'值');
axios.put('URL',params).then(function(result){
  console.log(result.data)
})
```

如下：

```js
var params = new URLSearchParams();
params.append('name', 'andy');
params.append('pwd', '1313');
axios.put('http://localhost:3000/post', params).then(function(result) {
    console.log(result.data);   //{name: "andy", pwd: 1212}
})
```

```js
//服务器路由地址
const bodyParser = require('body-parser');
app.use(bodyParser.urlencoded())

app.put('/post', (req, res) => {
    res.send(req.body)
})
```



### 响应结果

#### 响应结果的主要属性

**data**：实际响应结果的数据

​			一般是JSON格式的数据

**headers**：响应头信息

​			一般是{'Content-Type':'XXX/XXX;charset=utf-8'}

**status**：响应状态码

​			响应成功的正常状态是200

**statusText**：响应状态信息

​			响应成功的正常状态是 'ok'

``` js
axios.get('http://localhost:3000/axios')
  .then(function(res) {
    console.log(res);
    console.log(res.data);
    console.log(res.headers);
    console.log(res.status);
    console.log(res.statusText);
}, function(err) {
    console.log(err);
})
```





### 全局配置

- **设置响应超时时间**

如果响应耗时超过设定时间，就认为出错

```js
axios.defaults.timeout = 3000;
```

- **设置默认地址**

```js
axios.default.baseURL = 'http://localhost:3000'
```

如下：

```js
axios.default.baseURL = 'http://localhost:3000';

axios.get('test001')
  .then(function(res) {
    console.log(res.data);
});

axios.get('test002')
  .then(function(res) {
    console.log(res.data);
});
```

- **设置请求头**

```js
axio.default.headers['mytoken'] = 'hello'
```

```js
//服务端设置 运行跨域访问该服务
app.use('*',function(req,res,next){
   res.header('Access-Control-Allow-Origin','*')
   res.header('Access-Control-Allow-Methods','GET,POST,DELETE,PUT,OPTIONS');
   res.header('Access-Control-Allow-Headers','X-Requested-With');
   res.header('Access-Control-Allow-Headers','Content-Type');
  res.header('Access-Control-Allow-Headers','mytoken');
})
```



### axios请求拦截器

在**请求发出前设置一些信息**

通过参数 `config `

```js
axios.interceptors.request.use(function(config){
  
  // 设置在请求发出前的信息设置
  
  return config
},function(err){
  // 响应出错时的错误信息
});

axios.get('URL').then(function(result){
  console.log(result)
})
```

如下：

通过参数 `config `**获得请求地址**，可用来判断请求地址

```js
axios.interceptors.request.use(function(config) {

    console.log(config.url);   // http://localhost:3000/axios

    return config
});

axios.get('http://localhost:3000/axios').then(function(result) {
    console.log(result.data);
})
```

再比如：

在请求发出前**设置请求头**

```js
axios.interceptors.request.use(function(config) {

    config.headers.mytoken = 'hello'

    return config
});

axios.get('http://localhost:3000/axios').then(function(result) {
    console.log(result.data);
})
```

再比如：

发送请求前**设置token**

token用来给用户做标记

```js
axios.interceptors.request.use(function(config){
  config.params.token = 1;
})
```



### axios响应拦截器

在响应的数据返回前对数据进行加工处理

通过参数 `config `

```js
axios.interceptors.response.use(function(result){
  
  // 设置在请求发出前的信息设置
  
  return result;  //----被then接受
},function(err){
  // 响应出错时的错误信息
});

axios.get('URL').then(function(result){
  console.log(result)
})
```

如下：

通过响应拦截器，先获得并返回响应数据data

下面的`axios.then`中的参数的函数的参数就成为了上面拦截器返回的数据

不需要 `.data`了

```js
axios.interceptors.response.use(function(result) {

    var data = res.data

    return data
});

axios.get('http://localhost:3000/axios').then(function(result) {
    console.log(result);
})
```







```js
import axios from 'axios';

const instance = axios.create({
  	// 接口
    baseURL: "http://kumanxuan1.f3322.net:8001/index/index",
    //超时
  timeout: 5000
});

// 请求拦截器
instance.interceptors.request.use(config => {
    // 每次请求发送前都执行
  
    return config;
}, err => {
    return Promise.reject(err)
});

// 响应拦截器
instance.interceptors.response.use(result => {
    // 每次接受响应前都执行，然后再执行回调函数then
  
    return result;
}, err => {
    return Promise.reject(err)
});

export default instance;
```