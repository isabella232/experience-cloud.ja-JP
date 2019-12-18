---
title: Adobe Mobile SDKをモバイルAndroidアプリにインストールする
description: Launchプロパティの埋め込みコードを取得し、Webサイトに実装する方法を説明します。 このレッスンは、「モバイルAndroidアプリケーションでのExperience cloudの実装」チュートリアルの一部です。
seo-description: null
seo-title: Adobe Mobile SDKをモバイルAndroidアプリにインストールする
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: d669338b0e298c44c1ef2ed1a4491b8451d45f7e

---


# Mobile SDK のインストール

このレッスンでは、Launchプロパティの開発環境に対応する拡張機能と設定を持つMobile SDKを実装します。

## 前提条件

このレッスンでは、バス予約アプリにコードを追加します。まだコードを追加していない場合は、次の手順を実行します。

1. [Android studioのダウンロードとインストール](https://developer.android.com/studio)
1. バス予約アプ [リのダウンロード](https://github.com/Adobe-Marketing-Cloud/busbooking-mobileapps)

## 学習目標

このレッスンを最後まで学習すると、以下の内容を習得できます。

* モバイル起動プロパティのインストール手順の取得
* 開発環境、ステージング環境および実稼動環境の違いについて理解する
* ファイルを更 `build.gradle` 新してMobile SDKを追加します
* Mobile SDKのアプリへの読み込み
* SDKが正しく実装されていることを確認します
* アプリでのライフサイクル指標の有効化

## インストール手順の取得

モバイル起動プロパティのインストール手順は、モバイルアプリの特定の場所に追加するコードスニペットの集まりです。

上部のナビゲーショ `Environments` ンのタブをクリックして、環境ページに移動します。 開発環境、ステージング環境および実稼働環境が事前に作成されていることに注意してください。 これらは、コードの開発およびリリースプロセスの一般的な環境に対応しています。 コードは、開発環境で開発者が最初に記述します。 作業が完了したら、QA および他のチームがレビューできるよおう、ステージング環境に送信します。QAやその他のチームが満足したら、コードが実稼働環境に発行されます。実稼働環境は、訪問者がアプリをダウンロードする際に体験する公開環境です。

「起動」を使用すると、追加の開発環境が可能になります。この環境は、複数の開発者が異なるプロジェクトで同時に作業を行う大規模な組織で役立ちます。

このチュートリアルを完了するために必要な環境は、開発、ステージング、実稼働環境のみです。

![上部のナビゲーションで「環境」をクリックします。](images/mobile-launch-environments.png)

「開発」行で **[!UICONTROL 、「インストール]** 」アイコン「インストール」 ![アイコンをクリックし](images/mobile-launch-installIcon.png) 、埋め込みコードモーダルを開きます。

![アイコンをクリックして、埋め込みコードモーダルを開きます](images/mobile-launch-openEmbedCode.png)

手順を順を追って説明します。

## build.gradelファイルを更新します。

以前にWebサイトで「起動」を使用したことがある場合、最初に気付くことの1つは、Webサイトよりもモバイルアプリに対して多くのインストール手順があることです。

Android向けAdobe Mobile SDKは、Gradleを使用して、様々なコンポーネント間の依存関係を管理します。 最初に行うことの1つは、Adobe Mobile SDKの依存関係をBus Bookingアプリのbuild.gradleファイルに追加することです。

**build.gradleファイルを更新するには**

1. Android studioを開く
1. 「既存のAndroid studioプロジェクトを開く」を選択します。

   ![「既存のAndroid studioプロジェクトを開く」を選択します。](images/android/mobile-launch-install-openProject.png)

1. Bus Booking Androidフォルダーのルートにあるbuild.gradleファイルを開きます。

   ![Bus Booking Androidフォルダーのルートにあるbuild.gradleファイルを開きます](images/android/mobile-launch-install-openApp.png)

1. ドロップダウンを開き、[プロジェクト]ビューに切り替えます

   ![ドロップダウンから\[プロジェクト\]を選択します。](images/android/mobile-launch-install-openProjectView.png)

1. **Android/bus/build.gradleファイルを開きます** 。

   ![Android/bus/build.gradleを開きます。](images/android/mobile-launch-install-openGradle.png)

1. 起動インターフェイスで、オペレーティングシステムが `Android`

1. 「コピー」アイコンをクリックして、依存関係をクリップボードにコ ![ピー](images/mobile-launch-copyIcon.png)

   ![起動インターフェイスのクリップボードに依存関係をコピーする](images/android/mobile-launch-install-copyDependencies.png)

1. Android studioで、クリップボードから既存の依存関係の直後（ただし、閉じる前）に依存関係を貼り付けま `}`す
1. さらに、Adobe Target VEC Extensionをインストールする場合は、次の依存関係も追加する必要があります。

   ```java
   implementation 'com.google.code.gson:gson:2.8.2'
   implementation 'android.arch.lifecycle:extensions:1.1.1'
   implementation 'io.github.sac:SocketclusterClientJava:1.7.5'
   implementation 'com.android.support:support-annotations:28.0.0'
   implementation 'com.android.support:support-compat:28.0.0'
   implementation 'com.android.support:design:28.0.0'
   ```

1. 「今すぐ同期」リンクをクリックして、プロジェクトを同期します

   ![依存関係をbuild.gradleに貼り付けます。](images/android/mobile-launch-install-pasteDependencies.png)

## アプリの更新

次に、アプリを更新してSDKを読み込みます

**SDKを読み込むには**

1. バス予約アプリケーションでメインアプリケーションファイルを開きます。 **Android/バス/src/main/java/com.adobe.busbooking/DemoApplicationにあります。**

   ![DemoApplicationを開く](images/android/mobile-launch-install-openDemoApplication.png)

1. 「起動」インタフェースで、「初期化コードの追加」セ **[!UICONTROL クションまでスクロールし]** ます。
1. 「初期化コードの追加」セクションの最初の「コピー ![」アイコンをクリックし](images/mobile-launch-copyIcon.png) て、インポート文をコピーします **** 。

   ![インポート文をクリップボードにコピーします](images/android/mobile-launch-install-copyImports.png)

1. Android studioでは、ファイル内の既存のインポートの前に *これらの* import文をペースト `DemoApplication` します。 コア拡張機能には、コールバック、IDサービス、ライフサイクル指標、コンソールログなど、他の機能をサポートするライブラリが含まれています。

   ![インポート文をDemoApplicationファイルに貼り付けます。](images/android/mobile-launch-install-pasteImports.png)

1. 起動インターフェイスで、「初期化コードを追加」セクションの2番目の「コピー ![」アイコンをクリックして、Core拡張に関連する2](images/mobile-launch-copyIcon.png) 行をコピーします **** 。 2行目は、コンソールログ文をオンにします（使用可能なオプションは、「DEBUG」、「VERBOSE」、「WARNING」、「ERROR」です）。

   ![Coreステートメントのクリップボードへのコピー](images/android/mobile-launch-install-copyCore.png)

1. Android studioで、次のCoreステートメントをファイルに `DemoApplication` 次の直後に貼り付けます `super.onCreate()`
1. 行と行の前 `//` のコメントを削 `try` 除しま `catch` す。

   ![コア文をDemoApplicationファイルに貼り付けます。](images/android/mobile-launch-install-pasteCore.png)

1. 「起動」インターフェイスで、「初期化コードの追加」セクションの3番目の「 ![Copy](images/mobile-launch-copyIcon.png) 」アイコンをクリッ [!UICONTROL クして、拡張文をコピーします] 。

   ![Extensionステートメントをクリップボードにコピーします](images/android/mobile-launch-install-copyExtensions.png)

1. Android studioで、次の拡張文をセクションに貼り付け `try` ます。 プロパティ `MobileCore.configureWithAppID` の起動開発環境の識別子が含まれます。 これは重要です。アプリを実稼働環境にデプロイする準備が整ったら、この値を更新する必要があります。

   ![DemoApplicationファイルに拡張機能文を貼り付けます。](images/android/mobile-launch-install-pasteExtensions.png)

>[!NOTE] 起動インターフェイスで提供されるモバイルインストール手順には、ID、ライフサイクル、シグナル拡張のインポートと登録の文、およびライフサイクル指標の初期化が含まれます。 これらの拡張機能は、Mobile core拡張の一部と見なされます。 これらの拡張機能をアプリで使用しない場合は、これらの拡張機能に関連付けられた他のコードを読み込み、登録または実装する必要はありません。
>
> また、これらの拡張機能を使用する場合に考慮する必要がある追加の実装オプションがあります（例えば、ユーザーがアプリをバックグラウンドにしたとき、ライフサイクル収集を一時停止/再開できます）。 詳しくは、Mobile Core拡張機能のドキュメ [ントを参照してください](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core)

## 実装の検証

1. Android studioプロジェクトの保存
1. アプリケーションを実行し、エミュレーターで起動します。 エミュレーターデバイスを設定していない場合は、Android 4.1(API 16)以降を実行するデバイスを設定し、今すぐ設定します。

   ![アプリケーションを実行し、エミュレーターで起動します](images/android/mobile-launch-install-buildAndLaunch.png)

1. エミュレーターが起動し、アプリが完全に開いて予約画面が表示されるまで待ちます（数分かかる場合があります）。

   ![アプリが完全に開くまで待ちます](images/android/mobile-launch-install-simulator.png)

1. Android Studio Logcatで、Adobeサーバーに対して呼び出しが行われていることを確認します。

   ![アプリが完全に開くまで待ちます](images/android/mobile-launch-install-console.png)

以下に、特定の呼び出しの例を示します。

1. **起動設定を取得する呼び出し** (Logcatをフィルタ `adobedtm.com`ー)。 前のレッスンで入力した拡張設定をメモしておきます。 拡張機能を追加するにはアプリの更新が必要ですが、これらの設定は起動で外部的に管理し、いつでも変更できます。

   ```java
   03-14 16:30:29.484 24869-24930/com.adobe.busbooking D/ADBMobile: ConfigurationExtension - Cached configuration loaded.
    {"target.propertyToken":"","target.timeout":5,"global.privacy":"optedin","analytics.backdatePreviousSessionInfo":true,"analytics.offlineEnabled":true,"build.environment":"dev","rules.url":"https://assets.adobedtm.com/launch-EN360aefc739b04410816f751a95861744-development-rules.zip","experienceCloud.org":"7ABB3E6A5A7491460A495D61@AdobeOrg","target.clientCode":"techmarketingdemos","target.autoFetch":true,"target.fetchBackground":false,"lifecycle.sessionTimeout":300,"target.environmentId":"busbookingapp","analytics.server":"tmd.sc.omtrdc.net","analytics.rsids":"tmd-mobile-dev1","analytics.batchLimit":0,"property.id":"PRb4881271498b4f2cbaf67d38a8f3891a","global.ssl":true,"analytics.aamForwardingEnabled":true}
   ```

1. **IDサービスへの要求** (フィルターのログ `IdentityExtension`カット先)この例では、ID (`d_mid`)は既に設定済みで、再度レポートされています。

   ```java
   03-14 17:01:18.526 7743-7803/com.adobe.busbooking D/ADBMobile: IdentityExtension - Sending request (https://dpm.demdex.net/id?d_mid=59651426340521082405908216148091920022&d_ver=2&d_orgid=7ABB3E6A5A7491460A495D61%40AdobeOrg)
   ```

1. **Analyticsリクエスト** (ログのフィルタ `AnalyticsExtension`ー)

   ```java
   03-14 17:01:18.509 7743-7777/com.adobe.busbooking D/ADBMobile: AnalyticsExtension - Sending Analytics ID call (https://tmd.sc.omtrdc.net/id?mcorgid=7ABB3E6A5A7491460A495D61%40AdobeOrg&mid=59651426340521082405908216148091920022)
   ```

モバイルアプリにSDKが追加されました。

## アプリでのライフサイクル指標の有効化

ライフサイクル指標は、Experience Platform Mobile SDKを使用してアプリで簡単に有効にできる、環境ベースの指標およびディメンションです。 複数のExperience cloudソリューションで使用できるので、個々のソリューションをさらに掘り下げる前に、ここでそれらを有効にします。 これは、適切な場所にアプリにコードを2行追加するのと同じくらい簡単です。

### BusBookingActivityファイルへのコアライブラリのインポート

Adobe Experience Platform Mobile SDKを介してAPI呼び出しを行うには、ライブラリを適切なファイルに読み込む必要があります。 この場合、Lifecycle API呼び出しを利用するには、コアライブラリをインポートする必要があります。

1. Android studioでアプリを開き、作業していたDemoApplicationファイルと同じディレクトリにあるBusBookingActivityファイルを開きます。
1. ファイルの先頭に、次のMobileCoreのインポート文を追加して、関連するAPI呼び出しを使用できるようにします
   `import com.adobe.marketing.mobile.MobileCore;`

![モバイルコアライブラリの読み込み](images/android/mobile-launch-install-importMobileCore.png)

### ライフサイクルコードの追加

次に、ライフサイクル関数をトリガーするために、アプリのメインのonResume()関数にライフサイクルコードを追加します。

1. BusBookingActivityファイルを開きます
1. ファイルの下部付近を下にスクロールし、onResume()関数を探します
1. 次の2行のコードを行の下に追加し `super.onResume()` ます。

   ```java
    MobileCore.setApplication(getApplication());
    MobileCore.lifecycleStart(null);
   ```

![ライフサイクルコードの挿入](images/android/mobile-launch-install-lifecycle.png)

### ライフサイクルヒットの検証

アプリを実行すると、Android studioのデバッグセクションに1つ以上のライフサイクルメッセージが表示されます。

1. ビルドを実行し、アプリを実行するシミュレーターを選択します
1. シミュレーターを起動して実行したら、Android studioのデバッガーの「実行」セクションをクリックします
1. 検索する `internalaction=Lifecycle`
1. このキーと値のペア、および他のライフサイクル指標を含む行があることを確認します。

実際に表示される行は、ライフサイクル指標を含むAnalytics呼び出しです。

![ライフサイクルの検証](images/android/mobile-launch-install-validateLifecycle.png)

[次に、「Adobe Experience Platform IDサービスの追加」&gt;](id-service.md)
