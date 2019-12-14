---
title: Adobe Experience Platform Launchを使用したWebサイトへのAdobe Experience cloudの実装
description: Experience cloudを起動付きWebサイトに実装する方法は、Adobe Experience cloudソリューションをWebサイトに実装する方法を学ぶフロントエンド開発者またはテクニカルマーケターにとって最適なスタートポイントです。
seo-description: null
seo-title: Adobe Experience Platform Launchを使用したWebサイトへのAdobe Experience cloudの実装
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# 概要

_Experience cloudを起動によるWebサイトに実装する方法は_ 、Adobe Experience cloudソリューションをWebサイトに実装する方法を学ぶフロントエンド開発者やテクニカルマーケターにとって最適なスタートポイントです。

各レッスンには、Experience cloudの実装とその価値の理解に役立つハウツー練習と基本的な情報が含まれています。  引き出し線は、古いタグマネージャーであるDynamic Tag Managementから移行するお客様に役立つ情報を強調するために提供されています。 デモを完了するためのデモサイトが提供されているので、基本的な技術を安全な環境で学習することができます。このチュートリアルを完了したら、「起動」を使用して、お客様独自のWebサイトにすべてのマーケティングソリューションを実装する準備が整っている必要があります。

この作業を完了すると、次のことが可能になります。

* 起動プロパティの作成

* WebサイトへのLaunchプロパティのインストール

* 次のAdobe Experience cloudソリューションを追加します。
   * **[Adobe Experience Platform ID サービス](id-service.md)**
   * **[Adobe Target](target.md)**
   * **[Adobe Analytics](analytics.md)**
   * **[Adobe Audience Manager](audience-manager.md)**

* アドビのソリューションにデータを送信するためのルールとデータ要素を作成します。

* Adobe Experience cloudデバッガーを使用した実装の検証

* 開発、ステージングおよび実稼働環境を通じて、起動に関する変更を公開します。

>[!NOTE] 次のプラットフォームでも、同様のマルチソリューションチュートリアルを利用できます。
>
> * [モバイルiOS Swift™アプリケーションでのExperience cloudの実装](/help/mobile-ios-swift-implementation/index.md)
* [モバイルiOS Objective-CアプリケーションでのExperience cloudの実装](/help/mobile-ios-objective-c-implementation/index.md)
* [モバイルAndroidアプリケーションでのExperience cloudの実装](/help/mobile-android-implementation/index.md)


## 前提条件

このレッスンでは、練習を完了するために必要なAdobe IDと権限があることを前提としています。 そうでない場合は、Experience cloud管理者に連絡して、アクセスをリクエストする必要がある場合があります。

* 起動の場合、環境の開発、承認、発行、管理の各権限が必要です。 For more information on Launch permissions, see [the documentation](https://docs.adobe.com/content/help/en/launch/using/reference/admin/user-permissions.html).
* Adobe Analyticsの場合は、トラッキングサーバーと、このチュートリアルの完了に使用するレポートスイートを知っている必要があります。
* Audience Managerの場合、Audience Managerサブドメイン（「パートナー名」「パートナーID」または「パートナーサブドメイン」とも呼ばれる）を知っている必要があります。

また、HTML や JavaScript などのフロントエンド開発言語に精通していることを前提としています。これらの言語を習熟していなくてもレッスンを完了することはできますが、コードを快適に読んで理解できれば、レッスンを最大限活用することができます。

## Launch について

Adobe Experience Platform Launchは、アドビが提供する次世代のWebサイトタグおよびモバイルSDK管理機能です。 Launchを使用すると、関連する顧客エクスペリエンスを強化するために必要なすべての分析、マーケティング、広告ソリューションをシンプルに導入および管理できます。 起動に追加料金はかかりません。 Adobe Experience cloudのお客様は、これを利用できます。

Webサイトの起動により、デスクトップおよびモバイルサイトで使用される分析、マーケティング、広告ソリューションに関連するすべてのJavaScriptを一元的に管理できます。 例えば、Adobe Analyticsをデプロイする場合、「起動」はAppMeasurement javaScriptライブラリを管理し、変数を設定し、リクエストを実行します。

Launchコンテナタグは、DTMよりも60%軽く、Google Tag Managerよりも40%軽くなります。 コンテナのコンテンツは縮小され、カスタムコードを含みます。すべてがモジュール式です。項目が不要な場合は、ライブラリに含めません。その結果、高速でコンパクトな実装になります。

また、Launchは、サードパーティベンダーが拡張機能を作成し、Launchを通じてソリューションを簡単に展開できるようにするプラットフォームでもあります。 拡張機能は、UIとクライアントの起動機能を拡張するコード(JavaScript、HTML、CSS)のパッケージです。 Launch はオペレーティングシステム、拡張はタスクの遂行に使用されるアプリと考えることができます。

## レッスンについて

このレッスンでは、Adobe Experience cloudを偽の小売WebサイトLumaに実装します。 ルミ [ナンスサイトに](https://luma.enablementadobe.com/content/luma/us/en.html) 、現実的な実装を構築できる豊富なデータレイヤーと機能があります。 独自のExperience cloud組織に独自のLaunchプロパティを構築し、Experience cloudデバッガーを使用してホストされているLumaサイトにマッピングします。

[![Luma webサイト](images/overview-luma.png)](https://luma.enablementadobe.com/content/luma/us/en.html)

## ツールの取得

1. Because you will be using some browser-specific extensions, we recommend completing the tutorial using the [Chrome Web Browser](https://www.google.com/chrome/)
1. Add the [Adobe Experience Cloud Debugger](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj) extension to your Chrome browser
1. Download the [sample html page](https://www.enablementadobe.com/multi/web/basic-sample.html) (right-click on this link and click "Save Link As")
1. サンプルHTMLページに変更を加えることができるテキストエディターを取得します。 (ブラケットがない場合は、Bracketsを試すことをお勧めし [ます](http://brackets.io/))。
1. ルミナンスサイトのブ [ックマーク](https://luma.enablementadobe.com/content/luma/us/en.html)

>[!NOTE] このチュートリアルを読み、別のブラウザーで起動インターフェイスの手順を実行する間、Chromeで開くルミナンスサイトでこのチュートリアルを完了する方が簡単な場合があります。

始めよう！

[次の「起動プロパティの作成」&gt;](launch.md)