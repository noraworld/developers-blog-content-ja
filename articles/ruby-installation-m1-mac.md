---
title: "M1 Mac (macOS Monterey) で Ruby のインストールに失敗するときの対処法"
emoji: "💎"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["ruby", "mac", "macos"]
published: true
order: 154
layout: article
---

# 解決方法
M1 Mac で従来どおりに Ruby をインストールしようとするとビルドエラーが出てしまいます。

筆者の環境では以下のコマンドを実行することにより無事インストールすることができました。

まず、必要なパッケージをインストールします。

```shell:Shell
brew install openssl@1.1 readline libyaml
```

次に必要な環境変数をセットします。

```shell:shell
export RUBY_CONFIGURE_OPTS="--with-openssl-dir=$(brew --prefix openssl@1.1)"
export LDFLAGS="-L/opt/homebrew/opt/readline/lib"
export CPPFLAGS="-I/opt/homebrew/opt/readline/include"
export PKG_CONFIG_PATH="/opt/homebrew/opt/readline/lib/pkgconfig"
export optflags="-Wno-error=implicit-function-declaration"
export LDFLAGS="-L/opt/homebrew/opt/libffi/lib"
export CPPFLAGS="-I/opt/homebrew/opt/libffi/include"
export PKG_CONFIG_PATH="/opt/homebrew/opt/libffi/lib/pkgconfig"
export RUBY_CFLAGS="-w"
```

これでインストールできるようになるはずです。

```shell:Shell
asdf install ruby <RUBY_VERSION>
```

上記の例では asdf を使用していますが、rbenv などでも同様にインストールできるようになると思います。

環境変数をエクスポートする代わりに実行時に指定することも可能です。

```shell:Shell
RUBY_CONFIGURE_OPTS="--with-openssl-dir=$(brew --prefix openssl@1.1)" LDFLAGS="-L/opt/homebrew/opt/readline/lib" CPPFLAGS="-I/opt/homebrew/opt/readline/include" PKG_CONFIG_PATH="/opt/homebrew/opt/readline/lib/pkgconfig" optflags="-Wno-error=implicit-function-declaration" LDFLAGS="-L/opt/homebrew/opt/libffi/lib" CPPFLAGS="-I/opt/homebrew/opt/libffi/include" PKG_CONFIG_PATH="/opt/homebrew/opt/libffi/lib/pkgconfig" RUBY_CFLAGS="-w" asdf install ruby <RUBY_VERSION>
```



# 参考サイト
* [Install older Ruby versions on a M1 MacBook?](https://stackoverflow.com/a/69012677)
* [Suggested build environment](https://github.com/rbenv/ruby-build/wiki#macos)
* [BUILD FAILED (macOS 12.0.1 using ruby-build 20211019)](https://github.com/asdf-vm/asdf-ruby/issues/232#issuecomment-1062141372)
* [m1macにasdf install ruby latestで苦戦しまくった話](https://blog.mrym.tv/2021/10/m1mac-asdf-install-ruby-latest-error/)
