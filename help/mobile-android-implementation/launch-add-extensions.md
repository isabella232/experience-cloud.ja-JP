---
title: モバイル Launch プロパティへの拡張機能の追加
description: モバイル Launch プロパティに拡張機能を追加する方法について説明します。このレッスンは、「モバイル Android アプリケーションでの Experience Cloud の実装」チュートリアルの一部です。
seo-description: null
seo-title: モバイル Launch プロパティへの拡張機能の追加
solution: Experience Cloud
translation-type: ht
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# 拡張機能の追加

このレッスンでは、Launch プロパティに拡張機能を追加します。

Launch は、アドビおよびサードパーティベンダーが拡張機能を作成し、Launch を通じてソリューションを簡単に展開できるようにするプラットフォームです。拡張機能は、Launch インターフェイスとクライアント機能を拡張するコードのパッケージです。拡張機能を使用すると、Adobe Experience Platform Mobile SDK の特定のアプリに必要な部分のみを選択できます。Launch はオペレーティングシステム、拡張機能はタスクの遂行に使用されるアプリケーションと考えることができます。

アドビのソリューション（Target、Analytics、Audience Manager など）を実装する予定なので、これらのソリューションをサポートするために必要な拡張機能を追加します。

>[!CAUTION] モバイル Launch プロパティで拡張機能を追加または削除するには、アプリケーションを更新する必要があります。これは、Web サイトを更新しなくても、いつでも拡張機能を追加または削除できる Web Launch プロパティとは異なります。

## 前提条件

このレッスンを完了するには、Launch ユーザーアカウントで「拡張機能管理」権限が必要です。ユーザーインターフェイスオプションが使用できないためにこれらの手順を完了できない場合は、Experience Cloud 管理者に連絡してアクセス権をもらってください。Launch の権限について詳しくは、[ドキュメント](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/admin/user-permissions.html)を参照してください。

次のソリューションの詳細が必要です。

