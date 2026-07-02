---
marp: true
theme: gaia
paginate: true
---

<!-- _class: lead -->

# Feel FP essence

関数型プログラミング言語のエッセンスを感じる

2026-07-03

Webナイト宮崎 vol.22 〜てげ推し技術を語りたい〜

---
![bg right 80%](https://github.com/mather.png)

# 自己紹介

- 桑畑英資(43)
- 関数型プログラミング言語が好み
- Elm推し
- 認定こども園の園長 ←New!


---

# Elm: レコード構文

プロパティを一部だけ変更した「新しい値」を返す

```elm
type alias Model = { count: Int }

countup : Model -> Model
countup model = { model | count = model.count + 1 }
```

再代入や値を直接書き換えることもできない「不変性(immutable)」が言語レベルで制約になっている

---

# PHP 8.5 : Clone With

最近同じような機能を持つ `clone()` の使い方が導入された

```php
/* PHP 8.2 : readonly class */
readonly class Color {
    public function __construct(
        public int $red,
        public int $green,
        public int $blue,
        public int $alpha = 255,
    ) {}
}

clone($color, ['alpha' => 128]); /* プロパティを変えた新しい値を返す */
```

---

# Elm: 代数的データ型

`Shape` 型の取りうるパターンを列挙

```elm
type Shape
    = Circle Float
    | Rectangle Float Float

Circle 1.0  {- Shape型の値 -}
Rectangle 210.0 294.0
```

---
# Elm: パターンマッチング

```elm
pi : Float
pi = 3.14159265358979323846264338327950288419716939937510

shapeArea : Shape -> Float
shapeArea shape =
    case shape of
        Circle r -> 
            pi * r * r

        Rectangle w h -> 
            w * h
```

`r`, `w`, `h` などの変数に実際の値が束縛(binding)される

---

# Java 21 : sealed, record

2023年のJava21アップデートで同じような機能が追加された

```java
public sealed interface Shape permits Circle, Rectangle {}

public record Circle(double radius) implements Shape {}
public record Rectangle(double width, double height) implements Shape {}
```

（記述量はちょっと多いけど…）

---

# Java 21 : Pattern matching for switch

`switch` 式（あるいは文）で変数への束縛ができる

```java
switch (shape) {
  case Circle c    -> Math.PI * c.radius() * c.radius();
  case Rectangle r -> r.width() * r.height();
}
```
- `case default` が不要になっているのがポイント
- `switch` だとメンバー変数(`radius`など)の束縛まではできないっぽい

---
# Java 21 : Record Pattern

```java
if (shape instanceof Rectangle(double width, double height)) {
  return width * height;
}
```

`instanceof` ではメンバー変数の束縛が使えるようになっている

---

# PHP : [RFC] 代数的データ型

https://wiki.php.net/rfc/adts

PHPでもパターンマッチングなどを含む代数的データ型を実装すべきか、という議論は進められている

```php
// Capturing values out of a pattern and binding them to variables if matched.
$p is Point(x: 3, y: $y); // If $p->x === 3, bind $p->y to $y and return true.
$assoc is ['a' => 'A', 'b' => $b];
// If $assoc['a'] === 'A', bind $assoc['b'] to $b and return true.
```

---

# まとめ

これまで変数への再代入やmutableなオブジェクトを中心としてきたプログラミング言語でも、不変なデータ定義や代数的データ型が選択肢として使えるようになる例は多くなってきている。

関数型プログラミング言語をちょっと学んでおくと「〇〇ゼミで見たやつだ！」となるかも。

ということで **#miyazaki_fp** いかがですか？

https://miyazaki-fp.connpass.com/

---

## 参考

[PHP 8.5: Clone With](https://www.php.net/releases/8.5/ja.php#clone-with)

[PHP RFC: Algebraic Data Types](https://wiki.php.net/rfc/adts)

[Java 21: シール・クラス](https://docs.oracle.com/javase/jp/21/language/sealed-classes-and-interfaces.html)

[Java 21: パターン・マッチング](https://docs.oracle.com/javase/jp/21/language/pattern-matching.html)