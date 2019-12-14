---
title: モバイルiOS SwiftアプリケーションでのExperience cloudの実装
description: モバイルiOS SwiftアプリでのExperience cloudの実装は、モバイルiOS SwiftアプリでAdobe Experience cloudソリューションを実装する方法を学びたい場合に、モバイルアプリ開発者にとって最適な起点となります。
seo-description: null
seo-title: 'モバイルiOS SwiftアプリケーションでのExperience cloudの実装 '
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# 概要

_モバイルiOS Swift™アプリケーションへのExperience cloudの実装は_ 、iOS SwiftアプリでAdobe Experience cloudソリューションを実装する方法を学ぶモバイルアプリ開発者にとって最適なスタートポイントです。

![スウィフトロゴ](images/ios/swift/Swift_logo_horz_lockup_color_rgb.png)

各レッスンには、Experience cloudの実装とその価値の理解に役立つハウツー練習と基本的な情報が含まれています。  チュートリアルを完了するためのデモ用Swiftアプリが用意されているので、基本的なテクニックを安全な環境で学ぶことができます。 このチュートリアルを完了したら、お使いのiOS SwiftアプリにExperience cloudのすべてのソリューションを実装する準備が整います。

このチュートリアルを完了すると、次のことが可能になります。

* モバイル起動プロパティの作成

* Swiftアプリでの起動プロパティのインストール

* 次のAdobe Experience cloudソリューションを実装します。
   * **[Adobe Experience Platform ID サービス](id-service.md)**
   * **[Adobe Target](target-vec.md)**
   * **[Adobe Analytics](analytics.md)**
   * **[Adobe Audience Manager](audience-manager.md)**

* 開発、ステージングおよび実稼働環境を通じて、起動に関する変更を公開します。

>[!NOTE] 次のプラットフォームでも、同様のマルチソリューションチュートリアルを利用できます。
>
> * [モバイルiOS Objective-CアプリケーションでのExperience cloudの実装](/help/mobile-ios-objective-c-implementation/index.md)
> * [モバイルAndroid™アプリケーションでのExperience cloudの実装](/help/mobile-android-implementation/index.md)
* [Experience cloudのWebサイトへの導入（開始）](/help/website-implementation/index.md)


## 前提条件

このレッスンでは、練習を完了するために必要なAdobe IDと権限があることを前提としています。 そうでない場合は、Experience cloud管理者に連絡して、アクセスをリクエストする必要がある場合があります。

* 起動の場合、環境の開発、承認、発行、管理の各権限が必要です。 For more information on Launch permissions, see [the documentation](https://docs.adobe.com/content/help/en/launch/using/reference/admin/user-permissions.html).
* Targetの場合、Adobe targetインターフェイスへの承認者レベルのアクセス権が必要です
* Adobe Analyticsの場合は、トラッキングサーバーと、このチュートリアルの完了に使用するレポートスイートを知っている必要があります。

また、SwiftでのiOSの開発に精通していることを前提としています。 レッスンを完了するのにSwiftの修士である必要はありませんが、コードを読み、理解しやすくなれば、Swiftの修士になる必要があります。

## レッスンについて

このレッスンでは、お客様独自のExperience cloud組織を使用して、Adobe Experience cloudを「[Bus Booking](https://github.com/Adobe-Marketing-Cloud/busbooking-mobileapps)」と呼ばれるデモアプリに実装します。 このアプリには、基本的なExperience cloudモバイル実装を完了してから、自分のアプリで実装できるシンプルな機能がいくつか用意されています。

[![バス予約アプリ](images/mobile-busBookingApp.png)](https://github.com/Adobe-Marketing-Cloud/busbooking-mobileapps)

## ツールの取得

1. このチュートリアルを完了するには、Mac®を使用する必要があります
1. Xcodeのダウンロ [ード](https://developer.apple.com/xcode/)
1. バス予約アプ [リのダウンロード](https://github.com/Adobe-Marketing-Cloud/busbooking-mobileapps)
1. Cocoapodのインス [トール](https://guides.cocoapods.org/using/getting-started.html)

始めよう！

[次の「起動プロパティの作成」&gt;](launch-create-a-property.md)

_Apple、Appleのロゴ、iPad、iPhone、iPod、iPod touchは、Apple Inc.の米国およびその他の国々での登録商標です。 SwiftとSwiftのロゴはApple Inc.の商標です。_