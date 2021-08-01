# Ruby on Rails基础

<img src="https://programan.s3.amazonaws.com/uploads/content_image/ruby_on_rails_02.jpg" style="zoom:50%;" />

Ruby on Rails 是基于Ruby的WebApp开发框架，简称 Rails

Hulu、Twitch...



## 基本使用

### 环境前提

- Ruby
- rbenv/NVM
- rails
- SQLite3 (Rails默认)
- Node.js（Rails6+必要webpacker，需要nodejs环境）
- yarn（Rails6+必要yarn）



### Rails安装

安装Ruby on Rails

```bash
gem install rails

rails -v
# Rails 6.1.4
```

mac自带系统Ruby，但会因为系统权限无法使用gem下载rails，

所以需要先通过Ruby版本管理工具rbenv安装配置ruby环境

然后才能顺利通过gem下载rails

```bash
brew install rbenv
rbenv install 2.6.6
rbenv global 2.6.6

rbenv version
# 2.6.6 (set by /Users/用户/.rbenv/version)
ruby -v
# ruby 2.6.6p146 (2020-03-31 revision 67876) [x86_64-darwin20]
```



### Rails项目创建

```bash
rails new xxx
```

或在指定目录下，创建目录同名 Rails项目应用

```bash
cd xxx
rails new .
```

---

#### 指定Rails版本

```bash
rails _版本_ new xxx
```

```bash
rails _5.2.4_ new xxx
```

---

#### 指定Railsオプション

不指定的话默认，创建的Rails项目会默认使用SQLite3 ，

可设置为使用MySQL、PostgreSQL、MongoDB等

```bash
rails new [アプリ名] [オプション各種]
```

```bash
rails new demo -d postgresql
```



### Rails项目运行

```bash
rails server
# 简写
rails s
```

```bash
cd xxx
rails s

=> Booting Puma
=> Rails 6.1.4 application starting in development 
=> Run `bin/rails server --help` for more startup options
Puma starting in single mode...
* Puma version: 5.3.2 (ruby 2.6.6-p146) ("Sweetnighter")
*  Min threads: 5
*  Max threads: 5
*  Environment: development
*          PID: 82484
* Listening on http://127.0.0.1:3000
* Listening on http://[::1]:3000
Use Ctrl-C to stop
```

---

#### 端口号

Rails项目默认开启在 3000端口

```http
http://localhost:3000
```

指定项目的开启端口号

```bash
rails server -p 端口号
```

```bash
rails server -p 8080
```





## Rails版本

- rails 4

  需要jQuery

  对JavaScript的处理通过 **sprokets**

- rails 5

  不需要jQuery，使用原生JavaScript

  对JavaScript的处理通过 **sprokets**

  选配**webpacker**（webpack为Ruby on Rails封装的）

- rails 6

  对JavaScript的处理通过**webpacker**



Rails 6 以前

JavaScript文件存放在

```bash
app
|- assets
		|- javascript
```

Rails 6+

JavaScript文件单独存放在

```bash
app
|- assets
		|- javascript
|- javascript
		|- packs
				|- 自定义.js
```

详见视图view





### 指定版本的Rails

- 查看已经**本机**中下载的不同版本的Rails

```bash
gem list rail 

*** LOCAL GEMS ***

autoprefixer-rails (10.2.5.1)
jquery-rails (4.4.0)
rails (6.1.4)
rails-dom-testing (2.0.3)
rails-html-sanitizer (1.3.0)
railties (6.1.4)
sass-rails (6.0.0)
sassc-rails (2.1.2)
sprockets-rails (3.2.2)
```

- 下载指定版本Rails到本机

```bash
gem install rails -v 版本
```

- 从本机删除指定的Rails版本

```bash
gem uninstall rails -v 要删除的版本
gem uninstall railties -v 要删除的版本
```

- 创建指定版本的rails项目

```bash
rails _版本_ new 
```

```bash
rails _5.2.4_ new demo
```

在该项目中查看Rails版本时，为创建该项目的rails的版本

出了该项目目录后查看Rails版本时，为本机全局环境下的rails版本



### 移除coffeeScript













## Rails项目目录

```bash
xxx
|- app
		|- assets # 静态资源
				｜- images
				｜- stylesheets # CSS/Scss样式
				｜- javascript	# Rail6+ 不存在
		|- javascript	# Rail6 存在
		|- controllers	# C
		|- models		# M
		|- views		# V
|- config	# 项目配置部署初始化等相关
		|- routes.rb # 路由
|- db		# 数据库移植、Schema等相关
|- bin	# rails命令
|- helper	# Rails相关复杂的辅助逻辑
|- mailers # 与发送Email相关
|- lib	# 封装的模块组件
		｜- tasks # rake自动化
|- log	# 日志
|- test	# 测试
|- public	# 静态资源（不常用，多存放于assets目录下）
|- node_modules


|- Gemfile		# 应用所需的Gem依赖包
|- Gemfile.lock # 禁止修改，Gem之间版本的依赖关系
|- package.json
|- Rakefile # rake任务
```







## 简易实例

创建一个可通过URL访问首页的建议应用实例

步骤如下：

