---
title: モバイル起動プロパティへの拡張機能の追加
description: モバイルの起動プロパティに拡張機能を追加する方法について説明します。 このレッスンは、「モバイルAndroidアプリケーションでのExperience cloudの実装」チュートリアルの一部です。
seo-description: null
seo-title: モバイル起動プロパティへの拡張機能の追加
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# 拡張子を追加

このレッスンでは、Launchプロパティに拡張機能を追加します。

Launchは、アドビおよびサードパーティベンダーが拡張機能を作成し、Launchを通じてソリューションを簡単に展開できるようにするプラットフォームです。 拡張機能は、起動インターフェイスとクライアント機能を拡張するコードのパッケージです。 拡張機能を使用すると、特定のアプリに必要なAdobe Experience Platform Mobile SDKの一部のみを選択できます。  Launch はオペレーティングシステム、拡張はタスクの遂行に使用されるアプリと考えることができます。

アドビのソリューション（Target、Analytics、Audience Managerなど）を実装する予定なので、これらのソリューションをサポートするために必要な拡張を追加します。

>[!CAUTION] モバイル起動プロパティで拡張機能を追加または削除するには、アプリを更新する必要があります。 これは、Webサイトを更新しなくても、いつでも拡張機能を追加または削除できるWeb起動プロパティとは異なります。

## 前提条件

