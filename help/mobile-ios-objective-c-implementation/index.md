---
title: モバイル iOS Objective-C アプリケーションでの Experience Cloud の実装
description: モバイル iOS Objective-C アプリケーションに Experience Cloud を実装することは、モバイル iOS Objective-C アプリケーションに Adobe Experience Cloud ソリューションを実装する方法を学びたいモバイルアプリケーション開発者にとって最適な出発点です。
seo-description: null
seo-title: 'モバイル iOS Objective-C アプリケーションでの Experience Cloud の実装 '
solution: Experience Cloud
translation-type: ht
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# 概要

_モバイル iOS Objective-C アプリケーションに Experience Cloud を実装すること_&#x200B;は、iOS Objective-C アプリケーションに Adobe Experience Cloud ソリューションを実装する方法を学びたいモバイルアプリケーション開発者にとって最適な出発点です。

各レッスンには、Experience Cloud の実装およびその価値を理解するのに役立つ、ハウツー演習や基本的な情報が含まれています。チュートリアルを完了するためのデモ Objective-C アプリケーションが提供されているので、基本的な技術を安全な環境で学習することができます。このチュートリアルを完了すると、すべての Experience Cloud ソリューションを独自の iOS Objective-C アプリケーションに実装する準備が整います。

このチュートリアルでは、以下の内容について学習します。

* モバイル Launch プロパティを作成する。

* Objective-C アプリケーションで Launch プロパティをインストールする。

* 次の Adobe Experience Cloud ソリューションを実装する。
   * **[Adobe Experience Platform ID サービス](id-service.md)**
   * **[Adobe Target](target-vec.md)**
   * **[Adobe Analytics](analytics.md)**
   * **[Adobe Audience Manager](audience-manager.md)**

* 開発環境、ステージング環境および実稼動環境を通じて Launch で変更を公開する。

>[!NOTE] 次のプラットフォームでも、同様のマルチソリューションチュートリアルを利用できます。
>
> * [モバイル iOS Swift™ アプリケーションでの Experience Cloud の実装](/help/mobile-ios-swift-implementation/index.md)
* [モバイル Android™ アプリケーションでの Experience Cloud の実装](/help/mobile-android-implementation/index.md)
* [Launch を使用した Web サイトでの Experience Cloud の実装](/help/website-implementation/index.md)


## 前提条件

このレッスンでは、練習を完了するために必要な Adobe ID と権限があることを前提としています。Adobe ID と権限がない場合は、Experience Cloud 管理者に連絡して、アクセスをリクエストする必要がある場合があります。

* Launch の場合は、開発、承認、公開、拡張機能の管理、および環境の管理の権限が必要です。Launch の権限について詳しくは、[ドキュメント](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/admin/user-permissions.html)を参照してください。
* Target の場合は、Adobe Target インターフェイスへの承認者レベルのアクセス権が必要です。
* Adobe Analytics の場合は、このチュートリアルを完了させるために使用するトラッキングサーバーとレポートスイートについて把握しておく必要があります。

また、Objective-C を使用した iOS の開発に精通していることを前提としています。Objective-C を習熟していなくてもレッスンを完了することはできますが、コードを快適に読んで理解できれば、レッスンを最大限活用することができます。

## レッスンについて

このレッスンでは、自らの Experience Cloud 組織を使用して、Adobe Experience Cloud を[バス予約](https://github.com/Adobe-Marketing-Cloud/busbooking-mobileapps)と呼ばれるデモアプリケーションに実装します。このアプリケーションにはいくつかのシンプルな機能があり、自分のアプリケーションで Experience Cloud を実装する前に基本的な Experience Cloud のモバイル実装を完了することができます。

[![バス予約アプリケーション](images/mobile-busBookingApp.png)](https://github.com/Adobe-Marketing-Cloud/busbooking-mobileapps)

## ツールの取得

1. このチュートリアルを完了するには、Mac® を使用する必要があります。
1. [Xcode ](https://developer.apple.com/xcode/)をダウンロードします。
1. [バス予約アプリケーション](https://github.com/Adobe-Marketing-Cloud/busbooking-mobileapps)をダウンロードします。
1. [Cocoapod](https://guides.cocoapods.org/using/getting-started.html) をインストールします。

それでは、始めましょう。

[次：「Launch プロパティの作成」&gt;](launch-create-a-property.md)