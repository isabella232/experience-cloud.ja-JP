---
title: Adobe Audience ManagerのLaunchへの実装
description: サーバー側転送と起動を使用してAdobe Audience ManagerをWebサイトに実装する方法を説明します。 このレッスンは、「モバイルiOS Objective-CアプリケーションでのExperience cloudの実装」チュートリアルの一部です。
seo-description: null
seo-title: Adobe Audience ManagerのLaunchへの実装
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: e9dee6d0aa3b775d0ce617e2b2c57b56de491b8d

---


# Adobe Audience Managerの追加

このレッスンでは、サーバー側転送を使用してAdobe Audience ManagerをExperience Platform Mobile SDKに実装する手順を説明します。

[Adobe Audience Manager](https://docs.adobe.com/content/help/en/audience-manager/user-guide/aam-home.html) (AAM)は、オンラインオーディエンスデータ管理に関する業界トップのサービスを提供し、デジタル広告主や発行者に、販売の成功に役立つデータアセットを制御し活用するためのツールを提供します。

## 学習目標

このレッスンを最後まで学習すると、以下の内容を習得できます。

1. モバイルアプリでAudience Managerを実装する2つの主な方法について説明します
1. Analyticsビーコンのサーバー側転送を使用したAudience Managerの追加
1. Audience Managerの実装を検証します

## 前提条件

このレッスンを完了するには、次の作業が必要です。

1. 「起動の設定」セクションのレッスンを完了するには、「起動プロパティの作成 [」、「拡張機能の追加](launch-create-a-property.md)」、「ライブラリの作成 [」](launch-add-extensions.md)、「モバイルSDK [](launch-create-a-library.md)[](launch-install-the-mobile-sdk.md)のインストール」を行います。

1. Adobe Analyticsへの管理者アクセス権を持つユーザーは、このチュートリアルで使用するレポートスイートに対して「サーバー側転送」を有効にできます。 または、以下の手順に従って、組織の既存の管理に依頼することもできます。

Audience Managerをまだ実装していない場合は、次の手順に従ってAudience Managerサブドメ [インを取得してください](https://docs.adobe.com/content/help/en/audience-manager-learn/tutorials/web-implementation/how-to-identify-your-partner-id-or-subdomain.html)。

## 導入オプション

アプリにAudience Managerを実装するには、次の2つの方法があります。

* サーバー側転送(SSF) - Adobe Analyticsを使用するお客様向けに、これを実装する最も簡単で推奨される方法です。 Adobe Analyticsは、アドビのバックエンドでAAMにデータを転送するので、アプリからAudience Managerに直接リクエストを行う必要がありません。 また、これにより、主要な統合機能が有効になり、Audience Managerコードの実装およびデプロイメントのベストプラクティスに準拠します。

* クライアント側DIL — このアプローチは、Adobe Analyticsをお持ちでないお客様向けです。 アプリのAudience Managerメソッドは、Audience Managerにデータを直接送信します。 この場合、モバイルの「起動」プロパティを設定する際に、起動のAudience Manager拡張機能を使用します。

このチュートリアルの「拡張機能を追加」セクションでAnalytics拡張機能を以前に設定した場合 [](launch-add-extensions.md) 、AnalyticsからAudience Managerへのデータのサーバー側転送を開始するためにこのボックスをオンにしていました。 これにより、Audience Managerセグメントの応答を処理してアプリに戻すのに必要なコードが動的に挿入されます。 このチュートリアルでは、Audience Manager拡張機能を追加しません。これは、Adobe Analyticsをお持ちでない場合の使用例に限られるためです。

## サーバー側転送を有効にする

SSFの実装には、主に次の2つの手順があります。

1. Analytics管理コンソールで「切り替え」をオンにして、レポートスイートごとにAnalyticsからAudience Managerにデータを転 *送します*。
1. SDKコードを配置します。Analytics拡張 **のボックスをオンにして** 、AAMにデータを転送するだけで、「起動」を使用して行ったコードです。

### Analytics Admin Console でサーバー側転送を有効化する

Adobe AnalyticsからAdobe Audience Managerへのデータの転送を開始するには、Adobe Analytics管理コンソールの設定が必要です。 システムがデータの転送を開始するまでに最大4時間かかる場合があるので、転送のトラブルシューティングを行う際は、この点に注意してください。

#### Analytics管理コンソールでSSFを有効にするには

1. Experience Cloud UIを使用してAnalyticsにログインします。 Analyticsへの管理者アクセス権を持っていない場合は、Experience cloudまたはAnalyticsの管理者に問い合わせて、アクセス権を割り当てるか、これらの手順を実行する必要があります。

   ![Adobe Analytics UIにログインする](images/mobile-aam-logIntoAnalytics.png)

1. Analyticsの上部ナビゲーションから、管理者/レポー **[!UICONTROL トスイートを選択し]**、リストからAudience Managerに転送するレポートスイートを選択（または複数選択）します。

   ![管理コンソールに移動します。](images/mobile-aam-analyticsAdminConsoleReportSuites.png)

1. レポートスイート画面で、レポートスイートを選択し、設定の編集/一般/ **[!UICONTROL サーバー側転送を選択します]**。

   ![SSFメニューの選択](images/mobile-aam-selectSSFmenu.png)

   >[!WARNING] 上記のように、このメニュー項目を表示するには管理者権限が必要です。

1. サーバー側転送ページで、情報を読み、「レポートスイートのサーバー側転送を有効にする **** 」チェックボックスをオンにします。

1. Click **[!UICONTROL Save]**

   ![SSFのセットアップの完了](images/mobile-aam-enableSSFcomplete.png)

>[!NOTE] SSFはレポートスイートごとに有効にする必要があるので、実際のアプリのレポートスイートにSSFをデプロイする場合は、必ず実際のレポートスイートに対してこの手順を繰り返してください。
>
>また、SSFオプションが灰色表示の場合は、このオプションを有効にするには、レポートスイートをExperience cloud組織にマッピングする必要があります。 これについては、[ドキュメント](https://docs.adobe.com/content/help/en/core-services/interface/about-core-services/report-suite-mapping.html)で説明しています。

Adobe Experience Platform Identity Serviceが実装されている限り、このスイッチはAAMへのデータの実際の転送を開始します。 残りのSSF実装は、コード内で発生します。これは、Analytics拡張内のボックスをAAMに転送するようにオンにした場合に、「起動」で処理されました。

アプリに対してサーバー側転送コードが実装されました。

### サーバー側転送の検証

サーバー側転送が起動して実行されていることを検証する主な方法は、アプリからのAdobe Analyticsヒットに対する応答を確認することです。

AnalyticsからAudience Managerへのデータのサーバー側転送(SSF)を行っていない場合、Analyticsビーコンに対する応答は実際にはありません（2 x 2ピクセルを除く）。 ただし、SSFを有効にすると、Analyticsのリクエストと応答で検証できる項目があり、正しく機能していることを通知します。

Xcodeコンソールはビーコンに対する応答を表示しないので、Charles Proxyなど、応答を表示する別のデバッガ/パケットスニファーを使用する必要があります（下のスクリーンショットで示す内容）。

1. デバッガーを開き、Adobe Analyticsリクエ `b/ss`ストに表示する内容を制限するフィルターを適用します
1. 前の演習で作成したサンプルアプリの作成と実行
1. Analyticsリクエストの場合は、応答を確認します。 パラメーターとパラ `dcs_region` メーターを含 `uuid` み、「stuff」オブジェクトも含む必要があります。 このオブジェクトは、AAMセグメントIDがブラウザーに返される場所です（ユーザーが属するセグメントの場合、AAMでcookieの宛先に割り当てられます）。 「stuff」オブジェクトを持っている場合、SSFは動作しています。

   ![AA応答 — stuffオブジェクト](images/mobile-aam-AAresponseCharles.png)

>[!WARNING] 「False」の「Success」に注意 — 応答があり、すべてが機能しているように見える場合は、その「 **stuff** 」オブジェクトがあることを確認します。 そうしないと、応答に「status」:「SUCCESS」というメッセージが表示される場合があります。 これが正しく機能していない証拠で **す** 。 これが表示される場合は、「AAMに転送するには起動」の手順が完了していますが、Analytics管理コンソールでの転送がまだ完了していないことを意味します。 この場合は、Analytics管理コンソールでSSFが有効になっていることを確認する必要があります。 もし、まだ4時間も経っていないのなら、我慢しなさい。

![AA応答 — 偽の成功](images/mobile-aam-unsuccessful-SSF.png)

[次の「プロパティを公開」&gt;](publish.md)