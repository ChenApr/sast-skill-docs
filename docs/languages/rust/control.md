# Rust 中的流程控制

## 条件表达式

Rust 中的条件语句类似 C/C++，都是通过 `if` 和 `else` 来控制。

`if` 后面的条件可以省略括号，编译器也推荐这样做。

```rust
let age = 17;
if age < 18 {
    println!("您未成年");
} else {
    println!("您成年了");
}
```

注意，`if` 实际上是表达式而非语句，因此可以拥有值。

```rust
let age = 17;
let adulthood = if age < 18 {
    false
} else {
    true
};
```

Rust 是一门基于表达式的语言，这仅仅是冰山一角。读者在实际采用 Rust 编程时将会对这一设计有更深刻的理解。

!!! note "与 C++ 的对照"

    Rust 中没有 C/C++ 中常见的三元表达式，但是可以用 `if-else` 表达式来实现相同的功能。

## 循环语句

### `for` 循环

`for` 循环用于遍历容器中的每一个元素，这与 C 风格的 `for` 循环大为不同，而与 Python 更加类似。

```rust
let mut sum = 0;
for i in (1 .. 10) { // (a .. b) 创建了一个数字序列 Range，表示半开半闭区间 [a, b) 中的所有整数。
    sum += i;
}
// 实际上这里的求和可以通过迭代器用更优雅和快速的方法重写，此处仅仅作为演示。
let word: &str = "World";
    for letter in word.chars() { // 将 &str 类型转化为迭代器
        println!("{}", letter);
}
// 迭代器的简单示例
```

### `while` 循环

`while` 循环与 C/C++ 较为类似，仅仅给出代码样例。

```rust
let mut sum = 0;
let mut a = 1;
while a < 10 {
    sum += a;
    a += 1;
}
```

### `loop` 循环

`loop` 循环是显式的死循环，会一直重复执行直到显式 `break` 为止。

```rust
let mut sum = 0;
let mut a = 1;
loop {
    sum += a;
    a += 1;
    if a == 10 {
        break;
    }
}
```

实际上，以上的三种循环也都是表达式，而且 `loop` 中的 `break` 可以返回这个表达式的值。这里提供一个 `3n + 1` 问题的代码来展示它的表达式本质：

```rust
let mut a = 19112021; // 可以是要验证的任意大整数，只要不越界即可
let must_be_one = loop {
    if a == 1 {
        break a;
    } else if a % 2 == 1 {
        a = 3 * a + 1;
    } else {
        a = a / 2;
    }
}; // must_be_one 的值应当为 1
// 如果你发现程序没有结束，请多等一会
```

### 循环标签

如果存在嵌套循环，`break`、`continue` 都只作用于此时最内层的循环。在Rust中，你可以为特定循环指定 循环标签（loop label），然后将标签与 break 或 continue 一起使用，使这些关键字作用于所标记的循环，实现跳出多重循环的目的。

在有这个特性之前，「跳出多重循环」其实颇为棘手——因为最优雅的方式是声名狼藉的"goto"。虽然「把循环封装在一个函数内用return跳出」是个好办法，但并不总适合这样做。

```rust
let matrix = vec![
    vec![1, 2, 3],
    vec![4, 5, 6],
    vec![7, 8, 9]
];

let target = 5;
let mut found = false;

'outer: for row in &matrix { // 将循环标签放在循环前面. 格式为 'label
    for &num in row {
        if num == target {
            found = true;
            break 'outer;  // 使用循环标签直接跳出外层循环
        }
    }
}

if found {
    println!("找到了目标元素！");
} else {
    println!("未找到目标元素。");
}
```