* 1 つの Analytics レポートスイート ID。このチュートリアルで使用できるテスト／開発用レポートトスイートがない場合は、作成してください。方法が分からない場合は、[ドキュメント](https://docs.adobe.com/content/help/ja-JP/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)を参照してください。

* Analytics トラッキングサーバー。トラッキングサーバーは、現在の実装、アドビコンサルタントまたはカスタマーケア担当者から取得できます。

## 学習内容

このレッスンを最後まで学習すると、以下の内容を習得できます。

* モバイル Launch プロパティへの拡張機能の追加
*  Analytics 拡張機能の設定
* Target および Target VEC 拡張機能の設定

>[!NOTE] Adobe Audience Manager は、Analytics 拡張機能の設定を介して実装できるので、このチュートリアルで Audience Manager 拡張機能を追加する必要はありません

## プリインストール済みの拡張機能の確認

1. 「**[!UICONTROL 拡張機能]**」タブをクリックして、拡張機能ページに移動します。
1. Mobile Core と `Profile` 拡張機能は、新しいモバイルプロパティに事前にインストールされています。
1. Core 拡張機能の「**[!UICONTROL 設定]**」ボタンをクリックして、設定を確認します。

   ![拡張機能タブに移動する](images/mobile-extensions-installed-default.png)

1. Mobile Core 拡張機能は、アプリケーションの実装に必要なコア Adobe Experience Platform Mobile SDK を表します。コアには、Experience Cloud ID サービス、データイベントハブ、ルールエンジン、再利用可能なネットワーク、ディスクアクセスルーチンなど、すべてのアドビおよびサードパーティの拡張機能で必要とされる機能とフレームワークの共通セットが含まれます。Mobile Core 拡張機能について詳しくは、[ドキュメント](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core)を参照してください。

   1. Experience Cloud 組織 ID は自動的に検出され、事前に設定されます。
   1. 「Experience Cloud サーバー」フィールドを使用すると、訪問者 ID サービスリクエストのカスタムエンドポイントを指定できます。このチュートリアルでは、デフォルト設定を使用します（空白のままにします）。
   1. 「セッションタイムアウト」フィールドでは、アプリケーションのライフサイクルセッションがいつタイムアウトするかを指定できます。デフォルトでは、アプリケーションが 300 秒間バックグラウンドにある場合はタイムアウトします。このチュートリアルではデフォルト設定を使用します。

1. 設定を変更していないので、「**[!UICONTROL キャンセル]**」をクリックして、拡張機能の設定を終了します。

   ![「キャンセル」をクリックして設定を終了する](images/mobile-extensions-core-cancel.png)

1. Profile 拡張機能を使用すると、SDK はクライアントサイドプロファイルにデータを保存できます。設定はないので、表示されません。Profile 拡張機能について詳しくは、[ドキュメント](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/profile)を参照してください。

## ソリューション拡張機能の追加

次に、もっと興味深い部分に進みます。このチュートリアルで実装するソリューションの拡張機能を追加します。モバイルアプリケーションで Launch を使用する場合、拡張機能が追加または削除されるたびにアプリケーションを更新する必要があります。後で時間を節約するために、このレッスンではすべての拡張機能を追加します。会社がライセンスを取得していないソリューションはスキップしてください。

### Adobe Analytics 拡張機能の追加

>[!NOTE] Adobe Analytics のライセンスをお持ちでない場合は、この節をスキップできます。現時点では、モバイルプロパティ用の Analytics 拡張機能は SDK 設定の管理にのみ使用され、Launch にはルールアクションなどのインターフェイスオプションは追加されません。

**拡張機能を追加するには、以下を実行します。**

1. 「カタログ」タブをクリックして、_アンインストールされた_&#x200B;拡張機能を表示します。

1. **[!UICONTROL Adobe Analytics]** 拡張機能を見つけて、**[!UICONTROL インストール]**&#x200B;をクリックします。

   ![拡張機能カタログに移動し、「インストール」をクリックして Analytics 拡張機能を追加する](images/mobile-extensions-catalog-installAnalytics.png)

1. 事前入力されたリストから&#x200B;**[!UICONTROL レポートスイート]**&#x200B;を選択します。これらは、アプリケーションがデータを送信する先のレポートスイートです。開発環境、ステージング環境および実稼動環境用に異なるレポートスイートを選択できます。
1. **[!UICONTROL Analytics トラッキングサーバー]**&#x200B;は、事前に設定されている場合、事前に設定されたリストから選択する場合、手動で入力する必要がある場合があります。これは、ビーコンの送信先のドメインです。通常は、`yoursite.sc.omtrdc.net` の形式です。
1. 「**[!UICONTROL オフラインが有効]**」チェックボックスをオンにします。「オフラインが有効」チェックボックスを選択すると、Analytics のヒットは、デバイスがオフラインのときにキューに入れられ、デバイスがオンラインに戻ったときに後で送信されます。オフライントラッキングを使用するには、レポートスイートでタイムスタンプが有効になっていることを&#x200B;**確認**&#x200B;します。詳しくは、[ドキュメント](https://docs.adobe.com/content/help/ja-JP/analytics/implementation/javascript-implementation/offline-tracking.html)を参照してください。
1. 「**[!UICONTROL Audience Manager 転送]**」チェックボックスをオンにします。これにより、Analytics データが Audience Manager に転送されるので、アプリケーションから Audience Manager に追加の呼び出しをおこなう必要がありません。この練習では、Audience Manager があり、Analytics からデータを転送すると仮定します。Audience Manager がない場合は、独自の実装用に Analytics を設定する際には、このチェックボックスをオンにしないでください。
1. 「**[!UICONTROL 前回のセッション情報の日付を遡る]**」チェックボックスをオンにします。
1. 「**[!UICONTROL 保存]**」ボタンをクリックします。

   ![拡張機能カタログに移動し、「インストール」をクリックして Analytics 拡張機能を追加する](images/mobile-extensions-analytics-settings.png)

### Target 拡張機能の追加

Adobe Target には、Adobe Target と Adobe Target VEC の 2 つの正式な拡張機能があります。Adobe Target は、以前のモバイル SDK のユーザーに馴染みのあるすべての API をサポートしています。Adobe Target VEC 拡張機能には、Target の Visual Experience Composer のサポートが追加されています。これにより、マーケターは、What-You-See-Is-What-You-Get（WYSIWYG）インターフェイスでページ上の画像およびテキスト要素を変更する単純なアクティビティを構築できます。このチュートリアルでは、両方を使用します。

>[!NOTE] Adobe Target のライセンスをお持ちでない場合は、この節をスキップできます。現時点では、モバイルプロパティ用の Target 拡張は SDK 設定の管理にのみ使用され、Launch にはルールアクションなどのインターフェイスオプションは追加されません。

**拡張機能を追加するには、以下を実行します。**

1. 「カタログ」タブをクリックして、_アンインストールされた_&#x200B;拡張機能を表示します。

1. **[!UICONTROL Adobe Target]**&#x200B;拡張機能を探し、**[!UICONTROL インストール]**&#x200B;をクリックします。

   ![拡張機能カタログに移動し、「インストール」をクリックして Target 拡張機能を追加する](images/mobile-extensions-catalog-installTarget.png)

1. **[!UICONTROL クライアントコード]**&#x200B;が事前設定されます。
1. **[!UICONTROL 環境 ID]** は空白のままにします。この設定は、Adobe Target の [Hosts](https://docs.adobe.com/help/fr-FR/target/using/administer/hosts.html) 機能と組み合わせて使用され、異なるレポート環境（開発、ステージング、実稼動など）にデータを送信できます。デフォルトでは、データは実稼動環境に送信されます。
1. 「**[!UICONTROL Target Workspace プロパティ]**」は空白のままにします。この設定は、Target Premium の [Enterprise ユーザー権限](https://docs.adobe.com/content/help/ja-JP/target/using/administer/manage-users/enterprise/property-channel.html)機能と組み合わせて使用されます。
1. **[!UICONTROL タイムアウト]**&#x200B;は 5 秒のままにします。この設定は、アプリケーションがデフォルトコンテンツを表示するまでに Target の応答を待つ時間を制御します。
1. 「**[!UICONTROL 保存]**」ボタンをクリックします。

   ![Target の設定](images/mobile-extensions-target-settings.png)

### Target VEC 拡張機能の追加

これで、Target 拡張機能が追加されたので、Target VEC 拡張機能を追加できます。

>[!NOTE] Adobe Target のライセンスをお持ちでない場合は、この節をスキップできます。現時点では、モバイルプロパティ用の Target VEC 拡張機能は SDK 設定の管理にのみ使用され、Launch にはルールアクションなどのインターフェイスオプションは追加されません。

**拡張機能を追加するには、以下を実行します。**

1. 「カタログ」タブをクリックして、_アンインストールされた_&#x200B;拡張機能を表示します。

1. **[!UICONTROL Adobe Target VEC]**&#x200B;拡張機能を見つけて、**[!UICONTROL インストール]**&#x200B;をクリックします。

   ![拡張機能カタログに移動し、「インストール」をクリックして Target 拡張機能を追加する](images/mobile-extensions-catalog-installTargetVEC.png)

1. **[!UICONTROL Target キャンペーンの自動取得]**`ON`を有効にします。これにより、アプリケーションが最初に読み込まれるときにすべての Target アクティビティがプリフェッチされ、実行する必要のある要求の数が減ります。
1. **[!UICONTROL バックグラウンドで取得]**`OFF`をそのままにします。この設定は、`Auto-Fetch Target Campaigns` が使用されている場合にのみ表示されます。この設定を `OFF` にすると、アプリケーションのホーム画面で VEC アクティビティを実行できますが、ホーム画面が表示される前に Target 要求が完了したか、またはタイムアウトしたことを確認するために、アプリケーションの起動に遅延が追加されます。ホーム画面でアクティビティを実行している場合はこの設定を `OFF` のままにし、実行していない場合は `ON` に切り替えることをお勧めします。この設定は、Launch インターフェイスで、アプリケーションを更新せずにいつでも変更できます。
1. 「**[!UICONTROL 保存]**」ボタンをクリックします。

   ![Target VEC の設定](images/mobile-extensions-targetVEC-settings.png)

これで作業は完了です。これで、プロパティに拡張機能が追加され、ライブラリに追加できます。

[次：「ライブラリの作成」&gt;](launch-create-a-library.md)