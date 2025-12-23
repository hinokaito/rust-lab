# イテレーター

## 基底
### iter()の挙動
```rust
let v = vec![1, 0, 1, 7];
println!("{:?}", v.iter());
```
### by_ref()
by_ref()は、 **イテレーターを消費せずに&mut iterとして使う** ためのもの。<br>
以下のコードでは、イテレーターを消費するtake()を使用した後、iterを使おうとするとどうなるか確かめる。
```rust
// by_ref()あり
let v = vec![1, 0, 1, 7];
let mut iter = v.iter();
let first: Vec<i32> = iter.by_ref().take(2).copied().collect();
let rest: Vec<i32> = iter.copied().collect();
println!("{:?} {:?}", first, rest);
```
```rust
// by_ref()なし
let v = vec![1, 0, 1, 7];
let mut iter = v.iter();
let first: Vec<i32> = iter.take(2).copied().collect(); // move
let rest: Vec<i32> = iter.copied().collect();
println!("{:?} {:?}", first, rest);
```

## 使用率ランキング
これらだけは覚えておく。
### 1. collect
イテレーターの要素を指定した型で収集
```rust
let v = vec![1, 0, 1, 7];
let collected: Vec<i32> = v.iter().copied().collect();
println!("{:?}", collected);
```

### 2. map
要素の変換
```rust
let v = vec![1, 0, 1, 7];
let doubled: Vec<i32> = v.iter().map(|x| *x * 2).collect();
println!("{:?}", doubled);
```

### 3. filter
条件抽出
```rust
let v = vec![1, 0, 1, 7];
let upper_five: Vec<i32> = v.iter().filter(|&&x| x > 5).copied().collect();
println!("{:?}", upper_five);
```

### 4. for_each
副作用処理
```rust
let v = vec![1, 0, 1, 7];
v.iter().for_each(|x| println!("{}", x));
```

### 5. enumerate
インデックス付き反復。タプルで返す。
```rust
let v = vec![1, 0, 1, 7];
v.iter().enumerate()
    .for_each(|(index, value)| println!("index{}: {}", index, value));

for (index, value) in v.iter().enumerate() {
    println!("index{}: {}", index, value);
}
```

### 6. find
最初に一致する要素。Optionを返す。
```rust
let v = vec![1, 0, 1, 7];
match v.iter().find(|&&x| x == 0) {
    Some(_) => println!("0があります"),
    None => println!("0はありません"),
}
```

### 7. any/all
- any: 一つでも条件に当てはまればtrue
- all: すべてが条件に当てはまればtrue
```rust
let v = vec![1, 0, 1, 7];
println!("{}", v.iter().all(|x| *x > 5));
println!("{}", v.iter().any(|x| *x > 5));
```

### 8. fold
畳み込み。雪だるまのようなもの。<br>
accは、アキュームレーターの略で、「累積値」「集約値」の意味。慣例的に使う。<br>
挙動を可視化すると、下の例の場合こうなる。
|step|acc(累積値)|x(要素)|acc / x|
|---|---|---|---|
|初期|1000|-|-|
|1回目|1000|2|500|
|2回目|500|5|100|
|3回目|100|2|50|
|4回目|50|5|10|
```rust
let v = vec![2, 5, 2, 5];
let div = v.iter().fold(1000, |acc, x| acc / x);
println!("{}", div);
```

### 9. sum/product
- sum: 要素の和
- product: 要素の積
```rust
let v = vec![1, 0, 1, 7];
let sum: i32 = v.iter().sum();
let product: i32 = v.iter().product();
println!("{} {}", sum, product)
```

### 10. cloned/copied
- cloned: 参照をクローンする。参照のイテレータから、値をクローンしたイテレータを作成
- copied: 参照をコピーする。コピー可能な型の参照のイテレータから、値をコピーしたイテレータを作成
```rust
v.iter().cloned().collect(); // vのクローンが作成される
v.iter().copied().collect(); // vのコピーが作成される
```

## チェックしたいとき
### all(), any()
```rust
let v = vec![1, 0, 1, 7]; // 5を超える要素はあるが、すべてではない
println!("{}", v.iter().all(|x| *x > 5));
println!("{}", v.iter().any(|x| *x > 5));
```

## 結合したいとき
### chain()
```rust
let v1 = vec![1, 0];
let v2 = vec![1, 7];
let v: Vec<i32> = v1.iter().chain(v2.iter()).copied().collect();

println!("{:?}", v);
```