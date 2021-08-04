## 组件创建

- 函数创建（函数组件）
- 类创建（类组件）



### 函数组件

为了区分函数组件的和其他函数

- 函数名的**首字母大写**

- 必须要有 JSX结构作为函数**返回值**

  若没有定义返回值，会报错

  若不需要返回值，也要写上null，不会渲染任何内容

```react
const HelloComponent = () => (
  	<div>Hello Component</div>
  )
}

const NullComponent = () => null
```

使用组件时，组件名作为标签名

组件名必须大写，不然组件调用时会被当作HTML一般标签渲染（当然会报错）

可以双标签，可以单标签

```react
ReactDOM.render(
  <div>

    <HelloComponent></HelloComponent>
    
    <HelloComponent/>

  </div>,
  document.getElementById('root')
)
```



### 类组件

通过ES6的class创建的组件

为了区分类组件的和其他类

- 类组件名的**首字母大写**

- 必须要**继承 React.Component父类**，从而使用其提供的方法和属性
- 组件中必须要有 **render( ) 方法**
- render( ) 方法必须要有**return返回值**，返回 JSX结构

同函数组件一样，若不想返回JSX结构就返回一个null（正经人不会这么做）

```react
import React from 'react';

class Hello extends React.Component {
  render() {
    return (
      <div>Hello</div>
    )
  }
}
```

使用组件时，组件名作为标签名

组件名必须大写，不然组件调用时会被当作HTML一般标签渲染（当然会报错）

可以双标签，可以单标签

```react
ReactDOM.render(
  <div>

    <Hello />

  </div >,
  document.getElementById('root')
)
```



### 抽离独立组件文件

每个组件应该被抽离到单独的JS文件中

- 每个文件中必须 **import导入React和React.Component**

- 每个组件必须要被 **export default导出**，以供其他使用

```react
import React, { Component } from 'react'

export default class Hello extends Component {
    render() {
        return (
            <div>
                HelloComponent 
            </div>
        )
    }
}
```

使用时，import导入组件的文件后使用该组件

```react
import React from 'react';
import ReactDOM from 'react-dom';


import Hello from './components/Hello.jsx'

ReactDOM.render(
  <div>

    <Hello/>

  </div>,
  document.getElementById('root')
)
```









## 事件处理

### 事件绑定

通过on+事件名，并且驼峰命名法

```js
on事件名称 = { 处理函数 }
```

#### 类组件

```jsx
on事件名称 = { this.类组件的方法 }
```

```react
import React, { Component } from 'react'

export default class Hello extends Component {

    click() {
        console.log('Clicked');
    }

    mouse() {
        console.log('Went througt');
    }



    render() {
        return (
            <button
                onClick={ this.click }
                onMouseLeave={ this.mouse }
            >点击</button>
        )
    }
}
```

---

#### 函数组件

```react
on事件名称 = { 函数组件的方法 }
```

```react
const Hello = () => {
  function click() {
    console.log('Clicked');
  }
  function mouse() {
    console.log('Went through');
  }
  return (
    <button
      onClick={click}
      onMouseLeave={mouse}
    >点击</button>
  )
}


ReactDOM.render(
  <div>
    	<Hello/>
  </div >,
  document.getElementById('root')
)
```



### 事件的触发

```js
on事件名称 = { 方法 }
// 触发了相应事件后才调用

on事件名称 = { 方法() }
// 在刚绑定到了虚拟DOM 上时就调用，后面不会被触发
```



### 事件对象 event

在事件内部可直接通过事件的参数获取事件对象event

不用通过虚拟DOM节点调用事件时传入，事件定义时就可获取

```react
import React, { Component } from 'react'

export default class Hello extends Component {

    click(e) {
        console.log('Clicked', e);
    }

    render() {
        return (
            <button onClick={this.click}>
            	点击
          	</button>
        )
    }
}
```

React的已经封装好，事件对象兼容任意浏览器

---

#### 阻止事件默认行为

如下：阻止a连接的跳转行为

```react
import React, { Component } from 'react'

export default class Hello extends Component {

    click(e) {
        // console.log('Clicked', e);
        e.preventDefault();
        console.log('阻止了默认行为');
    }

    render() {
        return (
            <a href="https://www.baidu.com/"
                onClick={this.click}
            >点击</a>
        )
    }
}
```





