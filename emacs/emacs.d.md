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