1. 设置控制器
2. 设置控制器的方法
3. 设置控制器的方法对应的路由
4. 设置控制器的方法对应路由的模版
5. 浏览器访问对应URL

---

#### 1. 创建控制器

创建一个对应页面的控制器

在项目app目录的controllers目录中，创建控制器同名文件 

```bash
app
|-controllers
	|- 控制器名_controller.rb
```

如下：创建对应首页的控制器home文件，即  **home_controller.rb**

```bash
app
|-controllers
	|- home_controller.rb
```



#### 2. 定义控制器的方法

在对应页面的控制器文件中，定义一个控制器的动作方法（实质是个class类）

该控制器方法对应一个模版

```ruby
class 控制器 < ApplicationController
    class 方法名
    end
end
```

如下：在控制器home中，定义一个对应URL地址的方法 **index**

```ruby
class HomeController < ApplicationController
    class index
    end
end
```



#### 3. 设置路由

浏览器访问URL地址时，实质上是指向了某个控制器的某个方法对应的视图

设置一个对应某个控制器的某个方法的URL路由地址

```ruby
Rails.application.routes.draw do

  get "URL" => "控制器#方法"

end
```

如下：将控制器home中的Index方法对应的URL地址设为 **/** 

```ruby
Rails.application.routes.draw do

  get "/" => "home#index"

end
```

或如下，将rails项目的首页路由设为控制器home中的Index方法

```ruby
Rails.application.routes.draw do

  root "home#index"

end
```



#### 4. 设置模版

设置一个某控制器的某方法对应的模版

每个模版将会浏览器访问某应路由时，渲染为对应该路由的页面

在项目app目录的views目录中控制器同名目录下，创建控制器方法同名的ERB模版

```bash
app
|-views
	|- 控制器同名目录
		｜- 方法名.html.erb
```

如下：创建与控制器home的Index方法同名的ERB模版文件 **index.html.erb**

```bash
app
|-views
	|- home
		｜- Index.html.erb
```

```erb
<h1>
  欢迎到首页home
  首页路径为 /
</h1>
```



#### 5. 浏览器访问首页

请求的URL地址实质上是指向了某个控制器的某个方法对应的视图

浏览器访问了路由文件中设定的URL地址时，会被路由指派给该URL路径所对应的控制器中的对应方法，然后页面渲染出该方法所对应的模版

如下：访问Rails项目的首页 **/**

```http
localhost:3000/
```

模版被解析为HTML并渲染到页面

```html
<h1>
  欢迎到首页home
  首页路径为 /
</h1>
```











## MVC 模式

Rails采用MVC模式

**MVC**（**M**odel-**V**iew-**C**ontroller）模型-视图-控制器

- M：Model模型（来自数据库的数据）

- V：Views视图（用户界面的HTML模版）

- C：Controller控制器（应用逻辑）

<img src="https://cdn-ak.f.st-hatena.com/images/fotolife/f/frmski/20181007/20181007232634.png" style="zoom:50%;" />

请求的URL地址实质上是指向了某个控制器的某个方法对应的视图

1. 浏览器访问指定的URL地址

1. 被路由指派给对应的控制器Controller的对应方法

2. 该控制器在view目录中查询与其方法同名的ERB视图文件，找不到就抛出异常

3. 该方法获取数据库中数据模型Model，并导入对应的视图ERB模版
4. 模版渲染数据，Rails将视图模版响应返回到页面



浏览器访问URL时，

会被路由指派给该URL路径所对应的控制器Controller中的对应方法

控制器方法获取数据库中数据Model，并导入对应的ERB模版

然后页面渲染出该方法所对应的模版



请求的URL地址实质上是指向了某个控制器的某个方法对应的视图

浏览器访问了路由文件中设定的URL地址时，会被路由指派给该URL路径所对应的控制器中的对应方法，然后页面渲染出该方法所对应的模版

URL——>路由——>控制器——>方法—[获取数据导入模版]—>方法同名视图模版







## C 控制器

### 原理

- **每一个页面都对应一个控制器Controller**

- **每一个控制器的方法Action对应一个View视图模版**

在项目app目录的controllers目录中，创建控制器文件 ，并在该控制器文件中定义方法

控制器Controller实质上是个class类，所有控制器都继承了ApplicationController

控制器的动作方法实质方法函数

```ruby
class 控制器名 < ApplicationController
  def 方法名
  end
  def 方法名
  end
end
```



### 创建 控制器 + 定义方法

#### 1. 手动创建

在项目app目录的controllers目录中，创建控制器同名文件 

并在该控制器文件中定义方法

```bash
app
|-controllers
	|- 控制器名_controller.rb
```

```ruby
class 控制器 < ApplicationController
    def 方法名
    end
end
```

如下：

在项目app目录的controllers目录中,

创建对应一个首页的控制器home文件  **home_controller.rb**

并在其中定义一个将来对应URL地址的方法 **index**

```bash
app
|-controllers
	|- home_controller.rb
```

```ruby
class HomeController < ApplicationController
    def index
    end
end
```

---

#### 2. 路由自动生成

详见路由

---

#### 3. rails命令生成

手动生成太麻烦并且不全，可通过rails命令快速生成控制器和方法：

