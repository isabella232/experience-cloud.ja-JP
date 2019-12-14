---
title: 起動埋め込みコードの実装
description: Launchプロパティの埋め込みコードを取得し、Webサイトに実装する方法を説明します。 このレッスンは、「WebサイトにExperience cloudを導入する（起動）」チュートリアルの一部です。
seo-description: null
seo-title: 起動埋め込みコードの実装
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: 14e46fa7d806f2713a69fe5d3a0a8c326de26d23

---


# Launch 埋め込みコードの追加

このレッスンでは、Launchプロパティの開発環境の非同期埋め込みコードを実装します。 途中で、起動の2つの主要な概念、環境と埋め込みコードについて学びます。

## 学習目標

このレッスンを最後まで学習すると、以下の内容を習得できます。

* Launchプロパティの埋め込みコードの取得
* 開発環境、ステージング環境および実稼動環境の違いについて理解する
* HTMLドキュメントへのLaunch埋め込みコードの追加
* Explain the optimal location of the Launch embed code in relation to other code in the `<head>` of an html document

## 埋め込みコードのコピー

The embed code is a `<script>` tag that you put on your webpages to load and execute the logic you build in Launch. ライブラリを非同期で読み込むと、ブラウザーは引き続きページの読み込みを行い、起動ライブラリを取得して、並行して実行します。 この場合、`<head>` に配置する埋め込みコードは 1 つだけです。(When Launch is deployed synchronously, there are two embed codes, one which you put in the `<head>` and another which you put before the `</body>`).

プロパティの概要画面から、「`Environments`」タブをクリックして環境ページに移動します。開発環境、ステージング環境および実稼働環境が事前に作成されていることに注意してください。

![上部のナビゲーションで「環境」をクリックします。](images/launch-environments.png)

開発環境、ステージング環境、および実稼働環境は、コード開発やリリースプロセスの一般的な環境に対応しています。コードは、開発環境で開発者が最初に記述します。 作業が完了したら、QA および他のチームがレビューできるよおう、ステージング環境に送信します。QAやその他のチームが満足したら、コードが実稼働環境に発行されます。実稼働環境は、訪問者がWebサイトに来たときに体験する公開環境です。

「起動」を使用すると、追加の開発環境が可能になります。この環境は、複数の開発者が異なるプロジェクトで同時に作業を行う大規模な組織で役立ちます。

このチュートリアルを完了するために必要な環境は、これらだけです。 環境では、起動ライブラリの異なる作業バージョンを異なるURLでホストできるので、新しい機能を安全に追加し、適切なユーザー（開発者、QAエンジニア、公開など）が利用できます。適切な時間に

次に、埋め込みコードをコピーします。

1. In the **[!UICONTROL Development]** row, click the Install icon ![Install icon](images/launch-installIcon.png) to open the modal.

1. 「起動」は、デフォルトで非同期埋め込みコードに設定されます

1. コピーアイコンコピーアイコン ![をクリックして](images/launch-copyIcon.png) 、埋め込みコードをクリップボードにコピーします。

1. Click **[!UICONTROL Close]** to close the modal.

   ![インストールアイコン](images/launch-copyInstallCode.png)

## Implement the Embed Code in the `<head>` of the Sample HTML Page

The embed code should be implemented in the `<head>` element of all HTML pages that will share the property. You might have one or several template files which control the `<head>` globally across the site, making it a straightforward process to add Launch.

まだサンプルのhtmlページをダウンロードしていな [い場合は](https://www.enablementadobe.com/multi/web/basic-sample.html) （このリンクを右クリックし、「リンクに名前を付けて保存」をクリックして）、コードエディターで開きます。 [ブラケットは](http://brackets.io/) 、必要に応じてオープンソースの無料エディタです。

34行目前後の既存の埋め込みコードをクリップボード上の埋め込みコードに置き換えて、ページを保存します。 次に、Webブラウザーでページを開きます。 If you are loading the page using the `file://` protocol, you will need to add "https:" at the beginning of the embed code URL in your code editor). サンプルページの 33～36 行は次のようになります。

```html
    <!--Launch Header Embed Code: REPLACE LINE 39 WITH THE EMBED CODE FROM YOUR OWN DEVELOPMENT ENVIRONMENT-->
    <script src="https://assets.adobedtm.com/launch-ENa21cfed3f06f4ddf9690de8077b39e81-development.min.js" async></script>
    <!--/Launch Header Embed Code-->
```

Webブラウザーの開発者ツールを開き、「ネットワーク」タブに移動します。 At this point you should see a 404 error for the Launch environment URL:
![404 error](images/samplepage-404.png)

この起動環境にまだライブラリをビルドしていないので、404エラーが必要です。 これについては、次のレッスンでおこないます。404 エラーではなく「Failed」メッセージが表示された場合は、埋め込みコードに `https://` プロトコルを追加し忘れている可能性があります。`file://` プロトコルを使用してサンプルページを読み込む場合にのみ、`https://` プロトコルを指定する必要があります。変更を加え、404 エラーが表示されるまでページをリロードします。