### this指向

以类组件为例子

类组件的特点是有状态 state数据的存在，[详见状态组件 无状态组件]()

类组件的方法也多用来操作**类本身自己的数据**



虚拟DOM调用类中方法函数时会出现this指向的各种问题，主要围绕：

- 方法中 **this是否指向类本身**
- 方法能否直接使用 `this.state` 来获取类本身的状态 state
- 方法能否直接使用 `this.方法` 来操作类本身



但虚拟DOM直接调用类中方法函数时，

类方法中 this指向 **undefined**，并不是指向类自己本身，如下：

```react
import React, { Component } from 'react'

export default class Hello extends Component {

    state = { num: 0 }

    fn() {
        console.log(this);		// undefined
    }

    render() {
        return (
            <div>
                <button onClick={this.fn}>点击</button>
            </div>
        )
    }
}
```

因为函数的this执行该函数的调用者，

虚拟DOM直接调用类中方法时，该方法并不是当前被类组件所调用 ，

从而导致在方法中无法获取当前类本身的状态数据和操作类本身的数据，

所以需要在调用方法时**修改 this的指向**：

---

#### 1. 箭头函数调用类实例方法

可利用箭头函数特点，调用修改方法的this

不是让DMO直接调用方法，而是让DMO**通过一个箭头函数去调用类的实例方法**

> 箭头函数本身不绑定this，箭头函数的this指向所在作用域的this，
>
> 该箭头函数是在 render() 中，所以this指向render() 作用域中的this，
>
> render() 方法是由当前类组件调用执行渲染的，
>
> 所以**箭头函数中的this最终指向了当前类组件**，
>
> 虚拟DMO通过该箭头函数来调用类中方法时，**成了由当前这个类组件调用方法**，
>
> 所以此时方法中的this是指向当前类，即可以对当前类进行操作

```react
import React, { Component } from 'react'
export default class Hello extends Component {

    fn() {
        console.log(this);
    }

    render() {
        return (
            <div>
                <button onClick={ this.fn }>   {/* undefined */}
                  普通调用
            		</button>

                <button onClick={() => this.fn()}>   {/* 当前类组件 */}
                  通过箭头函数调用
            		</button>
            </div>
        )
    }
}
```

---

#### 2.  bind( )

可利用ES5的bind方法，直接修改函数的this指向

**Function.prototype.bind()**

可在每次虚拟DMO调用方法时修改指向

```react
export default class Hello extends Component {

    fn() {
        console.log(this);
    }

    render() {
        return (
            <div>
                <button onClick={this.fn.bind( this )}>
                  普通调用
            		</button>
            </div>
        )
    }
}
```

或，预先在类组件的constructor构造器中定义该方法的指向

```react
export default class Hello extends Component {

    constructor(){
        super()
        this.fn = this.fn.bind(this)
    }

    fn() {
        console.log(this);
    }

  
    render() {
        return (
            <div>
                <button onClick={ this.fn }>
                  普通调用
            		</button>
            </div>
        )
    }
}
```

---

#### 3.  箭头函数定义类的实例方法

上述两种方法是在虚拟DMO调用类组件方法时修改调用者的this指向

也可在定义类组件方法时，**直接以箭头函数的形式定义该类组件的方法函数**

因为这个方法自身就是个箭头函数，该方法的this永远指向所在的当前类组件

虚拟DOM调用该方法时不会因为调用者的this从而导致该方法中this的指向

**也是推荐的做法**

```react
export default class Hello extends Component {

    fn = () => {
        console.log(this);
    }


    render() {
        return (
            <div>
                <button onClick={ this.fn }>
                  普通调用
            		</button>
            </div>
        )
    }
}
```







### 事件参数











## 状态组件 无状态组件

组件除了根据定义方式区分，

还可根据是否有  **状态 state**  分为：

- 有状态组件（类组件）

- 无状态组件（函数组件）

函数组件没有自己的数据，只负责展示静态JSX结构

类组件有自己的数据，通过操作数据变化动态更新UI，实现页面动态变化