```bash
rails generate controller 控制器 [方法名]
# 简写
rails g controller 控制器名 [方法]
```

rails 6中，通过该命令自动在项目目录中生成：

- 控制器文件：**xxx_controller.rb**

- 控制器同名的**视图模版目录**，并在该目录下

  生成和对应控制器方法同名的模版文件 **xxx.html.erb**

- 控制器对应的同名test测试文件

- 控制器对应的同名helper文件

- 控制器对应的同名样式scss文件

```bash
项目
|- app
		|- controllers
				|- #控制器同名_controller.rb
		|- views
				|- #控制器同名目录
						｜- #控制器的方法同名.html.erb
		|- assets
				|- stylesheets
						|- #控制器同名.scss
		|- helpers
				|- #控制器同名_helper.rb
|- test
		|- controllers
				|- #控制器同名_controller_test.rb
```

如下：通过命令创建控制器home 和 index方法

```bash
rails g controller home index

Running via Spring preloader in process 98606
      create  app/controllers/home_controller.rb
       route  get 'home/index'
      invoke  erb
       exist    app/views/home
      create    app/views/home/index.html.erb
      invoke  test_unit
      create    test/controllers/home_controller_test.rb
      invoke  helper
      create    app/helpers/home_helper.rb
      invoke    test_unit
      invoke  assets
      invoke    scss
      create      app/assets/stylesheets/home.scss
```

会自动生成以下内容：

```bash
项目目录
|- app
		|- controllers
				|- home_controller.rb
		|- views
				|- home
						｜-index.html.erb
		|- assets
				|- stylesheets
						|- home.scss
		|- helpers
				|- home_helper.rb
|- test
		|- controllers
				|- home_controller_test.rb
```







## V 视图

每一个视图都对应一个路由指派的某控制器的某方法

每个视图会根据浏览器的URL请求路径，

将导入自 对应的某控制器Controller的某方法的数据 渲染为HTML结构，

然后响应返回到浏览器页面



Rails的视图采用ERB模版，视图文件后缀名为 **.html.erb**



视图模版存放在项目app目录的views目录中，

存于对应的控制器同名目录下，

模版文件名分别是对应的控制器的方法名

```bash
app
|-views
	｜- layouts
			｜- application.html.erb
			｜- mailer.html.erb
			｜- mailer.text.erb.html.erb
	｜- 控制器同名目录
			｜- 方法名.html.erb
			｜- 方法名.html.erb
```



### 通用模版

所有模版的通用骨架模版

```erb
<!DOCTYPE html>
<html>
  <head>
    <title>项目名称</title>
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <%= csrf_meta_tags %>
    <%= csp_meta_tag %>
    <%= stylesheet_link_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>
    <%= javascript_pack_tag 'application', 'data-turbolinks-track': 'reload' %>
  </head>

  <body>
    <%= yield %>
  </body>
</html>
```

 **<%= yield %>** 的位置将用来填入创建的其余各个页面的模版内容，

所以可以考虑将 导航Nav等通用部分放入通用模版

```erb
<!DOCTYPE html>
<html>
  <head>
    <title>项目名称</title>
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <%= csrf_meta_tags %>
    <%= csp_meta_tag %>
    <%= stylesheet_link_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>
    <%= javascript_pack_tag 'application', 'data-turbolinks-track': 'reload' %>
  </head>

  <body>
    
    <div class="navigation_bar">
      <%= link_to "项目一","/first" %>
			<%= link_to "项目二","/second" %>
			<%= link_to "项目三","/third" %>
    </div>
    
    <%= yield %>
  </body>
</html>
```







### ERB模版

如下：

创建与控制器home的Index方法同名的ERB模版文件 **index.html.erb**

```bash
app
|-views
	|- home
		｜- Index.html.erb
```

```erb
<h1>
  欢迎到首页home
  首页路径为 /
</h1>
```



只执行Ruby代码

```erb
<% link_to "first","/first" %>
```

执行Ruby代码并显示

```erb
<%= link_to "first","/first" %>
```





#### 导入变量

#### 循环











### 引入 CSS

#### 通用样式文件

项目通用模版的CSS样式存放于中：

**app/assets/stylesheets/application.css**

并已经被rails导入了通用模版

```bash
app
|- assets
		|- stylesheets
				|- application.css
```



#### 自定义样式文件

各个页面的样式会被分别存放于对应控制器同名的css文件中：

**app/assets/stylesheets/控制器名.css**

```bash
app
|- assets
		|- stylesheets
				|- application.cs
				|- #控制器同名文件.css
```

若通过rails命令生成控制器，则会自动生成对应的SCSS样式文件

**app/assets/stylesheets/控制器名.scss**

```bash
app
|- assets
		|- stylesheets
				|- application.cs
				|- #控制器同名文件.css
```





### 引入 JavaScript

Rails 6+ 中 JavaScript文件的存放位置改变

