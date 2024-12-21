---
title: "#1 Rustで数当てゲーム"
emoji: 8️⃣
type: tech
topics:
  - rust
  - 初心者
  - Game
published: true
---

## はじめに

Rustで数当てゲームを作ってみたのでプログラミングを始めたばかりの初心者に向けて記事にしました。
この記事のコードは[GitHub]にあります

## プログラムの解説

### プログラムの流れ

1. 1から100までのランダムな数字を生成
2. 入力を受け取る
3. 入力した文字列をを数字に変換
4. 入力された数字とランダムな数を比較する
    - ランダムな数値と入力された数値が同じなら終了
    - 小さいか大きいかを表示し、もう一度2から始める

### ランダムな数字を生成する

```rust
thread_rng().gen_range(1..=100)
```

これは説明すると長くなるのでぐぐってください。
`1..=100`の部分は範囲を表します。数字を変更すれば生成される範囲を指定できます。

:::details 例

```rust
let random: i32 = thread_rng().gen_range(1..=100);
println!("{}", random);
```

このプログラムを実行すると1 ~ 100のランダムな数字が表示されます。

:::

### 入力した数を受け取る

```rust
fn input() -> String {
    let mut input = String::new();
    std::io::stdin().read_line(&mut input).unwrap(); 
    input.trim().to_string() // 改行を削除 "123\n" -> "123"
}
```

この関数はターミナルで入力した文字列を取得する関数です。

```rust
let mut input = String::new();
```

で入力した文字列を入れる変数を用意します

```rust
std::io::stdin().read_line(&mut input).unwrap();
```

この行は入力した文字列を`input`変数に代入します。

```rust
input.trim().to_string()
```

最後に`input`変数には最後に改行文字(\n)が入ってしまっているのでそれを削除して、戻り値と知して返します

:::details input関数

```rust
let input = input();
println!("あなたが入力した文字は{}です！", input);
```

このプログラムを実行すると、`{}`の部分に入力した文字列が入った文字列が出力されます。

:::

### 入力した文字列を数字に変換する

```rust
let input_number: i32 = input.parse().unwrap();
```

どこをどう比較しているか説明
もし`input`変数が文字列に変換できない場合はエラーになります。

### 入力された数字とランダムな数を比較する

```rust
if input_number == guess_number {
    println!("You guessed correctly!");
    break;
} else if input_number < guess_number {
    println!("You guessed too low!");
} else if input_number > guess_number {
    println!("You guessed too high!");
}
```

入力がランダムな数値と、
一致した場合は文を表示して、`loop`を終了
小さい場合は二番目の文を表示、
大きい場合は三番目の文を表示します。

## おわりに

:::details 全体のコード

```rust
use rand::{thread_rng, Rng};

fn main() {
    let guess_number = thread_rng().gen_range(1..=100);
    println!("Guess a number between 1 and 100");

    loop {
        let input = input();

        let input_number: i32 = input.parse().unwrap();

        if input_number == guess_number {

            println!("You guessed correctly!");
            break;
        } else if input_number < guess_number {

            println!("You guessed too low!");
        } else if input_number > guess_number {

            println!("You guessed too high!");
        }
    }
    println!("The number was: {}", guess_number);
}

fn input() -> String {
    let mut input = String::new();
    std::io::stdin().read_line(&mut input).unwrap();
    input.trim().to_string()
}
```

:::

解説するほどのプログラムではないと思いますが、記事の練習として書いてみました。
今後もこのような内容の記事を書いていきたいと思っているので修正などのアドバイスを是非コメントでしてもらえるとありがたいです！

[GitHub]: https://github.com/daizyoo/number-guessing
