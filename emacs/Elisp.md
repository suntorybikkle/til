# Emacs Lisp

Emacsを拡張するためのプログラミング言語
`C-x C-e`を関数の末尾で使うと、関数を評価でき、結果はミニバッファに出現する
もっと試したい場合はscratchなどを使う

## 基本構文

```el
(+ 1 2 3 4)
```

- 括弧で表現されたS式と呼ばれる記述方式を使う
- 括弧の戦闘が関数名で残りが引数となる
- 上記のS式を評価すると10が戻り値として返される

## 変数宣言

Emacs-Lispでは名前空間が一つしかない点に注意

```el
(setq inhibit-startup-screen t)
```

- `setq`関数は左の設定項目である変数に、値を代入する
- 上記例では、初期画面を表示しない設定をしている
- `t`は設定オン、`0`は設定オフ

```el
(defvar a 1)		; a=1 値の初期化 (setqと同じで左の変数に値を代入する)
(defvar a 2)		; a=1 変数は変更されない (defvarでは値の書き換えができない)
(setq a 3)		; a=3setqでは値が書き換えられる
(setq a 1 b 2 c 3)	; 同時に複数の変数を代入可能
```

一時変数にはletを用いる

```el
(setq x 1)
(let ((x (+ x 3))
      (y (+ x 2)))  ; 一時変数を2つ定義
      (+ x y))      ; 7

(let* ((x (+ x 3))  ; 似た機能をもつlet*がある
       (y (+ x 2))) ; y定義の段階でlet*の場合x=4となっている
       (+ x y))     ; 10

```

## if文, when文

```el
(if 条件 nil以外の場合 nilの場合)
```

- 条件がいわゆるtrueの時がnil以外の場合に相当する


```el
(when 条件 nil 以外)
```

- 条件自体も評価の対象であり、すべての関数が戻り値を返す
- 以下の例では、拡張機能を正しく読み込んだ場合のみ次を評価される

```el
(when (require 'php-mode nil t)
	(add-to-list 'auto-mode-alist '("\\.ctp\\'" . php-mode)) ; 読み込みに成功したときのみ評価される
```

## 関数定義

```el
(defun square (a)
  (* a a))

(square 3) ; 9
```

## Lexical Binding

- lexical-bindingは非nilで有効化される
  - ファイルのヘッダー行で`lexical-binding: t`を記述すればよい
- 有効化時は、関数で変数を利用する際に、利用側の構造内で定義された変数がしか利用できない
- 以下は有効化時の例

```el
(let ((x 1))    ; xはレキシカルにバインドされる。
  (+ x 3))
       ⇒ 4

(defun getx ()
  x)            ; この関数内では、xは“自由”に使用される。

(let ((x 1))    ; xはレキシカルにバインドされる。
  (getx))
  error→ Symbol's value as variable is void: 
```

## cl-lib

- Common Lispというプログラミング言語の関数が使える
- `(require 'cl-lib)`で読み込める

## 連想リスト

- keyとvalueをもつマップのこと
- `'((a . 1) (b . 2) (c . 3))`のように定義する
- `assoc`はkeyに一致するitemを取得できる
  - `(assoc 'b '((a . 1) (b . 2) (c . 3)))`これで`(b . 2)`が取得可能

## `car`, `cdr`

- Emacsのリストは先頭部分とそれ以外でわかれている
- それぞれを取り出すのが`car`, `cdr`
- `(car '(1 2 3 4))` 1
- `(cdr '(1 2 3 4))` (2 3 4)
- 省略記法もある`(caddr '(1 2 3 4))` 3
- 一方結合`(cons 1 '(2 3))` (1 2 3)

## `mapcar`

- 関数を引数にとる高階関数
- 引数のリストに対して、関数をそれぞれ適用する
- `(mapcar '1+ '(1 2 3))`これで`(2 3 4)`が取得可能
