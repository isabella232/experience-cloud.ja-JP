---
title: Targetの起動での実装
description: Launch with at.js、ページ読み込みリクエスト、パラメーター、注文リクエストおよびカスタムのヘッダー/フッターコードを使用してAdobe targetを実装する方法について説明します。 このレッスンは、「WebサイトにExperience cloudを導入する（起動）」チュートリアルの一部です。
seo-title: Targetの起動での実装
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# Adobe Target の追加

このレッスンでは、ページ読み込みリクエストと [カスタムパラメーターを使用して](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/target-extension/overview.html) 、Adobe Target拡張機能を実装します。

[Adobe targetは](https://docs.adobe.com/content/help/en/target/using/target-home.html) 、顧客の体験をカスタマイズしてパーソナライズするために必要なすべてを提供するAdobe Marketing cloudソリューションです。Webやモバイルサイト、アプリ、ソーシャルメディア、その他のデジタルチャネルの売上を最大化できます。

## 学習目標

このレッスンを最後まで学習すると、以下の内容を習得できます。

* 非同期起動埋め込みコードを使用するTargetの使用時にちらつきを管理するために使用する、非表示前のスニペットの追加
* Target v2拡張機能の追加
* ページ読み込みリクエスト（旧称「グローバルmbox」）を実行
* ページ読み込み要求にパラメーターを追加します
* プロファイルとエンティティのパラメーターをページ型リクエストに追加する方法の説明
* 必要なパラメーターを指定して注文確認要求を実行します
* ライブラリヘッダーやライブラリフッターコードなどの高度な設定を追加する方法の説明
* Target実装の検証

## 前提条件

To complete the lessons in this section, you must first complete the lessons in [Configure Launch](launch.md) and [Add the Identity Service](id-service.md).

## ターゲットの事前非表示スニペットの追加

開始する前に、起動の埋め込みコードを少し更新する必要があります。 Launch埋め込みコードが非同期で読み込まれると、Targetライブラリが完全に読み込まれ、コンテンツの入れ替えが実行される前に、ページのレンダリングが完了する場合があります。 これにより、「ちらつき」と呼ばれるものが発生し、デフォルトコンテンツが短く表示されてから、Targetで指定されたパーソナライズされたコンテンツに置き換えられます。 このちらつきを回避する場合は、「起動」の非同期埋め込みコードの直前に、特別な事前非表示スニペットをハードコーディングすることを強くお勧めします。

これは既にLumaサイトで行われていますが、実装を理解するために、サンプルページで行います。 次のコード行をコピーします。

```html
<script>
    //prehiding snippet for Adobe Target with asynchronous Launch deployment
    (function(g,b,d,f){(function(a,c,d){if(a){var e=b.createElement("style");e.id=c;e.innerHTML=d;a.appendChild(e)}})(b.getElementsByTagName("head")[0],"at-body-style",d);setTimeout(function(){var a=b.getElementsByTagName("head")[0];if(a){var c=b.getElementById("at-body-style");c&&a.removeChild(c)}},f)})(window,document,"body {opacity: 0 !important}",3E3);
</script>
```

サンプルページを開き、以下の図のように、Launch埋め込みコードの直前に貼り付けます（行番号が異なる場合は気にしないでください）。拡張機![能の上にカーソルを置く](images/target-prehidingSnippet.png)

サンプルページを再度読み込みます。ページが表示されるまでの3秒間、ページが非表示になっています。 この動作は一時的なもので、Targetをデプロイすると終了します。 この事前非表示の動作は、スニペットの最後にある2つの設定で制御されます。この設定はカスタマイズできますが、通常はデフォルト設定のままにしておくことをお勧めします。

* `body {opacity: 0 !important}` targetが読み込まれるまでの前の非表示に使用するCSS定義を指定します。 デフォルトでは、ボディ全体が非表示になります。 例えば、ナビゲーションの下にあるすべてのコンテンツを折り返す、簡単に識別できるコンテナ要素を含むDOM構造が整っていて、ナビゲーションをテストしたりパーソナライズしたいと思わない場合は、この設定を使用して前の非表示をコンテナ要素に制限できます。
* `3E3` これは、前の非表示のタイムアウト設定を指定します。 デフォルトでは、Targetが3秒後に読み込まれない場合、ページが表示されます。 これは極めてまれです。

For more details and to obtain the un-minified pre-hiding snippet, please see [the Adobe Target extension with an asynchronous deployment&#x200B;](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/target-extension/overview.html#adobe-target-extension-with-an-asynchronous-deployment).

## Target拡張機能の追加

Adobe target拡張機能は、最新のWeb、at.js用のTargetのJavaScript SDKを使用したクライアント側実装をサポートします。 Customers still using Target's older library, mbox.js, [should upgrade to at.js 2.x](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/mbox-implement/migrate-mbox/target-atjs-implementation.html) in order to use Launch.

Target v2拡張は、次の2つの主要な部分で構成されています。

1. 拡張の設定。コアライブラリ設定を管理します
1. 次の操作を実行するルールアクション：
   1. Targetの読み込み(at.js 2.x)
   1. ページ読み込みリクエストにパラメータを追加
   1. Add Params to All Requests
   1. Fire Page Load Request

この最初の練習では、拡張機能を追加し、設定を確認します。 後の練習では、このアクションを使用します。

**拡張機能を追加するには**

1. エクステンション/ **[!UICONTROL カタログに移動]**
1. Adobe Targetの拡 `target` 張機能をすばやく見つけるには、フィルターを入力します。 Adobe targetとAdobe Target v2の2つの拡張子があります。 このチュートリアルでは、従来のWebサイトとシングルページアプリケーション(SPA)の両方に最適な最新バージョンのat.js（現在は2.x）を使用する拡張機能のv2バージョンを使用します。
1. Click **[!UICONTROL Install]**

   ![Target v2拡張機能のインストール](images/target-installExtension.png)

1. 拡張機能を追加すると、多くのat.js設定が読み込まれますが、以下の図に示すように、すべてのat.js設定がTargetインターフェイスから読み込まれるわけではありません。 読み込まれない設定の1つはタイムアウトで、拡張機能の追加後は常に3000 msになります。 このチュートリアルでは、デフォルト設定をそのままにしておきます。左側には、現在のバージョンの拡張に付属しているat.jsバージョンが表示されます。

1. 「ライブラ **[!UICONTROL リとビルドに保存」をクリックします]**

   ![拡張機能の保存](images/target-saveExtension.png)

現時点では、Targetは実際に何も実行していないので、検証する必要はありません。

>[!NOTE] Target拡張の各バージョンには、at.jsの特定のバージョンが付属しています。このバージョンは、拡張の説明に記載されています。 at.jsのバージョンを更新するには、Target拡張を更新します。

## ターゲットの読み込みとページ読み込みリクエストの実行

マーケティング担当者は、Targetを使用して、コンテンツのテストとターゲット化を行う際のページ上の訪問者エクスペリエンスを制御します。 ページの表示におけるこの重要な役割のため、ページの表示に与える影響を最小限に抑えるために、できる限り早くTargetを読み込む必要があります。 この節では、Target javaScriptライブラリat.jsを読み込み、ページ読み込みリクエスト（以前のバージョンのat.jsでは「グローバルmbox」と呼ばれます）を実行します。

「データ要素、ル `All Pages - Library Loaded` ールおよびライブラリの追加[](launch-data-elements-rules.md)」のレッスンで作成したルールは、ページの読み込み時に可能な限り早くトリガーされているので、Targetの実装に使用できます。

**ターゲットを読み込むには**

1. Go to the **[!UICONTROL Rules]** in the top navigation and then click on `All Pages - Library Loaded` to open the rule editor

   ![すべてのページを開く — ライブラリ読み込み済みルール](images/target-editRule.png)

1. Under Actions, click the ![Click the Plus icon](images/icon-plus.png) to add a new action

   ![プラスアイコンをクリックして、新しいアクションを追加します](images/target-addLoadTargetAction.png)

1. 拡張 **[!UICONTROL 機能/Adobe Target v2を選択します。]**

1. アクション **[!UICONTROL タイプ/ターゲットを読み込みを選択します]**

1. Click **[!UICONTROL Keep Changes]**

   ![「変更を保持」をクリックします](images/target-addLoadTargetAction-keepChanges.png)

With the `Load Target` action added, at.js will load on the page. ただし、アクションを追加するまで、Targetリクエストは実行され `Fire Page Load Request` ません。

**ページ読み込みリクエストを実行するには**

1. 「アクション」で、プラスアイコ ![ンをもう一度クリックし](images/icon-plus.png) 、別のアクションを追加します

   ![別のアクションを追加するには、プラスアイコンをクリックします](images/target-addGlobalMboxAction.png)

1. 拡張 **[!UICONTROL 機能/Adobe Target v2を選択します。]**

1. アクショ **[!UICONTROL ンタイプ/ページ読み込みリクエストを実行を選択します]**

1. ページの非表示と、事前非表示に使用するCSSセレクターに関連して、ページ読み込みリクエストに使用できる設定がいくつかあります。 これらの設定は、ページ上にハードコードされた事前非表示スニペットと連動します。 デフォルト設定はそのままにしておきます。

1. Click **[!UICONTROL Keep Changes]**

   ![ページ読み込み要求アクションの実行](images/target-fireGlobalMbox.png)

1. アクションの後に新しいアクションが順に追 `Load Target` 加され、アクションがこの順序で実行されます。 アクションをドラッグ&amp;ドロップして順序を並べ替えることができますが、このシナリオでは `Load Target` 、前に配置する必要がありま `Fire Page Load Request`す。

1. 「ライブラ **[!UICONTROL リとビルドに保存」をクリックします]**

   ![保存してビルド](images/target-fireGlobalMbox-saveAndBuild.png)

### ページ型リクエストの検証

これで、Target v2拡張機能を追加し、とアクションを実行したので、Launchプロパティが使用されるすべてのページに対してペ `Load Target``Fire Page Load Request` ージ読み込みリクエストが行われます。

**「ターゲットを読み込み」および「ページ読み込みリクエストを実行」アクションを検証するには**

1. サンプルページを再度読み込みます。ページが表示されるまでにかかっていた 3 秒の遅延がなくなりました。If you are loading the sample page using the `file://` protocol, you should do this step in Firefox or Safari browsers since Chrome will not fire a Target request when using the `file://` protocol.

1. Open the [Luma site](https://luma.enablementadobe.com/content/luma/us/en.html)

1. Make sure the Debugger is mapping the Launch property to *your* Development environment, as described in the [earlier lesson](launch-switch-environments.md)

   ![デバッガーに表示される起動開発環境](images/switchEnvironments-debuggerOnWeRetail.png)

1. デバッガーの「概要」タブに移動します。

1. In the `Launch` section, confirm that `Target` appears under the `Extensions` heading

1. セクション `Target` で、at.jsライブラリのバージョンが表示されることを確認します

   ![デバッガーの「概要」タブにTargetが表示されることを確認します。](images/target-summaryTab.png)

1. 最後に、タブに移動し、ク `Target` ライアントコードを展開して、ページ読み込みリクエストが表示されることを確認します。

   ![ページ読み込み要求が行われたことを確認します](images/target-debugger-globalMbox.png)

おめでとう！ Targetを実装しました。

## パラメータの追加

Targetリクエストにパラメーターを渡すと、ターゲット設定、テストおよびパーソナライゼーションアクティビティに強力な機能が追加されます。 Launch拡張では、パラメーターを渡すための2つのアクションが提供されます。

1. `Add Params to Page Load Request`を呼び出し、ページ読み込みリクエストにパラメーターを追加します( [targetPageParams()メソッドに相当)](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/functions-overview/cmp-atjs-functions.html) 。

1. `Add Params to All Requests`を呼び出すと、ページ読み込み要求に加えてカスタムコードアクションから行われた追加の要求や、サイトにハードコードされた( [targetPageParamsAll()メソッドと同じ](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/functions-overview/cmp-atjs-functions.html) )など、すべてのTarget要求にパラメーターが追加されます。

These actions can be used *before* the `Load Target` action and can set different parameters on different pages based on your rule configurations. 顧客IDをIDサービスと共に設定する場合に使用したルールの順序付け機能を使用して、ページ読み込み要求を実行するルールの前に、イベン `Library Loaded` トに追加のパラメーターを設定します。
>[!TIP] ほとんどの実装ではアクティビティ配信にページ型リクエストが使用されるので、通常はアクションを使用するだけで十分 `Add Params to Page Load Requests` です。

### リクエスト(mbox)パラメーター

パラメーターは、カスタムデータをTargetに渡し、パーソナライゼーション機能を強化するために使用されます。 ページ名やテンプレートなど、閲覧セッション中に頻繁に変更される属性に最適です。 持続しないでください。

「データ要素、ルール `Page Name` 、ライブラリの追加」で先ほど作成し [たデータ要素をリクエストパラメーターとして追加します](launch-data-elements-rules.md) 。

**リクエストパラメーターを追加するには**

1. 上部ナビゲーションの&#x200B;**[!UICONTROL ルール]**&#x200B;に移動してから、「`All Pages - Library Loaded`」をクリックしてルールエディターを開きます。

   ![すべてのページを開く — ライブラリ読み込み済みルール](images/target-editRule.png)

1. Under Actions, click the ![Click the Plus icon](images/icon-plus.png) to add a new action

   ![プラスアイコンをクリックして、新しいアクションを追加します](images/target-addParamsAction.png)

1. 拡張 **[!UICONTROL 機能/Adobe Target v2を選択します。]**

1. アクショ **[!UICONTROL ンタイプ/ページ読み込みリクエストにパラメーターを追加を選択します]**

1. 名前と `pageName` して入力 **[!UICONTROL する]**

1. Click the ![data element icon](images/icon-dataElement.png) to open the data element modal

1. データ要素をク `Page Name` リックします

1. 「 **[!UICONTROL Select]** 」ボタン

   ![「選択」ボタンをクリックします](images/target-mboxParam-pageName.png)

1. Click **[!UICONTROL Keep Changes]**

   ![「変更を保持」をクリックします](images/target-addPageName-keepChanges.png)

1. アクションの左端をクリック&amp;ドラッグして、ア `Add Params to Page Load Request` クションの前のアクションを並べ替えます(ア `Fire Page Load Request` クションの前と後のアクションも可能 `Load Target`)。

1. 「ライブラ **[!UICONTROL リとビルドに保存」をクリックします]**

   ![「ライブラリとビルドに保存」をクリックします](images/target-rearrangeActions.png)

#### リクエストパラメーターの検証

at.js 2.xリクエストで渡されたカスタムパラメーターは、デバッガーに容易に表示されないので、ブラウザーの開発者ツールを利用します。

**pageNameリクエストパラメーターを検証するには**

1. Lumaサイトを再読み込みし、独自のLaunchプロパティにマッピングされていることを確認します
1. ブラウザーの開発者ツールを開く
1. [ネットワーク]タブをクリックします。
1. リクエストのフィルタ `tt.omtrdc` ー（Targetリクエストの場合はCNAMEドメイン）
1. &gt; &gt;セクショ `Headers` ンを展開 `Request Payload` し `execute.pageLoad.parameters` て、パラメータと値を `pageName` 検証します。
   ![デバッガーのpageNameパラメーター](images/target-debugger-pageName-browser.png)

<!--Now go to the **[!UICONTROL Target]** tab in the Debugger. Expand your client code and look at the requests. You should see the new `pageName` parameter passed in the request:

![pageName parameter in the debugger](images/target-debugger-pageName.png)-->

### プロファイルパラメーター

リクエストパラメーターと同様に、プロファイルパラメーターもTargetリクエストを通じて渡されます。 However, profile parameters get stored in Target's visitor profile database and will persist for the [duration of the visitor's profile](https://docs.adobe.com/content/help/en/target/using/audiences/visitor-profiles/visitor-profile-lifetime.html). サイトの1ページに設定し、別のページのTargetアクティビティで使用できます。 以下に、自動車の Web サイトの例を示します。訪問者が車両のページに移動したときに、「profile.lastViewed=sportscar」というプロファイルパラメーターを渡すと、その車両に対する関心度を記録できます。 訪問者が他の車以外のページを閲覧した場合、最後に閲覧した車両に基づいてコンテンツをターゲット設定できます。  プロファイルパラメーターは、ほとんど変更されない、または特定のページでのみ使用できる属性に最適です

You won't pass any profile parameters in this tutorial, but the workflow is almost identical to what you just did when passing the `pageName` parameter. 違いの 1 つは、プロファイルパラメーターに `profile.` プレフィックスを付ける必要があるという点です。This is what a profile parameter called "userType" would look like in the `Add Params to Page Load Request` action:

![プロファイルパラメーターの設定](images/target-profileParameter.png)

### エンティティパラメーター

エンティティパラメーターは、主に次の 3 つの理由により、[Recommendation の実装](https://docs.adobe.com/content/help/en/target/using/recommendations/plan-implement.html)で使用される特別なパラメーターです。

1. 製品のレコメンデーションをトリガーするためのキー。例えば、「製品 Xを閲覧し、Y も閲覧した人」などのレコメンデーションアルゴリズムを使用する場合、「X」はレコメンデーションの「キー」になります。訪問者が現在閲覧している製品の sku（`entity.id`）またはカテゴリー（`entity.categoryId`）です。
1. 「最近表示した製品」や「最も頻繁に表示した製品」など、レコメンデーションアルゴリズムに対する訪問者の行動を収集するには
1. レコメンデーションカタログを生成する場合。Recommendationsには、Webサイト上のすべての製品または記事のデータベースが含まれているので、レコメンデーションオファーで提供できます。 例えば、商品をレコメンデーションする場合は一般的に、商品名（`entity.name`）や画像（`entity.thumbnailUrl`）などの属性を表示する必要があります。一部のお客様は、バックエンドフィードを使用してカタログにデータを入力しますが、Targetリクエストのエンティティパラメーターを使用してデータを入力することもできます。

このチュートリアルでは、プロファイルパラメーターを渡す必要はありませんが、リクエストパラメーターを渡す際に行ったのと同じワークフローです。パラメーターに接頭辞「entity」を付けた名前を付けます。 `pageName` 関連するデータ要素にマップします。 一部の一般的なエンティティには、使用する必要がある予約名が含まれています（例：製品SKUのentity.id）。 This is what it would look like to set entity parameters in the `Add Params to Page Load Request` action:

![エンティティパラメータの追加](images/target-entityParameters.png)

### 顧客 ID パラメーターの追加

Adobe Experience Platform IDサービスを使用して顧客IDを収集すると、Adobe Experience cloudの顧客属性機能を使用してCRMデータをTargetに [簡単にインポートできます](https://docs.adobe.com/content/help/en/target/using/audiences/visitor-profiles/working-with-customer-attributes.html) 。 It also enables [cross-device visitor stitching](https://docs.adobe.com/content/help/en/target/using/integrate/experience-cloud-device-co-op.html), allowing you to maintain a consistent user experience as your customers switch between their laptops and their mobile devices.

ページ読み込み要求を実行する前に、IDサービスのアクションに顧客IDを設 `Set Customer IDs` 定する必要があります。 そのためには、サイトに次の機能があることを確認します。

* 「埋め込みコードを起動」の前に、ページで顧客IDを使用できる必要があります
* Adobe Experience Platform Identity Service Extensionがインストールされている必要があります
* You must use the `Set Customer IDs` action in a rule that fires at the "Library Loaded (Page Top)" event
* Use the `Fire Page Load Request` action in a rule that fires *after* the "Set Customer IDs" action

前のレッスン「Adobe Experience Platform IDサ [ービスの追加](id-service.md)」では、「顧客IDの設定」アク `All Pages - Library Loaded - Authenticated - 10` ションを実行するルールを作成しました。 このルールの設定はとなっ `Order` てい `10`るので、顧客IDは、の設定を持つルールからページ読み込み要求が発生する前 `All Pages - Library Loaded` に設定 `Order` されま `50`す。 したがって、Targetの顧客IDの収集を既に実装済みです。

#### 顧客IDの検証

at.js 2.xリクエストで渡されたカスタムパラメーターは、デバッガーに容易に表示されないので、ブラウザーの開発者ツールを利用します。

**顧客IDを検証するには**

1. Open the [Luma site](https://luma.enablementadobe.com/content/luma/us/en.html)

1. Make sure the Debugger is mapping the Launch property to *your* Development environment, as described in the [earlier lesson](launch-switch-environments.md)

   ![デバッガーに表示される起動開発環境](images/switchEnvironments-debuggerOnWeRetail.png)

1. 資格情報を使用してLumaサイトにログイ `test@adobe.com`ン/`test`
1. Return to the [Luma homepage](https://luma.enablementadobe.com/content/luma/us/en.html)

1. ブラウザーの開発者ツールを開く
1. [ネットワーク]タブをクリックします。
1. リクエストのフィルタ `tt.omtrdc` ー（Targetリクエストの場合はCNAMEドメイン）
1. 「&gt;」 `Headers` &gt;「 `Request Payload` 」セク `id.customerIds.0` ションを展開し、顧客IDの設定と値を検証します。
   ![デバッガーの顧客ID設定](images/target-debugger-customerId-browser.png)
<!--
1. Open the Debugger
1. Go to the Target tab
1. Expand your client code
1. You should see parameters in the latest Target request for `vst.crm_id.id` and `vst.crm_id.authState`. `vst.crm_id.id` should have a value of the hashed email address and `vst.crm_id.authState` should have a value of `1` to represent `authenticated`. Note that `crm_id` is the `Integration Code` you specified in the Identity Service configuration and must align with the key you use in your [Customer Attributes data file](https://docs.adobe.com/content/help/en/core-services/interface/customer-attributes/t-crs-usecase.html):

![The Customer Id details should be visible as custom parameters in the Target request](images/target-debugger-customerId.png)-->

>[!WARNING] Adobe Experience Platform IDサービスでは、複数のIDをサービスに送信できますが、最初のIDのみがTargetに送信されます。

### プロパティトークンパラメーターの追加

>[!NOTE] これは、Target Premiumのお客様向けのオプションの演習です。

The property token is a reserved parameter used with the Target Premium [Enterprise User Permissions](https://docs.adobe.com/content/help/en/target/using/administer/manage-users/enterprise/property-channel.html) feature. Experience cloud組織の各メンバーに各プロパティに異なる権限を割り当てるために、異なるデジタルプロパティを定義するために使用されます。 例えば、1つのユーザーグループがWebサイト上でTargetアクティビティを設定でき、モバイルアプリケーション内では設定できないようにするとします。

Targetプロパティは、起動プロパティおよびAnalyticsレポートスイートに似ています。 複数のブランド、Webサイト、マーケティングチームを持つ企業では、Webサイトやモバイルアプリごとに異なるTargetプロパティ、Launchプロパティ、Analyticsレポートスイートを使用する場合があります。 起動プロパティは埋め込みコードによって区別され、AnalyticsレポートスイートはレポートスイートIDによって区別され、Targetプロパティはプロパティトークンパラメーターによって区別されます。

プロパティトークンは、リクエストパラメーターと同様に実装されます。 パラメーターに「at_property」という名前を付け、Targetインターフェイスで指定された値に貼り付けます。  1つの起動プロパティを持つ複数のサイトを実装する場合は、データ要素を介してat_property値を管理できます。

Target Premiumのお客様の場合、Tutorialプロパティにプロパティトークンを実装する場合は、次のオプションの演習を行います。

1. 別のタブで、Targetユーザーインターフェイスを開きます

1. 設定/プロ **[!UICONTROL パティに移動]**

1. 使用するプロパティを指定し、「 **[!UICONTROL &lt;/&gt;」をクリッ]** クします（または新しいプロパティを作成します）。

1. Copy the `at_property` value to your clipboard

   ![Adobe targetインターフェイスからのプロパティトークンの取得](images/target-addATProperty-targetProperties.png)

1. In your Launch tab, go to the **[!UICONTROL Rules]** in the top navigation and then click on `All Pages - Library Loaded` to open the rule editor.

   ![すべてのページを開く — ライブラリ読み込み済みルール](images/target-editRule.png)

1. 「アクション」で、アクション `Adobe Target - Add Params to Page Load Request` をクリックして、 `Action Configuration`

   ![「ページ読み込みリクエストにパラメータを追加」アクションを開く](images/target-openParamsAction.png)

1. パラメータの `pageName` 下で、「追加」ボタンをクリ **[!UICONTROL ック]** します

   ![「ページ読み込みリクエストにパラメータを追加」アクションを開く](images/target-addATProperty.png)

1. Name the parameter `at_property` and paste in the value you copied from the Target interface

1. Click **[!UICONTROL Keep Changes]**

   ![「変更を保持」をクリックします](images/target-addATProperty-keepChanges.png)

1. 「ライブラ **[!UICONTROL リとビルドに保存」をクリックします]**
   ![「保存してライブラリにビルド」をクリックします](images/target-addATProperty-save.png)

#### プロパティトークンの検証

at.js 2.xリクエストで渡されたカスタムパラメーターは、デバッガーに容易に表示されないので、ブラウザーの開発者ツールを利用します。

**Property tokenパラメーターを検証するには**

1. Open the [Luma site](https://luma.enablementadobe.com/content/luma/us/en.html)
1. Make sure the Debugger is mapping the Launch property to *your* Development environment, as described in the [earlier lesson](launch-switch-environments.md)

   ![デバッガーに表示される起動開発環境](images/switchEnvironments-debuggerOnWeRetail.png)

1. ブラウザーの開発者ツールを開く
1. [ネットワーク]タブをクリックします。
1. リクエストのフィルタ `tt.omtrdc` ー（Targetリクエストの場合はCNAMEドメイン）
1. &gt; &gt;セクショ `Headers` ンを展 `Request Payload` 開し `property.token` て、値を検証します。
   ![プロパティトークンは、すべてのリクエストでat_propertyパラメーターとして表示される必要があります](images/target-debugger-atProperty-browser.png)

<!--
1. Go to the `Target` tab
1. Expand your client code
1. You should see the parameter for "at_property" in every page load request request as you browse the site:

![The Property Token should be visible as the at_property parameter in every request](images/target-debugger-atProperty.png)-->

## カスタムリクエストの追加

### 注文確認リクエストの追加

注文確認リクエストは、注文の詳細をTargetに送信するために使用される特別なタイプのリクエストです。 3つの特定のリクエストパラメーターorderId、orderTotalおよびproductPurchasedIdを含めると、通常のTargetリクエストが1つの注文リクエストに変わります。 売上高のレポートに加えて、注文リクエストでは次の処理も行います。

1. 不測の注文の再送信の重複を排除
1. 極端な注文をフィルタリングする（合計が平均の標準偏差から 3 以上離れている注文）
1. バックグラウンドで異なるアルゴリズムを使用して、統計的信頼性を計算する
1. 個々の注文詳細に対し、ダウンロード可能な特別監査レポートを作成する

ベストプラクティスは、小売以外のサイトでも、すべての注文ファネルで確認リクエストを使用し、注文を行うことです。 例えば、リードジェネレーションサイトには通常、生成された一意の「リード ID」を持つリードファネルが最後にあります。これらのサイトは、静的な値(例："1")をorderTotalに対して使用します。

Analytics for Target(A4T)の統合をレポートのほとんどに使用しているお客様は、注文リクエストも実装する必要があります。これは、A4Tが、自動配分、自動パーソナライゼーション、自動ターゲットなどのアクティビティタイプとまだ互換性がないためです。 さらに、注文リクエストは、Recommendationsの実装で重要な要素であり、購入行動に基づくアルゴリズムの活用に役立ちます。

注文確認要求は、注文確認ページまたはイベントでのみトリガーされるルールから実行されます。 多くの場合、Adobe Analytics購入イベントを設定するルールと組み合わせることができます。 これは、適切なデータ要素を使用して、orderId、orderTotalおよびproductPurchasedIdパラメーターを設定し、Core拡張のカスタムコードアクションを使用して設定する必要があります。

ルーマサイトで注文確認リクエストを実行する必要があるデータ要素とルールを追加します。 既に複数のデータ要素を作成しているので、これらの手順は省略されます。

**注文IDのデータ要素を作成するには**

1. 上部ナビゲ **[!UICONTROL ーションで]** 「データ要素」をクリックします。
1. 「データ要 **[!UICONTROL 素を追加」をクリックします]**
1. Name the data element `Order Id`
1. データ **[!UICONTROL 要素の種類/JavaScript変数を選択します。]**
1. を `digitalData.cart.orderId``JavaScript variable name`
1. Check the `Clean text` option
1. 「ラ **[!UICONTROL イブラリに保存]**」をクリックします（注文確認要求のすべての変更を行うまで、ライブラリは構築されません）

**買い物かごの金額のデータ要素を作成するには**

1. 「データ要 **[!UICONTROL 素を追加」をクリックします]**
1. Name the data element `Cart Amount`
1. データ **[!UICONTROL 要素の種類/JavaScript変数を選択します。]**
1. を `digitalData.cart.cartAmount``JavaScript variable name`
1. Check the `Clean text` option
1. 「ライブラ **[!UICONTROL リに保存」をクリックします]**

**買い物かごSKU(Target)のデータ要素を作成するには**

1. 「データ要 **[!UICONTROL 素を追加」をクリックします]**
1. Name the data element `Cart SKUs (Target)`
1. データ要 **[!UICONTROL 素の種類/カスタムコードを選択します]**
1. Target の場合、skus はコンマ区切りリストである必要があります。このカスタムコードは、データレイヤー配列を適切な形式に再フォーマットします。 カスタムコードエディターで、以下を貼り付けます。

   ```javascript
   var targetProdSkus="";
   for (var i=0; i<digitalData.cart.cartEntries.length; i++) {
     if(i>0) {
       targetProdSkus = targetProdSkus + ",";
     }
     targetProdSkus = targetProdSkus + digitalData.cart.cartEntries[i].sku;
   }
   return targetProdSkus;
   ```

1. Check the `Force lowercase value` option
1. Check the `Clean text` option
1. 「ライブラ **[!UICONTROL リに保存」をクリック]**

次に、これらのデータ要素を注文確認ページのパラメーターとして使用して注文確認リクエストを実行するルールを作成する必要があります。

**注文確認ページのルールを作成するには**

1. Click **[!UICONTROL Rules]** in the top navigation
1. Click **[!UICONTROL Add Rule]**
1. Name the rule `Order Confirmation Page - Library Loaded - 60`
1. イベント/ **[!UICONTROL 追加をクリックします。]**
   1. イベント **[!UICONTROL タイプ/ライブラリ読み込み（ページの上部）を選択します。]**
   1. アクション `Order` の後に起動するように、をに変更します(こ `60` れは、に設定され `Load Target` ているル `All Pages - Library Loaded` ールにありま `Order``50`す)。
   1. Click **[!UICONTROL Keep Changes]**
1. 条件/ **[!UICONTROL 追加をクリックします]**
   1. 条件タ **[!UICONTROL イプ/クエリ文字列なしのパスを選択]**
   1. Enterに対し `Path equals` て `thank-you.html`
   1. 「正規表現」オプションをオンにして、ロジックをからに変更 `equals` しま `contains` す(この機能を使用し `Test` て、テストがURLと共に成功することを確認できます)。 `https://luma.enablementadobe.com/content/luma/us/en/user/checkout/order/thank-you.html`

      ![姓と名のダミー値を入力します](images/target-orderConfirm-test.png)

   1. Click **[!UICONTROL Keep Changes]**
1. アクショ **[!UICONTROL ン/追加をクリックします]**
   1. アクショ **[!UICONTROL ンタイプ/カスタムコードを選択します]**
   1. Click **[!UICONTROL Open Editor]**
   1. Paste the following code into the `Edit Code` modal

      ```javascript
      adobe.target.getOffer({
        "mbox": "orderConfirmPage",
        "params":{
           "orderId": _satellite.getVar('Order Id'),
           "orderTotal": _satellite.getVar('Cart Amount'),
          "productPurchasedId": _satellite.getVar('Cart SKUs (Target)')
        },
        "success": function(offer) {
          adobe.target.applyOffer({
            "mbox": "orderConfirmPage",
            "offer": offer
          });
        },
        "error": function(status, error) {
          console.log('Error', status, error);
        }
      });
      ```

   1. Click **[!UICONTROL Save]** to save the custom code
   1. 「変更を **[!UICONTROL 保持」をクリックし]** 、アクションを保持します
1. 「ライブラ **[!UICONTROL リとビルドに保存」をクリックします]**

#### 注文確認要求の検証

at.js 2.xリクエストで渡されたカスタムパラメーターは、デバッガーに容易に表示されないので、ブラウザーの開発者ツールを利用します。

1. Open the [Luma site](https://luma.enablementadobe.com/content/luma/us/en.html)

1. Make sure the Debugger is mapping the Launch property to *your* Development environment, as described in the [earlier lesson](launch-switch-environments.md)

   ![デバッガーに表示される起動開発環境](images/switchEnvironments-debuggerOnWeRetail.png)

1. サイトを閲覧し、買い物かごに複数の製品を追加します。
1. チェックアウトを続行
1. チェックアウトプロセスでは、必須のフィールドは `First Name` と `Last Name`

   ![姓と名のダミー値を入力します](images/target-testOrderCart.png)

1. On the Review Order page, be sure to click the `Place Order` button
1. ブラウザーの開発者ツールを開く
1. [ネットワーク]タブをクリックします。
1. リクエストのフィルタ `tt.omtrdc` ー（Targetリクエストの場合はCNAMEドメイン）
1. 2つ目のリクエストをクリックします
1. &gt; &gt;セクシ `Headers` ョンを展 `Request Payload` 開し `execute.mboxes.0` て、リクエスト名と注文パラメーターを検証します。
   ![デバッガーでのリクエスト設定の並べ替え](images/target-debugger-orderConfirmPage-browser.png)
<!--
1. Look in the Debugger
1. Go to the Target tab
1. Expand your client code
1. You should see the `orderConfirmPage` request as the latest Target request with the orderId, orderTotal, and productPurchasedId parameters populated with the details of your order

   ![orderConfirmPage request with required parameters](images/target-debugger-orderConfirmPage.png)-->

### カスタムリクエスト

ページの読み込みと注文の確認のリクエスト以外にTargetリクエストを行う必要がある場合は、まれに発生します。 例えば、パーソナライゼーションに使用する重要なデータは、埋め込みコードの起動の前にページで定義されていない場合があります。ページの下部にハードコードされたり、非同期APIリクエストから返されたりします。 このデータは、追加のリクエストを使用してTargetに送信できますが、既にページが表示されるので、このリクエストをコンテンツ配信に使用するのは最適ではありません。 これは、後で使用するために訪問者プロファイルを拡張する（プロファイルパラメーターを使用する）ため、またはRecommendationsカタログにデータを埋め込むために使用できます。

In these circumstances, use the Custom Code action in the Core extension to fire a request using the
[getOffer()](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/functions-overview/adobe-target-getoffer.html)/[applyOffer()](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/functions-overview/adobe-target-applyoffer.html) and [trackEvent()](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/functions-overview/adobe-target-trackevent.html)
methods. This is very similar to what you just did in the [Order
Confirmation request](#order-confirmation-request) exercise, but you will just use a different request name and will not use the special order parameters. Be sure to use the **[!UICONTROL Load Target]** action before making Target requests from custom code.

## ライブラリのヘッダーとライブラリのフッター

Targetユーザーインターフェイスのat.jsを編集画面には、at.jsファイルの直前または直後に実行するカスタムJavaScriptを貼り付けることができます。 The Library Header is sometimes used to override at.js settings via the
[targetGlobalSettings()](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/functions-overview/targetgobalsettings.html) function or pass data from third parties using the [Data Providers](https://docs.adobe.com/content/help/en/target-learn/tutorials/integrations/use-data-providers-to-integrate-third-party-data.html) feature. ライブラリフッターは、 [at.jsカスタムイベントリスナーの追加に使用される場合があり](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/functions-overview/atjs-custom-events.html) ます。

この機能を起動で複製するには、コア拡張機能でカスタムコードアクションを使用し、「ターゲットを読み込み」アクションの前（ライブラリヘッダー）または後（ライブラリフッター）にアクションを並べます。 This can be done in the same rule as the `Load Target` action (as pictured below) or in separate rules with events or order settings that will reliably fire before or after the rule containing `Load Target`:

![アクションシーケンスのライブラリヘッダーとフッター](images/target-libraryHeaderFooter.png)

カスタムヘッダーやフッターの使用例について詳しくは、以下のリソースを参照してください。

* [dataProviders を使用してサードパーティデータを Adobe Target に統合する](https://docs.adobe.com/content/help/en/target-learn/tutorials/integrations/use-data-providers-to-integrate-third-party-data.html)
* [dataProviders を実装してサードパーティデータを Adobe Target に統合する](https://docs.adobe.com/content/help/en/target-learn/tutorials/integrations/implement-data-providers-to-integrate-third-party-data.html)
* [Adobe Target での応答トークンと at.js カスタムイベントの使用](https://docs.adobe.com/content/help/en/target-learn/tutorials/integrations/use-response-tokens-and-atjs-custom-events.html)

[次の「Adobe Analyticsを追加」&gt;](analytics.md)
