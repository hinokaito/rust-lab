# イテレータ

## S Tier
### iter()          
不変参照でイテレート
```rust
let v = vec![1, 0, 1, 7];
println!("{:?}", v.iter());
```

### iter_mut()      
可変参照でイテレート
```rust
let mut v = vec![1, 0, 1, 7];
let mut iter = v.iter_mut();
if let Some(x) = iter.next() {
    *x = 10;
}
println!("{:?}", v)
```

### into_iter()     
所有権を消費してイテレート
```rust
let v = vec![1, 0, 1, 7];
let mut iter = v.into_iter();
println!("{:?}", v);
```

### map()           
要素変換
```rust
let v = vec![1, 0, 1, 7];
let doubled: Vec<i32> = v.iter().map(|x| *x * 2).collect();
println!("{:?}", doubled);
```

### filter()        
条件で絞り込み
```rust
let v = vec![1, 0, 1, 7];
let upper_five: Vec<i32> = v.iter().filter(|&&x| x > 5).copied().collect();
println!("{:?}", upper_five);
```

### for_each()      
副作用処理
```rust
let v = vec![1, 0, 1, 7];
v.iter().for_each(|x| println!("{}", x));
```

### collect()       
iterator → Vec/HashMap等
```rust
let v = vec![1, 0, 1, 7];
let collected: Vec<i32> = v.iter().copied().collect();
println!("{:?}", collected);
```

### enumerate()     
index付きイテレート
```rust
let v = vec![1, 0, 1, 7];
v.iter().enumerate()
    .for_each(|(index, value)| println!("index{}: {}", index, value));

for (index, value) in v.iter().enumerate() {
    println!("index{}: {}", index, value);
}
```

<hr>

## A Tier
### find()	
条件に合う最初の要素
```rust
let v = vec![1, 0, 1, 7];
match v.iter().find(|&&x| x == 0) {
    Some(_) => println!("0があります"),
    None => println!("0はありません"),
}
```

### any()	
条件を満たす要素があるか
```rust
let v = vec![1, 0, 1, 7];
println!("{}", v.iter().any(|x| *x > 0));
```

### all()	
全要素が条件を満たすか
```rust
let v = vec![1, 0, 1, 7];
println!("{}", v.iter().all(|x| *x > 0));
```

### fold()	
累積処理
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

### count()	
要素数
```rust
let v = vec![1, 0, 1, 7];
let x = v.iter().count();
println!("{:?}", x);
```

### zip()	
複数イテレータを束ねる
```rust
let v = vec![1, 0, 1, 7];
let x: Vec<i32> = v.iter().zip(v.iter()).map(|(a, b)| a * b).collect();
println!("{:?}", x);
```

### cloned() / copied()	
- cloned: 参照をクローンする。参照のイテレータから、値をクローンしたイテレータを作成
- copied: 参照をコピーする。コピー可能な型の参照のイテレータから、値をコピーしたイテレータを作成
```rust
v.iter().cloned().collect(); // vのクローンが作成される
v.iter().copied().collect(); // vのコピーが作成される
```

<hr>

## B Tier
### filter_map()	
filter + map 同時
```rust
let v = vec![1, 0, 1, 7, 8];
let x: Vec<i32> = v.iter().filter_map(|&x| if x % 2 == 0 { Some(x) } else { None }).collect();
println!("{:?}", x)
```

### flat_map()	
ネストされたイテレータを平坦化
```rust
let v = vec![vec![1, 0], vec![1, 7]];
let flattened: Vec<i32> = v.iter().flat_map(|v| v.iter().copied()).collect();
println!("{:?}", flattened)
```

### take()	
先頭n個
```rust
let v = vec![1, 0, 1, 7];
let taken: Vec<i32> =  v.iter().take(2).copied().collect();
println!("{:?}", taken);
```

### skip()	
先頭n個をスキップ
```rust
let v = vec![1, 0, 1, 7];
let x: Vec<i32> = v.iter().skip(2).copied().collect();
println!("{:?}", x);
```

