# bash の設定ファイルについて

bash の設定ファイルとして、`~/.bash_profile` や `~/.bashrc` などがあるが、それぞれの役割・使い方についてのまとめ

## 設定ファイルの読み込まれ方

- bash が対話的なログインシェル or `bash --login` で起動
  - `/etc/profile` があれば読み込み実行
  - `~/.bash_profile`, `~/.bash_login`, `~/.profile` の優先順位でファイルを探し、最初に見つかったものを読み込み実行
    - 2番目以降は読まれない
- ログインシェル終了時
  - `~/.bash_logout` があれば読み込み実行
- ログインシェルではない対話的なシェルとして起動
  - `~/.bashrc` があれば読み込み実行

## 現在の設定方針

- `~/.bash_profile`
  - `~/.profile` と `~/.bashrc` を読み込むだけ

```bash:~/.bash_profile
# git bash の書き方？
test -f ~/.profile && . ~/.profile
test -f ~/.profile && . ~/.bashrc
```

- `~/.profile`
  - シェルに依存しないもの (環境変数など) を記述
- `~/.bashrc`
  - bash でしか使わないものを記述
  - ハマることがあるため、標準出力・エラー出力が出るものは書いてはダメらしい

## 参考文献

- [日本語 man bash](http://linuxjm.osdn.jp/html/GNU_bash/man1/bash.1.html#lbAH)
- [Bash: .bashrcと.bash_profileの違いを今度こそ理解する](https://techracho.bpsinc.jp/hachi8833/2021_07_08/66396)
- [.bash_profile ? .bashrc ? いろいろあるけどこいつらなにもの？](https://qiita.com/hirokishirai/items/5a529c8395c4b336bf31)
