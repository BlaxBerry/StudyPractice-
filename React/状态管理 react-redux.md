# react-redux

Redux是其他团队开发的，

看到Redux的需求很高，react官方就自己开发了一个插件react-redux，

可以更方便在react中使用rudux





<img src="https://pbs.twimg.com/media/E5luiTyVEAAFJ3T?format=jpg&name=medium" style="zoom:50%;" />







```bash
yarn add react-redux
```





```js
src
|- redux
		|- store
|- component
		|- index.jsx
|- container
		|- index.jsx
|- app.js
```





```js
import componentUI from '../../compnoent/index.jsx'

import store from '../../redux/store.js'

import {connect} from 'react-redux

// 创建并暴露容器组件
export default connect()(componentUI)
```





