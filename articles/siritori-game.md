---
title: "#2 Rustでしりとり"
emoji: "🍑"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Rust", "初心者", "game"]
published: true
---

## はじめに

[前回]に続き、初心者向けにプログラムを書いたので記事にしてみました。
今回「しりとり」を作ってみました。

プログラムの全体は最後に載せるのですが[GitHub]にもあります。

## 本編

### 変数宣言

まず最初に入力した単語を記録する変数と、現在のプレイヤーを記録する変数を宣言します。

```rust
// 入力した単語を保存する配列
let mut content: Vec<String> = vec![];
// プレイヤー
let mut player = true;
```

player1なら`true`、player2なら`false`表します

```rust
println!("プレイヤー: {}", if player { 1 } else { 2 });
```

### 最初の単語

```rust
// 最初の単語
let first_section = "しりとり".to_string();
// 最後の一文字
let mut previous_last = last(&first_section);
```

`first_section`変数には最初の単語が入っています。
`previous_last`変数には`first_section`の最後の一文字が入っています。
[last関数]に文字列を渡すと、最後の一文字が戻り値として返されます。

### last関数

```rust
fn last(section: &str) -> char {
    let chars: Vec<char> = section.chars().collect();
    chars[chars.len() - 1]
}
```

`section`を`chars`メソッドを使い一文字区切りにします

```rust
let section = "こんにちは";
let chars: Vec<char> = section.chars();
println!("{:?}", chars);
// 出力結果
// ['こ', 'ん', 'に', 'ち', 'は']
```

そしてその配列の最後の要素を取り出すことで最後の一文字を取り出せます

:::details 他のやり方

他にもこんなやり方があります

```rust
let section = "こんにちは";
let last = section.chars().last().unwrap();
println!("{}", last);
```

この方法は`chars`メソッドを使った後に一度配列にするのではなく、`chars`メソッドの戻り値は`Iterator`トレイトを実装している型なので`last`メソッドを呼び出しています。
`last`メソッドの説明は[こちら]

:::

### 最初の単語の表示と保存

```rust
println!("{}", first_section);
content.push(first_section);
```

この行では最初の単語を表示し、`content`変数に最初の単語を保存しています。
`push`メソッドは配列の最後に要素を追加します。

### 単語の入力

```rust
fn input() -> String {
    let mut line = String::new();
    std::io::stdin().read_line(&mut line).unwrap();
    line.trim().to_string()
}
```

この関数を呼び出すとターミナルで入力した文字列を取得することができます。

難しいので説明はしまっておきます。

:::details 説明

```rust
let mut line = String::new();
```

入力した文字列を入れるための配列を作成します。

```rust
std::io::stdin().read_line(&mut line).unwrap();
```

この行は少し難しいのですが、簡単に説明します。

`&line`と`&mut line`の違いは変更できるかできないか、です。

```rust
std::io::stdin().read_line(&line).unwrap();
```

:::

```rust
let input = input();
```

この行で入力した値を`input`変数として宣言します。

### 入力した単語の判定

#### check関数

```rust
fn check(section: &str, previous_last: char) -> bool {
    let chars: Vec<char> = section.chars().collect();

    if chars[0] == previous_last {
        return true;
    } else {
        return false;
    }
}
```

このは第一引数の文字列の最後が第二引数と一致するかを判定する関数です。

```rust
if input.is_empty() || !check(&input, previous_last) {
    println!("{}で始まる単語を入力してください", previous_last);
    continue;
}
```

入力した単語が空白、最初の文字が前回の単語の最後の文字か、のどちらかが`true`なら中のif文を実行します。

`input.is_empty()`は`input`が空白なら`true`を返すので、先に判定することでもし空白ならすぐに`if`文内のプログラムを実行します。
その後、[check関数]を使用して前回の単語の最後の文字から始まっているか確認します。
もし一致しなかったら`{前回の単語の最後}で始まる単語を入力してください`と表示してもう一度入力させます。

`input.is_empty() || !check(&input, previous_last)`がどちらとも`false`ならそのまま処理を進めていきます。

### previous_lastの更新

次に単語の最後の文字を更新します。
入力した単語の最後の文字を[last関数]を使用して取得します。

```rust
previous_last = last(&input);
```

### 単語を記録

```rust
content.push(input.clone());
```

### プレイヤーのターン変更

```rust
// !true == false
// !false == true
player = !player;
```

プレイヤーは`bool`型で表現されているので、`!`演算子を使って意味を反転させて代入します。

### game_over_check関数

```rust
fn game_over_check(section: &str) -> bool {
    let last = last(section);
    if last == 'ん' {
        return true;
    } else {
        return false;
    }
}
```

この関数は引数の文字列の最後の文字が「ん」だったら`true`を返します。

### 「ん」がついたか判定

```rust
if game_over_check(&input) {
    println!("`ん`がついたので負けです");
    break;
}
```

入力した文字列を[game_over_check関数]にわたして`true`ならメッセージを表示してループを終了します。

## おわりに

修正やアドバイスがあればぜひコメントお願いします！

:::details 全体

```rust
fn main() {
    let mut content: Vec<String> = vec![];
    let mut player = true;

    let first_section = "しりとり".to_string();
    let mut previous_last = last(&first_section);

    println!("{}", first_section);
    content.push(first_section);

    loop {
        println!("プレイヤー: {}", if player { 1 } else { 2 });
        let input = input();

        if input.is_empty() || !check(&input, previous_last) {
            println!("{}で始まる単語を入力してください", previous_last);
            continue;
        }

        previous_last = last(&input);

        content.push(input.clone());
        player = !player;

        if game_over_check(&input) {
            println!("`ん`がついたので負けです");
            break;
        }
    }
    println!("プレイヤー{}の負けです", if player { 1 } else { 2 });
}

fn game_over_check(section: &str) -> bool {
    let last = last(section);
    if last == 'ん' {
        return true;
    } else {
        return false;
    }
}

fn check(section: &str, previous_last: char) -> bool {
    let chars: Vec<char> = section.chars().collect();

    if chars[0] == previous_last {
        return true;
    } else {
        return false;
    }
}

fn last(section: &str) -> char {
    let chars: Vec<char> = section.chars().collect();
    chars[chars.len() - 1]
}

fn input() -> String {
    let mut line = String::new();
    std::io::stdin().read_line(&mut line).unwrap();
    line.trim().to_string()
}
```

:::

[last関数]: (#last関数)
[check関数]: (#check関数)
[game_over_check関数]: (#game_over_check関数)

[GitHub]: https://github.com/daizyoo/siritori.git
[前回]: https://zenn.dev/daizyoo/articles/number-guessing
[こちら]: https://qiita.com/lo48576/items/34887794c146042aebf1#last-iteratort---optiont
