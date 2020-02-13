---
title: モバイル iOS Swift アプリケーションへの Adobe Mobile SDK のインストール
description: Launch プロパティの埋め込みコードを取得し、Web サイトに実装する方法を説明します。このレッスンは、「モバイル iOS Swift アプリケーションでの Experience Cloud の実装」チュートリアルの一部です。
seo-description: null
seo-title: モバイル iOS Swift アプリケーションへの Adobe Mobile SDK のインストール
solution: Experience Cloud
translation-type: ht
source-git-commit: d669338b0e298c44c1ef2ed1a4491b8451d45f7e

---


# Mobile SDK のインストール

このレッスンでは、Launch プロパティの開発環境に対応する拡張機能と設定を持つ Mobile SDK を実装します。

## 学習内容

このレッスンを最後まで学習すると、以下の内容を習得できます。

* モバイル Launch プロパティのインストール手順を取得する
* 開発環境、ステージング環境および実稼動環境の違いについて理解する
* ポッドファイルを作成、編集する
* Mobile SDK を AppDelegate ファイルに読み込む
* SDK が正しく実装されていることを確認する

## インストール手順の取得

モバイル Launch プロパティのインストール手順は、ターミナルで実行するまたはモバイルアプリケーションで特定の場所に追加するコードスニペットの集まりです。

上部のナビゲーションで「`Environments`」タブをクリックして、環境ページに移動します。開発環境、ステージング環境、および実稼動環境は、事前に作成されています。これらは、コード開発やリリースプロセスの一般的な環境に対応しています。コードは、開発環境の開発者が最初に記述します。作業が完了したら、QA および他のチームがレビューできるよう、ステージング環境に送信します。QA チームやその他のチームが満足したら、コードは実稼動環境に公開されます。この環境は、訪問者がアプリケーションをダウンロードしたときに表示される、公開環境です。

Launch では、追加の開発環境を許可します。これは、複数の開発者が同時に異なるプロジェクトで作業する大規模な組織で便利です。

このチュートリアルを完了するために必要な環境は、開発、ステージング、実稼動環境のみです。

![上部のナビゲーションで「環境」をクリックする](images/mobile-launch-environments.png)

**[!UICONTROL 開発]**&#x200B;行でインストールアイコンをクリックして![インストールアイコン](images/mobile-launch-installIcon.png)埋め込みコードモーダルを開きます。

![アイコンをクリックしてモーダルを開く](images/mobile-launch-openEmbedCode.png)

手順を順を追って説明します。

## ポッドファイルの作成とポッドのインストール

以前に Web サイトで Launch を使用したことがある場合、最初に気付くことの 1 つは、Web プロパティよりもこのモーダルに対して多くの情報があることです。

