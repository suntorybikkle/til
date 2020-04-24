# .emacs.d

設定方法、内容などについてつらつら

## 変数について

- `user-emacs-directory`
  - `.emacs.d`のディレクトリを指す
- `window-system`
  - 名前の通りwindow-systemがわかる
  - x, w32, mac, nsがあるらしい
  - `-nw`で実行したときは`nil`でした
- `system-typee`
  - OSの設定がわかる
  - wsl環境では`gnu/linux`普通にlinuxということ
- `tool-bar-mode`
  - `tool-bar-mode -1`でツールバー(新規ファイルとかののアイコンがある場所)をなくせる
  - GUIモードで実行しているならばなくしてよさそう
- `menu-bar-mode`
  - 上記と同様に`-1`でメニューバー(Fileとかのある場所)をなくせる
- `scroll-bar-mode`
  - 同様に右のスクロールバーをなくせる
- `load-path`
  - Lispファイルを読み込に行くディレクトリ一覧
  - パッケージ管理ツールで使われるディレクトリなどは最初から含まれている
  - 自分で書いたLispパッケージなどのための独自ディレクトリを読み込みたいときに追加する
  - `(add-to-list 'load-path "/hogehoge")`

## 関数などについて

- `expand-file-name` filename &optional directory
  - filenameを絶対パスで表示する
  - directoryがあれば、filenameをその後ろに展開する
- `provide`
  - 自作Lispファイルを読み込むには`load-path`に配置するだけでは`require`で取得できない
  - `provide 'hoge`のように定義することで、`hoge.el`を`require 'hoge`可能になる
- `package-installed-p` package
  - packageがインストール済みであれば`t`他は`nil`を返す

## テクニック

- Emacsのバージョン毎にパッケージ管理のディレクトリを分ける
  - `package-user-dir`はElpaのインストール先、読み込み先を指す

```el
(let ((versioned-package-dir
       (expand-file-name (format "elpa-%s.%s" emacs-major-version emacs-minor-version)
                                user-emacs-directory))) ; Emacsのバージョンを指定したディレクトリ名
				(setq package-user-dir versioned-package-dir))
```


