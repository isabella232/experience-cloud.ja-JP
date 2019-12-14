---
title: Mobile iOS SwiftアプリへのAdobe Mobile SDKのインストール
description: Launchプロパティの埋め込みコードを取得し、Webサイトに実装する方法を説明します。 このレッスンは、「Mobile iOS SwiftアプリケーションでのExperience cloudの実装」チュートリアルの一部です。
seo-description: null
seo-title: Mobile iOS SwiftアプリへのAdobe Mobile SDKのインストール
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: d669338b0e298c44c1ef2ed1a4491b8451d45f7e

---


# Mobile SDK のインストール

このレッスンでは、Launchプロパティの開発環境に対応する拡張機能と設定を持つMobile SDKを実装します。

## 学習目標

このレッスンを最後まで学習すると、以下の内容を習得できます。

* モバイル起動プロパティのインストール手順の取得
* 開発環境、ステージング環境および実稼動環境の違いについて理解する
* ポッドファイルの作成と編集
* AppDelegateファイルへのMobile SDKの読み込み
* SDKが正しく実装されていることを確認します

## インストール手順の取得

モバイル起動プロパティのインストール手順は、ターミナルで実行するか、モバイルアプリの特定の場所に追加するコードスニペットの集まりです。

上部のナビゲーショ `Environments` ンのタブをクリックして、環境ページに移動します。 開発環境、ステージング環境および実稼働環境が事前に作成されていることに注意してください。 これらは、コードの開発およびリリースプロセスの一般的な環境に対応しています。 コードは、開発環境で開発者が最初に記述します。 作業が完了したら、QA および他のチームがレビューできるよおう、ステージング環境に送信します。QAやその他のチームが満足したら、コードが実稼働環境に発行されます。実稼働環境は、訪問者がアプリをダウンロードする際に体験する公開環境です。

「起動」を使用すると、追加の開発環境が可能になります。この環境は、複数の開発者が異なるプロジェクトで同時に作業を行う大規模な組織で役立ちます。

このチュートリアルを完了するために必要な環境は、開発、ステージング、実稼働環境のみです。

![上部のナビゲーションで「環境」をクリックします。](images/mobile-launch-environments.png)

「開発」行で **[!UICONTROL 、「インストール]** 」アイコン「インストール」 ![アイコンをクリックし](images/mobile-launch-installIcon.png) 、埋め込みコードモーダルを開きます。

![アイコンをクリックして、埋め込みコードモーダルを開きます](images/mobile-launch-openEmbedCode.png)

手順を順を追って説明します。

## ポッドファイルの作成とポッドのインストール

以前にWebサイトで「起動」を使用したことがある場合、最初に気付くことの1つは、このモーダルにはWebプロパティよりも多くの情報が含まれていることです。

