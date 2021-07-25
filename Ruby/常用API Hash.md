| メソッド名 | 説明                         | 文                  |
| ---------- | ---------------------------- | ------------------- |
| delete     | 指定した要素を削除           | hash.delete(キー)   |
| keys       | Hash内のキー情報を配列で返す | hash.keys           |
| values     | Hash内の値の情報を配列で返す | hash.values         |
| key?       | キーがHash内に存在するか確認 | hash.key(キー)      |
| has_key?   | キーがHash内に存在するか確認 | hash.has_key(キー)  |
| has_value? | 値がHash内に存在するか確認   | hash.has_value(値)) |
| include?   | キーがHash内に存在するか確認 | hash.include(キー)  |
| empty?     | Hashが空かどうか確認         | hash.empty?         |
| length     | Hashの要素数を返す           | hash.length         |
| size       | Hashの要素数を返す           | hash.size           |





Array->**Hashへの変換**

to_h

```ruby
arr = [[:name, "andy"], [:age, 28]]
p arr.to_h

# {name:'andy',age:28}
```