JavaScript文件被单独存放在 **app/javascript/packs/**

```bash
app
|- assets
		|- javascript
|- javascript
		|- packs
				|- #控制器同名目录
						|- #方法同名文件.js
```

如下：

在 app/views/home/index.html.erb 中

导入 app/javascript/packs/home/index.js 文件：

```erb
<h1>Wellcom Home</h1>
<%= javascript_pack_tag 'home/index' %>
```

模版最终被解析为

```html
<h1>Wellcom Home</h1>
<script src="/pack/js/home/inde 编译后的代码 .js"></script>
```





### 导入Bootstrap

Rails 6+ 以上的场合导入Bootstrap样式，

[官网Bootstrap Ruby Gem](https://github.com/twbs/bootstrap-rubygem/blob/master/README.md#a-ruby-on-rails)

1. 项目的 Gemfiel 中 加入bootstrap

```ruby
gem 'bootstrap', '~> 5.0.1'
```

2. bundler 安装 Gemfiel 中所有依赖包，并**重启项目服务器**

```bash
bundle install
```

3. 修改项目的公用样式文件后缀

   重命名  app/assets/stylesheets/**application.css** 

   为 app/assets/stylesheets/**application.scss**

4. 清空公用样式文件内容，并导入 Bootstrap

```scss
@import "bootstrap";
```

必须清空`app/assets/stylesheets/application.scss` 文件中的`*= require` 和 `*= require_tree`，否则Bootstrap导入不生效

---

若想导入Bootstrap 的JavaScript，

Rails 5+ 的场合需要

1. Gemfile中添加：并下载

```ruby
gem 'jquery-rails'
```

2. 在`application.js`中添加：

```js
//= require jquery3
//= require popper
//= require bootstrap-sprockets
```







## R 路由

浏览器访问指定的URL地址时，

会被路由指派给对应的控制器Controller的对应方法

若控制器中不存在该方法，则报错

路由都存放在在项目config目录下的路由文件 **routes.rb** 中

```bash
xxx
|- config
	|- routes.rb
```



### 传统方式 定义路由

分别设定 URL地址和对应控制器方法 的对应关系，

**一个地址只能对应一个控制器的一个Action方法**

路由根据URL**请求路径的不同**，指派与请求地址对应的控制器方法

| URL地址 | Route                         | 对应Action     |
| ------- | ----------------------------- | -------------- |
| /users  | get "/users" => "users#index" | users 的 index |
|         |                               |                |
| /users  | get "/users" => "users#index" |                |
| /users  | get "/users" => "users#index" |                |
| /users  | get "/users" => "users#index" |                |
| /users  | get "/users" => "users#index" |                |

---

#### URL, to: "" 形式

调用封装好的路由函数，分别传入两了个参数：

参数1: URL地址，  参数2: 对象（key是“to”，value是“控制器#方法”）

```ruby
Rails.application.routes.draw do
 
   HTTP请求方法 "URL地址", to: "控制器#方法"
  
end
```

---

#### Hash 形式

写法可精简为key=>value的Hash形式

```ruby
Rails.application.routes.draw do
 
    HTTP请求方法 "URL地址" => "控制器#方法"
  
end
```

如下：

指派URL地址  **/**  为控制器home中的Index方法

指派URL地址  **/about**  为控制器home中的About方法

浏览器地址栏访问指定URL时，页面会渲染出对应控制器的对应方法所对应的模版

```ruby
Rails.application.routes.draw do

  get "/" => "home#Index"
  get "/about" => "home#About"

end
```



### RESTful 定义路由

#### resources

Rails 的路由还支持并建议 RESTful的设计方式

通过 **resources :控制器** ，自动创建出几个固定的请求地址和控制器方法的对应

**同一个地址可根据请求方式的不同，对应控制器的不同方法**

路由根据URL**请求方式的不同**，指派与请求方式对应的控制器方法

```ruby
Rails.application.routes.draw do
	
  resources :控制器名

end
```

如下：

通过 resources指定控制器后，raisl自动生成7个相关路由

```ruby
Rails.application.routes.draw do
	
  resources :user

end
```

| 请求方式  | 请求地址       | 默认对应Action | 含义 |
| --------- | -------------- | -------------- | ---- |
| GET       | /user          | user#index     |      |
| GET       | /user/new      | user#new       |      |
| POST      | /user          | user#create    |      |
| GET       | /user/:id      | user#show      |      |
| GET       | /user/:id/edit | user#edit      |      |
| PATCH/PUT | /user/:id      | user#update    |      |
| DELETE    | /user/:id      | user#destory   |      |

可通过 **`rails routes`** 检查生成的所有路由

```bash
rails routes

	Prefix 	 		Verb   			URI Pattern  	 					Controller#Action
user_index 		GET    		/user(.:format)   					user#index
            	POST   		/user(.:format)  						user#create
new_user 			GET    		/user/new(.:format)     		user#new
edit_user 		GET    		/user/:id/edit(.:format) 		user#edit
user 					GET    		/user/:id(.:format)					user#show
 							PATCH  		/user/:id(.:format) 				user#update
 							PUT 			/user/:id(.:format)  				user#update		
 							DELETE  	/user/:id(.:format)  				user#destory
```

然后，需要分别定义路由对应的控制器方法和模版

```ruby
class BooksController < ApplicationController
  def index
  end
  def show
  end
  def new
  end
  def create
  end
  def edit
  end
  def update
  end
  def destory
  end
end
```