一般是将这两种组件相互结合来实现应用的复杂功能和展示









## state

**状态 state** 即数据，是组件（类组件）内部的私有数据，

是组件自身的数据，只能在该组件内部自己使用

一个组件可有多个数据，state的 **值是对象**



### 定义状态

state定义在类组件的 **constructor构造函数中**

#### 构造函数中

```react
constructor() {
  super();
  
  this.state = {
    num: 100
  }

}
```

如下：

```react
import React, { Component } from 'react'

export default class Hello extends Component {

    constructor() {
        super();

        this.state = {
            num: 100
        }
    }

    render() {
        return (
            <div></div>
        )
    }
}
```

---

#### ES6属性简写

也可以使用ES6的属性转化语法，简写：

```react
import React, { Component } from 'react'

export default class Hello extends Component {

    state = {
        num: 100
    }

    render() {
        return (
            <div></div>
        )
    }
}
```



### 使用

在类组件中通过 **this.state** 获取该组件的状态state对象

setState() 自动实现了state状态的修改，然后自动渲染页面更新UI（数据驱动视图）

```react
import React, { Component } from 'react'

export default class Hello extends Component {

    state = {
        num: 100
    }

    render() {
        return (
            <div>{this.state.num}</div>
        )
    }
}
```



### 修改状态

修改该组件state中的数据的值不能直接被修改，必须通过 **this.setState( ) 方法** 

以对象的键值对形式写入要修改的数据和修改后的值

```react
this.setState({
  state中的数据: 修改后的值
})
```

```react
import React, { Component } from 'react'

export default class Hello extends Component {

    state = {
        num: 0
    }

    render() {
        return (
            <div>
                <div>数值：{this.state.num}</div>

                <button onClick={() => {
                    this.setState({ 
                      num: this.state.num + 1 
                    })
                }}> +1 </button>
            </div>
        )
    }
}
```

**注意：**

JSX 结构主要是展示视图，逻辑处理最好单独放入类方法

注意，若通过调用类组件中的方法来修改类中状态state的数据时，则调用方法时必须 **修改 this的指向**，不然在方法中调用this.setState( ) 时，会因为this为 undefined 而报错

详见 [修改 this指向 的3种方法]()

```react
import React, { Component } from 'react'

export default class Hello extends Component {

    state = {
        num: 0
    }

    add() {
        this.setState({
            num: this.state.num + 1
        })
    }

    render() {
        return (
            <div>
                <div>数值：{this.state.num}</div>
                <button onClick={ this.add.bind(this) }>
                  +1 
            		</button>
            </div>
        )
    }
}
```









## 表单处理

React中有两种表单处理的方式：

- 受控组件
- 非受控组件



### 受控组件

受控组件是指：

- 表单**输入值value/check的来源 **受React组件控制

  将组件状态state与表单输入值value绑定

- 表单**输入值value/check的变化** 受React组件控制

  由组件的setState方法控制管理表单输入值value的变化

  

1. 组件的状态state中定义一个数据，用来存放表单的输入值value/check

2. 然后组件状态state的该数据赋值给表单的value值

3. 给表单绑定onchange事件组件方法

4. 当表达输入值value/check变化时，通过 setState() 修改状态state中的数据



#### 文本框

通过this.state.属性、onChange事件、this.setState() 来控制表单value属性

```jsx
<input type="text" value=""/>
```

```react
export default class Hello extends Component {

    state = {
        inputVal: ''
    }

    fn = (e) => {
        this.setState({
            inputVal: e.target.value
        })
    }

    render() {
        return (
            <div>
                <input type="text"
                    value={ this.state.inputVal }
                    onChange={ this.fn }/>

                <p>{this.state.inputVal}</p>
            
            </div>
        )
    }
}
```

---

#### 富文本框

和文本框一样，

通过this.state.属性、onChange事件、this.setState() 来控制表单value属性

```html
<textarea value=""></textarea>
```

```react
export default class Hello extends Component {

    state = {
        content: ''
    }

    fn = (e) => {
        this.setState({
            content: e.target.value
        })
    }

    render() {
        return (
            <div>
                <textarea
                    value={this.state.content}
                    onChange={this.fn}
                ></textarea>
            </div>
        )
    }
}
```

