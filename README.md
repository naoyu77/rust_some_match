# Rust match の学習メモ

## match の基本

`match` はどんな型にも使える分岐の仕組み。enum、整数、文字列、boolなど何でも使える。

```rust
match 調べたい値 {
    パターン1 => 処理1,
    パターン2 => 処理2,
}
```

## Option型に対するmatch

`Option` は enum で、`Some(値)` か `None` のどちらか。

```rust
let six = Some(6);

match six {
    Some(v) => println!("{}", v),       // Some なら中身を v に入れて使える
    None => println!("値がありません"),   // None ならこっち
}
```

## 整数に対するmatch

```rust
let dice_roll = 9;
match dice_roll {
    3 => add_fancy_hat(),
    7 => remove_fancy_hat(),
    other => move_player(other),  // それ以外 → other に値が入る
}
```

## パターンの網羅性

Rustの `match` はすべてのパターンを網羅しないとコンパイルエラーになる。

## catch-allパターンとプレースホルダー

`match` では個別に書かなかったパターンをまとめて受け取る方法が2つある。

### catch-allパターン（変数名を使う）

具体的な値でもキーワードでもない名前を書くと、自動的に変数として扱われ、残り全部に一致する。
値が変数に入るので、処理の中で使える。

```rust
match dice_roll {
    3 => add_fancy_hat(),
    7 => remove_fancy_hat(),
    other => move_player(other),  // 3でも7でもない値が other に入る
}
```

`other` は特別なキーワードではなく、ただの変数名。`n` でも `x` でも好きな名前でOK。

### プレースホルダー `_`

値を使わない場合は `_` で捨てる。

```rust
match dice_roll {
    3 => add_fancy_hat(),
    7 => remove_fancy_hat(),
    _ => (),  // それ以外は何もしない（値も使わない）
}
```

### 違いまとめ

- catch-all（`other` など） → 残り全部に一致 + 値を**使える**
- プレースホルダー（`_`） → 残り全部に一致 + 値を**捨てる**

変数名をつけて値を使わないと、コンパイラが「使ってないよ」と警告を出す。
値を使わないなら `_` を使うのが正しい書き方。

```rust
// ❌ 警告が出る（other を使っていない）
other => (),

// ✅ 正しい（値を使わないなら _ で捨てる）
_ => (),
```

どちらも**必ず最後に書く**。先に書くと後のパターンに到達できなくなり、コンパイラが警告を出す。

## ユニット値 `()`

`()` は「何もない」を表す値。他の言語の `void` に近い。

```rust
_ => ()  // それ以外のときは何もしない
```

`println!` など、返す値が特にない関数は `()` を返す。

## match の返り値

最後の式にセミコロン `;` をつけなければ、match の結果が関数の返り値になる。

```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        None => None,
        Some(i) => Some(i + 1),
    } // ← セミコロンなし = これが返り値
}
```
