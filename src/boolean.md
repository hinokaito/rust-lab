# 真偽値

## S Tier
### then
条件がtureのときにだけクロージャを実行してSomeを返す。<br>
遅延評価(trueのときにのみクロージャを実行)
```rust
let x = 10;
let result = (x > 5).then(|| x * 2);

println!("{:?}", result);
```

### then_some
条件がtrueのときに値を直接渡す。<br>
即時評価
```rust
let x = 4;
let result = (x % 2 == 0).then_some("even");

println!("{:?}", result)
```

<hr>

## A Tier
### ! (NOT)
```rust
let b = true;
println!("{}", !b);
```

### && (AND)
```rust
let t = true;
let f = false;
println!("{}", t && f);
```

### || (OR)
```rust
let t = true;
let f = false;
println!("{}", t || f);
```

<hr>

## B Tier
### bool as 数値
```rust
let t = true;
let f = false;

let int = t as i32;
let uint = f as u32;

println!("{} {}", int, uint)
```

### 数値 as bool
```rust
let t = 1;
let f = 0;

let x: usize = t as usize;
let y: f64 = f as f64;

println!("{} {}", x, y);
```

## C Tier
### ^ (XOR)
```rust
let t = true;
let f = false;
println!("{}", t ^ f);
```

### <,>,max,min
```rust
let t = true;
let f = false;

println!("{} {} {}", t<f, t.max(f), t.min(f));
```