---

#### 下拉选框

也和文本框一样，

通过this.state.属性、onChange事件、this.setState() 来控制表单value属性

```html
<select value="">
  <option value=""></option>
</select>
```

```react
export default class Hello extends Component {

    state = {
        selectVal: 'lili'		// 默认选中 lili
    }

    fn = (e) => {
        this.setState({
            selectVal: e.target.value
        })
    }

    render() {
        return (
            <div>
                <select 
                  value={ this.state.selectVal } 
                  onChange={ this.fn }>
                    <option value="andy">Andy</option>
                    <option value="tom">Tom</option>
                    <option value="lili">Lili</option>
                </select>
            </div>
        )
    }
}
```

---

#### 复选框

通过this.state.属性、onChange事件、this.setState() 来控制表单check属性

```react
export default class Hello extends Component {

    state = {
        isChecked: false
    }

    fn = (e) => {
        this.setState({
            isChecked: e.target.checked
        })
    }

    render() {
        return (
            <div>
                <input type="checkbox" name="sex" 
                  checked={this.state.isChecked} 
                  onChange={this.fn} />
            </div>
        )
    }
}
```

---

#### 多表单统一处理

- 根据事件对象的表单属性区分表单类型，

  复选框的输入属性名为checked，其余文本框的输入属性名为value

- 建议为了方便区分：

  将组件状态state中的数据名分别和表单name属性的值统一

  将组件状态state中的数据分名别和表单value/checked属性的值统一

```react
export default class Hello extends Component {

    state = {
        text:'',
        textarea:'',
        select:'',
        checkbox: false
    }

    fn = (e) => {
        const value = e.target.type === 'checkbox'
            ? e.target.checked
            : e.target.value;
        this.setState({
            [e.target.name]: value
        });
    }

    render() {
        return (
            <div>
                <input type="text" 
                  	name="text"
                    value={this.state.text}
                    onChange={this.fn} /><span>{this.state.inputVal}</span>
                <hr />

                <textarea 
                  	name="textarea"
                    value={this.state.textarea}
                    onChange={this.fn}
                ></textarea>
                <hr />

                <select 
                  	name="select"
                    value={this.state.select}
                    onChange={this.fn}>
                    <option value="andy">Andy</option>
                    <option value="tom">Tom</option>
                    <option value="lili">Lili</option>
                </select>

                <hr />
                <input type="checkbox" 
                  	name="checkbox"
                    checked={this.state.checkbox}
                    onChange={this.fn} />
            </div>
        )
    }
}
```





### 非受控组件

使用 **ref** 从DOM节点上获取表单的输入值

不太常用

这是通过操作DMO的方式获取数据，React更推荐的还是受控组件的方式

```react
export default class Hello extends Component {
    constructor(){
        super()
				// 创建ref对象
        this.val = React.createRef()
    }

    fn=()=>{
      	// 获得DMO节点input
        console.log(this.val.current);
      
      	// 获取获得DMO节点input的value属性的值
        console.log(this.val.current.value); 
    }

    render() {
        return (
            <div>
                <input type="text" 
                  ref={this.val} />
                <button onClick={this.fn}>获取value</button>
            </div>
        )
    }
}
```







## 组件通信

组件之间共有 3种 通讯方式：

- 父组件——>子组件
- 子组件——>父组件
- 兄弟组件——>兄弟组件

---

### 父 —> 子

1. 父组件的状态state中的数据

2. 将数据以标签属性方式传入父组件内的子组件

3. 子组件内通过 **props属性** 获取对象形式的数据

[详见props]()

---

### 子 —> 父

通过**回调函数**的形式，只要子组件调用就会传递

1. 父组件内定义一个函数

2. 父组件将该函数传入子组件，仅传入不调用

3. 子组件内通过 this.props 调用父组件传递进来的该函数

   **常常通过事件触发该回调函数**

4. 子组件内将自身状态state中的数据以参数形式传入该函数

5. 父组件传入的函数内接受参数来获取子组件传入的数据，

   再通过 this.setState 将数据存入父组件的状态中，

   完成传递

如下：

子组件通过onClick事件触发父组件传入的getData函数，

