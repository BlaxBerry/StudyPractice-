# Vue组件 Components

![](https://img10.360buyimg.com/uba/jfs/t17725/89/1051890999/28032/d2a32f5e/5ab8fe1aN70a87b81.png)

组件主要特点体现在各个组件的组合、重用

尽可能把页面内容拆成一个个的独立可复用的小组件

比如把头部、导航，底部等部分拆分，组合

[TOC]



## 全局组件

### 注册

```js
Vue.component('组件名',{
  data:function(){
    return {
      组件数据名:值
    }
  },
    template:组件模版内容
})
```

如下：

注册一个demo组件，并在页面中使用

```html
<div id="app">
  <demo></demo>
  
  <div>
    <demo></demo>
  </div>
  
</div>

<script>
        Vue.component("demo", {
            template: "<h2>i am H2 tag</h2>"
        })
        let app = new Vue({
            el: "#app",

        })
</script>
```

再如下：

注册一个按钮组件，并添加点击事件

```html
<div id="app">
  <btn-counter></btn-counter>
</div>

<script>
 Vue.component('btn-counter'，{
		 data:function(){
  			 return {
           num：0
         }
		 }，
     template：'<button @click="num++">点击了 {{num}}）次</button>'
 });
  
  new Vue({
    el:'#app'
  })
</script>
```



### 组件内部属性

#### data属性

**data是个函数**，用**return**返回对象形式的数据，数据是键值对形式

虽然Vue实例也是个组件，但Vue实例对象不同的是，

Vue实例的data属性是个对象，

而组件的data是个函数，返回值是个对象

```js
Vue.component('helloComponent',{
	data:function(){
    return {
      message:'hello',
      arr:[1,2,3]
    }
  }
})
```

---

#### template属性

**template 是字符串形式的模版**

建议使用ES6的模版字符串，使模版结构清晰

- **模版必须是单一的根元素**，不能是同级标签

（不能是并列的兄弟元素，只能是单个元素，但可以嵌套多个同级的子元素）

```js
Vue.component('hello', {
  template: `
            <div>
                <h3>hello the 2sd</h3>
                <demo></demo>
                <demo></demo>
            </div>`
});
```

- template模版可以直接使用组件的data属性中的数据

```js
Vue.component('hello', {
  data:function(){
    return {
      str:'hello',
      arr:['tom','jerry']
    }
  },
  template: '<h1>{{str}} {{arr[0]}} and {{arr[1]}}</h1>'
});
```

---

#### 其他属性

组件内部也可以写其他的属性，比如methods等属性

```js
Vue.component("demo", {
  data: function() {
    return {
      count: 0
    }
  },
  template: `
            <div>
                <button @click="handle">clicked {{count}} times</button>
            </div>
            `,
  methods: {
    handle: function() {
      this.count++
    }
  }
});
```



### 组件命名

#### 短横线

推荐

```js
Vue.component('my-component',{ })
```

#### 驼峰命名

不推荐

只能用于其他组件的字符串模版拼接

不能直接作为根组件标签使用到页面，

不然会报错

驼峰命名法命名的组件在页面中使用时可写成短横线格式

```js
Vue.component('myComponent',{ })
```





### 组件使用

全局组件要写入一个容器，不能直接在页面中使用，不然不会被解析，

且该容器必须被Vue实例对象挂载

组件使用时直接在页面中以标签的形式使用组件，

标签名就是组件名，如果注册时时驼峰命名，可在使用时写为短横线

```html
<body>
  <div id="app">
    <组件名1></组件名1>
    <组件名2></组件名2>
  </div>
  
  <script>
  	new Vue({
      el:"#app"
    });
    
    Vue.component('组件名1'{ })；
    Vue.component('组件名2'{ })
  </script>
</body>
```

---

#### 组件复用

组件可以重复使用 ，

每个组件的数据都是独立互不影响的

- **复用在同一容器内**

```html
<div id="app">
        <h3>hello the 1st</h3>
        <demo></demo>
        <demo></demo>
        <hello></hello>
</div>
<script>
  Vue.component("demo", {
    data: function() {
      return {
        count: 0
      }
    },
    template: `
					<div>
						<button @click="handle">clicked {{count}} times</button>
  				</div>
            `,
    methods: {
      handle: function() {
        this.count++
      }
    }
  });
  
	new Vue({
    el: "#app",
  })
</script>
```

- **复用在不同容器**

全局组件想被多个容器使用时，多个容器都要挂载Vue

即给所有需要使用的容器都绑定Vue实例

如下：在容器`container11` 和`container22`中都是用全局组件`hello`

需要先给两个容器都挂载上Vue，分定绑定Vue实例对象

```html
<body>
    <div id="container11">
        <hello></hello>
    </div>

    <div id="container22">
        <hello></hello>
    </div>

    <script>
        Vue.component('hello', {
            data: function() {
                return {
                    str: 'hello',
                    arr: ['tom', 'jerry']
                }
            },
            template: '<h1>{{str}} {{arr[0]}} and {{arr[1]}}</h1>'
        });

        new Vue({
            el: "#container11"
        })

        new Vue({
            el: "#container22"
        })
    </script>
</body>
```

---

#### 组件嵌套

一个组件中可以包含其他组件

如下：hello组件中嵌套了demo组件

```html
<div id="app">
  
        <hello></hello>
  
</div>

<script>
  Vue.component("demo", {
    template: `
					<div>
						<button @click="handle">click</button>
  				</div>
            `
  });

  Vue.component('hello', {
    template: `
            <div>
                <h3>hello the 2sd</h3>
                <demo></demo>
                <demo></demo>
            </div>`
     });

	new Vue({
    el: "#app",
  })
</script>
```









## 局部组件

### 注册  components属性

在Vue实例对象的 **`components属性 `**中注册局部组件对象

和全局组件相同

data属性是个函数，用retutn返回对象形式的数据，

template属性是字符串形式的模版

命名规则也是短横线或驼峰，页面中也是只能用短横线格式

```js
new Vue({
  el:'#app',
  components:{
    组件名:{
      data:function(){
    		组件数据名:值
  		},
  		template:组件模版
    }
  }
})
```

也可以在外部定义组件，实例对象内直接调用，是代码整洁

```js
  var componentA = {
  	data:function(){ return {} },
  	template:''
 	}
	var componentB = {
  	data:function(){ return {} },
  	template:''
	}

	new Vue({
  	el:'#app',
  	components:{
    	'component-A':componentA,
    	'component-B':componentB
  	}
	})
```

如下

```html
<div id="app">
  <component-A></component-A>
  <component-B></component-B>
</div>
<script>
  var componentA = {
  	data:function(){ 
    	return {
        msg:'hello Tom'
      }},
  	template:'<h1>{{msg}}</h1>'
 	};
  
	var componentB = {
  	data:function(){ 
    	return {
        msg:'hello Jerry'
      }},
  	template:'<h1>{{msg}}</h1>'
	};

	new Vue({
  	el:'#app',
  	components:{
    	'component-A':componentA,
    	'component-B':componentB
  	}
	})
</script>
```

---

### 和全局组件的区别

全局组件可在任何挂载了Vue的容器内使用

```html
<body>
    <div id="container11">
        <hello></hello>
    </div>

    <div id="container22">
        <hello></hello>
    </div>

    <script>
        Vue.component('hello', {
            data: function() {
                return {
                    str: 'hello',
                    arr: ['tom', 'jerry']
                }
            },
            template: '<h1>{{str}} {{arr[0]}} and {{arr[1]}}</h1>'
        });

        new Vue({
            el: "#container11"
        })

        new Vue({
            el: "#container22"
        })
    </script>
</body>
```

局部组件只能在当前组件的父组件中使用，

否则会报错找不到组件，不会被页面渲染

```html
<body>
    <div id="container11">
        <My-component></My-component>
      	<My-component></My-component>
    </div>

    <div id="container22">
        <My-component></My-component>   <!--没渲染-->
        <My-component></My-component>
    </div>

    <script>
        new Vue({
            el: '#container11',
            data: {
                str: 'Vue instance'
            },
            components: {
                'MyComponent': {
                    data: function() {
                        return {
                            str: 'component'
                        }
                    },
                    template: '<h1>{{str}}</h1>'
                }
            }
        })
    </script>
</body>
```

---

### data的数据

局部组件不能直接使用定义在Vue实例对象的data中的数据，

局部组件只能使用来自组件内部的data中的数据

验证如下：

`Vue实例对象中的data`内定义一个字符串数据 str，值是 `Vue instance`，

`局部组件内的data`也定义一个字符串数据 str，值是 `component`

组件模版用插值语法插入str，

最终页面渲染的是组件自己的data属性中的数据 str `component`

不是Vue实例对象的data的数据

```html
<body>
    <div id="app">
        <My-component></My-component>
    </div>
    <script>
        new Vue({
            el: '#app',
            data: {
                str: 'Vue instance'
            },
            components: {
                'MyComponent': {
                    data: function() {
                        return {
                            str: 'component'
                        }
                    },
                    template: '<h1>{{str}}</h1>'
                }
            }
        })
    </script>
</body>
```



## 父子组件

components属性中定义父组件,

在父组件的components属性再定义子组件，

子组件模版写在父组件的模版中

```js
components:{
  'father':{
    template:`
              <div>
		              <h2>i am father</h2>
		              <child></child>
              </div>
		`,
    components:{
      'child':{
        template:'<h3>i am child</h3>'
      }
    }
  }
}
```

如下：

```html
<body>
    <div id="app">
        <father></father>
    </div>
    <script>
        new Vue({
            el: '#app',
            components: {
                "father": {
                    data: function() {
                        return {
                            msg: 'i am father'
                        }
                    },
                    template: `
                    <div>
                        <h2>{{msg}}</h2>    
                        <child></child>
                    </div>
                    `,
                    components: {
                        "child": {
                            data: function() {
                                return {
                                    msg: 'i am child'
                                }
                            },
                            template: '<h2>{{msg}}</h2>'
                        }
                    }
                }
            }
        })
    </script>
</body>
```

---

#### <template\></template\>

也可以写入`<template></template>` 标签

`<template></template>` 标签不会在页面中显示

如下：

```html
<body>
    <div id="app">
        <father></father>
    </div>

    <template id="father">
        <div>
            <h2>i am father</h2>
            <child></child>
        </div>
    </template>

    <script>
        new Vue({
            el: '#app',
            components: {
                "father": {
                    data: function() {
                        return {
                            msg: 'i am father'
                        }
                    },
                    template: '#father',
                    components: {
                        "child": {
                            data: function() {
                                return {
                                    msg: 'i am child'
                                }
                            },
                            template: '<h2>{{msg}}</h2>'
                        }
                    }
                }
            }
        })
    </script>
</body>
```





## 组件间的数据交互  

因为组件只能用该组件自己的data属性中定义的数据，

不能直接用其父组件中的数据

---

### 父组件 —> 子组件

#### props属性值 和 模版的自定义属性

1. **子组件标签上通过自定义属性绑定父组件的数据**

   数据有两种：静态和动态，动态数据用 `v-bind`

```html
<!--v-bind绑定动态数据-->
<子组件 :自定义属性名='数据名'></子组件>

<!--静态数据-->
<子组件 自定义属性名=‘hello’></子组件>
```

2. **然后在子组件对象通过props属性获得这个自定义属性**

   用数组方式获取该自定义属性

   即在子组件内拿到了传入的父组件的数据

```js
//局部组件
components:{
  子组件:{
    props:['自定义属性名']，
    template:'<div>{{自定义属性名}}</div>'
  }
}

//全局组件
Vue.component('组件名',{
  props:['自定义属性名']，
  template:'<div>{{自定义属性名}}</div>'
})
```

如下：

若子组件想使用父组件内的数据str和info，

先把数据作为子组件child的自定义属性，用v-bind写入标签

然后在子组件内通过props属性，用数组方式获取该自定义属性

然后就可以在子组件内使用父组件的数据了

```html
<!--局部组件中的父传数据给子-->
<body>
    <div id="app">
        <child :str='str' :info='info' ></child>
    </div>

    <script>
        new Vue({
            el: '#app',
            data: {
                msg: 'i am father',
                str: 'hello'，
              	info: 'from father'
              
            },
            components: {
                "child": {
                    props: ['str','info'],
                    data: function() {
                        return {
                            msg: 'i am child'
                        }
                    },
                    template: '<h1>{{msg}},{{str}},{{info}}</h1>'
                }
            }
        })
    </script>
</body>
```

```html
<!--全局组件中的父传数据给子-->
<body>
    <div id="app">
        <child :str='str' :info='info'></child>
    </div>

    <script>
        Vue.component('child', {
            props: ['str','info'],
          	data:function(){
              return {
                msg:'i am child'
              }
            },
            template: '<h1>{{msg}},{{str}},{{info}}</h1>'
        });

        new Vue({
            el: '#app',
            data: {
                msg: 'i am father',
                str: 'hello',
                info:'from father'
            }
        })
    </script>
</body>
```

---

#### 传递多个数据（属性）

传递时数据可以是多个，作为数组的多个元素即可

```js
props:['自定义属性名','自定义属性名',....]
```

```html
<子组件 :自定义属性名='Vue数据名' 自定义属性名='静态数据'></子组件>
```

---

#### 数据和自定义属性的命名

因为JavaScript规则，在props数组中接受的的属性名只能是驼峰命名

因为HTML标签不区分大小写，页面标签中的属性名只能是短横线格式

但是在字符串的模版中，可以使用短横线格式的属性名

```html
<!--局部组件中的父传数据给子-->
<body>
    <div id="app">
        <child :child-str='str' :child-info='info' ></child>
    </div>

    <script>
        new Vue({
            el: '#app',
            data: {
                msg: 'i am father',
                str: 'hello'，
              	info: 'from father'
              
            },
            components: {
                "child": {
                    props: ['childStr','childInfo'],
                    data: function() {
                        return {
                            msg: 'i am child'
                        }
                    },
                    template: '<h1>{{msg}},{{child-str}},{{child-info}}</h1>'
                }
            }
        })
    </script>
</body>
```

---

#### props属性值类型

- 字符串型数据
- 数值型数据
- 数值型数据
- 对象型数据
- 布尔型数据

自定义属性绑定的布尔值和数值数据若不是用v-bind设定给子组件的模版的话，

子组件内props属性接受到的都是字符串

所以除了写死的静态数据外，绑定自定义属性时必须用v-bind绑定

```html
<body>
    <div id="app">
        <child :str='str' string-num='12' :num='12' string-bool=true :bool=true :arr='arr' :obj='obj'></child>
    </div>

    <script>
        new Vue({
            el: '#app',
            data: {
                str: 'hello',
                arr: [1, 2, 3, 'A', 'B'],
                obj: {
                    name: 'andy',
                    age: 28
                }
            },
            components: {
                "child": {
                    props: ['str', 'stringNum', 'num', 'stringBool', 'bool', 'arr', 'obj'],
                    template: `
                        <div>
                            <div>{{str + '----' + typeof str}}</div>
                            <div>{{stringNum + '----' + typeof stringNum}}</div>
                            <div>{{num + '----' + typeof num}}</div>
                            <div>{{stringBool+ '----' + typeof stringBool}}</div>
                            <div>{{bool+ '----' + typeof bool}}</div>

                            <ul>
                                <li :key='index' v-for="item,index in arr">{{index}}--{{item}}</li>    
                            </ul>

                            <div>
                                <span>{{obj.name}}</span>   
                                <span>{{obj.age}}</span>     
                            </div>
                        </div>
                    `
                }
            }
        })
    </script>
</body>
```



![](https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=4256217190,2885724795&fm=11&gp=0.jpg)



### 子组件——>父组件

#### 直接修改props获得的数据（不推荐）

子组件可以直接操作父组件的数据，虽然不报错，**但是不推荐**

不建议不推荐直接在子组件内修改通过props获取的父组件的数据

```html
<!--先把父组件的数据，一个数组传递给子组件
子组件v-for遍历数组
并子组件内绑定一个事件，改变数组数据，并传递回 父组件-->

<body>
    <div id="app">
        <h3>array in father: {{arr}}</h3>
        <child :arr='arr'></child>
    </div>

    <script>
        new Vue({
            el: "#app",
            data: {
                arr: ['apple', 'pear', 'banana']
            },
            components: {
                'child': {
                    props: ['arr'],
                    template: `
                        <div>
                            <ul>
                                <li v-for='item in arr'>{{item}}</li>    
                            </ul>
                            <button @click='add'>click to add</button>    
                        </div>
                    `,
                    methods: {
                        add() {
                            this.arr.push('lemon')
                        }
                    }
                }
            }
        })
    </script>
</body>
```

因为**props传递数据的原则是单项的**

仅允许父组件的数据传递给子组件，但是子组件不能直接操作修改到父组件，

不然会容易出现逻辑错误

所以不建议不推荐直接在子组件内修改通过props获取的父组件的数据

应该通过 `$emit`在子组件内操作父组件的数据



#### $emit() 

若想在子组件内操作父组件的数据，需要通过自定义事件，

原理可理解为触发子组件的事件，转换成触发父组件的事件，从而修改父组件的数据，

所以需要通过子组件内的`$emit() `

1. **子组件内注册自定义事件**

   在template中给组件用`v-on `事件去绑定`$emit(自定义事件名)`自定义事件

```js
Vue.component('子组件名',{
  template:`<子组件 v-on:事件类型='$emit(“自定义事件”)'></子组件>`
})
```

```js
Vue.component('子组件名',{
  template:`<子组件 v-on:事件类型='子组件事件名'></子组件>`,
  methods:{
    子组件事件名(){
      this.$emit(自定义事件名)
    }
  }
})
```

2. **父组件中的子组件模版通过自定义事件绑定父组件的方法**

   通过`$emit`注册的自定义事件去触发父组件中定义的方法

   从而实现通过自组件触发事件去操作父组件的数据

```html
<父组件>
  <子组件 v-on:自定义事件=‘父组件的方法’></子组件>
</父组件>
```

可理解为：

**子组件触发自定义事件——（$emit子组件自定义事件）———父组件事件——修改父组件数据**

**从而实现通过触发子组件自身的事件去执行父组件的事件，从而修改父组件的数据**

如下：

用Vue实例对象模拟父子组件，Vue实例对象也是个组件，所以挂载Vue的容器可视为父组件

在子组件的 `template` 中通过`v-on:click` 绑定 `$emit()` 自定义事件；

在父组件中给子组件的模版通过该自定义事件`enlarge-size`绑定修改父组件数据父组件的事件`handle`；

即可通过子组件的自定义事件操作父组件事件从而操作父组件的数据`fontSize`了

```html
<body>
    <div id="app">
        <div :style="{fontSize: fontSize +'px'}">from father---{{msg}}</div>
        <child @enlarge-size='handle'></child>
    </div>

    <script>
        Vue.component('child', {
            template: `
						<button @click="$emit('enlarge-size')">click fontSize +1</button>
						`
        });
      
        new Vue({
            el: '#app',
            data: {
              	msg: 'hello',
                fontSize: 14
            },
            methods: {
                handle() {
                    this.fontSize += 1
                }
            }
        })
    </script>
</body>
```

```html
<body>
    <div id="app">
        <div :style="{fontSize: fontSize +'px'}">from father---{{msg}}</div>
        <child @enlarge-size='handle'></child>
    </div>

    <script>
        Vue.component('child', {
            template: `
						<button @click="fun">click fontSize +1</button>
						`,
            methods: {
                fun() {
                    this.$emit('enlarge-size')
                }
            }
        });

        new Vue({
            el: '#app',
            data: {
                msg: 'hello',
                fontSize: 14
            },
            methods: {
                handle() {
                    this.fontSize += 1
                }
            }
        })
    </script>
</body>
```







#### $event

 `$emit()` 除了可通过自定义事件去触发父组件事件，还可以通过第二个参数把数据传出，

数据传出后需要传入父组件的事件，从而实现在父组件内操作子组件的数据

过程如下：

1. **子组件通过参数传出数据**

   在template 中通过`$emit(自定义事件名,参数)` 触发自定义事件，

   并通过参数把子组件的数据/或静态固定数据传出

```js
Vue.component('子组件名',{
  template:`<子组件 v-on:事件类型='$emit(“自定义事件”,静态数据)'></子组件>`
})
```

```js
Vue.component('子组件名',{
  data:function(){
    return {
      数据:值
    }
  }
  template:`<子组件 v-on:事件类型='子组件事件名'></子组件>`,
  methods:{
    子组件事件名(){
      this.$emit(自定义事件名, this.数据)
    }
  }
})
```

2. **$event 接受传递的数据**

   给子组件模版通过注册的自定义事件绑定父组件的事件时，

   通过`$event`接受自组件传递来的数据，并作为参数传入父组件事件方法函数，

   即可实现把子组件的数据传入父组件，父组件事件操作子组件数据

```html
<父组件>
  <子组件 v-on:自定义事件=‘父组件的事件($event)’></子组件>
</父组件>
```

可理解为：

**子组件触发自定义事件——($emit(自定义事件,数据) 传出数据)——父组件事件——（$event接受数据）——修改父组件数据**

自组件传出数据，并在在通过自定义事件实现子组件事件触发父组件事件时，

把数据传入父组件事件，从而实现子组件数据传入父组件

如下：

```html
<body>
    <div id="app">
        <div :style="{fontSize: fontSize +'px'}">from father---{{msg}}</div>
        <child @enlarge-size='handle($event)'></child>
    </div>

    <script>
        Vue.component('child', {
            template: `
						<div>
                <button @click="$emit('enlarge-size',1)">click fontSize +1</button>
						    <button @click="$emit('enlarge-size',10)">click fontSize +10</button>    
            </div>
						`
        })
        new Vue({
            el: '#app',
            data: {
                msg: 'hello',
                fontSize: 14
            },
            methods: {
                handle(val) {
                    this.fontSize = val
                }
            }
        })
    </script>
</body>

```

```html
<body>
    <div id="app">
        <div :style="{fontSize: fontSize +'px'}">from father---{{msg}}</div>
        <child @enlarge-size='handle($event)'></child>
    </div>

    <script>
        Vue.component('child', {
            data: function() {
                return {
                    num: 30
                }
            },
            template: `<button @click="fun">click fontSize +1</button>`,
            methods: {
                fun() {
                    this.$emit('enlarge-size', this.num)
                }
            }
        });

        new Vue({
            el: '#app',
            data: {
                msg: 'hello',
                fontSize: 14
            },
            methods: {
                handle(val) {
                    this.fontSize = val
                }
            }
        })
    </script>
</body>
```

再比如，点击自组件改变父组件字体颜色：

```html

<body>
    <div id="app">
        <h2 :style="{color:fontColor}">{{msg}}</h2>
        <demo @change-color="handle($event)"></demo>
    </div>

    <script>
        new Vue({
            el: '#app',
            data: {
                msg: 'hello',
                fontColor: 'red'
            },
            methods: {
                handle(val) {
                   this.fontColor = val
                }
            },

            components: {
                'demo': {
                    data: function() {
                        return {
                            color: 'blue'
                        }
                    },
                    template: '<button @click="fun">change Color to Blue</button>',
                    methods: {
                        fun() {
                            this.$emit('change-color', this.color)
                        }
                    }
                }
            }
        })
    </script>
</body>
```



### 兄弟组件——>兄弟组件

兄弟组件之间的数据交互需要通过一个**`事件中心`** 来实现

#### 事件中心

通过自定义事件，组件把自己链接到事件中心（监听）

然后通过组件自己的事件，触发同样链接到事件中心的兄弟组件的自定义事件（触发）

在链接（监听）的同时接受数据，并操作自身

实现当前组件操作兄弟组件的数据

![](https://img-blog.csdnimg.cn/20200920234839544.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0tvbmdLb25nX1JhYw==,size_16,color_FFFFFF,t_70)



1. **声明一个全局事件中心**

   通过new一个单独的Vue实例对象作为事件中心

```js
var hub = new Vue()
```

2. **用事件中心对组件的自定义事件进行监听**

   在当前组件内的生命周期钩子函数`mounted`中，通过事件中心监听组件自身的自定义事件

   相当于把组件自己的自定义事件存放入事件中心

   第二个参数是个函数，

   函数可通过参数接受兄弟组件传来的兄弟组件的数据，

   在函数内结合获得的来自兄弟组件的数据，对当前组件自身数据进行操作

```js
mounted:function(){
  hub.$on('当前组件的自定义事件名'，(兄弟组件传递来的数据)=>{
    this.数据 //操作逻辑
	})；
}
```

3. **通过当前组件的事件触发兄弟组件的自定义事件**

   给当前组件绑定事件，在事件呢通过通过事件中心`$emit()`

   触发被事件中心监听了的兄弟组件的事件名，即存让入事件中心的兄弟组件的自定义事件

   并可通过参数把自身数据传出给兄弟组件

```js
template:`<当前组件 v-on:事件类型=“当前组件的事件名”></当前组件>`,
methods:{
  当前组件的事件名(){
    hub.$emit('兄弟组件的自定义事件名'，传递给兄弟组件的this.数据)
  }
}
```

4. 用事件中心对自定义事件进行销毁

   等兄弟组件的自定义事件完成任务后，可在Vue实例对象中通过事件中心对组件的自定义事件进行销毁

```js
hub.$off('指定组件的自定义事件名称')；
```

---

如下：

分别有 `Tom`， `Jerry `两个组件，

点击各自组件的按钮，操作兄弟组件的数据

1. 声明一个全局事件中心

2. 分别在两个兄弟组件中的`mounted`属性中，通过事件中心`hub.$on()`监听自身的自定义事件,

   接受兄弟组件传入的数据，并修改自身数据

3. 分别在两个兄弟组件的template模版中，绑定自身组件的methods属性的事件，

4. 分别在两个兄弟组件中的methods属性中，

   通过事件中心`hub.$emit()`链接触发兄弟组件的自定义事件，并传出数据

5. 该例子把销毁事件定义在Vue实例中，也就是两个子组件的父组件中

   销毁两个组件的自定义事件

```html
<body>
    <div id="app">
        <Tom></Tom>
        <hr>
        <Jerry></Jerry>
    </div>

    <script>
        const hub = new Vue();

        new Vue({
            el: "#app",

            components: {
                'Tom': {
                    data: function() {
                        return {
                            num: 0,
                            addNum: 10
                        }
                    },
                    template: `
                    <div>
                        <div>Tom: {{num}}</div>
                        <button @click="handle">Jerry's number +10</button>
                    </div>
                    `,
                    mounted: function() {
                        hub.$on('TomsEvent', (val) => {
                            this.num += val
                        })
                    },
                    methods: {
                        handle() {
                            hub.$emit('JerrysEvent', 10)
                        }
                    }
                },

                'Jerry': {
                    data: function() {
                        return {
                            num: 0
                        }
                    },
                    template: `
                    <div>
                        <div>Jerry: {{num}}</div>
                        <button @click="handle">Tom's number +20</button>
                    </div>
                    `,
                    mounted: function() {
                        hub.$on('JerrysEvent', (val) => {
                            this.num += val
                        })
                    },
                    methods: {
                        handle() {
                            hub.$emit('TomsEvent', 20)
                        }
                    }
                }
            }
        })
    </script>
</body>
```

```html
<body>
    <div id="app">
        <Tom></Tom>
        <Jerry></Jerry>

        <hr>
        <button @click='handle'>销毁事件</button>
    </div>

    <script>
        //事件中心
        var hub = new Vue();

        Vue.component('Tom', {
            data: function() {
                return {
                    num: 0
                }
            },
            template: `
            <div>
                <div>Tom {{num}}</div>
                <button @click='addJerryNum'>number of Jerry +10</button>
            </div>
            `,
            mounted: function() {
                //监听事件
                hub.$on('Tom-event', (val) => {
                    this.num += val
                })
            },
            methods: {
                addJerryNum() {
                    //触发事件中心监听的兄弟组件的事件，并传递数据
                    hub.$emit('Jerry-event', 10)
                }
            }
        });

        Vue.component('Jerry', {
            data: function() {
                return {
                    num: 0
                }
            },
            template: `
            <div>
                <div>Jerry {{num}}</div>
                <button @click='addTomNum'>number of Jerry +20</button>
            </div>
            `,
            mounted: function() {
                hub.$on('Jerry-event', (val) => {
                    this.num += val
                })
            },
            methods: {
                addTomNum() {
                    hub.$emit('Tom-event', 20)
                }
            }
        });

        new Vue({
            el: "#app",
            methods: {
                handle() {
                    hub.$off('Tom-event');
                    hub.$off('Jerry-event');
                }
            }
        })
    </script>
</body>
```





## 组件插槽

页面中可能多个地方会用到相同布局，就需要通过组件来实现复用，

但不同位置的相同组件会需要部分地方的内容不同，

此时需要用 **插槽 solt标签** 用来决定组件显示的内容

插槽可理解为一个占位符预留位置，通过插槽预留待填充的空白位

**在子组件中定义插槽，在父组件中的子组件标签位置添加代码修改插槽内容**

从而实现复用相同组件的同时有部分不一样的内容

---

###  <slot\></slot\> 插槽标签

1. **用`<slot\></slot\>`标签在组件中定义插槽**

   子组件的template中给模版添加`<slot\></slot\>`标签

   可理解为预留等待填充标签的空位

```js
Vue.component('子组件名',{
  template:`
			<div>
					<slot></slot>
			</div>
	`
})
```

2. **父组件中给子组件的标签内添加内容**

   该内容会反映到自组件模版中的`<slot\></slot\>`标签内

```html
<父组件>
  <子组件>添加内容</子组件>
</父组件>
```

如下：

```html
<body>
    <div id="app">
        <child>i am content</child>
    </div>

    <script>
        Vue.component('child', {
            template: `
            <div>
                <h3>Hello</h3>
                <h3>i am child</h3>   
                <slot></slot> 
                <slot></slot> 
            </div>`
        })

        new Vue({
            el: "#app"
        })
    </script>
</body>
```



### 插槽默认内容

没在父组件若有给子组件的标签内添加内容，则`<slot\></slot\>`使用默认内容

若父组件给子组件的标签内添加了内容，则`<slot\></slot\>`使用添加的内容

```html
<body>
    <div id="app">
        <child>i am content</child>

        <hr>
        <child></child>
    </div>

    <script>
        Vue.component('child', {
            template: `
            <div>
                <h3>Hello</h3>
                <h3>i am child</h3>
                <slot>i m default</slot> 
            </div>`
        })

        new Vue({
            el: "#app"
        })
    </script>
</body>
```



### 具名插槽

就是具有名字的插槽

子组件中的slot插槽可写多个，

但是在父组件中给子组件标签内写入内容的话，子组件中的插槽则会全被改为写入的内容

所以需要用名字来区分各个slot插槽

1. **子组件内用`name`属性给`<slot></slot>`插槽加名称**

   在子组件模版中可以定义多个插槽，用name区分

```js
Vue.component('子组件名',{
  template:`
			<div>
					<slot name="插槽名"></slot>
					<slot name="插槽名"></slot>
			</div>
	`
})
```

2. 父组件中给一个标签添加v-slot指令和指定插槽名

   用name名称匹配子组件模版中的插槽

   把在父组件中写入子组件标签的内容匹配填充到指定插槽中

```html
<父组件>
  <子组件>
  	<任意标签 slot=”插槽名“></任意标签>
    <任意标签 slot=”插槽名“></任意标签> 
  </子组件>
</父组件>
```

如下：

指定匹配了名称的插槽内容，别填充到指定插槽中

没有指定插槽名的内容，填充到默认插槽中

```html
<body>
    <div id="app">
        <demo-component>
            <h1 slot="top">顶部标题</h1>

            <p>内容</p>
            <p>内容</p>

            <h1 slot="bottom">底部结尾</h1>
        </demo-component>
    </div>

    <script>
        Vue.component('demo-component', {
            template: `
            <div class="container">

                <header>
                    <slot name="top"></slot>
                </header>
                   
                <main>
                    <slot></slot>    
                </main>

                <footer>
                    <slot name="bottom"></slot>
                </footer>
            </div>`
        })

        new Vue({
            el: "#app"
        })
    </script>
</body>
```

最终页面DOM结构如下：

```html
<div id="app">
  <div class="container">
    <header>
      <h1>顶部标题</h1>
    </header>
    <main>
      <p>内容</p>
      <p>内容</p>
    </main>
    <footer>
      <h1>底部结尾</h1>
    </footer>
  </div>
</div>
```

但是有个不足，

把插槽名添加到标签中的话是一个标签只能对一个插槽，

如果想在插槽中填充多个标签

可在父组件中通过`<template\></template\>` 



### <template\></template\>标签

用于包裹多条内容

1. 在父组件的子组件标签内注册`<template\></template\>` 标签

2. 给`<template\></template\>` 添加slot属性和插槽名，

```html
<父组件>
  <子组件>
    <template slot="插槽名">
    	<任意标签></任意标签> 
      <任意标签></任意标签> 
      <任意标签></任意标签> 
    </template>  
  </子组件>
</父组件>
```

3. 把`<template\></template\>` 的内容填充给匹配的子组件模版中的插槽

```js
Vue.component('子组件名',{
  template:`
			<div>
					<slot name="插槽名"></slot>
					<slot name="插槽名"></slot>
			</div>
	`
})
```

`<template\></template\>` 标签不会在页面中显示，

只是其内容被充填给了指定名称的子组件插槽

如下：

```html
<body>
    <div id="app">
        <demo-component>
            <template slot="top">
                <h1>顶部标题</h1>
                <h3>副标题</h3>
                <h3>副标题</h3>
            </template>

            <p>内容</p>
            <p>内容</p>

            <template slot="bottom">
                <h1>底部结尾</h1>
                <h3>副标题</h3>
                <h3>副标题</h3>
            </template>

        </demo-component>
    </div>

    <script>
        Vue.component('demo-component', {
            template: `
            <div class="container">

                <header>
                    <slot name="top"></slot>
                </header>
                   
                <main>
                    <slot></slot>    
                </main>

                <footer>
                    <slot name="bottom"></slot>
                </footer>

            </div>`
        })

        new Vue({
            el: "#app"
        })
    </script>
</body>
```

最终页面DOM结构如下：

```html
<div id="app">
  <div class="container">
    <header>
      <h1>顶部标题</h1>
      <h3>副标题</h3>
      <h3>副标题</h3>
    </header>
    <main>
      <p>内容</p>
      <p>内容</p>
    </main>
    <footer>
      <h1>底部结尾</h1>
      <h3>副标题</h3>      
      <h3>副标题</h3>
    </footer>
  </div>
</div>、
```



### 作用域插槽

**父组件对子组件内容（模版结构）进行加工处理**

父组件中获得子组件的数据，在父组件中对子组件的模版内容加工处理

1. **子组件模版的slot插槽中通过`自定义属性` 传出数据**

   可以传出多个数据

```js
template:'<solt :自定义属性名=子组件的数据 :自定义属性名=子组件的数据><solt>'
```

2. **父组件中在`<templat>` 中 通过`slot-scope`指令获得子组件的数据**

   子组件传出的数据都被`slot-scope`后的自定义变量接受

```html
<父组件>
  <子组件>
    <template slot-scope="自定变量">
     	自定义变量.子组件的自定义属性
    </template>
  </子组件>
</父组件>
```

如下：

子组件获得父组件的数据，`v-for`动态生成模版，

父组件中控制指定的子组件的模版样式，`id==2` 的字体变为红色

子组件通过 `自定义属性="数据"` 把数据每一个对象`item`传出

父组件`template`标签中通过 `slot-scope="slotProps"`获得传出数据

子组件的数据`item`在 `slotProps.自定义属性`中,

即`v-if=“slotProps.info.id==2”`

```html
<style>
  .watch {
    font-weight: 700;
    color: red;
  }
</style>

<body>
    <div id="app">
        <demo :list="list">
            <template slot-scope='slotProps'>
                <div v-if="slotProps.info.id==2" class="watch">{{slotProps.info.name}}</div>
            </template>
        </demo>
    </div>

    <script>
        new Vue({
            el: "#app",
            data: {
                list: [{
                    id: 1,
                    name: 'apple'
                }, {
                    id: 2,
                    name: 'orange'
                }, {
                    id: 3,
                    name: 'banana'
                }]
            },
            components: {
                "demo": {
                    props: ["list"],
                    template: `
                   
                        <ul>
                            <li :key="item.id" v-for="item in list">
                                <slot :info="item">{{item.name}}</slot>
                            </li>
                        </ul>    
                    
                    `
                }
            }
        })
    </script>
</body>
```

再比如：

子组件通过`props`接受父组件的数据，并根据数据通过`v-for`动态遍历生成html结构

通过`slot插槽`预留待填充的空白位

用`<button></button>` 来充填插槽内容

然后给按钮绑定事件，但是此时

因为HTML结构是组件生成的，所以在父组件内无法直接修改和获得组件结构数据

所以需要通过子组件的自定义属性传出数据

传出来子组件的`item` 和`index `两个数据

父组件内的子组件中，再用`<template></template>`标签包裹住填充的内容

再给作为填充内容的按钮通过`slot-scope`指令获得子组件传出的数据

把获得的数据作为参数，传入父组件的事件方法函数

从而实现操作子组件的结构

```html
<body>
    <div id="app">
   
        <demo :students="students">
            <template slot-scope="slotProps">
                <button @click='edit(slotProps)'>get info</button>
            </template>
        </demo>

    </div>

    <script>
        new Vue({
            el: "#app",
            data: {
                students: [{
                    name: 'Andy',
                    age: 28
                }, {
                    name: 'Tom',
                    age: 18
                }, {
                    name: 'James',
                    age: 25
                }],
            },
            methods: {
                edit(val) {
                    console.log(val.index, val.item.name, val.item.age);
                }
            },
            components: {
                "demo": {
                    props: ['students'],
                    template: `
                        <table>
                            <thead>
                                <tr>
                                    <th>Name</th>
                                    <th>Age</th>   
                                    <th>Option</th>       
                                </tr>    
                            </thead>
                            <tbody>
                                <tr v-for="item,index in students" :key="index">
                                    <td>{{item.name}}</td>
                                    <td>{{item.age}}</td>   
                                    <td><slot :item="item" :index="index"></slot></td>       
                                </tr>    
                            </tbody>
                        </table>
                    `
                }
            }
        })
    </script>
</body>
```





## 单文件组件

后缀名是 **.vue**的文件`XXX.vue`

里面可以同时写入结构、逻辑、样式

**<template\> **标签写结构

**<script\>** 标签写逻辑

**<style\> **标签写样式

template里必须有**一个根节点**

script里的**data**写成函数，return返回一个对象

可是用**import** 导入样式

```vue
<template>
	<div id="app"></div>
</template>

<script>
  import "./css/base.css"
  
  export default {
    data(){
      return {
        
      }
    },
    methods{}
  }
</script>

<style>
  #app {
    font-size:20px
  }
</style>
```

可通过VSCode的插件**vetur**,

<img src="https://vuejsexpo.com/content/images/2020/02/vetur.png" style="zoom:33%;" />

在文件中仅输入`vue`即可快速生成结构



### 导入第三方包 与使用

单文件组件导入第三方的包（模块、库）时，

先定位到文件所在目录，然后下载相关的包

```bash
cd XXX

npm install axios
```

**import** 导入下载的包

```js
import axios from "axios"
```

```vue
<template>
	<div id="app">
    <button @click=ask></button>
  </div>
</template>

<script>
  import axios from "axios"
  
  export default {
    methods{
    	ask(){
        axios.get("XXXXXX",{
          params:{
            key:value
          }
        }).then(res=>{
          concole.log(res.data)
        })
      }
  }
  }
</script>

<style>
  #app {
    font-size:20px
  }
</style>
```



### 组件 抽取与导入

在项目下创建components目录文件夹，将组件都写入其中

需要组件的.vue文件中用import导入组件，

在vue的components属性中注册组件，

然后在需要的地方使用组件名的标签即可

```vue
<template>
	<div id="app">
    
    <AAA></AAA>
    
    <AAA></AAA>
    <BBB></BBB>
    
  </div>
</template>

<script>
  import AAA form "@/components/AAA.vue";
  import BBB form "@/components/BBB.vue"
  
  
  export default {
    data(){
      return {}
    },
   components:{
     // AAA:AAA
     AAA,
     BBB
   }
  }
</script>

<style>
  #app {
    font-size:20px
  }
</style>
```





## 快速原型开发

就是打开一个单文本组件**.vue**文件

### 环境配置

安装使用Node.js

```bash
node -v

npm -v
```

下载小工具

使得能开启服务器运行单文本组件**.vue**

```bash
npm install -g @vue/cli-service-global
```



### 开始服务器运行文件

在单文本组件**.vue**文件所在目录下，打开终端输入：

```bash
vue serve 文件名.vue
```

```bash
 DONE  Compiled successfully in 2528ms                                           下午9:07:44


  App running at:
  - Local:   http://localhost:8080/ 
  - Network: http://192.168.1.2:8080/

  Note that the development build is not optimized.
  To create a production build, run npm run build.


```

然后在浏览器输入域名`localhost:8080`，即可访问**.vue**文件







## 组件调试工具 devtools

识别页面中的Vue组件

可以更清楚的看到Vue组件的层级关系

并可以直接对Vue组件的数据进行调试

![](https://raw.githubusercontent.com/vuejs/vue-devtools/dev/media/screenshot-shadow.png)

[devtools github](https://github.com/vuejs/vue-devtools)

1. 按照说明提示克隆仓库、安装、构建

2. 打开chrome扩展页面
3. 选中开发者模式
4. 加载已解压的扩展
5. 选中`vue-devtools/packages/shell-chrome/`完成扩展的加载