| メソッド名 | 説明                         | 文                                  |
| ---------- | ---------------------------- | ----------------------------------- |
| «          | 要素を末尾に追加             | array « 要素                        |
| push       | 要素を末尾に追加(複数可)     | array.push(要素1,要素2, ..)         |
| append     | 要素を末尾に追加(複数可)     | array.append(要素1,要素2, ..)       |
| unshift    | 要素を先頭に追加             | array.unshift(要素)                 |
| insert     | 要素を指定した場所に追加     | array.insert(インデックス番号,要素) |
| pop        | 末尾の要素を取り除く         | array.pop                           |
| shift      | 先頭の要素を取り除く         | array.shift                         |
| delete_at  | 指定した場所の要素を取り除く | array.delete_at(インデックス番号)   |
| first      | 先頭の要素を返す             | array.first                         |
| last       | 末尾の要素を返す             | array.last                          |
| length     | 配列の要素数を返す           | array.length                        |



HASH ->**Arrayへの変換**

to_a

```ruby
obj={
    name:'andy',
    age:28
}

p obj.to_a

# [[:name, "andy"], [:age, 28]]
```