---

#### resource

定义单数资源路径（不需要传入参数识别，比如id） 

```ruby
Rails.application.routes.draw do
	
  resource :控制器

end
```

如下：

通过 resource指定控制器后，raisl自动生成6个相关路由

```ruby
Rails.application.routes.draw do
	
  resource :user

end
```

| 请求方式  | 请求地址   | 默认对应Action | 含义 |
| --------- | ---------- | -------------- | ---- |
| GET       | /user  | user#show      |      |
| GET       | /user/new  | user#new       |      |
| POST      | /user      | user#create    |      |
| GET       | /user/edit | user#edit      |      |
| PATCH/PUT | /user      | user#update    |      |
| DELETE    | /user      | user#destory   |      |

可通过 **`rails routes`** 检查生成的所有路由

```bash
rails routes
	
	Prefix 			Verb   			URI Pattern          Controller#Action
new_user 			GET    		/user/new(.:format)      users#new
edit_user 		GET    		/user/edit(.:format)     users#edit
user 					GET    		/user(.:format)          users#show
     					PATCH  		/user(.:format)          users#update
    					PUT    		/user(.:format)          users#update
    					DELETE 		/user(.:format)          users#destroy
  						POST   		/user(.:format)          users#create
```



### 定义路由总结

定义单数资源路径（不需要传参）：

1. 传统方式定义路由
2. **resource :控制器**

定义单数资源路径（需要通过参数识别，比如 id）：

1. 传统方式定义路由
2. **resources :控制器**





### 路由前缀（命名空间）

比如用于管理系统

#### 命名空间包裹方式

```ruby
Rails.application.routes.draw do
	
  namespace :前缀名 do
    resource :控制器
  end

end
```

如下：通过 resources指定控制器，并添加前缀

```ruby
Rails.application.routes.draw do
	
 namespace :admin do
    resources :user
 end

end
```

| 请求方式  | 请求地址                 | 默认对应Action         | 含义 |
| --------- | ------------------------ | ---------------------- | ---- |
| GET       | /**admin**/user          | **admin**/user#index   |      |
| GET       | /**admin**/user/new      | **admin**/user#new     |      |
| POST      | /**admin**/user          | **admin**/user#create  |      |
| GET       | /**admin**/user/:id      | **admin**/user#show    |      |
| GET       | /**admin**/user/:id/edit | **admin**/user#edit    |      |
| PATCH/PUT | /**admin**/user/:id      | **admin**/user#update  |      |
| DELETE    | /**admin**/user/:id      | **admin**/user#destory |      |

对应的控制器文件也要被创建在 **admin/目录**下

---

#### Scope

1. 不想在URL中显示前缀名，

   但控制器文件放在前缀名同名目录下

   ```ruby
   Rails.application.routes.draw do
     
     scope module: '前缀名' do
        resources :控制器
     end
     
     resources :控制器, modlue: '前缀'
   end
   ```

   如下：不在URL中显示 admin/ 前缀名

   ```ruby
   Rails.application.routes.draw do
     
     scope module: 'admin' do
        resource :users
     end
     
     resources :users, modlue: 'admin'
     
   end
   ```

   ---

2. 在URL中显示前缀名，

   但控制器不放在前缀名同名目录下

   ```ruby
   Rails.application.routes.draw do
     
     scope '前缀名' do
        resources :控制器
     end
     
   end
   ```

   如下：在URL中显示 admin/ 前缀名但控制器文件不放入 admin/目录下

   ```ruby
   Rails.application.routes.draw do
     
     scope 'admin' do
        resource :users
     end
     
   end
   ```

   

### 嵌入路由

```ruby
Rails.application.routes.draw do
  
  resource :users do
    resource :blogs
  end
  
end
```

| 请求方式 | 请求地址                 | 默认对应Action |
| -------- | ------------------------ | -------------- |
| GET      | /user/:id/blogs          | blogs#index    |
| POST     | /user/:id/blogs          | blogs#create   |
| GET      | /user/:id/blogs/new      | blogs#new      |
| GET      | /user/:id/blogs/:id/edit | blogs#edit     |
| GET      | /user/:id/blogs/:id      | blogs#show     |
| PATCH    | /user/:id/blogs/:id      | blogs#update   |
| PUT      | /user/:id/blogs/:id      | blogs#update   |
| DELETE   | /user/:id/blogs/:id      | blogs#destory  |








### 请求参数

Rail多采用放入请求路径的params形式传参

当然，也可自定义query查询字符串形式传参





### 

### 定义多个路由

```ruby
Rails.application.routes.draw do
	
  resources :user, :sessions

end
```















#### 命名路由







#### 查看所有路由

```bash
rails routes
```



#### 请求方式

#### 设定首页