并将数据自身状态state的name作为参数传入该函数

父组件内接收到并通过setState存入自身的状态，完成子传父

```react
class Son extends React.Component {
  state = {
    name: 'andy'
  }

  send = () => {
    this.props.getData(this.state.name)
  }
  
  render() {
    return (
      <div>
        <button onClick={this.send}>发送</button>
      </div>
    )
  }
}


class Father extends React.Component {
	state = {
    name: ''
  }

  getData = (data) => {
   	// console.log("show data in Father",data);
    this.setState({
      name: data
    })
  }
  
  render() {
    return (
      <div>	
        <Son getData={this.getData} />
      
        <p>来自子组件的数据：{this.state.name}</p>
      </div>
    )
  }
}
```

---

### 兄弟 —> 兄弟

React中通过**状态提升**

1. 将兄弟组件的共享的状态传递到最近的公共父组件中，

2. 公共父组件管理这个共享状态，并提供操作该状态的方法

3. 子组件通过props接受状态和操作方法

```react
class Father extends React.Component {
  state = {
    num: 100
  }
  add = () => {
    this.setState({
      num: this.state.num + 1
    })
  }
  render() {
    return (
      <div>
        <Child01 add={this.add} />
        <Child02 num={this.state.num} />
      </div>
    )
  }
}


const Child01 = (props) => {
  return (
    <button onClick={props.add}> +1 </button>
  )

}

const Child02 = (props) => {
  return (
    <h3>计算结果：{props.num}</h3>
  )
}
```











## props

用于接受**父组件传递来**的数据

### 传入子组件

以**标签属性的形式**，从父组件将任意类型数据传入子组件

- 字符串
- 数值
- 数组
- 对象
- 函数
- JSX结构

除了字符串类型的数据可直接传入以外，其余数据表达式需要使用 **{ }** 

```react
<Father>
		<Son 
    	name = "andy"
      age = {29}
      arr = { [1, 2, 3] }
      obj = { { a: 1, b: 2 } }
      fun = { () => { console.log('hello'); } }
   	/>	
</Father>
```



### 子组件接受

在子组件内props属性的数据以对象形式被接受

props属性在子组件内是只读属性，不能被修改

---

#### 函数组件 接受props

在函数组件中直接通过 **props** 获取父组件传入的数据

作为函数组件的参数

```react
function Son(props) {
  
  console.log(props);		//{name="andy",age:29}
  
  return (
    <div>
      <div>{props.name}</div>
      <div>{props.age}</div>
    </div>
  )
}


ReactDOM.render(
  <div>
			<Son 
      	name="andy" 
      	age={29}
    	/>

  </div >,
  document.getElementById('root')
)
```

---

#### 类组件 接受props

在类组件中通过 **this.props **获取父组件传入的数据

```react
class Son extends React.Component {
  
  render() {
    console.log(this.props);	//{name="andy",age:29}
    
    return (
      <div>
        <div>{this.props.name}</div>
        <div>{this.props.age}</div>
      </div>
    )
  }
}

ReactDOM.render(
  <div>
			<Son
      	name="andy"
      	age={29}
    	/>
  </div >,
  document.getElementById('root')
)
```

若类组件中有 constructor构造函数，

必须将props作为参数传递给 **super()方法**（用于继承父类的数据）

否则无法在该类组件中使用props属性，props会成为undefined

```react
class Son extends React.Component {
  constructor(props){
    super(props)
    console.log(this.props);	//{name="andy",age:29}
  }
  
  render() { 
    return (
      <div>
        <div>{this.props.name}</div>
        <div>{this.props.age}</div>
      </div>
    )
  }
}

ReactDOM.render(
  <div>
			<Son
      	name="andy"
      	age={29}
    	/>
  </div >,
  document.getElementById('root')
)
```









## context

context用于**跨组建传递数据**

虽然父子组件通讯时使用props，

但若嵌套层级特别深，props逐层传递会太过繁琐（正经人不这么用）

此时使用context实现跨组件传递数据

1. 调用React.createContext()

   创建 Provider和Consumer组件，分别用于提供数据和使用数据

2. 使用Provider组件作为父节点









## 生命周期





render-props

高阶组件