### chain()	
Iterator結合
```rust
let v1 = vec![1, 0];
let v2 = vec![1, 7];
let v: Vec<i32> = v1.iter().chain(v2.iter()).copied().collect();

println!("{:?}", v);
```

### inspect()	
デバッグ用副作用
```rust
let v = vec![1, 0, 1, 7];

let result: Vec<_> = v.iter()
    .inspect(|x| println!(" {}", x))
    .filter(|x| *x % 2 == 0)
    .inspect(|x| println!("フィルター反応: {}", x))
    .collect();
```

### position()	
条件に合うindex
```rust
let v = vec![1, 0, 1, 7];
let pos = v.iter().position(|&x| x == 0);

println!("{:?}", pos);
```

<hr>

## C Tier
### peekable()	
次の要素を覗く
```rust
let vec = vec![1, 0, 1, 7];
let mut peekable = vec.iter().peekable(); // peekableを作る
println!("{:?}", peekable.peek()); // peekableでないものにはpeek()は使えない
println!("{:?}", peekable.peek());
println!("{:?}", peekable.peek());
println!("{:?}", peekable.next());
println!("{:?}", peekable.next());
println!("{:?}", peekable.next());
```

### scan()	
状態付きmap
```rust
let v = vec![1, 0, 1, 7];
let sums: Vec<_> = v.iter()
    .scan(0, |state, &x| {
        *state += x;
        Some(*state)
    })
    .collect();

println!("{:?}", sums)
```

### step_by()	
n個飛ばし
```rust
let v = vec![1, 0, 1, 7];
let x: Vec<i32> = v.iter().step_by(2).copied().collect();
println!("{:?}", x);
```

### cycle()	
無限ループ
```rust
let v = vec![1, 0, 1, 7];
let result: Vec<_> = v.iter()
    .cycle()
    .take(8) // 8要素満ちたら取り出す
    .cloned()
    .collect();

println!("{:?}", result)
```

### rev()	
逆順
```rust
let v = vec![1, 0, 1, 7];
let reversed: Vec<i32> = v.iter().rev().copied().collect();
println!("{:?}", reversed)
```

### partition()	
2グループに分割
```rust
let v = vec![1, 0, 1, 7, 8];
let (evens, odds): (Vec<i32>, Vec<i32>) = v.iter().partition(|x| *x % 2 == 0);
println!("偶数{:?} 奇数{:?}", evens, odds)
```

### try_fold()
失敗したら即中断するfold
```rust
let v = vec![1, 0, 1, 7];
let result = v.iter().try_fold(1, |acc, &x| {
    if x == 0 {
        Err("division by zero") // または Noneでも
    } else {
        Ok(acc * x) // または Some()でも
    }
});

println!("{:?}", result)
```

<hr>

## Tier外
### next()
イテレータの次の要素を取る
```rust
let v = vec![1, 0, 1, 7];
let mut iter = v.iter();
println!("{:?}", iter.next());
println!("{:?}", iter.next());
println!("{:?}", iter.next());
println!("{:?}", iter.next());
println!("{:?}", iter.next());
```


### by_ref()
イテレータを消費せずに&mut iterとして使う
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

### sum/product
- sum: 要素の和
- product: 要素の積
```rust
let v = vec![1, 0, 1, 7];
let sum: i32 = v.iter().sum();
let product: i32 = v.iter().product();
println!("{} {}", sum, product)
```

### take_while
条件を満たす間要素を取得する
```rust
let v = vec![1, 0, 1, 7];
let taken: Vec<i32> = v.iter().take_while(|&&x| x < 5).copied().collect();
println!("{:?}", taken);
```

### skip_while
条件を満たす間、要素をスキップする
```rust
let v = vec![1, 0, 1, 7];
let x: Vec<i32> = v.iter().skip_while(|&&x| x < 5).copied().collect();
println!("{:?}", x);
```