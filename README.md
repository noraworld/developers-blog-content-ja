# 技術記事置き場 (日本語)
日本語の技術記事のマスタデータを管理するためのリポジトリである。

## Git リポジトリで管理する理由
Qiita のような技術記事を投稿するプラットフォームはいろいろあるし、なんなら技術記事専用のプラットフォームでなくとも、はてなブログのようなブログプラットフォームに投稿することもできる。

しかし、これらのプラットフォームがサービス終了したり、または、すでに利用しているプラットフォームよりも魅力的に感じた別のプラットフォームを見つけたりした際に、今まで投稿したすべての記事を移行しなければならない。サービスが終了してしまえばデータは失われるし、別のプラットフォームに移行する際はデータの移行がめんどくさいと感じる。データの安全性・利便性を考慮して、技術記事のマスタデータは Git で管理したいと思った。

## Git リポジトリで管理しようと思ったきっかけ
きっかけとしては、メインで利用するプラットフォームを Qiita から Zenn に移行しようと思ったことである。そのため、技術記事のマスタデータとしての役割を果たすが、現時点ではディレクトリ構成やファイル構成などは Zenn のフォーマットに準拠する形で管理されている。

2016 年 2 月 16 日に Qiita に技術記事を初投稿してから 2021 年 2 月 20 日現在でちょうど 5 年経つが、この 5 年間、技術記事は Qiita に投稿してきた。

Qiita は SEO がとにかく強力で、かなりの頻度で Qiita の記事がヒットするし、検索クエリによっては自分自身の技術記事がトップに表示されることも稀ではない。だからたくさんの人に見てもらえるし、IT 企業へのアピールにも利用できるし、逆に「Qiita の記事を拝見しました」というメッセージを企業側からいただくこともある。

しかし、知名度や転職の材料にはなるが、いくら Qiita で記事を投稿しても、お金を稼ぐことはできない。それで Qiita から Zenn に移行しようと決めた。1 ヶ月ほど前に友人が Zenn に技術記事を投稿しているのを見て、Zenn について調べてみると、GitHub リポジトリをマスタデータとして記事を投稿できる点と、お金を稼ぐことができる点に魅力を感じて移行を決意した。

「移行」という言葉を使用しているが、実際には Qiita はやめない。今のところは、今後も継続して Qiita に投稿しようと思っている (つまり Qiita と Zenn の同時投稿) が、あくまでメインは Zenn で、Qiita はサブみたいな位置づけにしようと思っている。Qiita は知名度向上や転職材料のために継続し、Zenn は副業として技術記事でお金を稼ぐため、あるいはマスタデータとして Git で技術記事を管理するついでとして、開始する。

## リポジトリ内で使用する言語について
このリポジトリは日本語の技術記事を管理するためのリポジトリなので、README や Git のコミットメッセージも日本語で記述することにする。このリポジトリ内でわざわざ英語を使用する必要はないし、そのほうがわかりやすいと思うからである。