## 導入のベストプラクティスの開始

サンプルページで実証されているLaunch実装のベストプラクティスのいくつかを見てみましょう。

* **データレイヤー**：

   * We *strongly* recommend creating a digital data layer on your site containing all of the attributes needed to populate variables in Analytics, Target, and other marketing solutions. このサンプルページには非常にシンプルなデータレイヤーのみが含まれていますが、実際のデータレイヤーにはページ、訪問者、買い物かごの詳細などの詳細情報が多数含まれる可能性があります。For more info on data layers, please see [Customer Experience Digital Data Layer 1.0](https://www.w3.org/2013/12/ceddl-201312.pdf)

   * Target、顧客属性およびAnalyticsで可能な操作を最大限にするために、「埋め込みコードを起動」の前にデータレイヤーを定義します。

* **JavaScriptヘルパーライブラリ**:ページのにJQueryなどのライブラリが既に実装され `<head>` ている場合は、LaunchとTargetで構文を活用するために、Launchの前にそのライブラリを読み込みます

* **HTML5 doctype**:TargetではHTML5ドキュメントタイプが必要です

* **preconnect および dns-prefetch**：ページの読み込み時間を改善するには preconnect および dns-prefetch を使用します。See also: [https://w3c.github.io/resource-hints/](https://w3c.github.io/resource-hints/)

* **非同期Target実装のスニペットを事前に非表示にします**。この点については、Targetのレッスンで詳しく説明しますが、非同期的なLaunch埋め込みコードを使用してTargetを導入する場合は、コンテンツのちらつきを管理するために、Launch埋め込みコードの前に事前に非表示のスニペットをページにハードコードする必要があります

これらのベストプラクティスについて、推奨順にまとめました。アカウント固有の詳細を示すプレースホルダーがいくつかあります。

```html
<!doctype html>
<html lang="en">
<head>
    <title>Basic Demo</title>
    <!--Preconnect and DNS-Prefetch to improve page load time. REPLACE "techmarketingdemos" WITH YOUR OWN AAM PARTNER ID, TARGET CLIENT CODE, AND ANALYTICS TRACKING SERVER-->
    <link rel="preconnect" href="//dpm.demdex.net">
    <link rel="preconnect" href="//fast.techmarketingdemos.demdex.net">
    <link rel="preconnect" href="//techmarketingdemos.demdex.net">
    <link rel="preconnect" href="//cm.everesttech.net">
    <link rel="preconnect" href="//techmarketingdemos.tt.omtrdc.net">
    <link rel="preconnect" href="//techmarketingdemos.sc.omtrdc.net">
    <link rel="dns-prefetch" href="//dpm.demdex.net">
    <link rel="dns-prefetch" href="//fast.techmarketingdemos.demdex.net">
    <link rel="dns-prefetch" href="//techmarketingdemos.demdex.net">
    <link rel="dns-prefetch" href="//cm.everesttech.net">
    <link rel="dns-prefetch" href="//techmarketingdemos.tt.omtrdc.net">
    <link rel="dns-prefetch" href="//techmarketingdemos.sc.omtrdc.net">
    <!--/Preconnect and DNS-Prefetch-->
    <!--Data Layer to enable rich data collection and targeting-->
    <script>
    var digitalData = {
        "page": {
            "pageInfo" : {
                "pageName": "Home"
                }
            }
    };
    </script>
    <!--/Data Layer-->
    <!--jQuery or other helper libraries-->
    <script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
    <!--/jQuery-->
    <!--prehiding snippet for Adobe Target with asynchronous Launch deployment-->
    <script>
        (function(g,b,d,f){(function(a,c,d){if(a){var e=b.createElement("style");e.id=c;e.innerHTML=d;a.appendChild(e)}})(b.getElementsByTagName("head")[0],"at-body-style",d);setTimeout(function(){var a=b.getElementsByTagName("head")[0];if(a){var c=b.getElementById("at-body-style");c&&a.removeChild(c)}},f)})(window,document,"body {opacity: 0 !important}",3E3);
    </script>
    <!--/prehiding snippet for Adobe Target with asynchronous Launch deployment-->
    <!--Launch Header Embed Code: REPLACE LINE 39 WITH THE INSTALL CODE FROM YOUR OWN DEVELOPMENT ENVIRONMENT-->
    <script src="//assets.adobedtm.com/launch-EN93497c30fdf0424eb678d5f4ffac66dc.min.js" async></script>
    <!--/Launch Header Embed Code-->
</head>
<body>
    <h1>Launch by Adobe: Basic Demo</h1>
    <p>This is a very simple page to demonstrate basic concepts of Launch by Adobe</p>
</body>
</html>
```

これで、サイトに「開始」埋め込みコードを追加する方法が分かりました。

[次の「データ要素、ルールおよびライブラリの追加」&gt;](launch-data-elements-rules.md)