このレッスンを完了するには、起動ユーザーアカウントで「拡張機能を管理」する権限が必要です。 ユーザーインターフェイスオプションが使用できないためにこれらの手順を完了できない場合は、Experience cloud管理者にアクセス権を問い合わせてください。 For more information on Launch permissions, see [the documentation](https://docs.adobe.com/content/help/en/launch/using/reference/admin/user-permissions.html).

次のソリューションの詳細が必要です。

* 1つのAnalyticsレポートスイートID。 このチュートリアルで使用できるテスト/開発レポートスイートがない場合は、レポートスイートを作成してください。 方法がわからない場合は、[ドキュメント](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)を参照してください。

* Analyticsトラッキングサーバー。 現在の導入環境、アドビコンサルタントまたはカスタマーケア担当者からトラッキングサーバーを取得できます。

## 学習目標

このレッスンを最後まで学習すると、以下の内容を習得できます。

* モバイル起動プロパティへの拡張機能の追加
*  Analytics 拡張機能の設定
* TargetおよびTarget VEC拡張の設定

>[!NOTE] Adobe Audience Managerは、Analytics拡張機能の設定を介して実装できるので、このチュートリアルでAudience Manager拡張機能を追加する必要はありません

## プリインストール済みの拡張機能の確認

1. 「拡張」タブ **[!UICONTROL をクリックし]** 、拡張機能ページに移動します。
1. Mobile coreと拡張機能は、新しいmobileプ `Profile` ロパティに事前にインストールされています
1. Core拡張機能の **[!UICONTROL 「設定]** 」ボタンをクリックして、設定を確認します

   ![「拡張子」タブに移動します。](images/mobile-extensions-installed-default.png)

1. Mobile core拡張機能は、アプリの実装に必要なコアAdobe Experience Platform Mobile SDKを表します。 コアには、Experience Cloud IDサービス、データイベントハブ、ルールエンジン、再利用可能なネットワーク、ディスクアクセスルーチンなど、すべてのアドビおよびサードパーティの拡張機能で必要とされる機能とフレームワークの共通セットが含まれます。  Mobile core拡張機能について詳しくは、ドキュメントを参照 [してください](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core)。

   1. Experience cloud組織IDは自動的に検出され、事前に設定されます
   1. 「Experience Cloudサーバー」フィールドを使用すると、訪問者IDサービスリクエストのカスタムエンドポイントを指定できます。 このチュートリアルでは、デフォルト設定を使用します（空白のままにします）。
   1. 「セッションタイムアウト」フィールドでは、アプリのライフサイクルセッションがいつタイムアウトするかを指定できます。 デフォルトでは、アプリがバックグラウンドで300秒間動作している場合はタイムアウトになります。 このチュートリアルのデフォルト設定を使用します。

1. 設定を変更していないので、「キャンセル」をクリックし **[!UICONTROL て]** 、拡張機能の設定を終了します

   ![「キャンセル」をクリックして設定を終了します](images/mobile-extensions-core-cancel.png)

1. Profile拡張機能を使用すると、SDKはクライアント側プロファイルにデータを保存できます。 設定がないので、見る必要はありません。 Profile拡張機能について詳しくは、ドキュメントを参照 [してください](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/profile)。

## ソリューション拡張の追加

次に、楽しいパートに進み、このチュートリアルで実装するソリューションの拡張機能を追加します。 モバイルアプリケーションで「起動」を使用する場合、拡張機能が追加または削除されるたびに、アプリケーションを更新する必要があります。 後で時間を節約するために、このレッスンではすべての拡張機能を追加します。 会社がライセンスを取得していないソリューションはスキップしてください。

### Adobe Analytics拡張機能の追加

>[!NOTE] Adobe Analyticsのライセンスをお持ちでない場合は、この節をスキップできます。 現時点では、モバイルプロパティ用のAnalytics拡張機能はSDK設定の管理にのみ使用され、ルールアクションなどのインターフェイスオプションは「起動」に追加されません。

**拡張機能を追加するには**

1. 「カタログ」タブをクリックして、アンインストールされた拡張 _子を表示_ します

1. Adobe Analyticsの拡張機能を探し **[!UICONTROL て]** 、「インストール」をクリックし **[!UICONTROL ます]**

   ![拡張機能カタログに移動し、「インストール」をクリックしてAnalytics拡張機能を追加します](images/mobile-extensions-catalog-installAnalytics.png)

1. 事前入力さ **[!UICONTROL れたリストから]** 、レポートスイートを選択します。 これらは、アプリがデータを送信する先のレポートスイートです。 開発環境、ステージング環境および実稼働環境用に異なるレポートスイートを選択できます。
1. **[!UICONTROL Analyticsトラッキングサーバーは]** 、事前に設定されている場合や、事前に設定されたリストから選択するか、手動で入力する必要がある場合があります。 これは、ビーコンの送信先のドメインです。通常は、この形式です `yoursite.sc.omtrdc.net`。
1. 「オフラインが有効」のチェックボ **[!UICONTROL ックスをオンにしま]**&#x200B;す。 「オフラインが有効」チェックボックスを選択すると、Analyticsのヒットは、デバイスがオフラインのときにキューに入れられ、デバイスがオンラインに戻ったときに後で送信されます。 オフライン追跡を使用するには、レポートス **イートで** 「タイムスタンプ」が有効になっていることを確認します。 詳しくは、[ のドキュメント](https://docs.adobe.com/content/help/en/analytics/implementation/javascript-implementation/offline-tracking.html)を参照してください。
1. 「 **[!UICONTROL Audience Manager転送」のチェックボックスをオンにします]**。 これにより、AnalyticsデータがAudience Managerに転送されるので、アプリからAudience Managerに追加の呼び出しを行う必要がありません。 この練習では、Audience Managerがあり、Analyticsからデータを転送すると仮定します。 Audience Managerを持っていない場合は、独自の実装用にAnalyticsを設定する際に、このチェックボックスをオンにしないでください。
1. 前回のセッション情報の日付を遡 **[!UICONTROL るチェックボックスをオンにします]**
1. 「 **[!UICONTROL Save]** 」ボタン

   ![拡張機能カタログに移動し、「インストール」をクリックしてAnalytics拡張機能を追加します](images/mobile-extensions-analytics-settings.png)

### Target 拡張機能の追加

Adobe targetには、Adobe target拡張とAdobe Target VEC拡張の2つの正式な拡張機能があります。 Adobe targetは、以前のモバイルSDKのユーザーに馴染みのあるすべてのAPIをサポートしています。 Adobe Target VEC拡張には、TargetのVisual Experience Composerのサポートが追加されています。これにより、マーケターは、What-You-See-Is-What-You-Get(WYSIWYG)インターフェイスでページ上の画像およびテキスト要素を変更する単純なアクティビティを構築できます。 このチュートリアルでは、両方を使用します。

>[!NOTE] Adobe targetのライセンスをお持ちでない場合は、この節をスキップできます。 現時点では、モバイルプロパティ用のTarget拡張はSDK設定の管理にのみ使用され、ルールアクションなどのインターフェイスオプションは「起動」に追加されません。

**拡張機能を追加するには**

1. 「カタログ」タブをクリックして、アンインストールされた拡張 _子を表示_ します

1. Adobe Target **[!UICONTROL Extensionを探し、「インストール」をクリックし]****[!UICONTROL ます]**

   ![拡張機能カタログに移動し、「インストール」をクリックしてTarget拡張機能を追加します](images/mobile-extensions-catalog-installTarget.png)

1. クライ **[!UICONTROL アントコード]** が事前設定されます。
1. 環境IDは空 **[!UICONTROL 白のままにし]** 、 この設定は、Adobe targetの [Hosts](https://docs.adobe.com/help/en/target/using/administer/hosts.html) 機能と組み合わせて使用され、異なるレポート環境（開発、ステージング、実稼働など）にデータを送信できます。 デフォルトでは、データは実稼働環境に送信されます。
1. 「 **[!UICONTROL Target Workspace」プロパティは空白のままにします]** 。 この設定は、Target Premium [Enterpriseのユーザー権限機能と組み合わせて使用します](https://docs.adobe.com/content/help/en/target/using/administer/manage-users/enterprise/property-channel.html) 。
1. タイムアウ **[!UICONTROL トは]** 5秒のままにします。 この設定は、アプリがデフォルトコンテンツを表示するまでにTargetの応答を待つ時間を制御します。
1. 「 **[!UICONTROL Save]** 」ボタン

   ![Targetの設定](images/mobile-extensions-target-settings.png)

### Target VEC拡張の追加

これで、Target拡張が追加され、Target VEC拡張を追加できます。

>[!NOTE] Adobe targetのライセンスをお持ちでない場合は、この節をスキップできます。 現時点では、モバイルプロパティのTarget VEC拡張はSDK設定の管理にのみ使用され、ルールアクションなどのインターフェイスオプションは起動に追加されません。

**拡張機能を追加するには**

1. 「カタログ」タブをクリックして、アンインストールされた拡張 _子を表示_ します

1. Adobe Target VEC **[!UICONTROL Extensionを探し、「インストール」をクリックし]****[!UICONTROL ます]**

   ![拡張機能カタログに移動し、「インストール」をクリックしてTarget拡張機能を追加します](images/mobile-extensions-catalog-installTargetVEC.png)

1. ターゲッ **[!UICONTROL トキャンペーンの自動取得を有効にします]**`ON` 。 これにより、アプリが最初に読み込まれるときにすべてのTargetアクティビティがプリフェッチされ、実行する必要のあるリクエストの数が減ります。
1. バックグ **[!UICONTROL ラウンドで取得を終了]**`OFF`。 この設定は、が使用されている場合にの `Auto-Fetch Target Campaigns` み表示されます。  この設定を残す `OFF` と、アプリのホーム画面でVECアクティビティを実行できますが、ホーム画面が表示される前にTarget要求が完了したか、またはタイムアウトしたことを確認するために、アプリの起動に遅延が追加されます。 ホーム画面でアクティビティを実行して `OFF` いる場合はこの設定をそのままにし、実行していない場合は `ON` 切り替えることをお勧めします。  この設定は、起動インターフェイスで、アプリを更新せずにいつでも変更できます。
1. 「 **[!UICONTROL Save]** 」ボタン

   ![Target VECの設定](images/mobile-extensions-targetVEC-settings.png)

これで作業は完了です。これで、プロパティに拡張機能が追加され、ライブラリに追加できます。

[次の「ライブラリの作成」&gt;](launch-create-a-library.md)