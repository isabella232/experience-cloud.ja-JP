---
title: Adobe AnalyticsのLaunchへの実装
description: Adobe Analytics Launch拡張機能を使用したAdobe Analyticsの実装、ページビュービーコンの送信、変数の追加、イベントの追跡およびプラグインの追加の方法について説明します。 このレッスンは、「WebサイトにExperience cloudを導入する（起動）」チュートリアルの一部です。
seo-description: null
seo-title: Adobe AnalyticsのLaunchへの実装
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# Adobe Analytics の追加

このレッスンでは、 [Adobe Analytics拡張機能を実装し](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html) 、Adobe Analyticsにデータを送信するルールを作成します。

[Adobe Analytics](https://docs.adobe.com/content/help/en/analytics/landing/home.html) は、顧客像を把握し、顧客インテリジェンスを活用してビジネスを導く力をユーザーに提供する、業界最先端のソリューションです。

## 学習目標

このレッスンを最後まで学習すると、以下の内容を習得できます。

1. Adobe Analytics拡張機能の追加
1. 拡張機能を使用したグローバル変数の設定
1. ページビュービーコンの追加
1. ルールを使用した変数の追加
1. クリック追跡およびその他のイベントベースのビーコンの追加
1. Analyticsプラグインの追加

起動時にAnalytics用に実装できる多くの機能があります。 このレッスンでは、完全なものではありませんが、お客様独自のサイトに導入するために必要な主なテクニックの概要を明確に説明する必要があります。

## 前提条件

You should have already completed the lessons in [Configure Launch](launch.md) and [Add the Identity Service](id-service.md).

さらに、少なくとも1つのレポートスイートIDとトラッキングサーバーが必要です。 このチュートリアルで使用できるテスト/開発レポートスイートがない場合は、レポートスイートを作成してください。 方法がわからない場合は、[ドキュメント](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)を参照してください。現在の導入環境、アドビコンサルタントまたはカスタマーケア担当者からトラッキングサーバーを取得できます。

## Analytics 拡張機能の追加

Analyticsの拡張機能は、次の2つの主要な部分で構成されます。

1. 拡張機能設定。コア AppMeasurement.js library 設定を管理し、グローバル変数を設定できます。
1. 次の操作を実行するルールアクション：
   1. Set Variables
   1. Clear Variables
   1. Analyticsビーコンの送信

**Analytics拡張機能を追加するには**

1. エクステンション/ **[!UICONTROL カタログに移動]**
1. Adobe Analytics拡張機能の検索
1. Click **[!UICONTROL Install]**

   ![Analytics拡張機能のインストール](images/analytics-catalog-install.png)

1. ライ [!UICONTROL ブラリ管理/レポートスイートで]、各起動環境で使用するレポートスイートIDを入力します。 ボックスに入力し始めると、すべてのレポートスイートのリストが事前に入力されて表示されます。 （このチュートリアルでは、すべての環境に対して1つのレポートスイートを使用しても構いませんが、実際には、次の画像のように別々のレポートスイートを使用する必要があります）。

   ![レポートスイートIDの入力](images/analytics-config-reportSuite.png)

   >[!TIP] ライブラリの管理 [!UICONTROL 設定では] 、「ライブラリを管理するユーザー」オプションを使用す [!UICONTROL ると、ライブラリを最新の状態に維持しやすくなり]`AppMeasurement.js` ます。

1. 一般/ [!UICONTROL トラッキングサーバー](「`tmd.sc.omtrdc.net`」 Enter your SSL Tracking Server if your site supports `https://`

   ![トラッキングサーバーの入力](images/analytics-config-trackingServer.png)

1. 「グローバ [!UICONTROL ル変数」セクションで]、データ要 [!UICONTROL 素を使用してページ名] 変数を `Page Name` 設定します。 データ要素アイ ![コンをクリックし](images/icon-dataElement.png) 、モーダルを開き、ページデータ要素 `Page Name` を選択します。)

1. 「ライブラ **[!UICONTROL リとビルドに保存」をクリックします]**

   ![ページ名変数を設定し、](images/analytics-extension-pageName.png)

>[!NOTE] グローバル変数は、拡張機能設定またはルールアクションで設定できます。Be aware that when setting variables in the extension configuration, the data layer must be defined *before* the Launch embed codes.

## ページビュービーコンの送信

次に、Analyticsビーコンを起動するルールを作成し、拡張設定で設定され [!UICONTROL たページ名] 変数を送信します。

You have already created an "All Pages - Library Loaded" rule in the [Add a Data Element, a Rule and a Library](launch-data-elements-rules.md) lesson of this tutorial, which is triggered on every page when the Launch library loads. このル *ールは* 、Analyticsにも使用できますが、この設定では、Analyticsビーコンで使用されるすべてのデータレイヤー属性を、埋め込みコードの起動前に定義する必要があります。 データ収集をより柔軟に行うために、DOM readyでAnalyticsビーコンを実行するためにトリガーされる新しい「すべてのページ」ルールを作成します。

**ページビュービーコンを送信するには**

1. 上部のナビゲーシ **[!UICONTROL ョンの]** 「ルール」セクションに移動し、「ルールの追加」をクリ **[!UICONTROL ックします]**

   ![ルールを追加](images/target-addRule.png)

1. Name the rule `All Pages - DOM Ready`
1. Click **[!UICONTROL Events &gt; Add]** to open the `Event Configuration` screen

   ![ルールに名前を付け、イベントを追加します](images/analytics-domReady-nameAddAnalyticsEvent.png)

1. イベ **[!UICONTROL ントタイプ/DOM readyを選択します]** （ルールの順序は「50」です）。
1. Click **[!UICONTROL Keep Changes]**
   ![イベントの設定](images/analytics-configureEventDomReady.png)

1. Under Actions, click the ![Click the Plus icon](images/icon-plus.png) to add a new action

   ![プラスアイコンをクリックして、新しいアクションを追加します](images/analytics-ruleAddAction.png)

1. 拡張機 **[!UICONTROL 能/Adobe Analyticsを選択します。]**

1. アクショ **[!UICONTROL ンタイプ/ビーコンを送信を選択します]**

1. トラッキングは `s.t()` に設定したままにします。Note that if you wanted to make an `s.tl()` call in a click-event rule you could do that using the Send Beacon action, as well.

1. 「変更を保持」ボ **[!UICONTROL タンをクリックします]** 。

   ![プラスアイコンをクリックして、新しいアクションを追加します](images/analytics-sendBeacon.png)

1. 「ライブラ **[!UICONTROL リとビルドに保存」をクリックします]**

   ![「ライブラリとビルドに保存」をクリックします](images/analytics-saveToLibraryAndBuild.png)

### ページビュービーコンの検証

これで、Analyticsビーコンを送信するルールが作成され、Experience cloudデバッガーにリクエストが表示されます。

1. Open the [Luma site](https://luma.enablementadobe.com/content/luma/us/en.html) in your Chrome browser
1. デバッガーアイコン ![Experience cloudデバッガーを開く](images/analytics-debuggerIcon.png) 、 **[!UICONTROL Adobe Experience cloudデバッガーを開きます]**
1. Make sure the Debugger is mapping the Launch property to *your* Development environment, as described in the [earlier lesson](launch-switch-environments.md)

   ![デバッガーに表示される起動開発環境](images/switchEnvironments-debuggerOnWeRetail.png)

1. クリックして「解析」タブを開きます
1. レポートスイート名を展開して、そのレポートスイートに対して行われたすべてのリクエストを表示します
1. Page name変数と値を使用してリクエストが実行されたことを確認します

   ![ページヒットの検証](images/analytics-validatePageHit.png)

>[!NOTE] ページ名が表示されていない場合は、このページの手順をさかのぼり、何もミスがないことを確認してください。

## ルールによる変数の追加

When you configured the Analytics Extension, you populated the `pageName` variable in the extension configuration. Launch 埋め込みコードが読み込まれる前に、ページ上で値を利用できるのであれば、ここに eVar や prop などの他のグローバル変数を入力するとよいでしょう。

変数やイベントを設定する場所は、アクションを使用するルール内でより柔軟に指定で `Set Variables` きます。 ルールを使用すると、様々な条件で様々なAnalytics変数やイベントを設定できます。 For example, you could set the `prodView` only on product detail pages and the `purchase` event only on order confirmation pages. この節では、ルールを使用して変数を設定する方法について説明します。

### 使用例

製品の詳細ページ（PDP）は、小売サイトでのデータ収集に重要なポイントです。通常、Analyticsで、製品表示が発生し、どの製品が表示されたかを登録します。 これは、顧客に人気のある製品を把握するのに役立ちます。メディアサイトでは、記事ページやビデオページでも、このセクションで使用するのと同様のトラッキングテクニックを使用できます。  When you load a Product Detail Page, you might want to put that value into a "Page Type" `eVar`, as well as set some events and the product id. これにより、分析で以下を確認できます。

1. 製品詳細ページが読み込まれた回数
1. 表示される特定の製品と回数
1. その他の要因（キャンペーン、検索など）が、PDPの人々の負荷にどの程度影響を与えるか

### ページタイプのデータ要素の作成

まず、商品の詳細ページであるページを特定する必要があります。 データ要素を使用して行います。

**ページタイプのデータ要素を作成するには**

1. 上部ナビゲ **[!UICONTROL ーションで]** 「データ要素」をクリックします。
1. 「データ要 **[!UICONTROL 素を追加」をクリックします]**

   ![新しいデータ要素の追加](images/analytics-addDataElement.png)

1. Name the data element `Page Type`
1. データ **[!UICONTROL 要素の種類/JavaScript変数を選択します。]**
1. を `digitalData.page.category.type``JavaScript variable name`
1. Check the `Clean text` and `Force Lower Case` options
1. 「ライブラ **[!UICONTROL リとビルドに保存」をクリックします]**

   ![ページタイプ用の新しいデータ要素の追加](images/analytics-PageTypeDataElement.png)

### 製品IDのデータ要素を作成

次に、現在の製品の詳細ページの製品IDをデータ要素と共に収集します

**製品IDのデータ要素を作成するには**

1. 上部ナビゲ **[!UICONTROL ーションで]** 「データ要素」をクリックします。
1. 「データ要 **[!UICONTROL 素を追加」をクリックします]**

   ![新しいデータ要素の追加](images/analytics-addDataElement.png)

1. Name the data element `Product Id`
1. データ **[!UICONTROL 要素の種類/JavaScript変数を選択します。]**
1. を `digitalData.product.0.productInfo.sku``JavaScript variable name`
1. Check the `Force lowercase value` option
1. Check the `Clean text` option
1. 「ライブラ **[!UICONTROL リとビルドに保存」をクリックします]**

   ![ページタイプ用の新しいデータ要素の追加](images/analytics-ProductIdDataElement.png)

### Adobe Analytics 製品文字列拡張機能の追加

If you are already familiar with Adobe Analytics implementations, you are probably familiar with the [products variable](https://docs.adobe.com/content/help/en/analytics/components/variables/dimensions-reports/reports-products.html). products変数の構文は非常に特殊で、使用方法は状況に応じて少し異なります。 製品の入力を容易にするために、Launch Extension Marketplaceに3つの追加の拡張機能が既に作成されています。 この節では、アドビコンサルティングが製品の詳細ページで使用するために作成した拡張機能を追加します。

**拡張子を追加する`Adobe Analytics Product String`には**

1. エクステンション/カ [!UICONTROL タログページに移動] 。
1. アドビのコンサルテ `Adobe Analytics Product String` ィングサービスの拡張機能を探し、「インストール」をクリッ **[!UICONTROL クします]**
   ![アドビコンサルティングによるAdobe Analytics製品文字列拡張子の追加](images/analytics-addProductStringExtension.png)
1. 説明を読む
1. 「ライブラ **[!UICONTROL リとビルドに保存」をクリックします]**

   ![拡張機能を保存し、ライブラリに構築します](images/analytics-addProductStringExtensionSave.png)

### 製品の詳細ページ用のルールの作成

次に、新しいデータ要素と拡張機能を使用して、製品の詳細ページルールを作成します。 この機能に対しては、DOM readyによってトリガーされる別のページ型ルールを作成します。 However, you will use a condition so that it only fires on the Product Detail pages and the order setting so that it fires _before_ the rule that sends the beacon.

**製品の詳細ページのルールを作成するには**

1. 上部のナビゲーシ **[!UICONTROL ョンの]** 「ルール」セクションに移動し、「ルールの追加」をクリ **[!UICONTROL ックします]**

   ![ルールを追加](images/target-addRule.png)

1. Name the rule `Product Details - DOM Ready - 40`
1. Click **[!UICONTROL Events &gt; Add]** to open the `Event Configuration` screen

   ![ルールに名前を付け、イベントを追加します](images/analytics-domReadyAddEvent.png)

1. イベント **[!UICONTROL タイプ/DOM readyを選択します。]**
1. Set the **[!UICONTROL Order]** to 40, so that the rule will run *before* the rule containing the Analytics &gt; Send Beacon action
1. Click **[!UICONTROL Keep Changes]**
   ![イベントの設定](images/analytics-configDOMReadyEvent.png)

1. 「条 **[!UICONTROL 件」で]**、プラス ![アイコンをクリックして](images/icon-plus.png) 、画面を開き `Condition Configuration` ます
   ![プラスアイコンをクリックして、新しい条件を追加します](images/analytics-PDPRuleAddCondition.png)

   1. 条件タ **[!UICONTROL イプ/値の比較を選択]**
   1. Use the data element picker, choose `Page Type` in the first field
   1. [比較演 **[!UICONTROL 算子]** ]ドロップダウンで[次を含む]を選択します。
   1. 次のフィールド・タ `product-page` イプ（PDPのデータ・レイヤから取り出したページ・タイプ値の一意の部分）
   1. Click **[!UICONTROL Keep Changes]**

      ![条件の定義](images/analytics-PDP-condition.png)

1. Under Actions, click the ![Click the Plus icon](images/icon-plus.png) to add a new action

   ![プラスアイコンをクリックして、新しいアクションを追加します](images/analytics-PDPAddAction.png)

1. 拡張機 **[!UICONTROL 能/Adobe Analyticsを選択します。]**
1. アクシ **[!UICONTROL ョンタイプ/変数の設定を選択します]**
1. eVar1/ **[!UICONTROL 設定を選択し]** 、 `product detail page`
1. Set **[!UICONTROL event1]**, leaving the optional values blank
1. 「Events」で、「 **[!UICONTROL Add Another]** 」ボタンをクリックします
1. Set the **[!UICONTROL prodView]** event, leaving the optional values blank
1. Click **[!UICONTROL Keep Changes]**

   ![PDPルールでのAnalytics変数の設定](images/analytics-PDPsetVariables.png)

1. Under Actions, click the ![Click the Plus icon](images/icon-plus.png) to add a new action

   ![製品文字列に別のアクションを追加](images/analytics-PDPaddProductStringAction.png)

1. 拡張機能/ **[!UICONTROL Adobe Analytics製品文字列を選択します。]**
1. アクシ **[!UICONTROL ョンタイプ/s.productsを設定を選択します。]**

1. 「 **[!UICONTROL Analytics E-commerce Event]** 」セクションで、「 **[!UICONTROL prodView」を選択します。]**

1. 「 **[!UICONTROL Data layer variables for product data]** 」セクションで、データ要素ピッカーを使用してデータ要素 `Product Id` を選択します

1. Click **[!UICONTROL Keep Changes]**

   ![Adobe Analytics製品文字列拡張子を使用した製品文字列変数の追加](images/analytics-PDPaddProductString.png)

1. 「ライブラ **[!UICONTROL リとビルドに保存」をクリックします]**

   ![ルールの保存](images/analytics-PDP-saveRule.png)

### 製品の詳細ページデータの検証

ビーコンの送信前に変数を設定するルールを作成しました。 これで、Experience cloudデバッガーのヒットで新しいデータが発生していることを確認できます。

**製品の詳細ページのデータを検証するには**

1. Open the [Luma site](https://luma.enablementadobe.com/content/luma/us/en.html) in your Chrome browser
1. 任意の製品詳細ページに移動します。
1. デバッガーアイコン ![Experience cloudデバッガーを開く](images/analytics-debuggerIcon.png) 、 **[!UICONTROL Adobe Experience cloudデバッガーを開きます]**
1. 「解析」タブをクリックします
1. レポートスイートの展開
1. Notice the Product Detail Variables that are now in the debugger, namely that `eVar1` has been set to "product detail page", that the `Events` variable has been set to "event1" and "prodView", that the products variable is set with the product id of the product you are viewing, and that your Page Name is still set by the Analytics extension

   ![ページヒットの検証](images/analytics-validatePDPvars.png)

## トラックリンクビーコンの送信

ページが読み込まれると、通常、`s.t()` 関数によってトリガーされたページ読み込みビーコンが実行されます。This automatically increments a `page view` metric for the page listed in the `pageName` variable.

ただし、実行されるアクションはページビューとは「小さい」（または単に異なる）ので、サイトのページビュー数を増やさないようにしたい場合があります。 In this case, you will use the `s.tl()` function, which is commonly referred to as a "track link" request. トラックリンクリクエストと呼ばれていますが、必ずしもリンクのクリックでトリガーする訳ではありません。It can be triggered by *any* of the events that are available to you in the Launch rule builder, including your own custom JavaScript.

このチュートリアルでは、最も優れたJavaScriptイベ `s.tl()` ントの1つであるイベントを使用して呼び出しをトリガ `Enters Viewport` します。

### ユースケース

この使用例では、ユーザーがLumaのホームページを下にスクロールして、ページの「新着」セクションを表示でき *るかどうか* を知る必要があります。 ユーザーがそのセクションを閲覧しているかどうかに関して、当社で内部的な不一致が生じるので、Analyticsを使用して真実を判断する必要があります。

### 起動でのルールの作成

1. 上部ナビゲーションの **[!UICONTROL 「ルール]** 」セクションに移動し、「ルールの追加」をク **[!UICONTROL リックします]**
   ![ルールを追加](images/target-addRule.png)
1. Name the rule `Homepage - New Arrivals enters Viewport`
1. Click **[!UICONTROL Events &gt; Add]** to open the `Event Configuration` screen

   ![新しい着荷ルールの追加](images/analytics-newArrivalsRuleAdd2.png)

1. イベントタ **[!UICONTROL イプ/ビューポートに入るを選択しま]**&#x200B;す。 これにより、CSSセレクターを入力する必要があるフィールドが表示されます。このセレクターは、ブラウザーにルールが表示されたときにルールをトリガーする必要のあるページ上の項目を識別します。
1. ルミナンスのホームページに戻り、「新着」セクションまで下にスクロールします。
1. 「NEW ARRIVALS」タイトルとこのセクションの項目の間のスペースを右クリックし、右クリックメニューから「`Inspect`」を選択します。これで君の欲しいものに近づく。
1. そのすぐ近く、おそらく選択したセクションのすぐ下で、を持つdivを探していま `class="we-productgrid aem-GridColumn aem-GridColumn--default--12"`す。 この要素を見つけます。
1. この要素を右クリックし、コピー/セレクターを **[!UICONTROL コピーを選択します]**

   ![入口ビューポートイベントの設定](images/analytics-copyElementSelector.png)

1. 「起動」に戻り、この値をクリップボードからラベル付きのフィールドに貼り付けま `Elements matching the CSS selector`す。
   1. CSSセレクターの識別方法は、ユーザーが決定します。 このメソッドは、ページ上で特定の変更を行うと、このセレクターが壊れる場合があるので、少し脆弱です。 「起動」でCSSセレクターを使用する場合は、この点に注意してください。
1. Click **[!UICONTROL Keep Changes]**
   ![入口ビューポートイベントの設定](images/analytics-configEntersViewportEvent.png)

1. 「条件」で、プラスアイコ ![ンをクリックして](images/icon-plus.png) 、新しい条件を追加します
1. 条件タ **[!UICONTROL イプ/値の比較を選択]**
1. Use the data element picker, choose `Page Name` in the first field
1. [比較演 **[!UICONTROL 算子]** ]ドロップダウンで[等しい]を選択します。
1. 次のフィールドタイプ `content:we-retail:us:en` （これは、データレイヤーから取り込んだホームページのページ名です。このルールは、ホームページ上でのみ実行する必要があります）
1. Click **[!UICONTROL Keep Changes]**

   ![ホームページの条件の設定](images/analytics-configHomepageCondition.png)

1. Under Actions, click the ![Click the Plus icon](images/icon-plus.png) to add a new action
1. 拡張機 **[!UICONTROL 能/Adobe Analyticsを選択します。]**
1. アクシ **[!UICONTROL ョンタイプ/変数の設定を選択します]**
1. Set `eVar3` to `Home Page - New Arrivals`
1. Set `prop3` to `Home Page - New Arrivals`
1. Set the `Events` variable to `event3`
1. Click **[!UICONTROL Keep Changes]**

   ![入口ビューポートイベントの設定](images/analytics-configViewportAction.png)

1. Under Actions, click the ![Click the Plus icon](images/icon-plus.png) to add another new action

   ![ビーコン送信アクションの追加](images/analytics-newArrivalsSendBeacon2.png)

1. 拡張機 **[!UICONTROL 能/Adobe Analyticsを選択します。]**
1. アクショ **[!UICONTROL ンタイプ/ビーコンを送信を選択します]**
1. Choose the **[!UICONTROL s.tl()]** tracking option
1. 「リンク **[!UICONTROL 名]** 」フィールドに、と入力しま `Scrolled down to New Arrivals`す。 この値は、Analyticsのカスタムリンクレポートに配置されます。
1. Click **[!UICONTROL Keep Changes]**

   ![新着ビーコンの設定](images/analytics-configEntersViewportBeacon.png)

1. 「ライブラ **[!UICONTROL リとビルドに保存」をクリックします]**

   ![ルールを保存してビルド](images/analytics-saveCustomLinkRule.png)

### トラックリンクビーコンの検証

次に、サイトのホームページの「新着」セクションまでスクロールする場合に、このヒットが確実に含まれるようにします。 ホームページを初めて読み込むと、リクエストは行われませんが、下にスクロールしてセクションが表示されると、ヒットが新しい値で発生します。

1. Chromeブラウザ [ーで](https://luma.enablementadobe.com/content/luma/us/en.html) Lumaサイトを開き、ホームページの先頭に来ていることを確認します。
1. デバッガー **[!UICONTROL アイコン]** Experience Cloud Debugger ![を開き](images/analytics-debuggerIcon.png) 、 [!UICONTROL Adobe Experience cloudデバッガーを開きます]
1. 「解析」タブをクリックします
1. レポートスイートのヒットを展開する
1. ホームページの通常のページビューヒットにページ名などが付いています。 （ただし、eVar3やprop3には何も含まれません）。

   ![ページビューを使用するデバッガ](images/analytics-debuggerPageView.png)

1. デバッガーを開いたまま、「新着」セクションが表示されるまでサイトを下にスクロールします
1. デバッガーを再度表示すると、別のAnalyticsヒットが表示されるはずです。 このヒットには、設定したs.tl()ヒットに関連付けられた次のパラメーターが必要です。
   1. `LinkType = "link_o"` （つまり、ヒットがページビューヒットではなく、カスタムリンクヒットであることを意味します）。
   1. `LinkName = "Scrolled down to New Arrivals"`
   1. `prop3 = "Home Page - New Arrivals"`
   1. `eVar3 = "Home Page - New Arrivals"`
   1. `Events = "event3"`

      ![ページビューを使用するデバッガ](images/analytics-debuggerEntersViewport.png)

## プラグインの追加

プラグインは、製品に組み込まれていない特定の機能を実行するために、実装に追加できるJavaScriptコードの1つです。 プラグインは、お客様、他のアドビのお客様/パートナー、またはアドビコンサルティングが作成できます。

プラグインを実装するには、基本的に次の3つの手順があります。

1. doPlugins関数を含めます。この関数では、プラグインが参照されます。
1. プラグインのメイン関数コードの追加
1. 関数を呼び出し、変数などを設定するコードを含める。

### Analyticsオブジェクトのグローバルアクセスの許可

以下のdoPlugins関数を追加してプラグインを使用する場合は、Analyticsの実装でAnalyticsの「s」オブジェクトをグローバルに使用できるようにするためのチェックボックスをオンにする必要があります。

1. 拡張機能/インス **[!UICONTROL トール済みに移動します。]**

1. Adobe Analytics拡張機能で、「設定」をクリックし **[!UICONTROL ます]**

   ![Analyticsの設定](images/analytics-configureExtension.png)

1. 「ライ **[!UICONTROL ブラリ管理]**`Make tracker globally accessible`」で、「 ヘルプの吹き出しで確認できるように、これにより、トラッカーがwindow.sの下でグローバルにスコープされ、顧客のJavaScriptで参照する際に重要になります。

### doPlugins関数のインクルード

プラグインを追加するには、doPluginsという関数を追加する必要があります。 この関数は、デフォルトでは追加されませんが、追加されるとAppMeasurementライブラリによって処理され、Adobe Analyticsにヒットが送信される際に最後に呼び出されます。 したがって、この関数を使用すると、JavaScript を実行して、この方法を設定するよりも簡単に変数を設定できます。

1. Analytics拡張機能を使用している間に、下にスクロールして「 `Configure Tracker Using Custom Code.`
1. Click **[!UICONTROL Open Editor]**
1. コードエディターに次のコードを貼り付けます。

   ```javascript
   /* Plugin Config */
   s.usePlugins=true
   s.doPlugins=function(s) {
   /* Add calls to plugins here */
   }
   ```

1. このウィンドウを開いたままにして、次の手順に進みます。

### プラグインに関数コードを追加する

このコードで2つのプラグインを呼び出すとしますが、そのうちの1つはAppMeasurementライブラリに組み込まれているので、呼び出す関数を追加する必要はありません。 ただし、2番目の関数の場合は、関数コードも追加する必要があります。 この関数はgetValOnce()と呼ばれます。

### getValOnce()プラグイン

このプラグインの目的は、訪問者がページを更新したときにコード内の値が誤って複製されないようにするか、ブラウザーの「戻る」ボタンを使用して値が設定されたページに戻ることです。 このレッスンでは、イベントの複製を防ぐため `clickthrough` にこのレッスンを使用します。

このプラグインのコードは [Analytics のドキュメント](https://docs.adobe.com/content/help/en/analytics/implementation/javascript-implementation/plugins/getvalonce.html)で入手できますが、簡単にコピーして貼り付けられるよう、ここに含まれています。

1. 次のコードをコピーします。

   ```javascript
   /*
   * Plugin: getValOnce_v1.11
   */
   s.getValOnce=new Function("v","c","e","t",""
   +"var s=this,a=new Date,v=v?v:'',c=c?c:'s_gvo',e=e?e:0,i=t=='m'?6000"
   +"0:86400000,k=s.c_r(c);if(v){a.setTime(a.getTime()+e*i);s.c_w(c,v,e"
   +"==0?0:a);}return v==k?'':v");
   ```

1. それをAnalytics拡張機能のコードウィンドウ（まだ開いていない場合は、前の手順に従って再度開きます）にペーストします。 **doPlugins関数の完全な下** （関数の内部ではなく）にペーストします。

   ![プラグインコードの追加](images/analytics-doPluginsAndGeValOnceCode.png)

これで、このプラグインをdoPlugins内から呼び出すことができます。

### doPlugins 内からのプラグインの呼び出し

コードが存在し、参照できるようになったので、doPlugins関数内でプラグインの呼び出しを行うことができます。

最初に、「ユーティリティ」と呼ばれる、AppMeasurementライブラリに組み込まれたプラグインを呼び出します。 It is referred to as `s.Util.getQueryParam`, because it is part of the s object, is a built-in utility, and will grab values (based on a parameter) from the query string in the URL.

1. 次のコードをコピーします。

   ```javascript
   s.campaign = s.Util.getQueryParam("cid");
   ```

1. doPlugins関数に貼り付けます。 This will look for a parameter called `cid` in the current page URL and place it into the s.campaign variable.
1. 次に、次のコードをコピーし、getQueryParamの呼び出しのすぐ下に貼り付けて、getValOnce関数を呼び出します。

   ```javascript
   s.campaign=s.getValOnce(s.campaign,'s_cmp',30);
   ```

   このコードを使用すると、同じ値が30日間連続して2回以上送信されないようにすることができます（必要に応じてこのコードをカスタマイズする方法については、ドキュメントを参照してください）。

   ![doPluginsでのプラグインの呼び出し](images/analytics-doPluginsWithPlugins.png)

1. コードウィンドウの保存
1. 「ライブラ **[!UICONTROL リとビルドに保存」をクリックします]**

   ![doPluginsでのプラグインの呼び出し](images/analytics-saveExtensionAndBuild.png)

### プラグインの検証

これで、プラグインが機能していることを確認できます。

**プラグインを検証するには**

1. Open the [Luma site](https://luma.enablementadobe.com/content/luma/us/en.html) in your Chrome browser
1. デバッガーアイコン ![Experience cloudデバッガーを開く](images/analytics-debuggerIcon.png) 、 **[!UICONTROL Adobe Experience cloudデバッガーを開きます]**
1. 「解析」タブをクリックします
1. レポートスイートの展開
1. Analyticsヒットにキャンペーン変数がないことに注意してください。
1. Leaving the Debugger open, go back to the Luma site and add  `?cid=1234` to the URL and hit Enter to refresh the page with that query string included

   ![クエリ文字列の追加](images/analytics-cidAdded.png)

1. Check the Debugger and confirm that there is a second Analytics request with a Campaign variable set to `1234`

   ![getQueryParam手順1](images/analytics-getQueryParam1.png)

1. URLにクエリ文字列を残したまま、再びルミナンスページを更新します。
1. デバッガーで次のヒットをチェックすると、Campaign変数が存在しないはずです。 **** getValOnceプラグインは、重複がなく、別の人がキャンペーントラッキングコードから来たように見えることを確認しているからです。

   ![getQueryParam手順1](images/analytics-getQueryParam2.png)

1. BONUS: You can test this over and over by changing the value of the `cid` parameter in the query string. The Campaign variable should only be there if it is the **first** time you run the page with the value. If you are not seeing the Campaign value in the debugger, simply change the value of the `cid` in the query string of the URL, hit enter, and you should see it again in the debugger.

   >[!NOTE] 実際には、Analytics拡張の設定を含む、URLのクエリ文字列からパラメーターを取得する方法がいくつかあります。 ただし、これらの他のプラグイン以外のオプションでは、getValOnceプラグインを使用して行ったように、不要な複製を停止する機能は提供されません。 これは著者のお気に入りの方法ですが、どの方法が自分とニーズに最適かを判断する必要があります。

いい仕事！ Analyticsのレッスンが完了しました。 もちろん、Analyticsの実装を強化するためには他にも多くのことができますが、それによって、残りのニーズに対処するために必要なコアスキルが得られれば幸いです。

[次の「Adobe Audience Managerを追加」/](audience-manager.md)
