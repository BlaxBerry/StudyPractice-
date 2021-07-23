Ruby基础



## 变量

### 变量类型

- ローカル変数 	

- グローバル変数

- インスタンス変数、

- クラス変数

```ruby
a = "" 
$a = ""

class Example
  @@a = ""
  @@b = ""
  def initialize(params1,params2)
    @a = params1
    @b = params2
  end
end
```



### 変数スコープ

変数には **参照できる範囲** を意味する **スコープ** (範囲)があり

ローカル変数：どこで定義したかによって、スコープが変わる

グローバル変数：どこからでも参照できる

#### local变量

```ruby
a = 100
def fn
    a = 200
    puts a
end
fn

# 200
```

函数内部直接使用函数外部定义的local变量，报错

```ruby
a = 100
def fn
    puts a
end
fn

# undefined local variable or method `a' for main:Object (NameError)
```

#### 全局变量

```ruby
$a = 100

def fn
    a = 200
    puts $a
end
fn

# 100
```







### 赋值与使用

先使用后定义，报错

```ruby
puts name 
name = "Ruby"

# undefined local variable or method `a' for main:Object (NameError)
```





### 式展開

式展開は、文字列の中に変数や計算式の結果などを出力できる機能

在字符串中调用变量时，

- 除了local变量需要通过 **#{变量}** 解析

- global变量可直接通过 **#变量** 解析

- 类中的也可直接通过 **#@实例变量、#@@类变量** 解析 (详见类)

```ruby
$tom = 'tom'
andy = "andy"

puts "hello #{andy}"
puts "hello #$tom"

# hello andy
# hello tom
```

`#{}`の中に、計算式の結果も出力でき

```ruby
puts "sum is #{(10+20)/3}"
# sum is 10
```





## 常量

不可再次赋值

```ruby
A = 'hello andy'
A = 'hello tom'
puts A
# 結果
warning: already initialized constant A
warning: previous definition of A was here
hello tom
```









## 数据类型

Rubyの世界では、すべてのデータがオブジェクト

|  类型  | 含义 | 例子 |
| :----: | :--: | :--: |
| Number | 数值 |      |
| String |      |      |
| Ranges |      |      |
|  true  |      |      |
| false  |      |      |
|  nil   |      |      |
| Array  |      |      |
|  Hash  |      |      |
| Symbol |      |      |

**オブジェクトのクラスを確認・変換**

### 判断类型

通过**数据.class** 判断数据的类型

```ruby
数据.class
```

```ruby
puts 1.class        # Integer
puts (1.0).class    # Float

puts "hello".class  # String

puts nil.class			# NilClass

puts true.class			# TrueClass
puts false.class		# FalseClass

puts [1,2].class		# Array

puts :key.class			# Symbol

puts /a-z/.class		# Regexp
```



### 数值 Number

|   类型    | 含义 |
| :-------: | :--: |
| Integer类 | 整数 |
|  Float类  | 浮点 |

#### 算数操作

