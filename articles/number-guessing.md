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

Rustで数当てゲームを作ってみたので記事にしてみました。

## 対象読者

Rustやプログラミングを始めたばかりの初心者に向けて書いた記事です。

## 本記事のプログラム

[GitHub](https://github.com/daizyoo/number-guessing)に載せてあります

# プログラムの解説

## 必要な機能

- 1から100までのランダムな数字を生成
- 入力を受け取る
- 入力を数字に変換
- 入力した数字が大きいか小さいかを表示、もし正解の数字と同じなら、`You guessed correctly`と表示して無限ループを終了

## input関数

```rust
fn input() -> String {
    let mut input = String::new();
    std::io::stdin().read_line(&mut input).unwrap(); // 改行を削除 // "123\n" -> "123"
    input.trim().to_string()
}
```

この関数はターミナルで入力した文字列を取得する関数です。
:::details 例

```rust
fn main() {
    let input = input();
    println!("あなたが入力した文字は{}です！", input);
}
```

このプログラムを実行すると、`{}`の部分に入力した文字列が入った文字列が出力されます。
:::

## ランダムな数字を生成する

```rust
thread_rng().gen_range(1..=100)
```

:::details 例

```rust
let random: i32 = thread_rng().gen_range(1..=100);
println!("{}", random);
```

このプログラムを実行すると1 ~ 100のランダムな数字が表示されます。
:::
`.gen_range(始まり..=終わり)`こんな感じで使います。
`1..=100`を`1..100`にすると1 ~ 99が生成されます。
くわしくは[こちら](https://rs.nkmk.me/rust-range/)を見てください

## 入力した文字列を数字に変換する

```rust
let input_number: i32 = input.parse().unwrap();
```

もし`input`変数が文字列に変換できない場合はエラーになります。
> 例

```rust
let parse_number = "47".parse().unwrap(); // ok
let parse_number = "Hello".parse().unwrap(); // error
```

# おわりに

記事にするほどのプログラムではないと思いますが、記事の練習として書いてみました。
今後もこのような内容の記事を書いていきたいと思っているので修正などのアドバイスを是非コメントでしてもらえるとありがたいです！
