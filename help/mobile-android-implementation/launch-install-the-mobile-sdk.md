---
title: モバイル Android アプリケーションへの Adobe Mobile SDK のインストール
description: Launch プロパティの埋め込みコードを取得し、Web サイトに実装する方法を説明します。このレッスンは、「モバイル Android アプリケーションでの Experience Cloud の実装」チュートリアルの一部です。
seo-description: null
seo-title: モバイル Android アプリケーションへの Adobe Mobile SDK のインストール
solution: Experience Cloud
translation-type: ht
source-git-commit: d669338b0e298c44c1ef2ed1a4491b8451d45f7e

---


# Mobile SDK のインストール

このレッスンでは、Launch プロパティの開発環境に対応する拡張機能と設定を持つ Mobile SDK を実装します。

## 前提条件

このレッスンでは、バス予約アプリにコードを追加します。まだの場合は、次の手順を実行してください。

1. [Android Studio](https://developer.android.com/studio) をダウンロードしてインストールする
1. [バス予約アプリ](https://github.com/Adobe-Marketing-Cloud/busbooking-mobileapps)をダウンロードする。

## 学習内容

このレッスンを最後まで学習すると、以下の内容を習得できます。

* モバイル Launch プロパティのインストール手順を取得する
* 開発環境、ステージング環境および実稼動環境の違いについて理解する
* `build.gradle` ファイルを更新して Mobile SDK を追加する
* Mobile SDK をアプリに読み込む
* SDK が正しく実装されていることを確認する
* アプリケーションでライフサイクル指標を有効化する

## インストール手順の取得

モバイル Launch プロパティのインストール手順は、モバイルアプリケーションで特定の場所に追加するコードスニペットの集まりです。

上部のナビゲーションで「`Environments`」タブをクリックして、環境ページに移動します。開発環境、ステージング環境、および実稼動環境は、事前に作成されています。これらは、コード開発やリリースプロセスの一般的な環境に対応しています。コードは、開発環境の開発者が最初に記述します。作業が完了したら、QA および他のチームがレビューできるよう、ステージング環境に送信します。QA チームやその他のチームが満足したら、コードは実稼動環境に公開されます。この環境は、訪問者がアプリケーションをダウンロードしたときに表示される、公開環境です。

Launch では、追加の開発環境を許可します。これは、複数の開発者が同時に異なるプロジェクトで作業する大規模な組織で便利です。

このチュートリアルを完了するために必要な環境は、開発、ステージング、実稼動環境のみです。

![上部のナビゲーションで「環境」をクリックする](images/mobile-launch-environments.png)

**[!UICONTROL 開発]**&#x200B;行でインストールアイコンをクリックして![インストールアイコン](images/mobile-launch-installIcon.png)埋め込みコードモーダルを開きます。

![アイコンをクリックしてモーダルを開く](images/mobile-launch-openEmbedCode.png)

手順を順を追って説明します。

## build.gradle ファイルの更新

以前に Web サイトで Launch を使用したことがある場合、最初に気付くことの 1 つは、Web サイトよりもモバイルアプリに対して多くのインストール手順があることです。

Android 向け Adobe Mobile SDK は、Gradle を使用して、様々なコンポーネント間の依存関係を管理します。最初におこなうことの 1 つは、Adobe Mobile SDK の依存関係をバス予約アプリケーションの build.gradle ファイルに追加することです。

**Build.gradle ファイルを更新するには、以下を実行します。**

1. Android Studio を開きます。
1. 「既存の Android Studio プロジェクトを開く」を選択します。

   ![「既存の Android Studio プロジェクトを開く」を選択する](images/android/mobile-launch-install-openProject.png)

1. Bus Booking の Android フォルダーのルートにある build.gradle ファイルを開きます。

   ![Bus Booking の Android フォルダーのルートにある build.gradle ファイルを開く](images/android/mobile-launch-install-openApp.png)

1. ドロップダウンを開き、「プロジェクト」ビューに切り替えます。

   ![ドロップダウンから「プロジェクト」を選択する](images/android/mobile-launch-install-openProjectView.png)

1. **Android／bus／build.gradle** ファイルを開きます。

   ![Android／bus／build.gradle を開く](images/android/mobile-launch-install-openGradle.png)

1. Launch インターフェイスで、オペレーティングシステムが `Android` に設定されていることを確認します。

1. ![コピー](images/mobile-launch-copyIcon.png)アイコンをクリックして、依存関係をクリップボードにコピーします。

   ![Launch インターフェイスのクリップボードに依存関係をコピーする](images/android/mobile-launch-install-copyDependencies.png)

1. Android Studio で、クリップボードから既存の依存関係の直後（ただし、`}` を閉じる前）に依存関係を貼り付けます。
1. さらに、Adobe Target VEC 拡張機能をインストールする場合は、次の依存関係も追加する必要があります。

   ```java
   implementation 'com.google.code.gson:gson:2.8.2'
   implementation 'android.arch.lifecycle:extensions:1.1.1'
   implementation 'io.github.sac:SocketclusterClientJava:1.7.5'
   implementation 'com.android.support:support-annotations:28.0.0'
   implementation 'com.android.support:support-compat:28.0.0'
   implementation 'com.android.support:design:28.0.0'
   ```

1. 「今すぐ同期」リンクをクリックして、プロジェクトを同期します

   ![依存関係を build.gradle に貼り付ける](images/android/mobile-launch-install-pasteDependencies.png)

## アプリケーションの更新

次に、アプリケーションを更新して SDK を読み込みます。

**SDK を読み込むには、以下を実行します。**

1. バス予約アプリケーションでメインアプリケーションファイルを開きます。これは、**Android／bus／src／main／java／com.adobe.busbooking／DemoApplication**&#x200B;にあります。

   ![DemoApplication を開く](images/android/mobile-launch-install-openDemoApplication.png)

1. Launch インターフェイスで、「**[!UICONTROL 初期化コードの追加]**」セクションまでスクロールします。
1. 「**[!UICONTROL 初期化コードの追加]**」セクションの最初の![コピー](images/mobile-launch-copyIcon.png)アイコンをクリックして、import 文をコピーします。

   ![import 文をクリップボードにコピーする](images/android/mobile-launch-install-copyImports.png)

1. Android Studio では、`DemoApplication` ファイル内で、既存の import の&#x200B;*前に*&#x200B;これらの import 文をペーストします。コア拡張機能には、コールバック、ID サービス、ライフサイクル指標、コンソールログなど、他の機能をサポートするライブラリが含まれています。

   ![import 文を DemoApplication ファイルにペーストする](images/android/mobile-launch-install-pasteImports.png)

1. Launch インターフェイスで、「**[!UICONTROL 初期化コードの追加]**」セクションの 2 番目の![コピー](images/mobile-launch-copyIcon.png)アイコンをクリックして、コア拡張機能に関連する 2 行をコピーします。2 行目は、コンソールログ文をオンにします（使用可能なオプションは、「DEBUG」、「VERBOSE」、「WARNING」、「ERROR」です）。

   ![コア文をクリップボードにコピーする](images/android/mobile-launch-install-copyCore.png)

1. Android Studio で、`DemoApplication` ファイルの `super.onCreate()` の直後に次のコア文を貼り付けます。
1. `try` 行と `catch` 行の前の `//` コメントを削除します。

   ![コア文を DemoApplication ファイルにペーストする](images/android/mobile-launch-install-pasteCore.png)

1. Launch インターフェイスで、「[!UICONTROL 初期化コードの追加]」セクションの 3 番目の![コピー](images/mobile-launch-copyIcon.png)アイコンをクリックして、拡張機能文をコピーします。

   ![拡張機能文をクリップボードにコピーする](images/android/mobile-launch-install-copyExtensions.png)

1. Android Studio で、次の拡張機能文を `try` セクションに貼り付けます。`MobileCore.configureWithAppID` には、プロパティの Launch 開発環境での識別子が含まれます。これは重要です。アプリケーションを実稼動環境にデプロイする準備が整ったら、この値を更新する必要があります。

   ![DemoApplication ファイルに拡張機能文を貼り付ける](images/android/mobile-launch-install-pasteExtensions.png)

>[!NOTE] Launch インターフェイスで提供されるモバイルインストール手順には、ID 拡張機能、ライフサイクル拡張機能、シグナル拡張機能のインポート文と登録文、およびライフサイクル指標の初期化が含まれます。これらの拡張機能は、Mobile Core 拡張機能の一部とみなされます。これらの拡張機能をアプリケーションで使用しない場合は、これらの拡張機能に関連付けられた他のコードを読み込み、登録または実装する必要はありません。
>
> また、これらの拡張機能を使用する場合に考慮する必要がある追加の実装オプションがあります（例えば、ユーザーがアプリケーションをバックグラウンドにまわしたとき、ライフサイクル収集を一時停止/再開できます）。詳しくは、[Mobile Core 拡張機能のドキュメント](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core)を参照してください

## 実装の検証

1. Android Studio プロジェクトを保存します。
1. アプリケーションを実行し、エミュレーターで起動します。エミュレーターデバイスを設定していない場合は、Android 4.1（API 16）以降を実行するデバイスを設定し、今すぐ構成します。

   ![アプリケーションを実行し、エミュレーターで起動する](images/android/mobile-launch-install-buildAndLaunch.png)

1. エミュレーターが起動し、アプリケーションが完全に開いて予約画面が表示されるまで待ちます（数分かかる場合があります）。

   ![アプリケーションが完全に開くまで待つ](images/android/mobile-launch-install-simulator.png)

1. Android Studio Logcat で、アドビサーバーに対して呼び出しがおこなわれていることを確認します。

   ![アプリケーションが完全に開くまで待つ](images/android/mobile-launch-install-console.png)

以下に、特定の呼び出しの例を示します。

1. **Launch 設定を取得する呼び出し**（Logcat を`adobedtm.com` にフィルター）。前のレッスンで入力した拡張設定をメモしておきます。拡張機能を追加するにはアプリケーションの更新が必要ですが、これらの設定は Launch で外部的に管理し、いつでも変更できます。

   ```java
   03-14 16:30:29.484 24869-24930/com.adobe.busbooking D/ADBMobile: ConfigurationExtension - Cached configuration loaded.
    {"target.propertyToken":"","target.timeout":5,"global.privacy":"optedin","analytics.backdatePreviousSessionInfo":true,"analytics.offlineEnabled":true,"build.environment":"dev","rules.url":"https://assets.adobedtm.com/launch-EN360aefc739b04410816f751a95861744-development-rules.zip","experienceCloud.org":"7ABB3E6A5A7491460A495D61@AdobeOrg","target.clientCode":"techmarketingdemos","target.autoFetch":true,"target.fetchBackground":false,"lifecycle.sessionTimeout":300,"target.environmentId":"busbookingapp","analytics.server":"tmd.sc.omtrdc.net","analytics.rsids":"tmd-mobile-dev1","analytics.batchLimit":0,"property.id":"PRb4881271498b4f2cbaf67d38a8f3891a","global.ssl":true,"analytics.aamForwardingEnabled":true}
   ```

1. **ID サービスへの要求**（Logcat を`IdentityExtension` にフィルター）。この例では、ID（`d_mid`）は既に設定済みで、再度レポートされています。

   ```java
   03-14 17:01:18.526 7743-7803/com.adobe.busbooking D/ADBMobile: IdentityExtension - Sending request (https://dpm.demdex.net/id?d_mid=59651426340521082405908216148091920022&d_ver=2&d_orgid=7ABB3E6A5A7491460A495D61%40AdobeOrg)
   ```

1. **Analytics 要求**（Logcat を `AnalyticsExtension` にフィルター）。

   ```java
   03-14 17:01:18.509 7743-7777/com.adobe.busbooking D/ADBMobile: AnalyticsExtension - Sending Analytics ID call (https://tmd.sc.omtrdc.net/id?mcorgid=7ABB3E6A5A7491460A495D61%40AdobeOrg&mid=59651426340521082405908216148091920022)
   ```

モバイルアプリケーションに SDK が追加されました。

## アプリケーションでのライフサイクル指標の有効化

ライフサイクル指標は、Experience Platform Mobile SDK を使用してアプリケーションで簡単に有効にできる、環境ベースの指標およびディメンションです。ライフサイクル指標は複数の Experience Cloud ソリューションで使用できるので、個々のソリューションをさらに掘り下げる前に、ここで有効にします。これは、アプリケーションの適切な場所にコードを 2、3 行追加するのと同じくらい簡単です。

### BusBookingActivity ファイルへのコアライブラリのインポート

Adobe Experience Platform Mobile SDK を介して API 呼び出しをおこなうには、ライブラリを適切なファイルに読み込む必要があります。この場合、Lifecycle API 呼び出しを利用するには、コアライブラリをインポートする必要があります。

1. Android Studioでアプリケーションを開き、作業していた DemoApplication ファイルと同じディレクトリにある BusBookingActivity ファイルを開きます。
1. ファイルの先頭に次の Mobile Core の import 文を追加して、関連する API 呼び出しを使用できるようにします。
   `import com.adobe.marketing.mobile.MobileCore;`

![Mobile Core ライブラリの読み込み](images/android/mobile-launch-install-importMobileCore.png)

### ライフサイクルコードの追加

次に、ライフサイクル関数をトリガーするために、アプリケーションのメインの onResume() 関数にライフサイクルコードを追加します。

1. BusBookingActivity ファイルを開きます。
1. ファイルの下部付近を下にスクロールし、onResume() 関数を探します。
1. 次の 2 行のコードを`super.onResume()` 行の下に追加します。

   ```java
    MobileCore.setApplication(getApplication());
    MobileCore.lifecycleStart(null);
   ```

![ライフサイクルコードの挿入](images/android/mobile-launch-install-lifecycle.png)

### ライフサイクルヒットの検証

アプリケーションを実行すると、Android Studio のデバッグセクションに 1 つ以上のライフサイクルメッセージが表示されます。

1. ビルドを実行し、アプリを実行するシミュレーターを選択します。
1. シミュレーターを起動して実行したら、Android Studio のデバッガーで「実行」セクションをクリックします。
1. `internalaction=Lifecycle` を検索します
1. このキーと値のペア、および他のライフサイクル指標を含む行があることを確認します。

実際に表示される行は、ライフサイクル指標を含む Analytics の呼び出しです。

![ライフサイクルの検証](images/android/mobile-launch-install-validateLifecycle.png)

[次：「Adobe Experience Platform ID サービスの追加」&gt;](id-service.md)