+++
title = "Hugoブログ"
subtitle = "インストールからデプロイまで"
date = "2017-03-26T00:46:34-07:00"
tags = ["Blog","Hugo"]

+++

### Hugo & GitHub Pages


せっかくHugoでブログを書いているので、HugoのインストールからGitHub Pagesにデプロイするまでの方法を紹介します。
公式チュートリアルは全部英語なので、これからHugoを使おうと思っている人の参考になれば。自分用にもメモ。

<br>

### Hugoとは

一般的にウェブサイトは、ユーザーがページを訪問する度にウェブサーバが動的にHTMLファイルを生成する仕組みになっています。Hugoは、Markdownで書かれたコンテンツをHTMLとしてレンダリングし、ウェブサイトを事前に生成する静的サイトジェネレータです。ウェブサーバが動的にページをビルドする必要がないため、ページが表示されるまでの時間が短縮されます。静的サイトジェネレータはGo言語で書かれたHugoの他にも、Rubyで書かれたJekyllやOctopressなどがあります。


###  ローカルでセットアップ


①〜④でHugoのインストールからテーマの適用までをカバーします。<br>
＊このステップが既に完了している方はこちらへ → [HugoをGitHub Pagesにデプロイする](#ref-1)


**① Hugoをインストール** 

[ここから](https://github.com/spf13/hugo/releases)最新verをダウンロードするか、Homebrewを使って``` $ brew install hugo ```でインストールしてください。


**② 新しいプロジェクトを作成**

```  $ cd ```でホームディレクトリに移動して、```  $ hugo new site <project> ``` で新しいプロジェクトを作ります。
```<project> ```にはプロジェクト名を入れてください。




**③ テーマを選択** 

[Hugo Themesレポジトリ](https://github.com/spf13/hugoThemes)から、テーマを選んでthemeディレクトリにクローンしてください。
```<theme_URL> ```はテーマリポジトリのURLと置き換えてください。


<pre><code=shell>$ cd themes  
$ git clone &lt;theme_URL&gt;　
</code></pre>



**④ ローカルでプレビュー**


```  $ hugo server -t <theme> ``` でHugoをローカルで立ち上げて、http://localhost:1313/ でサイトが正しく表示されていることを確認してください。
問題がなければCtrl+Cでサーバを止めてください。

記事の追加は```$ hugo new post/new-post.md```で簡単にできます。
<a id="ref-1"></a>

<br>
###  HugoをGitHub Pagesにデプロイする

ここからはプロジェクトをGithub Pagesにホスティングをする方法を説明します。

まず、GitHubに2つのリポジトリを用意します。

1. ```<username>.github.io```リポジトリ (必ずこの名前を使ってください) <br>
2. ```<project>```リポジトリ（プロジェクト名や好きな名前を使ってください) 

```<username>.github.io```リポジトリはUser PagesというGitHub Pagesフォルダ専用のリポジトリで、必ずこのネーミングスキームである必要があります。ここにpublicフォルダを追加します。```<project>```リポジトリには、ブログのコンテンツを追加します。

リモートリポジトリを用意したら、ローカルリポジトリにリモートを追加して、全てをステージングエリアに登録しておきます。

<pre><code=shell>$ cd &lt;project&gt;
$ git init
$ git remote add origin https://github.com/&lt;username&gt;/&lt;project&gt;.git
$ git add .</code></pre>

次に、```$ hugo -t <theme>```でpublicフォルダを作成して、```<username>.github.io```リポジトリをサブモジュールとして追加します。
publicフォルダが既にある場合は```$ rm -rf public```で削除してください。```$ hugo -t <theme>```は既に存在するpublicフォルダを削除してくれないので、**毎回自分で削除する必要があります**。


<pre><code=shell>$ hugo -t &lt;theme&gt;
$ cd public
$ git init
$ cd ..
$ git submodule add https://github.com/&lt;username&gt;/&lt;username&gt;.github.io.git public</code></pre>

ここまでの作業をコミットし、 ```<project>```リポジトリにpushします。

<pre><code=sh>$ git commit -m "generate project"
$ git push origin master
</code></pre>

最後に、```<username>.github.io```リポジトリにpublicフォルダをpushします。

<pre><code=sh>$ cd public
$ git remote add public https://github.com/&lt;username&gt;/&lt;username&gt;.github.io.git
$ git add .
$ git commit -m "generate static site"
$ git push public master
</code></pre>


https://&lt;username&gt;.github.io にアクセスすればブログを確認できます。<br>
postの内容を反映させるには、MDファイルの```draft=true```を削除してください。


