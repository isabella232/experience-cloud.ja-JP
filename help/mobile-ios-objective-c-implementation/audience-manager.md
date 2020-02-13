---
title: Launch を使用した Adobe Audience Manager の実装
description: サーバーサイド転送と Launch を使用して Adobe Audience Manager を Web サイトに実装する方法を説明します。このレッスンは、「モバイル iOS Objective-C アプリケーションでの Experience Cloud の実装」チュートリアルの一部です。
seo-description: null
seo-title: Launch を使用した Adobe Audience Manager の実装
solution: Experience Cloud
translation-type: ht
source-git-commit: e9dee6d0aa3b775d0ce617e2b2c57b56de491b8d

---


# Adobe Audience Manager の追加

このレッスンでは、サーバーサイド転送を使用して Adobe Audience Manager を Experience Platform Mobile SDK に実装する手順を説明します。

[Adobe Audience Manager](https://docs.adobe.com/content/help/ja-JP/audience-manager/user-guide/aam-home.html)（AAM）は、オンラインでオーディエンスデータを管理する、業界をリードするサービスを提供しています。デジタル広告主やパブリッシャーは、データアセットを制御および活用して販売の成功を促進するために必要なツールを利用できます。

## 学習内容

このレッスンを最後まで学習すると、以下の内容を習得できます。

1. モバイルアプリケーションで Audience Manager を実装する 2 つの主な方法について説明する。
1. Analytics ビーコンのサーバーサイド転送を使用して Audience Manager を追加する。
1. Audience Manager の実装を検証する。

## 前提条件

このレッスンを完了するには、次が必要です。

1. 「Launch の設定」節のレッスン（[Launch プロパティの作成](launch-create-a-property.md)、[拡張機能の追加](launch-add-extensions.md)、[ライブラリの作成](launch-create-a-library.md)、[Mobile SDK のインストール](launch-install-the-mobile-sdk.md)）を完了していること。

1. このチュートリアルで使用するレポートスイートのサーバーサイド転送を有効化するための、Adobe Analytics への管理者アクセス。または、以下の手順に従って、組織の既存の管理に依頼することもできます。

Audience Manager をまだ実装していない場合は、次の手順に従って [Audience Manager サブドメインを取得](https://docs.adobe.com/content/help/en/audience-manager-learn/tutorials/web-implementation/how-to-identify-your-partner-id-or-subdomain.html)してください。

## 実装オプション

アプリケーションに Audience Manager を実装するには、次の 2 つの方法があります。

* サーバーサイド転送（SSF）— Adobe Analytics のユーザーにとって最も簡単な実装方法です（推奨）。Adobe Analytics は、アドビのバックエンドで AAM にデータを転送するので、アプリケーションから Audience Manager に直接要求を実行する必要がありません。これにより、主要統合機能が有効になり、Audience Manager のコードの実装とデプロイメントのベストプラクティスにも準拠します。

* クライアントサイド DIL — このアプローチは、Adobe Analytics をお持ちでないお客様向けです。アプリケーションの Audience Manager メソッドは、Audience Manager にデータを直接送信します。この場合、モバイル Launch プロパティを設定する際に、Launch の Audience Manager 拡張機能を使用します。

このチュートリアルの[拡張機能の追加](launch-add-extensions.md)節で Analytics 拡張機能を以前に設定したとき、Analytics から Audience Manager へのデータのサーバーサイド転送を開始するためのボックスをオンにしました。これにより、Audience Manager セグメントの応答を処理してアプリケーションに戻すのに必要なコードが動的に挿入されます。このチュートリアルでは、Audience Manager 拡張機能は追加しません。これは、Adobe Analytics をお持ちでない場合の使用例に限られるためです。

## サーバーサイド転送を有効にする

SSF の実装には、主に次の 2 つの手順があります。

1. Analytics Admin Console で「切り替え」をオンにして、*レポートスイートごとに* Analytics から Audience Manager にデータを転送する。
1. SDK コードを配置する。これは、Analytics 拡張機能のボックスをオンにして Launch 経由の AAM へのデータ転送を有効化したときに、**既におこなっています**。

### Analytics Admin Console でのサーバーサイド転送の有効化

Adobe Analytics から Adobe Audience Manager へのデータの転送を開始するには、Adobe Analytics Admin Console の設定が必要です。転送のトラブルシューティングをする際は、システムがデータの転送を開始するまでに最大 4 時間かかる場合があることに注意してください。

#### Analytics Admin Consoleで SSF を有効にするには、以下を実行します。

1. Experience Cloud UI を使用して Analytics にログインします。Analytics への管理者アクセス権を持っていない場合は、Experience Cloud または Analytics の管理者に問い合わせて、アクセス権を割り当ててもらうまたはこれらの手順を実行してもらう必要があります。

   ![Adobe Analytics UI へのログイン](images/mobile-aam-logIntoAnalytics.png)

1. Analytics の上部のナビゲーションから、**[!UICONTROL 管理者／レポートスイート]**&#x200B;を選択し、リストから Audience Manager に転送するレポートスイートを選択（または複数選択）します。

   ![クリックで Admin Console に移動する](images/mobile-aam-analyticsAdminConsoleReportSuites.png)

1. レポートスイート画面で、レポートスイートを選択し、**[!UICONTROL 設定の編集／一般／サーバーサイド転送]**&#x200B;を選択します。

   ![SSF メニューの選択](images/mobile-aam-selectSSFmenu.png)

   >[!WARNING] 上記のように、このメニュー項目を表示するには管理者権限が必要です。

1. サーバーサイド転送ページで、情報を読み、レポートスイートの「**[!UICONTROL サーバーサイド転送の有効化]**」チェックボックスをオンにします。

1. **[!UICONTROL 保存]**&#x200B;をクリックします。

   ![SSF セットアップの完了](images/mobile-aam-enableSSFcomplete.png)

>[!NOTE] SSF はレポートスイートごとに有効にする必要があるので、実際のアプリケーションのレポートスイートに SSF をデプロイする場合は、必ず実際のレポートスイートに対してこの手順を繰り返してください。
>
>また、SSF オプションが灰色表示になっている場合は、このオプションを有効にするには、レポートスイートを Experience Cloud 組織にマッピングする必要があります。これについては、[ドキュメント](https://docs.adobe.com/content/help/ja-JP/core-services/interface/about-core-services/report-suite-mapping.html)で説明しています。

Adobe Experience Platform ID サービスが実装されている限り、このスイッチは AAM へのデータの実際の転送を開始します。残りの SSF 実装は、コード内で発生します。これは、AAM に転送するために Analytics 拡張機能内でボックスをオンにした場合に、Launch で処理されました。

アプリケーションのサーバーサイド転送のコードが実装されました。

### サーバーサイド転送の検証

サーバーサイド転送がインストールおよび導入されていることを検証する主な方法は、Adobe Analytics のヒットへのアプリケーションからの応答を確認することです。

Analytics から Audience Manager へのデータのサーバーサイド転送（SSF）をおこなっていない場合、 ビーコンに対する応答はありません（2x2 ピクセル以外）。ただし、SSF を有効にすると、サーバーサイド転送が正しく機能していることを確認するために、Analytics の要求と応答で確認できる項目があります。

Xcode コンソールにはビーコンに対する応答を表示されないので、Charles Proxy など、応答を表示する別のデバッガー／パケットスニファーを使用する必要があります（下のスクリーンショットで示す内容）。

1. デバッガーを開き、フィルターを `b/ss` に適用して、表示内容を Adobe Analytics 要求に制限します。
1. 前の演習で作成したサンプルアプリケーションをビルドして実行します。
1. 任意の Analytics 要求への応答を確認します。応答には `dcs_region` パラメーター、`uuid` パラメーター、および「stuff」オブジェクトが含まれる必要があります。このオブジェクトは、AAM セグメント ID がブラウザーに返される場所です（これはユーザーが属するセグメントについておこなわれ、これらのセグメントは AAM で cookie の宛先に割り当てられます）。「stuff」オブジェクトがある場合、SSF は機能しています。

   ![AA 応答 — stuff オブジェクト](images/mobile-aam-AAresponseCharles.png)

>[!WARNING] 偽の「Success」には要注意です。応答があり、すべてが機能しているように見える場合は、「stuff」オブジェクトがあることを&#x200B;**確認**&#x200B;してください。「stuff」オブジェクトがない場合、応答に「"status":"SUCCESS"」というメッセージが表示されることがあります。変に思えますが、実際にはこれは、正しく機能して&#x200B;**いない**&#x200B;証拠です。「"status":"SUCCESS"」が表示される場合、Launch で AAM に転送する手順は完了していますが、Analytics Admin Console での転送がまだ完了していません。この場合は、Analytics Admin Console で SSF が有効になっていることを確認する必要があります。まだ 4 時間が経過していない場合は、もうしばらくお待ちください。

![AA 応答 — 偽の成功](images/mobile-aam-unsuccessful-SSF.png)

[次：「プロパティのパブリッシュ」&gt;](publish.md)