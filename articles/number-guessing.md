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

# はじめに

Rustで数当てゲームを作ってみたのでプログラミングを始めたばかりの初心者に向けて記事にしてみました。

[[Git hub](https://github.com/daizyoo/number-guessing)]




# プログラムの解説

## 追加する機能

- 1から100までのランダムな数字を生成
- 入力を受け取る
- 入力した文字列をを数字に変換
- 入力した数字が大きいか小さいかを表示、もし正解の数字と同じなら、`You guessed correctly`と表示して無限ループを終了
- 入力された数字とランダムな数を比較する
## ランダムな数字を生成する

```rust
use rand::{thread_rng, Rng};
thread_rng().gen_range(1..=100)
```

:::details 例

```rust
let random: i32 = thread_rng().gen_range(1..=100);
println!("{}", random);
```

このプログラムを実行すると1 ~ 100のランダムな数字が表示されます。
:::
書けたらクレートとインポートしたことの説明
`1..=100`を`1..100`にすると1 ~ 99が生成されます。
くわしくは[こちら](https://rs.nkmk.me/rust-range/)を見てください

## 入力した数を受け取る

```rust
fn input() -> String {
    let mut input = String::new();
    std::io::stdin().read_line(&mut input).unwrap(); // 改行を削除 // "123\n" -> "123"
    input.trim().to_string()
}
```
この関数はターミナルで入力した文字列を取得する関数です。
改行が入ってることの説明と切り取りについて説明
:::details input関数

```rust
fn main() {
    let input = input();
    println!("あなたが入力した文字は{}です！", input);
}
```

このプログラムを実行すると、`{}`の部分に入力した文字列が入った文字列が出力されます。
:::


## 入力した文字列を数字に変換する

```rust
let input_number: i32 = input.parse().unwrap();
```
どこをどう比較しているか説明
もし`input`変数が文字列に変換できない場合はエラーになります。

## 入力された数字とランダムな数を比較する
ここを関数化してくれるとありがたい
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
入力がランダムな数値と一致した場合
入力がランダムな数値より小さい場合
入力がランダムな数値より大きい場合
## 全体のコード
:::details number_guesser

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
# おわりに

記事にするほどのプログラムではないと思いますが、記事の練習として書いてみました。
今後もこのような内容の記事を書いていきたいと思っているので修正などのアドバイスを是非コメントでしてもらえるとありがたいです！

おまけとしてnumber＿gusser　gusserを書いても面白そう