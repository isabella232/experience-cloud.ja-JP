---
title: Launch を使用した Adobe Audience Manager の実装
description: サーバーサイド転送と Launch を使用して Adobe Audience Manager を Web サイトに実装する方法を説明します。このレッスンは、「Launch を使用した Web サイトでの Experience Cloud の実装」チュートリアルの一部です。
seo-description: null
seo-title: Launch を使用した Adobe Audience Manager の実装
solution: Experience Cloud
translation-type: ht
source-git-commit: e9dee6d0aa3b775d0ce617e2b2c57b56de491b8d

---


# Adobe Audience Manager の追加

このレッスンでは、サーバーサイド転送を使用して Adobe Audience Manager を有効にする手順を説明します。

[Adobe Audience Manager](https://docs.adobe.com/content/help/ja-JP/audience-manager/user-guide/aam-home.html)（AAM）は、オンラインでオーディエンスデータを管理する、業界をリードするサービスを提供しています。デジタル広告主やパブリッシャーは、データアセットを制御および活用して販売の成功を促進するために必要なツールを利用できます。

## 学習内容

このレッスンを最後まで学習すると、以下の内容を習得できます。

1. Web サイトで Audience Manager を実装する 2 つの主な方法について説明する。
1. Analytics ビーコンのサーバーサイド転送を使用して Audience Manager を追加する。
1. Audience Manager の実装を検証する。

## 前提条件

このレッスンを完了するには、次が必要です。

1. [Launch の設定](launch.md)、[Adobe Analytics の追加](analytics.md)、[ID サービスの追加](id-service.md)のレッスンを完了していること。

1. このチュートリアルで使用するレポートスイートのサーバーサイド転送を有効化するための、Adobe Analytics への管理者アクセス。または、以下の手順に従って、組織の既存の管理に依頼することもできます。

1. 「Audience Manager サブドメイン」（「パートナー名」、「パートナー ID」または「パートナーサブドメイン」とも呼ばれます）。実際の Web サイトで既に Audience Manager を実装している場合は、これを入手する最も簡単な方法は、実際の Web サイトにアクセスして、デバッガーを開くことです。サブドメインは、Audience Manager セクションの「概要」タブにあります。

   ![デバッガーを使用して、実際の Web サイトで Audience Manager サブドメインを検索する](images/aam-debugger-partner.png)

Audience Manager をまだ実装していない場合は、次の手順に従って [Audience Manager サブドメインを取得](https://docs.adobe.com/content/help/en/audience-manager-learn/tutorials/web-implementation/how-to-identify-your-partner-id-or-subdomain.html)してください。

## 実装オプション

Web サイトに Audience Manager を実装するには、次の 2 つの方法があります。

* **サーバーサイド転送（SSF）**— Adobe Analytics のユーザーにとって最も簡単な実装方法です（推奨）。Adobe Analytics はアドビのバックエンドでデータを AAM に転送し、ページ上での要求数を 1 つ減らすことができます。これにより、主要統合機能が有効になり、Audience Manager のコードの実装とデプロイメントのベストプラクティスにも準拠します。

* **クライアントサイド DIL** — このアプローチは、Adobe Analytics をお持ちでないお客様向けです。DIL コード（データ統合ライブラリコード、 AAM JavaScript 設定コード）は、Web ページから Audience Manager に直接データを転送します。

このチュートリアルでは Adobe Analytics を既にデプロイしているので、サーバーサイド転送を使用して Audience Manager をデプロイします。サーバーサイド転送の完全な説明と要件リストについては、[ドキュメント](https://docs.adobe.com/content/help/ja-JP/analytics/admin/admin-tools/server-side-forwarding/ssf.html)を読み、仕組みや要件、検証方法を確認してください。

## サーバーサイド転送を有効にする

SSF の実装には、主に次の 2 つの手順があります。

1. Analytics Admin Console で「切り替え」をオンにして、*レポートスイートごとに* Analytics から Audience Manager にデータを転送する。
1. コードを配置する（Launch を使用）。この機能を正しく動作させるには、Adobe Experience Platform Id サービス拡張機能と Analytics 拡張機能をインストールする必要があります（実際には AAM 拡張機能は必要&#x200B;*ありません*。これは、以下に説明されます）。

### Analytics Admin Console でのサーバーサイド転送の有効化

Adobe Analytics から Adobe Audience Manager へのデータの転送を開始するには、Adobe Analytics Admin Console の設定が必要です。データの転送の開始には最大 4 時間かかることがあるので、まずこの手順を実行してください。

#### Analytics Admin Consoleで SSF を有効にするには、以下を実行します。

1. Experience Cloud UI を使用して Analytics にログインします。Analytics への管理者アクセス権を持っていない場合は、Experience Cloud または Analytics の管理者に問い合わせて、アクセス権を割り当ててもらうまたはこれらの手順を実行してもらう必要があります。

   ![Adobe Analytics にログインする](images/aam-logIntoAnalytics.png)

1. Analytics の上部のナビゲーションから、**[!UICONTROL 管理者／レポートスイート]**&#x200B;を選択し、リストから Audience Manager に転送するレポートスイートを選択（または複数選択）します。

   ![クリックで Admin Console に移動する](images/aam-analyticsAdminConsoleReportSuites.png)

1. レポートスイート画面で、レポートスイートを選択し、**[!UICONTROL 設定の編集／一般／サーバーサイド転送]**&#x200B;を選択します。

   ![SSF メニューの選択](images/aam-selectSSFmenu.png)

   >[!WARNING] 上記のように、このメニュー項目を表示するには管理者権限が必要です。

1. サーバーサイド転送ページで、情報を読み、レポートスイートの「**[!UICONTROL サーバーサイド転送の有効化]**」チェックボックスをオンにします。

1. **[!UICONTROL 保存]**&#x200B;をクリックします。

   ![SSF セットアップの完了](images/aam-enableSSFcomplete.png)

>[!NOTE] SSF はレポートスイートごとに有効にする必要があるので、実際のアプリのレポートスイートに SSF をデプロイする場合は、必ず実際のレポートスイートに対してこの手順を繰り返してください。
>
>また、SSF オプションが灰色表示になっている場合は、このオプションを有効にするには、レポートスイートを Experience Cloud 組織にマッピングする必要があります。これについては、[ドキュメント](https://docs.adobe.com/content/help/ja-JP/core-services/interface/about-core-services/report-suite-mapping.html)で説明しています。

この手順が完了し、Adobe Experience Platform ID サービスを有効にしている場合、データは Analytics から AAM に転送されます。ただし、プロセスを完了して、応答が AAM からページに（および Audience Analytics 機能経由で Analytics に）正しく返されるようにするには、Launch で次の手順も完了する必要があります。これはごく簡単なので、心配無用です。

### Launch でのサーバーサイド転送の有効化

これは、SSF を有効にする2つの手順の 2 つ目です。Analytics Admin Console でスイッチを既に入れ替えているので、コードを追加するだけで済みます。右のボックスを選択するだけで Launch がこれを実行します。

>[!NOTE] Analytics データのサーバーサイド転送を AAM に実装する場合、Analytics 拡張機能を実際に AAM 拡張機能&#x200B;**ではなく**、Launch で編集／設定します。AAM 拡張機能は、Adobe Analytics を持たないクライアントサイド DIL 実装にのみ使用されます。したがって、以下の手順は、これを Analytics 拡張機能で設定する際にのみ正しくなります。

#### Launch で SSF を有効にするには、以下を実行します。

1. **[!UICONTROL 拡張機能／インストール済み]**&#x200B;に移動し、クリックして Analytics 拡張機能を設定します。

   ![ Analytics 拡張機能の設定](images/aam-configAnalyticsExtension.png)

1.  「`Adobe Audience Manager`」セクションを展開します。

1. 「**[!UICONTROL Analytics データを Audience Manager と自動的に共有]**」チェックボックスをオンにします。これにより、Audience Manager の「モジュール」（コード）が Analytics `AppMeasurement.js` の実装に追加されます。

1. 「Audience Manager サブドメイン」（「パートナー名」、「パートナー ID」または「パートナーサブドメイン」とも呼ばれます）を追加します。[Audience Manager サブドメインを取得](https://docs.adobe.com/content/help/en/audience-manager-learn/tutorials/web-implementation/how-to-identify-your-partner-id-or-subdomain.html)するには、次の手順に従います。

1. 「**[!UICONTROL ライブラリに保存してビルド]**」をクリックします。

   ![SSF の設定](images/aam-configLaunchSSF.png)

サーバーサイド転送コードが実装されました。

### サーバーサイド転送の検証

サーバーサイド転送がインストールおよび導入されていることを検証する主な方法は、Adobe Analytics のヒットへの応答を確認することです。これはもうすぐ説明します。その一方で、他にもいくつか、望みどおりに機能しているか確かめる手助けとなるものを見てみましょう。

#### コードが正しく読み込まれていることの確認

転送、特に AAM からページへの応答を処理するために Adobe Launch がインストールするコードは、Audience Manager「モジュール」と呼ばれます。Experience Cloud デバッガーを使用して、確実に読み込むことができます。

1. Luma サイトを開きます。
1. ブラウザーでデバッガーアイコンをクリックして、Experience Cloud デバッガーを開きます。
1. 「概要」タブを表示したまま、「Analytics」セクションまで下にスクロールします。
1. 「モジュール」セクションの下に **AudienceManagement** がリストされていることを確認します。

   ![デバッガーでの AAM モジュールの検証](images/aam-verifyAAMmodule.png)

#### デバッガーでのパートナー ID の確認

次に、デバッガーがコードから正しい「パートナー ID」（別名「パートナーサブドメイン」など）を取得していることを確認することもできます。

1. デバッガー内で、「概要」タブを表示したまま、「Audience Manager」セクションまで下にスクロールします。
1. 「パートナー」の下の「パートナー ID／サブドメイン」を確認します。

   ![デバッガーでのパートナー ID の確認](images/aam-verifyPartnerID.png)

>[!WARNING] デバッガーの「Audience Manager」セクションでは「DIL」（データ統合ライブラリ）を参照し、通常はここで実装したサーバーサイドのアプローチとは異なり、クライアントサイドの実装を参照していることに注意してください。実は、AAMの「Module」（この SSF アプローチで使用）は、クライアントサイドの DIL ライブラリと同じコードを多く使用しているので、このデバッガーは現在、このようにレポートしているのです。このチュートリアルの手順に従っており、この検証セクションの残りの項目が正しい場合は、サーバーサイド転送は機能しているので、ご安心ください。

#### Analytics 要求と応答の確認

これは重要です。Analytics から Audience Manager へのデータのサーバーサイド転送をおこなっていない場合、 ビーコンに対する応答はありません（2x2 ピクセル以外）。ただし、SSF を実行している場合は、要求と応答で確認できる項目があり、これが正しく機能していることを確認できます。現時点では、Experience Cloud デバッガーはビーコンへの応答の表示に対応していません。そのため、Charles Proxy やブラウザーの開発者ツールなど、別のデバッガー／パケットスニッファーを使用する必要があります。

1. ブラウザーで開発者ツールを開き、「ネットワーク」タブに移動します。
1. フィルターフィールドに `b/ss` と入力して、表示する内容を Adobe Analytics 要求に制限します。
1. ページの表示を更新し、Analytics 要求を確認します。

   ![開発者ツールを開く](images/aam-openTheJSConsole.png)

1. Analytics ビーコン（リクエスト）で、「callback」パラメーターを探します。これは、「`s_c_il[1].doPostbacks`」のように設定されます。

   ![AA リクエスト — コールバックパラメーター](images/aam-callbackParam.png)

1. Analytics ビーコンへの応答があります。応答には、要求で呼び出される doPostbacks への参照が含まれ、最も重要なのは「stuff」オブジェクトが含まれていることです。AAM セグメント ID は、ここでブラウザーに送信されます。「stuff」オブジェクトがある場合、SSF は機能しています。

   ![AA 応答 — stuff オブジェクト](images/aam-stuffObjectInResponse.png)

>[!WARNING] 偽の「Success」には要注意です。応答があり、すべてが機能しているように見える場合は、「stuff」オブジェクトがあることを&#x200B;**確認**&#x200B;してください。「stuff」オブジェクトがない場合、応答に「"status":"SUCCESS"」というメッセージが表示されることがあります。変に思えますが、実際にはこれは、正しく機能して&#x200B;**いない**&#x200B;証拠です。これが表示された場合、この 2 番目の手順（Launch のコード）は完了していますが、 Analytics Admin Console の転送（この節の最初の手順）はまだ完了していません。この場合は、Analytics Admin Console で SSF が有効になっていることを確認する必要があります。まだ 4 時間が経過していない場合は、もうしばらくお待ちください。

![AA 応答 — 偽の成功](images/aam-responseFalseSuccess.png)

[次：「Experience Cloud 統合」&gt;](integrations.md)