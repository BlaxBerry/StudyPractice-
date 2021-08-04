React UI组件库



## material-ui

![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcT-tdiZZFbgiUIC6a5zw5GFaJjCr4BTvQGSv6Y59NSEhWc7hdwCIFGLtcuZDQQW1TJXBow&usqp=CAU)





## ant-design

简称antd

<img src="https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=3689263143,1043756525&fm=26&gp=0.jpg" style="zoom:%;" />

### 使用

```bash
yarn add antd
```

```react
import 'antd/dist/antd.css'
import {Button} from 'antd'

export default class Home extends Component {
    render() {
        return (
            <div id="home">
            
                <Button type="primary">Primary Button</Button>
            
            </div>
        )
    }
}
```

```react
import {WechatFilled, AppleFilled } from '@ant-design/icons';

export default class Home extends Component {
    render() {
        return (
            <div id="home">
            
                <WechatFilled />
            		<AppleFilled />
            
            </div>
        )
    }
}
```

---

### 按需引入样式

面的例子加载了全部的 antd 组件的样式，

```react
import 'antd/dist/antd.css'
```

太大了，需要按需引入

1. 引入react-app-rewired 和 customize-cra

   不通过yarn eject暴露所有配置文件的前提下修改配置

```react
yarn add react-app-rewired customize-cra
```

2. 修改package.json文件

   因为修改了脚手架的默认配置，脚手架的默认脚本命令被修改

   需要修改package.json文件中的脚本命令

   ```js
     "scripts": {
       //"start": "react-scripts start",
       "start": "react-app-rewired start",
       //"build": "react-scripts build",
       "build": "react-app-rewired build",
       // "test": "react-scripts test",
       "test": "react-app-rewired test",
       "eject": "react-scripts eject"
     },
   ```

3.  引入 babel-plugin-import 按需加载

   ```
   yarn add babel-plugin-import
   ```

5. 项目目录下新建 **config-overrides.js** 文件

   用来设置修改配置的规则

   ```js
   const { override, fixBabelImports } = require('customize-cra')
   
   module.exports = override(
     fixBabelImports('import', {
       libraryName: 'antd',
       libraryDirectory: 'es',
       style: 'css',
     }),
   );
   ```

6. 不要在组件中再引入样式了，

   ```react
   // 删除组件中的antd样式全部引入
   import 'antd/dist/antd.css'
   ```

---

### 自定义主题

最笨的方法，F12检查类名，然后手写样式区覆盖

但是，工作量太大，且会出现权重问题无法覆盖

所以应该修改antd的底层样式文件

即修改antd的样式Less文件中的样式

1. 安装 less 和 less-loader

   ```
   yarn add less less-loader
   ```

2. 修改 **config-overrides.js** 

   如下，修改antd底层样式的主题色为crimson

   ```js
   const { override, fixBabelImports, addLessLoader } = require('customize-cra')
   
   module.exports = override(
       fixBabelImports('import', {
           libraryName: 'antd',
           libraryDirectory: 'es',
           // style: 'css',
           style: true,
       }),
   
       addLessLoader({
           javascriptEnabled: true,
           // modifyVars: { '@primary-color': '#1DA57A' },
           modifyVars: { '@primary-color': 'crimson' },
       }),
   );
   ```

3. 重启脚手架

4. 会报错

   ```js
   Failed to compile
   ./node_modules/antd/es/button/style/index.less (./node_modules/css-loader/dist/cjs.js??ref--5-oneOf-8-1!./node_modules/postcss-loader/src??postcss!./node_modules/resolve-url-loader??ref--5-oneOf-8-3!./node_modules/less-loader/dist/cjs.js??ref--5-oneOf-8-4!./node_modules/antd/es/button/style/index.less)
   
   TypeError: this.getOptions is not a function
   ```

   原因是less-loader版本过高

   ```bash
   yarn remove less-loader
   
   yarn add less-loader@7.1.0
   ```

5. 重启脚手架