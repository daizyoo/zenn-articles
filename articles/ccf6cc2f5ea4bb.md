---
title: "#2 Rustでまるばつゲーム"
emoji: ⭕️
type: tech
topics:
  - Rust
  - 初心者
  - Game
published: false
---

# はじめに

[前回]の記事で数当てゲームを作りました。
今回は2回目としてまるばつゲームを作ります。

## 対象読者

- プログラミング中級者
- Rustを勉強している人

## コード

[こちら]にあります。

# 本編

まずはプログラム全体の流れを決めていきます。
まるばつゲームのルールは以下の通りです。

- 3x3のマス目がある
- プレイヤーは交互に[o]か[x]をマス目に入力する
- 縦横斜めのどれかに3つ並べたプレイヤーの勝ち

## プログラムの流れ

次は全体の流れを決めていきます。

- ゲームスタート
- メインループ
  - フィールドを表示
  - プレイヤーの入力を受け取る
  - 勝敗を判定
  - 勝敗が決まったらゲーム終了
  - 勝敗が決まらなければターンを次のプレイヤーにし、フィールドを表示に戻る

要件がまとまったので実際にプログラムを書いていきます。

## Game構造体の作成

```rust
#[derive(Debug)]
struct Game {
    field: [[Option<bool>; 3]; 3],
    turn: bool,
}
```

`field`は3x3のマス目で、`turn`はプレイヤーのターンを表します。

```rust
impl Game {
    fn new() -> Self {
        Self {
            field: [[None; 3]; 3],
            turn: true,
        }
    }
}
```

これで`Game`構造体のインスタンスを作ることができます。

```rust
let game = Game::new();
println!("{:#?}", game);
```

## フィールド

`[[Option<bool>; 3]; 3]`は`Option<bool>`型の二次元配列で、
`field[0][2]`は1行目の3列目を表し、`field[1][2]`は2行目の3列目を表します。
`None`が空きマス、`Some(true)`が⭕️、`Some(false)`が❌を表します。

```rust
[
  None, None, None,
  None, None, None,
  None, None, None,
]
```

は

```rust
[ ][ ][ ]
[ ][ ][ ]
[ ][ ][ ]
```

になり、

```rust
[
  Some(false), None, Some(true),
  None, Some(true), None,
  Some(true), None, None,
]
```

は

```rust
[x][ ][o]
[ ][o][ ]
[o][ ][ ]
```

になります。

## フィールドの描画

```rust
fn draw(&self) {
    for y in self.field {
        for x in y {
            if let Some(b) = x {
                if b {
                    print!("[o]");
                } else {
                    print!("[x]");
                }
            } else {
                print!("[ ]");
            }
        }
        println!();
    }
}
```

フィールドの描画は`draw`メソッドで行います。
for文を二重で回すことで、フィールドの全てのマスを表示します。
一回目のfor文で横の行を、二回目のfor文で縦の列を表します。

`x`には`Option<bool>`型の値が入っています。
`if let Some(b) = x`で`x`が`Some(bool)`型の場合、`b`に`bool`を代入し、
`if b`で`b`が`true`の場合、`[o]`を出力し、`false`の場合、`[x]`を出力します。
`else`で`x`が`None`の場合、`[ ]`を出力します。

```rust
let game = Game::new();
game.draw();
```

## 入力

```rust
fn input() -> Result<usize, std::num::ParseIntError> {
    let mut input = String::new();
    std::io::stdin().read_line(&mut input).unwrap();
    input.trim().parse::<usize>()
}
```

[前回]: https://zenn.dev/daizyoo/articles/number-guessing "number-guessing"
[こちら]: https://github.com/daizyoo/tic-toc-toe "tic-toc-toe"
