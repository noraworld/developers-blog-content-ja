# 技術記事置き場 (日本語)
日本語の技術記事のマスタデータを管理するためのリポジトリである。

## Git リポジトリで管理する理由
Qiita のような技術記事を投稿するプラットフォームはいろいろあるし、なんなら技術記事専用のプラットフォームでなくとも、はてなブログのようなブログプラットフォームに投稿することもできる。

しかし、これらのプラットフォームがサービス終了したり、または、すでに利用しているプラットフォームよりも魅力的に感じた別のプラットフォームを見つけたりした際に、今まで投稿したすべての記事を移行しなければならない。サービスが終了してしまえばデータは失われるし、別のプラットフォームに移行する際はデータの移行がめんどくさいと感じる。データの安全性・利便性を考慮して、技術記事のマスタデータは Git で管理したいと思った。

## Git リポジトリで管理しようと思ったきっかけ
きっかけとしては、メインで利用するプラットフォームを Qiita から Zenn に移行しようと思ったことである。そのため、技術記事のマスタデータとしての役割を果たすが、現時点ではディレクトリ構成やファイル構成などは Zenn のフォーマットに準拠する形で管理されている。

2016 年 2 月 16 日に Qiita に技術記事を初投稿してから 2021 年 2 月 20 日現在でちょうど 5 年経つが、この 5 年間、技術記事は Qiita に投稿してきた。

Qiita は SEO がとにかく強力で、かなりの頻度で Qiita の記事がヒットするし、検索クエリによっては自分自身の技術記事がトップに表示されることも稀ではない。だからたくさんの人に見てもらえるし、IT 企業へのアピールにも利用できるし、逆に「Qiita の記事を拝見しました」というメッセージを企業側からいただくこともある。

しかし、知名度や転職の材料にはなるが、いくら Qiita で記事を投稿しても、お金を稼ぐことはできない。それで Qiita から Zenn に移行しようと決めた。1 ヶ月ほど前に友人が Zenn に技術記事を投稿しているのを見て、Zenn について調べてみると、GitHub リポジトリをマスタデータとして記事を投稿できる点と、お金を稼ぐことができる点に魅力を感じて移行を決意した。実際、Zenn としても、[情報を発信するエンジニアが対価を得られるようにすることをコンセプトとしている](https://zenn.dev/about)。

「移行」という言葉を使用しているが、実際には Qiita はやめない。今のところは、今後も継続して Qiita に投稿しようと思っている (つまり Qiita と Zenn の同時投稿) が、あくまでメインは Zenn で、Qiita はサブみたいな位置づけにしようと思っている。Qiita は知名度向上や転職材料のために継続し、Zenn は副業として技術記事でお金を稼ぐため、あるいはマスタデータとして Git で技術記事を管理するついでとして、開始する。

## リポジトリ内で使用する言語について
このリポジトリは日本語の技術記事を管理するためのリポジトリなので、README や Git のコミットメッセージも日本語で記述することにする。このリポジトリ内でわざわざ英語を使用する必要はないし、そのほうがわかりやすいと思うからである。

## Zenn における記事執筆時のルールについて
### HTML を使用しない
Qiita と違って、Zenn では HTML を解釈しないので、以下のような理由で HTML を使用することはできない。

* リンクを新しいタブで開かせる
* 画像のサイズを設定する
* 文字を着色する
* `<details>` タグを利用した、折りたたみ表現

### 絵文字はマルチバイト文字を使用する
Zenn では `:smile:` のようなショートコードでの記法では絵文字に変換されない。そのため `😀` のようにマルチバイトの絵文字を直接入力する必要がある。

### 自分自身の他の技術記事のリンク・記事内リンクに絶対パスを使用しない
絶対パスを指定すると Zenn の URL になってしまうため、相対パスで表現する。

これにより、どこに記事を公開しても、自分自身の他の技術記事のリンクや記事内のリンクは常に参照できるようになる。今後、Zenn 以外のプラットフォームに移行する際に少しでも楽をするため。

ただし、自分自身の他の技術記事に関しては、パスの部分がファイル名になっている必要があるため、プラットフォーム独自に記事 ID を生成して URL に使用している場合には対応不可。たとえば Qiita など。

### 画像を Zenn にアップロードしない
他のプラットフォームへの移行柔軟性を考えると、画像もプラットフォームに直接アップロードするのは得策ではない。

なぜなら、そのプラットフォームが運営されている限りは、画像の URL に外部 URL を使用しても参照できるが、そのプラットフォームが終了してしまうとリンク切れになってしまうからである。

そのため、画像も GitHub に置きたいと考えているが、GitHub はストレージサービスではないため、1 つのリポジトリであまり多くのストレージを使うのは良くない。

そのため、画像専用のリポジトリを作り、そこに画像を置くのが良いのではないかと考えている。そのリポジトリのストレージを使いすぎてしまったら、また新しいリポジトリを作成する、といった具合に。

### Zenn 独自の Markdown 拡張記法はなるべく使用しない
絶対に、ではないが、なるべく Zenn オリジナルの記法は使用しないように心がける。理由は以下の通り。

* 他のプラットフォーム (Qiita) との互換性のため
* Zenn から別のプラットフォームに今後移行することになった際に記事を修正する手間を極力減らすため

### topics に記号は利用可能
topics に記号が入っていると、`zenn preview` ではエラーが出るが、一部の記号は無視しても問題なさそう。例としては以下のようなものが挙げられる。

* Node.js
* docker-compose

上記の 2 つはアイコンもちゃんと設定されていたので、むしろ積極的に使うべき topics だろう。

ただし、不用意に記号を入れたり、以下のようにバージョンを入れたりするのは得策ではないだろう。

* Ruby3
* Ruby-3
* Ruby3.0
* Ruby-3.0

見た目があまり美しくないというのと、アイコンが設定されないため。

### 記事のタイトルは 70 文字まで
60 文字を超えると `zenn preview` でエラーが出るが、実際には 70 文字まで設定可能。60 文字を超えてもプレビューでエラーが出るだけで、デプロイは成功する。

しかし 70 文字を超えるとデプロイに失敗し記事が公開されない。

### slug のルール
slug (ファイル名、かつ、Zenn の記事の URL になるもの) は以下のルールに従う。

* 半角英数字とハイフンの組み合わせ
* 12 〜 50 文字の範囲内

これは `zenn new:article --slug <SLUG>` を実行せずに手動でファイル名を作ったとしても、このルールを守っていないとエラーになって Zenn に投稿できない。
