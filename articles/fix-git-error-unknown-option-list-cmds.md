---
title: "Git でサブコマンドを補完しようとしたときに Unknown option: --list-cmds と表示される問題の解決法"
emoji: "🦁"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Git", "git-completion", "git-completion.bash", "git-v2.18", "list-cmds"]
published: true
order: 51
---

# 症状
本日新しい MacBook Pro が届き、早速、環境構築をしていたのですが、[git-completion](https://github.com/git/git/tree/master/contrib/completion) を導入したところ、Git の補完でエラーが発生してしまいました。

```bash
$ git com  # <- ここでタブキーを押して git commit を補完しようとしたら下記のようなエラーが発生
Unknown option: --list-cmds=list-mainporcelain,others,nohelpers,alias,list-complete,config
```

# 原因
エラーの内容をコピペしてググったら以下の記事が一番最初に出てきました。

[.git-completion.bash producing error on macOS Sierra 10.12.6](https://apple.stackexchange.com/questions/327817/git-completion-bash-producing-error-on-macos-sierra-10-12-6)

アンサーを見てみると、どうやら Git 2.18 から新しく `--list-cmds` というオプションが追加されたようで、それが `git` コマンド側で認識されていないためにこのような原因が起こるようです。

このような問題が起きてしまった理由としては、ぼくの場合は、`git` コマンドを使用する際の Git (`/usr/bin/git`) と git-completion を使用する Git (`/usr/local/git`) が異なっていて、それぞれで別のバージョンを使用してしまったためでした。`git` コマンドを使用する Git のバージョンは 2.15.1 で、git-completion を使用する Git のバージョンが 2.18 だったので、2.18 で追加された新オプションを `git` コマンドを使用するほうの Git が認識していなかったためです。

# 解決策
原因がわかったらやることはシンプルで、単純に Git のバージョンをそろえてあげれば OK です。今回はとりあえず git-completion に新オプションが追加される前の v2.17.1 にダウングレードすればよさそうなので、v2.17.1 にチェックアウトしてみます。

```bash
$ cd /usr/local/git     # git-completion をクローンしたパスに置き換えてください
$ git checkout v2.17.1  # 補完はまだ効かないので注意！
```

チェックアウトしたらシェルを再起動します。

`git com` などと入力したあとにタブキーで補完して、エラーが出ずに `git commit` が補完されたら問題ないと思います。お疲れ様でした。
