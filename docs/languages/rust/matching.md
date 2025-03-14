# Rust 的枚举和匹配

## 枚举类型

Rust 的枚举通过关键字 `enum` 定义。

```rust
enum CoinType {
    Dime, // 一角
    Half, // 五角
    Yuan, // 一元
} // 这个枚举量仅仅标定了硬币的类型

struct Coin {
    kind: CoinType,
    value: u32
} // 这个结构体记录了硬币的类型和对应的面值

let coin = Coin {
    kind: CoinType::Dime,
    value: 10
};
```

枚举的每种变体 (variant) 可以:

- 没有数据(单位元变体)
- 有命名的数据域(结构体变体)
- 有不命名的有序数据域(元组变体)

```rust
enum Resultish { 
    Ok,
    Warning { code: i32, message: String },
    Err(String)
}
```

!!! note "与 C++ 的对照"

    C/C++ 的枚举类型底层是整数，不能拥有数据成员。这是 Rust 和 C/C++ 的一个很大的区别。
    枚举类型是 Rust 提供给我们的有力武器。善用枚举类，可以便捷地传递很多数据。

## 匹配

### `match` 表达式

`match` 表达式可以对枚举量进行匹配。

```rust
enum Operator {
    Add(i32, i32),
    Sub(i32, i32),
    Mul(i32, i32),
    Div(i32, i32),
    Rem(i32, i32)
}

let op = Operator::Add(10, 5);

let value = match op {
    Operator::Add(a, b) => a + b,
    Operator::Sub(a, b) => a - b,
    Operator::Mul(a, b) => a * b,
    Operator::Div(a, b) => a / b,
    _ => a % b // _ 匹配任意值，类似 default
};
```

!!! note "与 C++ 的对照"

    `match` 表达式与 C/C++ 的 `switch-case` 有类似之处，但远比后者强大。读者可能需要花一点时间来适应 Rust 的 `enum` 和 `match` 表达式，然后就可以享受它美妙的模式匹配语法了。

### `if-let` 表达式

`if-let` 提供了一种简化的模式匹配机制。

```rust
let op = Operator::Add(10, 5);

let value = if let Operator::Add(a, b) = op {
    a + b
} else {
    -1
};
```