iOS 向け Adobe Mobile SDK は、CocoaPods を使用して、様々なコンポーネント間の依存関係を管理します。[CocoaPods](https://cocoapods.org/) をまだ開発環境にインストールしていない場合は、CocoaPods Web サイトのインストール手順に従ってください。また、[バス予約アプリケーション](https://github.com/Adobe-Marketing-Cloud/busbooking-mobileapps)をまだダウンロードしていない場合は、ダウンロードしてローカルマシンに保存し、zip アーカイブをデスクトップに展開します。

**ポッドファイルを作成するには、以下を実行します。**

1. Mac® で`Terminal` アプリケーションを開きます。

1. バス予約 Swift アプリケーションを保存したプロジェクトフォルダーに移動します（例：`cd Desktop/busbooking-mobileapps-master/Swift/`）。

   ![プロジェクトディレクトリに移動する](images/ios/swift/mobile-launch-install-goToProjectDirectory.png)

1. Launch インターフェイスで、オペレーティングシステムを `iOS` に変更します。

1. 最初の iOS 命令 `pod init` をコピーするには、![コピー](images/mobile-launch-copyIcon.png)アイコンをクリックします。

   ![Launch インターフェイスのクリップボードに「pod init」をコピーする](images/ios/mobile-launch-install-copyPodInit.png)

1. ターミナルアプリケーションで、`pod init` コマンドを実行し、完了するまで待ちます。

   ![「pod init」の実行](images/ios/swift/mobile-launch-install-runPodInit.png)

1. ターミナルアプリケーションで、`open podfile` コマンドを使用してポッドファイルを開きます。

   ![「open podfile」の実行](images/ios/swift/mobile-launch-install-openPodfile.png)

1. ポッドファイルをどのアプリケーションで開くかを尋ねるダイアログがコンピューターで開く場合があります。任意のテキストエディター（例：`TextEdit`）を選択します。

1. Launch インターフェイスで、![コピー](images/mobile-launch-copyIcon.png)アイコンをクリックして、依存関係のリストをコピーします。前のレッスンで追加した各拡張機能に対応する線が表示されます。各拡張機能には、Mobile Core 拡張機能を基に構築された独自のコードセットがあり、アプリケーションの更新を介してのみ追加または削除できます。

   ![Launch インターフェイスのクリップボードに依存関係をコピーする](images/ios/mobile-launch-install-copyDependencies.png)

1. テキストエディターで、クリップボードから `# Pods for BusBookingSwift` 行の直後に依存関係を貼り付けます。

1. ポッドファイルの更新をテキストエディターに保存します。

   ![依存関係を追加して保存する](images/ios/swift/mobile-launch-install-addDependenciesAndSave.png)

1. これで、テキストエディターを閉じることができます。

1. Launch インターフェイスで、![コピー](images/mobile-launch-copyIcon.png)アイコンをクリックして、次の iOS 命令 `pod repo update` をコピーします。

   ![「pod repo update」をコピーする](images/ios/mobile-launch-install-copyPodRepoUpdate.png)

1. ターミナルアプリケーションで、`pod repo update` コマンドを実行し、コマンドが完了するまで待ちます（数分かかる場合があります）。

   ![「pod repo update」の実行](images/ios/swift/mobile-launch-install-podRepoUpdate.png)

1. Launch インターフェイスで、![コピー](images/mobile-launch-copyIcon.png)アイコンをクリックして、次の iOS 命令 `pod install` をコピーします。

   ![Launch インターフェイスのクリップボードにポッドのインストールをコピーする](images/ios/mobile-launch-install-copyPodInstall.png)

1. ターミナルアプリケーションで、`pod install` コマンドを実行し、完了するまで待ちます。

   ![「pod install」の実行](images/ios/swift/mobile-launch-install-podInstall.png)

1. これで、ターミナルウィンドウを閉じることができます。

1. ファインダーウィンドウを開き、バス予約アプリケーションを保存したフォルダーに移動し、BusBookingSwift.xcworkspace ファイル、ポッドファイル、Podfile.lock ファイル、および Pods フォルダーが作成されていることを確認します。

   ![ファインダーでのポッドの確認](images/ios/swift/mobile-launch-install-podsInFinder.png)

## AppDelegate を更新します。

次に、アプリケーションを更新して SDK を読み込みます。

1. Xcode で `BusBookingSwift.xcworkspace` ファイルを開きます。
1. `AppDelegate.swift` ファイルを開きます。

   ![AppDelegate ファイルを開く](images/ios/swift/mobile-launch-install-openAppDelegate.png)

1. Launch インターフェイスで、「**[!UICONTROL 初期化コードの追加]**」セクションまでスクロールし、使用する iOS 言語として&#x200B;**[!UICONTROL Swift]**&#x200B;を選択します。
1. 「**[!UICONTROL 初期化コードの追加]**」セクションの最初の![コピー](images/mobile-launch-copyIcon.png)アイコンをクリックして、import 文をコピーします。

   ![Swift import 文をクリップボードにコピーする](images/ios/swift/mobile-launch-install-copyImports.png)

1. Xcode で、次の import 文を、`AppDelegate.swift` ファイルの `UIKit` の import の後にペーストします。

   ![Swiftの import 文をAppDelegate ファイルにペーストする](images/ios/swift/mobile-launch-install-pasteImports.png)

1. Launch インターフェイスで、「**[!UICONTROL 初期化コードの追加]**」セクションの 2 番目の![コピー](images/mobile-launch-copyIcon.png)アイコンをクリックして、コア拡張機能に関連する 2 行をコピーします。最初の行では、コンソールログ文がオンになります（使用可能なオプションは、「debug」、「verbose」、「warning」、「error」です）。2 行目は、Launch 環境の一意の識別子を示します。これは重要です。アプリケーションを実稼動環境にデプロイする準備が整ったら、この値を更新する必要があります。

   ![コア文をクリップボードにコピーする](images/ios/swift/mobile-launch-install-copyCore.png)

1. Xcode で、次の Core 文を AppDelegate ファイルの `application(_:didFinishLaunchingWithOptions:)` メソッドの最上部にペーストします。

   ![コア文を AppDelegate ファイルにペーストする](images/ios/swift/mobile-launch-install-pasteCore.png)

1. Launch インターフェイスで、「[!UICONTROL 初期化コードの追加]」セクションの 3 番目の![コピー](images/mobile-launch-copyIcon.png)アイコンをクリックして、拡張機能文をコピーします。

   ![拡張機能文をクリップボードにコピーする](images/ios/swift/mobile-launch-install-copyExtensions.png)

1. Xcode で、次の extension 文を AppDelegate ファイルの `application(_:didFinishLaunchingWithOptions:)` メソッドの `return true` 行の直前にペーストします。

   ![拡張機能文を AppDelegate ファイルにペーストする](images/ios/swift/mobile-launch-install-pasteExtension.png)

>[!NOTE] Launch インターフェイスで提供されるモバイルインストール手順には、ID 拡張機能、ライフサイクル拡張機能、シグナル拡張機能のインポート文と登録文、およびライフサイクル指標の初期化が含まれます。これらの拡張機能は、Mobile Core 拡張機能の一部とみなされます。これらの拡張機能をアプリケーションで使用しない場合は、これらの拡張機能に関連付けられた他のコードを読み込み、登録または実装する必要はありません。
>
> また、これらの拡張機能を使用する場合に考慮する必要がある追加の実装オプションがあります（例えば、ユーザーがアプリケーションをバックグラウンドにまわしたとき、ライフサイクル収集を一時停止/再開できます）。詳しくは、[Mobile Core 拡張機能のドキュメント](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core)を参照してください

## 実装の検証

1. Xcode プロジェクトを保存します。
1. アプリケーションを実行し、シミュレーターで起動します。シミュレーターデバイスを設定していない場合は、iOS 10以降を実行するデバイスを設定し、今すぐ構成します。マウスで `Home` ボタンをクリックするのが簡単なので、iPhone 8 シミュレーターを使いたいと思います。

   ![アプリケーションを実行し、エミュレーターで起動する](images/ios/swift/mobile-launch-install-buildAndLaunch.png)

1. シミュレーターが起動し、アプリケーションが完全に開いて予約画面が表示されるまで待ちます（数分かかる場合があります）。

   ![アプリケーションが完全に開くまで待つ](images/ios/mobile-launch-install-simulator.png)

1. Xcode コンソールでアドビのサーバーに対する呼び出しがおこなわれていることを確認します。

   ![コンソールでの呼び出しの検索](images/ios/swift/mobile-launch-install-console.png)

以下に、特定の呼び出しの例を示します。

1. **Launch 設定を取得する呼び出し**（コンソールを `adobedtm.com` にフィルター）。前のレッスンで入力した拡張設定をメモしておきます。拡張機能を追加するにはアプリケーションの更新が必要ですが、これらの設定は Launch で外部的に管理し、いつでも変更できます。

   ```swift
   2019-01-15 12:11:44.518220-0500 BusDemoSwift[52399:5056293] [AMSDK DEBUG <RulesDownloader>]: Successfully downloaded Rules from 'https://assets.adobedtm.com/launch-EN360aefc739b04410816f751a95861744-development-rules.zip'
   
   {"target.propertyToken":"","target.timeout":5,"global.privacy":"optedin","analytics.backdatePreviousSessionInfo":true,"analytics.offlineEnabled":true,"build.environment":"dev","rules.url":"https://assets.adobedtm.com/launch-EN360aefc739b04410816f751a95861744-development-rules.zip","target.clientCode":"techmarketingdemos","experienceCloud.org":"7ABB3E6A5A7491460A495D61@AdobeOrg","target.autoFetch":true,"target.fetchBackground":true,"lifecycle.sessionTimeout":300,"target.environmentId":"busbookingapp","analytics.server":"tmd.sc.omtrdc.net","analytics.rsids":"tmd-mobile-dev1","analytics.batchLimit":0,"property.id":"PRb4881271498b4f2cbaf67d38a8f3891a","global.ssl":true,"analytics.aamForwardingEnabled":true}
   ```

1. **ID サービスへの要求**（コンソールを `demdex.net` にフィルター）。この例では、ID（`d_mid`）は既に設定済みで、再度レポートされています。

   ```swift
   2019-01-15 12:11:45.164590-0500 BusDemoSwift[52399:5056322] [AMSDK DEBUG <com.adobe.module.identity>]:
   
   Sending request (https://dpm.demdex.net/id?d_rtbd=json&d_ver=2&d_orgid=7ABB3E6A5A7491460A495D61@AdobeOrg&d_mid=17179986463578698626041670574784107777&d_blob=j8Odv6LonN4r3an7LhD3WZrU1bUpAkFkkiY1ncBR96t2PTI&dcs_region=9)
   ```

1. **ID サービスからの応答**（コンソールを `ID Service` にフィルター）。`mid`値が上記の要求内の値`d_mid`とどのように一致するかを確認します。

   ```swift
   2019-01-15 12:11:45.681821-0500 BusDemoSwift[52399:5056322] [AMSDK DEBUG <com.adobe.module.identity>]:
   
   ID Service - Got ID Response (mid: 17179986463578698626041670574784107777, blob: j8Odv6LonN4r3an7LhD3WZrU1bUpAkFkkiY1ncBR96t2PTI, hint: 9, ttl: "604800000 ms")
   ```

1. **Analytics 要求**（コンソールを `Analytics request` にフィルター）。

   ```swift
   2019-01-15 12:11:45.828465-0500 BusDemoSwift[52399:5056336] [AMSDK DEBUG <AnalyticsHitDatabase>]: Analytics request was sent with body
   
   (ndh=1&c.&a.&AppID=BusDemoSwift%201%20%281.0%29&CarrierName=%28null%29&DayOfWeek=3&DaysSinceFirstUse=0&DaysSinceLastUse=0&DeviceName=x86_64&HourOfDay=12&LaunchEvent=LaunchEvent&Launches=3&OSVersion=iOS%2012.1&Resolution=828x1792&RunMode=Application&TimeSinceLaunch=0&ignoredSessionLength=-1547572244&internalaction=Lifecycle&locale=en-US&.a&.c&aamb=j8Odv6LonN4r3an7LhD3WZrU1bUpAkFkkiY1ncBR96t2PTI&aamlh=9&ce=UTF-8&cp=foreground&mid=17179986463578698626041670574784107777&pageName=BusDemoSwift%201%20%281.0%29&pe=lnk_o&pev2=ADBINTERNAL%3ALifecycle&t=00%2F00%2F0000%2000%3A00%3A00%200%20300&ts=1547572305)
   ```

モバイルアプリケーションに SDK が追加されました。

[次：「Adobe Experience Platform ID サービスの追加」&gt;](id-service.md)