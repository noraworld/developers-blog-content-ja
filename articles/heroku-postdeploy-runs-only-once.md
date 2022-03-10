---
title: "Heroku の postdeploy が動作しない原因"
emoji: "🦁"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Heroku"]
published: true
order: 130
layout: article
---

# 一言でわかる結論
**[`postdeploy` はアプリの作成後に一度だけ実行される！](https://devcenter.heroku.com/ja/articles/github-integration-review-apps#:~:text=postdeploy%20%E3%81%AF%E3%82%A2%E3%83%97%E3%83%AA%E3%81%AE%E4%BD%9C%E6%88%90%E5%BE%8C%E3%81%AB%E4%B8%80%E5%BA%A6%E3%81%A0%E3%81%91%E5%AE%9F%E8%A1%8C%E3%81%95%E3%82%8C%E3%82%8B)**

これですべてを理解した場合は、これより先は読まなくても大丈夫。




# 原因と理由
Heroku では、リポジトリルートに `app.json` というファイルを作成し、その中にいろいろ記述することで Heroku 上でのデプロイ方法を指定することができる。

その中に `postdeploy` というものがある。これを利用することにより、Heroku 上でアプリがデプロイされたときに任意のコマンドを実行することができる。

```json:app.json
{
  // 省略
  "scripts": {
    "postdeploy": "RAILS_ENV=production bundle exec rails db:create && RAILS_ENV=production bundle exec rails db:migrate && RAILS_ENV=production bundle exec rails assets:precompile"
  }
  // 省略
}
```

上記の例では、Rails アプリケーションで、データベースを新規作成し、マイグレーションを実行し、アセットのプリコンパイルを実行している。

このようにアプリケーションのデプロイ後に任意のコマンドを実行できる便利な仕組みなのだが、いくら `app.json` を書き直しても動作しない。

その理由は、**`postdeploy` はアプリの作成後に一度だけしか実行されないからである**。つまり、一度デプロイされてから `app.json` を書き換えても、その内容は反映されないということ。

変更を反映させたい場合は、アプリを作成し直す必要がある。Review apps を利用している場合は、[一度 PR をクローズして再オープンすることでアプリを再作成することができる](https://devcenter.heroku.com/ja/articles/github-integration-review-apps#:~:text=%E3%83%AC%E3%83%93%E3%83%A5%E3%83%BC%E3%82%A2%E3%83%97%E3%83%AA%E3%81%AE%20postdeploy%E2%80%8B%20%E3%82%B9%E3%82%AF%E3%83%AA%E3%83%97%E3%83%88%E3%82%92%E5%86%8D%E5%AE%9F%E8%A1%8C%E3%81%99%E3%82%8B%E3%81%AB%E3%81%AF%E3%80%81%E3%83%97%E3%83%AB%E3%83%AA%E3%82%AF%E3%82%A8%E3%82%B9%E3%83%88%E3%82%92%E3%82%AF%E3%83%AD%E3%83%BC%E3%82%BA%E3%81%97%E3%81%A6%E5%86%8D%E5%BA%A6%E3%82%AA%E3%83%BC%E3%83%97%E3%83%B3%E3%81%99%E3%82%8B%E5%BF%85%E8%A6%81%E3%81%8C%E3%81%82%E3%82%8A%E3%81%BE%E3%81%99%E3%80%82%E3%81%9D%E3%81%86%E3%81%99%E3%82%8B%E3%81%A8%E3%80%81Heroku%20%E3%81%AB%E3%82%88%E3%81%A3%E3%81%A6%E3%83%AC%E3%83%93%E3%83%A5%E3%83%BC%E3%82%A2%E3%83%97%E3%83%AA%E3%81%8C%E7%A0%B4%E6%A3%84%E3%81%95%E3%82%8C%E3%80%81%E5%86%8D%E4%BD%9C%E6%88%90%E3%81%95%E3%82%8C%E3%81%BE%E3%81%99%E3%80%82)。




# 余談
ちなみに、`https://dashboard.heroku.com/apps/<APP_NAME>/app-json` にアクセスすると、`Output` というセクションに JSON ファイルがある。

この画面にある JSON は、`app.json` に似て非なるものであるようだ。

| Heroku 画面の `Output` |
| --- |
| ![](https://raw.githubusercontent.com/noraworld/developers-blog-media-ja/master/heroku-postdeploy-runs-only-once/Screen%20Shot%202022-03-10%20at%2020.11.55.png) |

| `app.json` |
| --- |
| ![](https://raw.githubusercontent.com/noraworld/developers-blog-media-ja/master/heroku-postdeploy-runs-only-once/Screen%20Shot%202022-03-10%20at%2020.13.06.png) |

この画面上の JSON の `"scripts"` の中身が空 (`{}`) になっていたとしても、`app.json` で正しく記述されており、かつ、アプリの初回作成時であれば、`"scripts"` (`"postdeploy"`) の中身が正しく実行されるはずだ。







# 参考サイト
* [レビューアプリ (新しいバージョン)](https://devcenter.heroku.com/ja/articles/github-integration-review-apps)
