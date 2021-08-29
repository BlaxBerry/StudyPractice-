# Vue-cli（脚手架）

脚手架

![](https://reffect.co.jp/wp-content/uploads/2019/06/vue_tue_cli.png)

[Vue-cli中文文档](https://cli.vuejs.org/zh/)

## 一、环境配置

确保Node.js 已经被安装

```bash
node --version

npm --version
```

### 安装脚cli手架

```bash
npm install -g @vue-cli
```

```bash
vue --version
@vue/cli 4.5.12
```



## 二、 项目创建 与 运行

### 1. 创建

在指定的目录下创建项目文件夹

```bash
cd XXX
vue create 项目名
```

```bash
vue create web
```

### 2. 项目配置

创建项目后回弹出选择配置的界面，

可以自行配置或选择默认 **Default( babel, eslint )**

```bash
Vue CLI v4.5.12
? Please pick a preset: (Use arrow keys)
❯ Default ([Vue 2] babel, eslint) 
  Default (Vue 3 Preview) ([Vue 3] babel, eslint) 
  Manually select features 
```

自行配置时，空格键选择，enter回车键下一步

然后会自动下载相应配置的内容到项目

### 3. 开启服务 运行项目 

按着流程配置完后，

在项目目录下运行下列代码，即可运行项目

```bash
cd XXX
npm run serve
```

加载编译后会提示创建项目成功，

并提示**项目端口号 `localhost：8080`**

```bash
 DONE  Compiled successfully in 2739ms                                3:56:10 AM


  App running at:
  - Local:   http://localhost:8080/ 
  - Network: http://192.168.1.2:8080/

  Note that the development build is not optimized.
  To create a production build, run npm run build.

```

即表明项目创建成功

浏览器访问`http://localhost:8080/`后会显示界面

<img src="https://junpeko.com/wp-content/uploads/2018/06/vue_eye.png" style="zoom: 50%;" />



## 二、目录结构

```js
XXX
|--node_modules
|--public
		|--index.html
		|--favicon.ico
|--src
		|--assets
		|--components
|--App.vue
|--main.js
|--package.json
|--package-lock.json
```

- **public目录**：可以替换favicon图标

- **src目录**：写代码的地方
  - **assets目录**：存放静态资源（图片）
  - **components目录**：公用组件
  - **views目录**：视图组件

- **main.js文件**：整个项目的入口文件

```js
import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false

new Vue({
  render: h => h(App), //把App组件替换渲染到id=“app”的节点上
}).$mount('#app')
```

- **App.vue**：顶级组件

是个单文件组件

里面包含了`模版 <template\>`，`JS <script>`，`样式 <style\>`

```vue
<template>
	<div id="app">
    <自定义组件名 :自定义属性=“数据”></自定义组件名>
  </div>
</template>

<script>
  //引入
  // import XXX from "XXX"
  import 自定义组件名 from "./components/组件文件.vue"
  
  //注册
  export default {
    name:"",
    data(){
      数据:值
    },
    components:{
      引入的自定义组件名
    }
  }
</script>
```

组件文件.vue

```vue
<template>
	<div id="app">
    <自定义组件名></自定义组件名>
  </div>
</template>

<script>
  
  export default {
    name:自定义组件名,
    props:[自定义属性名]
  }
</script>
```





## 更改项目配置 vue.config.js

在vue-cli 脚手架项目目录下（和 `package.json文件`同级）

创建一个**`vue.config.js` 文件**

在里面写项目相关的配置（ webpack）

更改后会覆盖原来的配置

使用CommonJS的写法

---

### port 启动端口

比如，更改vue脚手架项目的启动端口：

```js
module.exports = {
  devServer:{
    port: XXXXX
  }
}
```

更改完后，重新启动

```bash
cd 项目目录
npm run serve
```

---

### publicPath 公共路径（项目前缀）

默认情况下，vue脚手架会默认应用部署在一个域名的根目录上，

比如：`http://www.my-app.com/`

所以如果应用是备部署在一个字路径上时，会导致路径出错

需要手动指定该子路径

比如：`http://www.my-app.com/子路径名/`

所以需要在 **`vue.config.js` 文件** 中修改部署应用包时的基本URL

```js
module.exports= {
  publicPath:'/子目录名/'
}
```

但是会导致开发时访问项目，必须要在路径添加上子目录

会写起来麻烦点

可通过**环境变量**判断

node环境变量 process.env,NODE_ENV用来区分开发环境和生存环境

npm run build 的话环境变量是 production

npm run serve 的话环境变量是development

```js
process.env.NODE_ENV === 'production'?'/子目录名/':'';
```

```js
module.exports= {
  publicPath: process.env.NODE_ENV === 'production'?'/子目录名/':'';
}
```

比如用 `npm run build` 打包后的html文件内的导入的CSS、JS文件的路径

需要手动修改文件路径

修改最终的生成的各个文件的依赖地址

```js
module.exports= {
  publicPath:'./'
}
```

---

### proxy 代理

只要协议、域名、端口号，又一个不一致就会出现跨域问题，

导致无法请求

**尤其是开发环境时**，前端测试后端的端口

上线后基本不会出现跨域情况

如下：

后端在192.168.1.200 的P地址上写了接口，在3000端口上

前端的IP地址是192.168.1.30，启动的端口是8080，

此时就会有跨域，需要设置代理

```js
moudle.exports = {
  devServer:{
    proxy:{
      '/api':{
        target:'http:192.168.1.200:3000'
      }
    }
  }
}
```

```

```









































## 打包

npm run bulid

最终打包出的目录是dist目录

只需提交 dist文件夹即可



![](https://reffect.co.jp/wp-content/uploads/2020/04/vue_webpack-1024x585.png)