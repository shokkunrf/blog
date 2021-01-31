---
title: "First Post"
date: 2020-12-21T21:53:52+09:00
draft: false
tags:
  - "Hugo"
---

# ブログを始めてみた

## なんでブログを始めたの？

一番の理由は、メモの管理に困ってたから。  
これまでは、メモをディレクトリで分けていて、量が増えるにつれて探すことが難しくなっていた。  
また、作業の目的が書いていなかったり、前提条件がわからなかったりで振り返れないメモがあった。  
これらを、タグ機能を使って記事を管理し、公開することを意識して書くことで解決できるのはないかと思い、ブログを始めてみることにした。

## なんで Hugo を選んだの？

Hugo を使っている友人がいたから、お互いに困ったときに情報共有できると思って選んだ。  
他にも[Hugo のリポジトリ](https://github.com/gohugoio/hugo)の更新も続いてるみたいだし、[Jamstack](https://jamstack.org/generators/)を見ても人気みたいだし。  
あと[この Qiita 記事](https://qiita.com/yotsak/items/017734d5f873f4f194d4)につられて 30 分でできると思ったから。  
まぁ 2 つ目以降の理由なんてみんな後付けなんだけども。

## Hugo でブログを始める手順

技術ブログっぽいものも書いてみる。  
手順といっても[Hugo のクイックスタート](https://gohugo.io/getting-started/quick-start/)に沿って進めるだけ。  
この記事では Hugo [v0.79.1](https://github.com/gohugoio/hugo/releases/tag/v0.79.1)を使い、Debian 10 を前提とする。

### 1. Hugo をインストールする

1. [Hugo Releases](https://github.com/gohugoio/hugo/releases)から OS にあったパッケージをダウンロードし、インストールする  
   今回は `hugo_0.79.1_Linux-64bit.deb` をインストールする。

2. hugo がインストールされているか確認する

   ```sh
   hugo version
   ```

### 2. サイトを作成する

1. GitHub でリポジトリを作成する  
   今回は`blog`というリポジトリを作った。

2. ローカルにリポジトリをクローンする

   ```sh
   git clone git@github.com:shokkunrf/blog.git
   cd blog
   ```

3. リポジトリ内にサイトを作成する  
   `foo`は適当に付けた。好きな文字列で OK。

   ```sh
   hugo new site foo
   mv ./foo/* ./
   rm -R ./foo
   ```

### 3. テーマを追加する

1.  [Hugo のスタイルテーマ](https://themes.gohugo.io/)からテーマを選ぶ  
    クイックスタートにあった[Ananke](https://themes.gohugo.io/gohugo-theme-ananke/)を使ってみる。

2.  選んだテーマの GitHub リポジトリを`themes/`に追加する  
    `submodule`を使って追加することで元のテーマが更新されても簡単に適用できる。

    ```sh
    git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke
    ```

3.  設定を追加する  
    [Ananke の Getting Start](https://themes.gohugo.io/gohugo-theme-ananke/)や[config.toml のサンプル](https://github.com/theNewDynamic/gohugo-theme-ananke/blob/master/exampleSite/config.toml)を参考に`config.toml`を変更する。  
    とりあえず、以下さえあればローカルでの表示はできる。

    ```config.toml
    baseURL = "http://example.org/"
    languageCode = "en-us"
    title = "My New Hugo Site"
    theme = "ananke"
    ```

### 4. 記事を追加する

1. 記事を生成する
   `content`以下に`posts/first-post.md`が生成される。

   ```sh
   hugo new posts/first-post.md
   ```

2. 記事を編集する
   `draft: true`のままだと公開できないため注意。

   ```first-post.md
   ---
   title: "First Post"
   date: 2020-12-21T21:52:52+09:00
   draft: false
   tags:
   - hugo
   ---
   ここから本文
   ```

### 5. サーバを起動する

1. サーバを起動する

   ```sh
   hugo server -D
   ```

2. ブログを表示する  
   [`http://localhost:1313/`](http://localhost:1313/)にアクセスするとブログを表示できる。  
   `config.toml`を以下のように変更している場合、[`http://localhost:1313/path/`](http://localhost:1313/path/)にアクセスする必要がある。
   ```config.toml
   baseURL = "http://example.org/path"
   ```

### 6. サイトを生成する

1. 生成するディレクトリを変更する  
   GitHub Pages では`docs`以下を pages に公開する設定があるため、それに合わせて`config.toml`に以下を追加する。

   ```config.toml
   publishDir = "docs"
   ```

2. サイトを生成する

   ```sh
   hugo -D
   ```

### 7. サイトを公開する

1. リモートリポジトリに push する

   ```sh
   git push origin HEAD
   ```

これで完成！

## Hugo を使ってみた感想

ちょっと設定書いて、ちょっとコマンド叩くだけで完成！簡単！  
Hugo をライブラリだと思い込んでいたけど、ツールだったんだね。  
生成するにあたって、Golang でゴニョゴニョするのかと思っていたからちょっと身構えてた。

## ブログに対する意気込み

Twitter より濃いくらいのメモを IT に限らず書いていく。
(Twitter も短い Blog みたいな触れ込みだったね)  
とりあえず、140 字以上は書いたのでここまで。