Adobe Mobile SDK for iOSは、CocoaPodを使用して、様々なコンポーネント間の依存関係を管理します。 CocoaPodをまだ開発環境にインストールし [ていない場合は](https://cocoapods.org/) 、各社のWebサイトのインストール手順に従ってください。 また、 [Bus Bookingアプリをまだダウンロードしていない場合は](https://github.com/Adobe-Marketing-Cloud/busbooking-mobileapps)、ローカルマシンに保存し、zipアーカイブをデスクトップに展開します。

**ポッドファイルを作成するには**

1. Mac®でア `Terminal` プリケーションを開く

1. バス予約スウィフトアプリを保存したプロジェクトフォルダに移動します(例： `cd Desktop/busbooking-mobileapps-master/Swift/`)

   ![プロジェクトディレクトリに移動します。](images/ios/swift/mobile-launch-install-goToProjectDirectory.png)

1. 起動インターフェイスで、オペレーティングシステムを `iOS`

1. 最初のiOS命令をコピーするに `pod init`は、コピーアイコンをクリ ![ック](images/mobile-launch-copyIcon.png) します

   ![起動インターフェイスのクリップボードにポッドの初期設定をコピー](images/ios/mobile-launch-install-copyPodInit.png)

1. ターミナルアプリで、コマンドを実 `pod init` 行し、完了するまで待ちます

   ![ポッド初期化の実行](images/ios/swift/mobile-launch-install-runPodInit.png)

1. ターミナルアプリで、次のコマンドを使用してポッドファイルを開きま `open podfile` す。

   ![オープンポッドファイルの実行](images/ios/swift/mobile-launch-install-openPodfile.png)

1. ポッドファイルを開くアプリケーションを尋ねるダイアログがコンピューターで開く場合があります。 任意のテキストエディター( `TextEdit`

1. 「起動」インターフェイスで、「コピー」アイコンをクリックして、依存関係のリストをコ ![ピーします](images/mobile-launch-copyIcon.png) 。 前のレッスンで追加した各拡張機能に対応する線があることに注意してください。 各拡張機能には、Mobile core拡張機能を基に構築された独自のコードセットがあり、アプリのアップデートでのみ追加または削除できます。

   ![起動インターフェイスのクリップボードに依存関係をコピーする](images/ios/mobile-launch-install-copyDependencies.png)

1. テキストエディターで、クリップボードから行の直後に依存関係を貼り付けます `# Pods for BusBookingSwift`

1. ポッドファイルの更新をテキストエディターに保存します

   ![依存関係を追加して保存](images/ios/swift/mobile-launch-install-addDependenciesAndSave.png)

1. これで、テキストエディターを閉じることができます

1. 起動インターフェイスで、「コピー」アイコンをクリックして、次 `pod repo update`のiOS命令をコピ ![ー](images/mobile-launch-copyIcon.png) します

   ![ポッドレポの更新のコピー](images/ios/mobile-launch-install-copyPodRepoUpdate.png)

1. ターミナルアプリで、コマンドを実 `pod repo update` 行し、コマンドが完了するまで待ちます（数分かかる場合があります）

   ![ポッドレポの更新の実行](images/ios/swift/mobile-launch-install-podRepoUpdate.png)

1. 起動インターフェイスで、「コピー」アイコンをクリックして、次 `pod install`のiOS命令をコピ ![ー](images/mobile-launch-copyIcon.png) します

   ![起動インターフェイスのクリップボードにポッドのインストールをコピー](images/ios/mobile-launch-install-copyPodInstall.png)

1. ターミナルアプリで、コマンドを実 `pod install` 行し、完了するまで待ちます

   ![ポッドのインストールの実行](images/ios/swift/mobile-launch-install-podInstall.png)

1. これで、ターミナルウィンドウを閉じることができます

1. Finderウィンドウを開き、Bus Bookingアプリを保存したフォルダに移動し、BusBookingSwift.xcworkspaceファイル、Podfile、Podfile.lockファイル、Podsフォルダが作成されていることを確認します

   ![ファインダーでのポッドの確認](images/ios/swift/mobile-launch-install-podsInFinder.png)

## AppDelegateの更新

次に、アプリを更新してSDKを読み込みます

1. Xcodeでファ `BusBookingSwift.xcworkspace` イルを開く
1. Open the `AppDelegate.swift` file

   ![AppDelegateファイルを開きます](images/ios/swift/mobile-launch-install-openAppDelegate.png)

1. 起動インターフェイスで、「初期化コード **[!UICONTROL の追加]** 」セクションまでスクロールし、使用 **[!UICONTROL しているiOS言語として「Swift]** 」を選択します。
1. 「初期化コードの追加」セクションの最初の「コピー ![」アイコンをクリックし](images/mobile-launch-copyIcon.png) て、インポート文をコピーします **** 。

   ![Swiftインポート文をクリップボードにコピーします。](images/ios/swift/mobile-launch-install-copyImports.png)

1. Xcodeで、これらのimport文を、 `AppDelegate.swift``UIKit`

   ![Swiftのインポート文をAppDelegateファイルに貼り付けます](images/ios/swift/mobile-launch-install-pasteImports.png)

1. 起動インターフェイスで、「初期化コードを追加」セクションの2番目の「コピー ![」アイコンをクリックして、Core拡張に関連する2](images/mobile-launch-copyIcon.png) 行をコピーします **** 。 最初の行では、コンソールログ文がオンになります（使用可能なオプションは、「debug」、「verbose」、「warning」および「error」です）。 2行目は、起動環境の一意の識別子を示します。 これは重要です。アプリを実稼働環境にデプロイする準備が整ったら、この値を更新する必要があります。

   ![Coreステートメントのクリップボードへのコピー](images/ios/swift/mobile-launch-install-copyCore.png)

1. Xcodeで、次のCoreステートメントをメソッドの最上部にあるAppDelegateファイルに貼り付け `application(_:didFinishLaunchingWithOptions:)` ます。

   ![CoreステートメントをAppDelegateファイルに貼り付けます](images/ios/swift/mobile-launch-install-pasteCore.png)

1. 「起動」インターフェイスで、「初期化コードの追加」セクションの3番目の「 ![Copy](images/mobile-launch-copyIcon.png) 」アイコンをクリックし [!UICONTROL て、拡張文をコピーします] 。

   ![Extensionステートメントをクリップボードにコピーします](images/ios/swift/mobile-launch-install-copyExtensions.png)

1. Xcodeで、次の拡張文をAppDelegateファイルのメソッドの行の直前に `return true` 貼り付け `application(_:didFinishLaunchingWithOptions:)` ます。

   ![ExtensionステートメントをAppDelegateファイルに貼り付けます。](images/ios/swift/mobile-launch-install-pasteExtension.png)

>[!NOTE] 起動インターフェイスで提供されるモバイルインストール手順には、ID、ライフサイクル、シグナル拡張のインポートと登録の文、およびライフサイクル指標の初期化が含まれます。 これらの拡張機能は、Mobile core拡張の一部と見なされます。 これらの拡張機能をアプリで使用しない場合は、これらの拡張機能に関連付けられた他のコードを読み込み、登録または実装する必要はありません。
>
> また、これらの拡張機能を使用する場合に考慮する必要がある追加の実装オプションがあります（例えば、ユーザーがアプリをバックグラウンドにしたとき、ライフサイクル収集を一時停止/再開できます）。 詳しくは、Mobile Core拡張機能のドキュメ [ントを参照してください](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core)

## 実装の検証

1. Xcodeプロジェクトの保存
1. アプリを実行し、シミュレーターで起動します。 シミュレーターデバイスを設定していない場合は、iOS 10以降を実行するデバイスを設定し、今すぐ設定してください。 iPhone 8シミュレーターは、マウスでボタンをクリックするのが簡単なので、 `Home` 使いたいと思います。

   ![アプリケーションを実行し、エミュレーターで起動します](images/ios/swift/mobile-launch-install-buildAndLaunch.png)

1. シミュレーターが起動し、アプリが完全に開いて予約画面が表示されるまで待ちます（数分かかる場合があります）。

   ![アプリが完全に開くまで待ちます](images/ios/mobile-launch-install-simulator.png)

1. Xcodeコンソールで、Adobeサーバーに対して呼び出しが行われていることを確認します。

   ![コンソールでの呼び出しの検索](images/ios/swift/mobile-launch-install-console.png)

以下に、特定の呼び出しの例を示します。

1. **起動設定を取得する呼び出し** (コンソールをフィルタリング `adobedtm.com`)。 前のレッスンで入力した拡張設定をメモしておきます。 拡張機能を追加するにはアプリの更新が必要ですが、これらの設定は起動で外部的に管理し、いつでも変更できます。

   ```swift
   2019-01-15 12:11:44.518220-0500 BusDemoSwift[52399:5056293] [AMSDK DEBUG <RulesDownloader>]: Successfully downloaded Rules from 'https://assets.adobedtm.com/launch-EN360aefc739b04410816f751a95861744-development-rules.zip'
   
   {"target.propertyToken":"","target.timeout":5,"global.privacy":"optedin","analytics.backdatePreviousSessionInfo":true,"analytics.offlineEnabled":true,"build.environment":"dev","rules.url":"https://assets.adobedtm.com/launch-EN360aefc739b04410816f751a95861744-development-rules.zip","target.clientCode":"techmarketingdemos","experienceCloud.org":"7ABB3E6A5A7491460A495D61@AdobeOrg","target.autoFetch":true,"target.fetchBackground":true,"lifecycle.sessionTimeout":300,"target.environmentId":"busbookingapp","analytics.server":"tmd.sc.omtrdc.net","analytics.rsids":"tmd-mobile-dev1","analytics.batchLimit":0,"property.id":"PRb4881271498b4f2cbaf67d38a8f3891a","global.ssl":true,"analytics.aamForwardingEnabled":true}
   ```

1. **IDサービスへのリクエスト** (コンソールをフィルタリング `demdex.net`)この例では、ID(`d_mid`)は既に設定済みで、再度レポートされています。

   ```swift
   2019-01-15 12:11:45.164590-0500 BusDemoSwift[52399:5056322] [AMSDK DEBUG <com.adobe.module.identity>]:
   
   Sending request (https://dpm.demdex.net/id?d_rtbd=json&d_ver=2&d_orgid=7ABB3E6A5A7491460A495D61@AdobeOrg&d_mid=17179986463578698626041670574784107777&d_blob=j8Odv6LonN4r3an7LhD3WZrU1bUpAkFkkiY1ncBR96t2PTI&dcs_region=9)
   ```

1. **IDサービスからの応答** (コンソールをフィルタリング `ID Service`)。 値が上記のリクエ `mid` スト内の値とど `d_mid` のように一致するかを確認します。

   ```swift
   2019-01-15 12:11:45.681821-0500 BusDemoSwift[52399:5056322] [AMSDK DEBUG <com.adobe.module.identity>]:
   
   ID Service - Got ID Response (mid: 17179986463578698626041670574784107777, blob: j8Odv6LonN4r3an7LhD3WZrU1bUpAkFkkiY1ncBR96t2PTI, hint: 9, ttl: "604800000 ms")
   ```

1. **Analyticsリクエスト** (コンソールをフィルターして `Analytics request`)

   ```swift
   2019-01-15 12:11:45.828465-0500 BusDemoSwift[52399:5056336] [AMSDK DEBUG <AnalyticsHitDatabase>]: Analytics request was sent with body
   
   (ndh=1&c.&a.&AppID=BusDemoSwift%201%20%281.0%29&CarrierName=%28null%29&DayOfWeek=3&DaysSinceFirstUse=0&DaysSinceLastUse=0&DeviceName=x86_64&HourOfDay=12&LaunchEvent=LaunchEvent&Launches=3&OSVersion=iOS%2012.1&Resolution=828x1792&RunMode=Application&TimeSinceLaunch=0&ignoredSessionLength=-1547572244&internalaction=Lifecycle&locale=en-US&.a&.c&aamb=j8Odv6LonN4r3an7LhD3WZrU1bUpAkFkkiY1ncBR96t2PTI&aamlh=9&ce=UTF-8&cp=foreground&mid=17179986463578698626041670574784107777&pageName=BusDemoSwift%201%20%281.0%29&pe=lnk_o&pev2=ADBINTERNAL%3ALifecycle&t=00%2F00%2F0000%2000%3A00%3A00%200%20300&ts=1547572305)
   ```

モバイルアプリにSDKが追加されました。

[次に、「Adobe Experience Platform IDサービスの追加」&gt;](id-service.md)