- 基本の足し算（**+**）引き算（**-**）掛け算（*****）割り算（**/**）

- べき乗（******）

  指数

  演算子の左辺を右辺の値で累乗します。

  ```ruby
  puts 3**2
  # 9 
  ```

- 代入演算子 += -= *= /=

  ```ruby
  x += 5
  x -= 5
  x *= 5
  x /= 5  
  ```



### 字符串 String

Ruby好像没有隐式转换

```ruby
puts "1"      # 1
puts "1" * 3  # 111

puts 3 * "1"  # 报错
```

---

#### 转义字符

文字列リテラル

- \n：换行
- \s：空格

---

#### 单双引号

シングルクオートとダブルクオートの違い

两者都可用于字符串类型

但只有双引号可识别 **式展開や改行文字**

```ruby
puts "#{10 + 20}"  # 30
puts '#{10 + 20}'  # #{10 + 20}
```

```ruby
puts "Hello \n World"
=begin
Hello 
World
=end

puts 'Hello \n World'
# Hello \n World
```





### nil

nilオブジェクトは**何も存在しない**ことを表す

Rubyの`false`と`nil`のみが偽

----

#### 判断nil

```ruby
puts nil.nil? # true
puts 1.nil?   # false
puts "".nil?  # false

def fn
end
puts fn.nil? # true 

```

```ruby

```

```ruby

```



### symbol

シンボルは任意の文字列と一対一に対応するオブジェクト

文字列の代わりに用いることもでき

```ruby
`:' 識別子
`:' 変数名
`:' 演算子
```

---

#### 验证唯一性

**.objet_id** 验证

```ruby
puts "hello".object_id
puts "hello".object_id
# 70349442295420
# 70349442295280

hello = "hello"
puts :hello.object_id
puts :hello.object_id
# 1055388
# 1055388

# 当然同一个变量的id是相同的
puts hello.object_id
puts hello.object_id
# 70278201976100
# 70278201976100
```

**.equal?()** 验证

```ruby
puts :hello.equal?(:hello)
puts "hello".equal?("hello")

# true
# false
```

---

#### 用处

`名前'を指し示す時など、文字列そのものが必要なわけではない時に用います。

- ハッシュのキー { :key => "value" }
- アクセサの引数で渡すインスタンス変数名 attr_reader :name
- メソッド引数で渡すメソッド名 __send__ :to_s
- C の enum 的な使用 (値そのものは無視してよい場合)

```ruby

```

```ruby

```

```ruby

```







## 数组 Array

 **配列**

```ruby

```

Arrayオブジェクトの要素を参照する時

**インデックス** / **添字**

```ruby

```

**要素の追加**

加一个

```ruby
Arrayオブジェクト << "要素"
```

加多个

```ruby
Arrayオブジェクト.push(要素,要素)
```



**要素の削除**

```ruby
Arrayオブジェクト.delete_at(index)
```

**要素の変更**

```ruby
Arrayオブジェクト[index] = "変更したい要素"
```

```ruby

```









## 范围 Range

演算子式/範囲式

```ruby

```



## 哈希 Hash

```ruby
hash = {
  name: "andy", 
  age: 28, 
  address: "CN"
}
```

Hashオブジェクトの要素を参照する時

```ruby
hash[:name]
```

### key

```ruby
# キーに文字列を使用
string_hash = {
  "name" => "andy",
  "age" => 28,
  "address" => "CN"
}
# キーにシンボルを使用
symbol_hash = {
 	name: "andy", 
  age: 28, 
  address: "CN"
}
puts string_hash
puts symbol_hash

# {"name"=>"andy", "age"=>28, "address"=>"CN"} 
# {:name=>"andy", :age=28, :address=>"CN"}
```

文字列のキーとシンボルのキーを混在させることができ

```ruby
hash = {
 	name: "andy", 
  "age" => 28, 
  address: "CN"
}
p hash

# {:name=>"andy", "age"=>28, :address=>"CN"}
```

**要素の追加**

```ruby
ハッシュ名[キー] = 追加したい値
```

```ruby

```

```ruby

```







## 方法函数

### 定义与调用

```ruby
def 函数名
  函数体
end

函数名
```

先调用后定义，报错

```ruby
fn
def fn 
    puts 'helo'
end

# undefined local variable or method `fn' for main:Object (NameError)
```



### 返回值

每个方法默认都会返回一个值。

Ruby默认返回的值是最后一个语句的值

```ruby
def fn
    a = 100
    b = 200
    c = 300
end

p fn   # 300
```

---

#### return语句

用于从 Ruby 方法中返回一个或多个值。

```ruby
def fn
    a = 100
    b = 200
    c = 300
   	return a,b,c
end

p fn 	# [100, 200, 300]
```

- return后有指定的返回值，返回该指定值

- 若return后未给出指定返回值，返回 **nil** 

```ruby
def fn01
  	return 99
    a = 100
end
p fn01 	# 99


def fn02
  	return 
end
p fn02 	# nil
```



### 函数参数

```ruby
def 函数名(a,b)
  函数体
end

函数名(a,b)
```

---

#### 参数默认值

```ruby
def 函数名(a=默认值,b=默认值)
  函数体
end

函数名
函数名(a,b)
```

---

#### 可变参数

```ruby
def 函数名(*params)
  函数体
end

函数名(a)
函数名(a,b)
```

```ruby

```

```ruby

```

### 变量作用域

```ruby
a = 100
def fn
    a = 200
    puts a
end
fn

# 200
```

函数内部直接使用函数外部定义的local变量，报错

```ruby
a = 100
def fn
    puts a
end
fn

# undefined local variable or method `a' for main:Object (NameError)
```



## 语法

**制御構文**

### 判断

**条件分岐**

#### if 语句

```ruby
if 条件 
  xxx
end
```

一般省略 then，但若一行内书写 if 式，则必须以 then 隔开

```ruby
if 条件 then xxx end
```

#### if 修饰符

```ruby
xxx if 条件
```

#### if...else 语句

```ruby
if 条件1
  xxx
else
  xxx
end
```

#### if...elsif...else 语句

```ruby
if 条件1
  xxx
elsif 条件2
  xxx
else
  xxx
end
```

#### unless 语句

if 语句的相反，条件不成立则执行

```ruby
unless 条件
  xxx
end
```

#### unless 修饰符

```ruby
xxx unless 条件
```

#### case 语句

匹配判断

```ruby
case 比較したいオブジェクト
when 値1, 値2
   xxx
when 値3, 値4
   xxx
else
   xxx
end
```

一般省略 then，但若一行内书写 if 式，则必须以 then 隔开

```ruby
case 比較したいオブジェクト
when 値1 then xxx
when 値2 then xxx
end
```

---

### 循环

#### until 语句

直到满足条件前，一直循环执行

```ruby
until 条件
  xxx
end
```

一般省略do，但若一行内书写 if 式，则必须以 do 隔开

```ruby
until 条件 do xxx end
```

#### until 修饰符

```ruby
xxx until 条件

begin
  xxx
end until 条件
```

#### while 语句

满足条件就会一直循环执行（容易出现死循环）

```ruby
while 条件
  xxx
end
```

一般省略do，但若一行内书写 if 式，则必须以 do 隔开

```ruby
while 条件 do xxx end
```

#### while 修饰符

```ruby
xxx while 条件
  
begin
  xxx
end while 条件
```

#### for 语句

```ruby
for 计时器变量 in 范围 
  xxx
end
```

```ruby
for num in 0...5
  puts num
end
=begin
0,
1,
2,
3,
4
=end

for item in [1,2,3,4]
  puts item
end
=begin
1,
2,
3,
4
=end
```



#### break

终止循环该次迭代

繰り返し処理を抜ける装飾子

```ruby
for i in 0..5
    if i >= 3 
       break
    end
    puts i
end
=begin
0,
1,
2
=end
```

#### next

跳到循环的下一个迭代

次の繰り返し処理にジャンプ

```ruby
for i in 0..5
    if i == 3 && i == 4
       next
    end
    puts i
end
=begin
0,
1,
2
5
=end
```

#### redo

重新开始该次迭代

現在行っている繰り返し処理を再度繰り返す

```ruby
for i in 0..5
   if i < 2 
      puts i
      redo
   end
end
=begin
0,
0,
0,
...
死循环
=end
```







## 类 class

```ruby

```

```ruby

```

```ruby

```

```ruby

```



类中的函数可通过 **#** 直接调用实例变量、类变量、global变量

```ruby
#@实例变量
#@@类变量
#$global变量
```

```ruby
$a = "aaa"
class Example
    @@a = "a"
    @@b = "b"
    def initialize(params1,params2)
      @a = params1
      @b = params2
    end
    def say 
        puts "#@a,#@b"
        puts "#@@a,#@@b"
      	puts "#$a"
    end
end
p = Example.new(1,2)
p.say

# 10, 20
# a, b
# aaa
```



```ruby

```

```ruby

```

```ruby

```

```ruby

```

```ruby

```

```ruby

```



## 正则表达式

正規表現オブジェクト

```ruby

```

```ruby

```

```ruby

```

```ruby

```

```ruby

```

```ruby

```







## %記法





## gem和libary

他のプログラムから読み込んで使うためのプログラムを**ライブラリ**といい

Rubyのクラスもライブラリ

Rubyのライブラリには三種類に分類され：

- **組み込みライブラリ**

  `Integer`クラス `String`クラス `Array`クラス `Hash`クラス

- **標準添付ライブラリ**

  使用するときに`require`などの宣言が必要

  - `Date`クラス `fileutils` `csv`など

  ```ruby
  require "date"
  p Date.today
  ```

  - 別のファイルを読み込む

  ```ruby
  
  ```

- **外部ライブラリ（gem）**

  **Ruby on Rails**などもgemのひとつ

```ruby

```

```ruby

```

```ruby

```

```ruby

```

```ruby

```

```ruby

```
