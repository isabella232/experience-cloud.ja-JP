---
title: Adobe Experience Platform Launch を使用した Adobe Experience Cloud の実装
description: Web サイトアプリケーションに Experience Cloud を実装することは、Web サイトアプリケーションに Adobe Experience Cloud ソリューションを実装する方法を学びたいモバイルアプリケーション開発者にとって最適な出発点です。
seo-description: null
seo-title: Adobe Experience Platform Launch を使用した Adobe Experience Cloud の実装
solution: Experience Cloud
translation-type: ht
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# 概要

_Web サイトアプリケーションに Experience Cloud を実装すること_&#x200B;は、Web サイトアプリケーションに Adobe Experience Cloud ソリューションを実装する方法を学びたいモバイルアプリケーション開発者にとって最適な出発点です。

各レッスンには、Experience Cloud の実装およびその価値を理解するのに役立つ、ハウツー演習や基本的な情報が含まれています。以前のタグマネージャー（動的タグ管理）からの移行に役立つ情報を強調表示するための引き出し線が提供されます。デモを完了するためのデモサイトが提供されているので、基本的な技術を安全な環境で学習することができます。このチュートリアルを完了すると、すべての Experience Cloud ソリューションをLaunch を介して独自の Web サイトに実装する準備が整います。

このチュートリアルでは、以下の内容について学習します。

* Launch プロパティの作成

* Web サイトでの Launch プロパティのインストール

* 次の Adobe Experience Cloud ソリューションを追加する
   * **[Adobe Experience Platform ID サービス](id-service.md)**
   * **[Adobe Target](target.md)**
   * **[Adobe Analytics](analytics.md)**
   * **[Adobe Audience Manager](audience-manager.md)**

* ルールとデータ要素を作成し、データをアドビソリューションに送信する

* Adobe Experience Cloud デバッガーを使用して実装を検証する

* 開発環境、ステージング環境および実稼動環境を通じて Launch で変更を公開する

>[!NOTE] 次のプラットフォームでも、同様のマルチソリューションチュートリアルを利用できます。
>
> * [モバイル iOS Swift™ アプリケーションでの Experience Cloud の実装](/help/mobile-ios-swift-implementation/index.md)
* [モバイル iOS Objective-C アプリケーションでの Experience Cloud の実装](/help/mobile-ios-objective-c-implementation/index.md)
* [モバイル Android アプリケーションでの Experience Cloud の実装](/help/mobile-android-implementation/index.md)


## 前提条件

このレッスンでは、練習を完了するために必要な Adobe ID と権限があることを前提としています。Adobe ID と権限がない場合は、Experience Cloud 管理者に連絡して、アクセスをリクエストする必要がある場合があります。

* Launch の場合は、開発、承認、公開、拡張機能の管理、および環境の管理の権限が必要です。Launch の権限について詳しくは、[ドキュメント](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/admin/user-permissions.html)を参照してください。
* Adobe Analytics の場合は、このチュートリアルを完了させるために使用するトラッキングサーバーとレポートスイートについて把握しておく必要があります。
* Audience Manager の場合は、Audience Manager サブドメイン（パートナー名、パートナー ID またはパートナーサブドメインとも呼ばれます）を把握しておく必要があります。

また、HTML や JavaScript などのフロントエンド開発言語に精通していることを前提としています。これらの言語を習熟していなくてもレッスンを完了することはできますが、コードを快適に読んで理解できれば、レッスンを最大限活用することができます。

## Launch について

Adobe Experience Platform Launch は、アドビが提供する次世代型の Web サイトタグおよびモバイル SDK の管理機能です。Launch は、顧客体験の実現に必要なすべての分析、マーケティングおよび広告のソリューションをデプロイおよび管理するためのシンプルな手段を提供します。Launch では、追加料金はかかりません。Adobe Experience Cloud のお客様は、Launch を利用できます。

Web サイト用の Launch は、デスクトップおよびモバイルサイトで使用される分析、マーケティング、広告ソリューションに関連するすべての JavaScript を一元的に管理できます。例えば、Adobe Analytics をデプロイする場合、Launch は AppMeasurement JavaScript ライブラリを管理し、変数を設定し、リクエストを実行します。

Launch コンテナタグは、DTM よりも 60 ％軽量で、Google Tag Manager よりも 40 ％軽量です。コンテナのコンテンツは縮小され、カスタムコードを含みます。すべてがモジュール式です。項目が不要な場合は、ライブラリに含めません。その結果、高速でコンパクトな実装になります。

また、Launch は、サードパーティベンダーが拡張機能を作成し、Launch を通じてソリューションを簡単に展開できるようにするプラットフォームでもあります。拡張機能は、Launch UI とクライアント機能を拡張するコードのパッケージ（JavaScript、HTML および CSS）です。Launch はオペレーティングシステム、拡張機能はタスクの遂行に使用されるアプリケーションと考えることができます。

## レッスンについて

このレッスンでは、Adobe Experience Cloud を練習用の小売 Web サイト「Luma」に実装します。[Luma サイト](https://luma.enablementadobe.com/content/luma/us/en.html)には、現実的な実装を構築できる豊富なデータレイヤーと機能があります。独自の Experience Cloud 組織に独自の Launch プロパティを構築し、Experience Cloud デバッガーを使用してホストされている Luma サイトにマッピングします。

[![Luma Web サイト](images/overview-luma.png)](https://luma.enablementadobe.com/content/luma/us/en.html)

## ツールの取得

1. 拡張機能にはブラウザー固有のものがあるため、[Chrome Web ブラウザー](https://www.google.com/chrome/)を使用してチュートリアルを完了することをお勧めします。
1. [Adobe Experience Cloud デバッガー](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)拡張機能を Chrome ブラウザーに追加します。
1. [サンプル HTML ページ](https://www.enablementadobe.com/multi/web/basic-sample.html)のダウンロード（このリンクを右クリックして「名前を付けて保存」をクリック）します。
1. サンプルの HTML ページを変更できるテキストエディターを取得します。（エディターがない場合は、[Brackets](http://brackets.io/) を試すことをお勧めします）。
1. [Luma サイト](https://luma.enablementadobe.com/content/luma/us/en.html)をブックマークします。

>[!NOTE] このチュートリアルを完了するには、Chrome で Luma サイトを開いたままにして、別のブラウザーでこのチュートリアルを読んでLaunch インターフェイスの手順を完了する方が簡単な場合があります。

それでは、始めましょう。

[次：「Launch プロパティの作成」&gt;](launch.md)