或如下：将项目的首页的根路由 **/** 设为对应控制器 **home**

```ruby
Rails.application.routes.draw do
 
  root "home#"
  
end
```







### link_to 连接

前端HTML无法判断路由是否存在，

所以，即使该链接跳转的路由不存在于后段，

该连接依然会被显示到页面，这是不合理的

```erb
<a href="/first">first</a>
<a href="/second">second</a>
<a href="/third">third</a>
```

所以在 Rails的 ERB模版中，

通过 **link_to** 方法判断路由文件 routes.rb 中是否设定了该连接跳转的路由地址

若存在则显示该连接，若不存在则页面上不显示该连接

```erb
<%= link_to "连接的文本","路由" %>
```

```erb
<%= link_to "第一个","/first" %>
<%= link_to "第二个","/second" %>
<%= link_to "第三个","/third" %>
```

routes.rb

```ruby
Rails.application.routes.draw do
  
  get "/first", to: "home#First"
  get "/second", to: "home#Second"
  get "/third", to: "home#Third"

end
```

注意：

 **link_to **方法只是根据路由文件routes.rb 判断该连接是否应该在页面显示

若该路由地址对应的控制器方法对应的模版不存在的话，也会因为无法跳转而报错的，但不影响该连接在页面上的显示

若想验证连接跳转的路由地址是否正确，可通过 link_to 方法的**自动添加地址** 的功能，

若不报错则说明该链接的路由地址可以使用，对应的模版也没问题。相反若报错，则说明该路由地址对应的模版不存在



#### 自动添加连接地址

自动给连接的href添加上路由**全部的URL地址**

该路由地址对应的模版必须存在，否则报错无法添加

```erb
<%= link_to "连接的文本",路由地址_url %>
```

```html
<a href="http://域名:端口号/路由名称"></a>
```

如下：

```erb
<%= link_to "First",first_url %>

<a href="http://localhost:3000/first"></a>
```

---

自动给连接的href添加上**路由地址**

该路由地址对应的模版必须存在，否则报错无法添加

```erb
<%= link_to "连接的文本",路由地址_path %>
```

```html
<a href="/路由名称"></a>
```

如下：

```erb
<%= link_to "First",first_path %>

<a href="/first"></a>
```



#### 连接内容为HTML标签

```erb
<%= link_to 路由地址 do%>
    <h1>链接内容</h1>
<% end %>
```

```erb
<%= link_to first_path do%>
    <h1>第一个</h1>
<% end %>
```

当一个连接的内容不是文本，而是HTML标签时，

若直接在 link_to 连接内容写入HTML标签的话，

因为 Rails的保护机制，会**强制转为字符串**

```ruby
<%= link_to "<h1>第一个</h1>","/first" %>
```

```
<a href="/first"> <h1>第一个</h1> </a>
```

---

也可以给HTML标签的内容后添上 **.html_safe**

```ruby
<%= link_to "<h1>第一个</h1>".html_safe,"/first" %>
```









### 路由参数

#### query形式传参

> ```ruby
> <%= link_to "链接内容", 路由地址, 查询字符串参数: 值 %>
> ```
>
> ```erb
> <%= link_to "查询", "/first", name: "andy", age: 28 %>
> ```
>
> 或可以写成：
>
> ```erb
> <%= link_to "/first", name: "andy", age: 28 do %>
>     查询
> <% end %>
> ```
>
> 链接的请求URL路径为
>
> ```js
> /?name=andy&age=28
> ```
>
> 



#### params形式

```ruby
```

```ruby
Rails.application.routes.draw do
  
  get "/user/:id", to: "home#search"

end
```

```erb
<%= link_to search_path do%>
	<h1>查找</h1>
<% end %>
```







### resource 定义路由

#### index方法

> 一覧展示画面

一般用于 主页

```ruby
class BooksController < ApplicationController
  def index
  end
end
```

```http
/books
```



---

#### show方法

> 個別詳細画面

```ruby
class BooksController < ApplicationController
  def show
  end
end
```





---

#### new方法

> 新規登録画面

```ruby
class BooksController < ApplicationController
  def new
  end
end
```

```http
/boos/new
```





---

#### create方法

> 新規登録画面での入力処理

```ruby
class BooksController < ApplicationController
  def create
  end
end
```





---

#### edit方法

> 編集画面

```ruby
class BooksController < ApplicationController
  def index
  end
end
```

```http
/books/2/edit
```





---

#### update方法

> 編集画面での入力処理

```ruby
class BooksController < ApplicationController
  def update
  end
end
```

```http
/books/2
```





---

#### destory方法

> 一覧画面での削除処理

```ruby
class BooksController < ApplicationController
  def destory
  end
end
```

```http
/books/2
```





## M 数据

### ActiveRecord

是rails中一个重要的gem

完成了 **ORM数据映射**（Object Relational Model）

即，将 数据库中的数据表 映射到Ruby代码的对象中









### 设置支持数据库

Rails支持关系型数据库与非关系型数据库

- SQLite（Rails默认数据库）

- MySQL

- PostgreSQL
- Oracle
- SQLServer
- MongoDB

Rails默认使用SQLite数据库，

若创建应用时不指定数据库，Rails会自动安装**sqlite3**这个gem 来支持SQLite数据库

也可在创建Rails项目时，手动指定该项目的数据库

```bash
rails new 项目名称 -d 数据库
```

```bash
rails new demo -d postgresql
```



#### 设置 PostgreSQL

1. 

```bash
```

2. rails scaffold

```bash
rails scaffold blog tite:string content:text
```

model目录下生成blog.rb 

3. 开启数据库

```bash
brew services start postgresql
```

4. 创建

```bash
ails db:create
```





### 数据库迁移

- 关系型数据库的迁移

  使用ActiveRecord这个gem

- 对于非关系型数据库，比如MongoDB

  使用Mongoid/MongoMapper等gem



#### 数据库配置文件

Rails的数据库配置文件位置：

```bash
xxx
|-config
	|- database.yaml
```

Rails的数据库分别对应Rails的三种开发环境：

- development环境
- test环境
- production环境

```yaml
# 三个环境的通用配置
default: &default
  adapter: sqlite3
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  timeout: 5000


# 开发环境
development:
  <<: *default	#继承Default
  database: db/development.sqlite3

#测试环境
test:
  <<: *default	#继承Default
  database: db/test.sqlite3

#生产环境
production:
  <<: *default	#继承Default
  database: db/production.sqlite3
```







迁移到MySQL数据库

#### 1. 修改Gemfiel的gem

Rails默认是安装了 sqlite3 这个gem来生成SQLite数据库的支持，

若迁移到MySQL数据库，需要移除 sqlite3 改用 **mysql2** 这个gem，

即删除Gemfile中的 `gem 'sqlite3'`，并写入

```ruby
gem 'mysql2'
```

然后 bundle 安装

```bash
bundle install
```

#### 2. 修改数据库配置文件

Rails默认使用的SQLite是个文档型数据库，不需要端口号进程链接

但是MySQL是个关系型数据库，需要有启动的端口号和链接等设置

```yaml
default: &default
  adapter: mysql2
  encoding: utf8
  pool: 5
  host: 127.0.0.1
  username: root
  password: admin123

development:
  <<: *default
  database: demo_development

test:
  <<: *default
  database: demo_test

production:
  <<: *default
  database: demo_production
```

3.创建数据库

```
rake db:create
```







### yaml

Ruby提出的一种文件格式

Rails的配置文件多用yaml格式

其中可使用Ruby的继承等面向对象的形式，比如：

数据库配置文件中，3个环境的配置继承引用了了default配置

```yaml
# 三个环境的通用配置
default: &default
  adapter: sqlite3
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  timeout: 5000


# 开发环境
development:
  <<: *default	#继承Default
  database: db/development.sqlite3

#测试环境
test:
  <<: *default	#继承Default
  database: db/test.sqlite3

#生产环境
production:
  <<: *default	#继承Default
  database: db/production.sqlite3
```















## 

## 视图 View

### ERB模版

action方法名.html.erb

- 模版

```erb
<div>
  <%=@返回值%>
  
  <%@任意代码%>
  
  <%#注释%>
   
  <%if false%>
    <li>隐藏</li>
  <%end%>
  
  <%if true%>
    <li>显示</li>
  <%end%>
</div>
```

```erb
<ul>
    <li>name: <%=@msg[:name]%></li>
    <li>age: <%=@msg[:age]%></li>
    <li>address: <%=@msg[:address]%></li>
  
    <li>
        <% age =@msg[:age]+1 %>
        明年 <%=age%> 岁
    </li>
  
    <li><%#我是注释%></li>
  
    <%if @msg[:age]>18%>
    <li>成年人</li>
    <%end%>
</ul>
```

- 控制器方法

```ruby
class User < ApplicationController
  def default
    @模版变量 = 值
  end
end
```

```ruby
class User < ApplicationController
  def default
    @msg =  {
      name: 'andy',
      age: 28,
      address: 'CN'
    }
  end
end
```

- 页面展示

```erb
<ul>
  <li>name: andy</li>
  <li>age: 28</li>
  <li>address: CN</li>
  <li>明年 29 岁</li>  
  <li></li>
  <li>成年人</li>
</ul>
```



### 通用模版

**app/views/layout/application.html.erb**

所有模版的通用骨架模版

模版会被放入骨架模版的 **<body\> <%= yield %></body\>** 中

```erb
<!DOCTYPE html>
<html>
  <head>
    <title>Demo</title>
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <%= csrf_meta_tags %>
    <%= csp_meta_tag %>

    <%= stylesheet_link_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>
    <%= javascript_pack_tag 'application', 'data-turbolinks-track': 'reload' %>
  </head>

  <body>
    <%= yield %>
  </body>
</html>
```













## 模型 Model

ORM映射

### ?



---

- 对关系型数据库RDBMS的支持

  通过Gem： **ActiveRecord**

- 对非关系型数据库NoSQL的支持

  - MongoDB通过Gem：**Mongoid 或 MongoMapper**

  - 其他还有 **Redis 或 HBase**



### 数据库配置文件

**config/databa.yml**

**yaml**是Ruby提出的类似JSON的格式，可以实现继承和数组等

Rails应用默认支持SQLite数据库

```yaml
default: &default
  adapter: sqlite3
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  timeout: 5000

development:
  <<: *default
  database: db/development.sqlite3

test:
  <<: *default
  database: db/test.sqlite3

production:
  <<: *default
  database: db/production.sqlite3

```

```bash
adapter:   使用するデータベース種類
encoding:  文字コード
reconnect: 再接続するかどうか
database:  データベース名
pool:      コネクションプーリングで使用するコネクションの上限
username:  ユーザー名
password:  パスワード
host:      MySQLが動作しているホスト名
```

#### 

### 数据库迁移

比如，迁移到 **MySQL**

#### 1. 创建应用

```bash
rails new 应用名名 --database=mysql
# 简写
rails new 应用名 -d mysql
```

创建的项目的数据库配置文件 config/database.yml 如下：

```yaml
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password:
  socket: /tmp/mysql.sock

development:
  <<: *default
  database: demomysql_development

test:
  <<: *default
  database: demomysql_test

production:
  <<: *default
  database: demomysql_production
  username: demomysql
  password: <%= ENV['DEMOMYSQL_DATABASE_PASSWORD'] %>

```

#### 2. Rails连接MySQL











## 路由 Routes

**config/routes.rb**

通过URL请求到控制器Controller的action方法

```ruby
Rails.application.routes.draw do

end
```



### 设置路由

```ruby
Rails.application.routes.draw do
	
 	get 'URL地址', to: '控制器#方法'
  
	# 或者
  
	get 'URL地址' => '控制器#方法'
  
end
```

```http
http://localhost:3000/控制器/方法
```

如下：

1. 分别设定了两个路由

- /welcome/inde
- /welcome/about

```ruby
Rails.application.routes.draw do
  
	get '/welcome/index'=>'welcome#index'
  get '/welcome/about'=>'welcome#about'

end
```

2. 当浏览器访问到路由时，会访问控制中对应的方法

```ruby
class WelcomeController < ApplicationController
  def index
    xxx
  end

  def about
    xxx
  end
end
```

3. 然后根据访问的方法，分别响应对应方法同名的ERB模版文件

   即根据浏览器访问的URL地址，分别响应模版页面

-  app/views/welcome/index.html.erb
-  app/views/welcome/index.html.erb



### 自动生成路由

除了一一写出路由和与之对应的控制器方法的做法外，

也可以通过自动生成路由的方式，简化路由的写法

---

#### resource :控制器

通过 **resource :控制器** 自动创建出固定的几个URL地址和对应的控制器方法的映射

```ruby
Rails.application.routes.draw do
	
 resource :控制器
  
end
```

然后在对应的控制器中，定义出与自动生成生的路由一一映射的方法，就可以通过浏览器地址访问自动生的固定的几个URL了：

|       URL        | 访问的控制器的方法 |  HTTP请求   |         方法含义         |
| :--------------: | :----------------: | :---------: | :----------------------: |
|     /控制器      |     index 方法     |     GET     |         一覧画面         |
|    /控制器:id    |     show 方法      |     GET     |       個別詳細画面       |
|   /控制器/new    |      new 方法      |     GET     |       新規登録画面       |
|  /控制器/create  |    create 方法     |    POST     | 新規登録画面での入力処理 |
| /控制器/:id/edit |     edit 方法      |     GET     |         編集画面         |
|   /控制器/:id    |    update 方法     | PATCH / PUT |   編集画面での入力処理   |
|   /控制器/:id    |    destory 方法    |   DELETE    |   一覧画面での削除処理   |

```ruby
class WelcomeController < ApplicationController
  def index
  end
  def show
  end
  def new
  end
  def create
  end
  def edit
  end
  def update
  end
  def destory
  end
end
```





如下：

```ruby
Rails.application.routes.draw do
	
 resource :welcome
  
end
```





#### 检查自动生成的路由

```bash
rails routes
```

如下：

```ruby
Rails.application.routes.draw do
	
 resource :welcome
  
end
```

```bash
Prefix Verb   			URI Pattern   									Controller#Action  

GET    							/welcome(.:format)              welcome#index                                                        
POST   							/welcome(.:format)              welcome#create                                                                
new_welcome GET     /welcome/new(.:format)          welcome#new                                                     
edit_welcome GET    /welcome/:id/edit(.:format)     welcome#edit                                                               
welcome GET    			/welcome/:id(.:format)          welcome#show                                                                  
PATCH  							/welcome/:id(.:format)          welcome#update                                                             
PUT    							/welcome/:id(.:format)          welcome#update                                                              
DELETE 							/welcome/:id(.:format)          welcome#destroy                                                           
```









### 命名路由

```ruby
Rails.application.routes.draw do
	
  get '/user/:id', to: 'user#show', as: 'user'
  
end
```

```ruby
user_path(user) # /user/2
user_path(id)
user_path(user) # http://localhost:3000/user/2
```





### RESTful

定义每个URL请求的请求方式，

同一个URL地址可以拥有多个请求方式

- GET
- POST
- PUT / PATCH
- DELETE

```ruby
Rails.application.routes.draw do
	
  resource :users
  
end
```

```ruby
Rails.application.routes.draw do
	
  resources :users, :sessions
  
end
```

```ruby
Rails.application.routes.draw do
	
  scope '/admin' do
    resources :users
  end
  
end
```



GET



















# Rails进阶

Rails与前端HTML/CSS/JS



购物网站



数据库控制



WebAPI前端架构    Mobile连接 （React例子）



云端架设		Linux VPS/ Heroku

