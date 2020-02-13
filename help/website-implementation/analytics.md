---
title: Launch を使用した Adobe Analytics の実装
description: Adobe Analytics Launch 拡張機能を使用した Adobe Analytics の実装方法、ページビュービーコンの送信方法、変数の追加方法、イベントのトラッキング方法、プラグインの追加方法について説明します。このレッスンは、「Launch を使用した Web サイトでの Experience Cloud の実装」チュートリアルの一部です。
seo-description: null
seo-title: Launch を使用した Adobe Analytics の実装
solution: Experience Cloud
translation-type: ht
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# Adobe Analytics の追加

このレッスンでは、[Adobe Analytics 拡張機能](https://docs.adobe.com/content/help/ja-JP/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html)を実装し、Adobe Analytics にデータを送信するルールを作成します。

[Adobe Analytics](https://docs.adobe.com/content/help/ja-JP/analytics/landing/home.html) は、顧客像を把握し、顧客インテリジェンスを活用してビジネスを導く力をユーザーに提供する、業界最先端のソリューションです。

## 学習内容

このレッスンを最後まで学習すると、以下の内容を習得できます。

1. Adobe Analytics 拡張機能の追加
1. 拡張機能を使用したグローバル変数の設定
1. ページビュービーコンの追加
1. ルールを使用した変数の追加
1. クリック追跡およびその他のイベントベースのビーコンの追加
1. Analytics プラグインの追加

Launch では、Analytics に多くの機能を実装できます。このレッスンは完全なものではありませんが、お客様独自のサイトで実装するために必要となる、主な技術の概要について明確に説明します。

## 前提条件

[Launch の設定](launch.md)および [ID サービスの追加](id-service.md)チュートリアルを既に完了していること。

さらに、1 つ以上のレポートスイート ID とトラッキングサーバーが必要です。このチュートリアルで使用できるテスト／開発用レポートトスイートがない場合は、作成してください。方法が分からない場合は、[ドキュメント](https://docs.adobe.com/content/help/ja-JP/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)を参照してください。トラッキングサーバーは、現在の実装、アドビコンサルタントまたはカスタマーケア担当者から取得できます。

## Analytics 拡張機能の追加

Analytics 拡張機能は、次の 2 つの主要部分で構成されます。

1. 拡張機能設定。コア AppMeasurement.js library 設定を管理し、グローバル変数を設定できます。
1. 次の操作を実行するルールアクション：
   1. Set Variables
   1. Clear Variables
   1. Analytics ビーコンの送信

**Analytics 拡張機能を追加するには、以下を実行します。**

1. **[!UICONTROL 拡張機能／カタログ]**&#x200B;に移動します。
1. Adobe Analytics 拡張機能を検索します。
1. **[!UICONTROL インストール]**&#x200B;をクリックします。

   ![Analytics 拡張機能のインストール](images/analytics-catalog-install.png)

1. [!UICONTROL ライブラリ管理／レポートスイート]で、各 Launch 環境で使用するレポートスイート ID を入力します。ボックスに入力し始めると、すべてのレポートスイートのリストが事前に入力されて表示されます。（このチュートリアルでは、すべての環境に対して 1 つのレポートスイートを使用しても構いませんが、実際の環境では、次の画像のように別々のレポートスイートを使用する必要があります）。

   ![レポートスイート ID の入力](images/analytics-config-reportSuite.png)

   >[!TIP] [!UICONTROL ライブラリの管理]設定では私に代わってライブラリを管理するオプションを使用すると、`AppMeasurement.js` ライブラリを最新の状態に維持しやすくなります。

1. [!UICONTROL 一般／トラッキングサーバー]で、トラッキングサーバー（例：`tmd.sc.omtrdc.net`）を入力します。サイトが `https://` に対応している場合は、SSL トラッキングサーバーを入力します。

   ![トラッキングサーバーの入力](images/analytics-config-trackingServer.png)

1. [!UICONTROL 「グローバル変数」セクション]で、`Page Name` データ要素を使用して[!UICONTROL ページ名]変数を設定します。![データ要素アイコン](images/icon-dataElement.png)をクリックしてモーダルを開き、ページの `Page Name` データ要素を選択します。

1. **[!UICONTROL ライブラリに保存してビルド]**&#x200B;をクリックします。

   ![ページ名変数の設定と保存](images/analytics-extension-pageName.png)

>[!NOTE] グローバル変数は、拡張機能設定またはルールアクションで設定できます。拡張機能設定で変数を設定する場合は、Launch 埋め込みコードの&#x200B;*前に*&#x200B;データレイヤーを定義する必要があります。

## ページビュービーコンの送信

次に、Analytics ビーコンを起動するルールを作成します。このルールは、拡張機能設定で設定された[!UICONTROL ページ名]変数を送信します。

このチュートリアルの[データ要素、ルールおよびライブラリの追加](launch-data-elements-rules.md)チュートリアルで既に、Launch ライブラリを読み込むときに、各ページでトリガーされる「All Pages - Library Loaded」ルールを作成しています。このルールを Analytics にも使用&#x200B;*してもいい*&#x200B;ですが、この設定では、Analytics ビーコンで使用されるすべてのデータレイヤー属性を、Launch 埋め込みコードの前に定義する必要があります。データ収集をより柔軟に実行できるようにするには、「DOM Ready」でトリガーして Analytics ビーコンを起動する、新しい「all pages」ルールを作成します。

**ページビュービーコンを送信するには、以下を実行します。**

1. 上部のナビゲーションの「**[!UICONTROL ルール]**」セクションに移動し、**[!UICONTROL ルールの追加]**&#x200B;をクリックします。

   ![ルールの追加](images/target-addRule.png)

1. ルール名を設定します。`All Pages - DOM Ready`
1. **[!UICONTROL イベント／追加]**&#x200B;をクリックして、`Event Configuration` 画面を開きます。

   ![ルールに名前を付けてイベントを追加する](images/analytics-domReady-nameAddAnalyticsEvent.png)

1. **[!UICONTROL イベントタイプ／DOM Ready]**&#x200B;を選択します（ルールの順序は「50」です）。
1. 「**[!UICONTROL 変更を保存]**」をクリックします。
   ![イベントの設定](images/analytics-configureEventDomReady.png)

1. 「アクション」の下の![プラスアイコン](images/icon-plus.png)をクリックして、新しいアクションを追加します。

   ![「+」アイコンをクリックし、新しいアクションを追加する](images/analytics-ruleAddAction.png)

1. **[!UICONTROL 拡張機能／Adobe Analytics]** を選択します。

1. **[!UICONTROL アクションタイプ／ビーコンを送信]**&#x200B;を選択します。

1. トラッキングは `s.t()` に設定したままにします。クリックイベントルールで `s.tl()` を呼び出す場合は、「ビーコンを送信」アクションでおこなうことも使用できます。

1. 「**[!UICONTROL 変更を保存]**」をクリックします。

   ![「+」アイコンをクリックし、新しいアクションを追加する](images/analytics-sendBeacon.png)

1. **[!UICONTROL ライブラリに保存してビルド]**&#x200B;をクリックします。

   ![「ライブラリに保存してビルド」をクリックする](images/analytics-saveToLibraryAndBuild.png)

### ページビュービーコンの検証

ビーコンを送信するためのルールを作成したら、Experience Cloud デバッガーでそのリクエストを表示できるようになります。

1. Chrome ブラウザーで [Luma サイト](https://luma.enablementadobe.com/content/luma/us/en.html)を開きます。
1. デバッガーアイコン![Experience Cloud デバッガーを開く](images/analytics-debuggerIcon.png)をクリックして、**[!UICONTROL Adobe Experience Cloud デバッガー]**&#x200B;を開きます。
1. [前のレッスン](launch-switch-environments.md)で説明したように、デバッガーが Launch プロパティを&#x200B;*自分の*&#x200B;開発環境にマッピングしていることを確認します。

   ![デバッガーに表示される Launch 開発環境](images/switchEnvironments-debuggerOnWeRetail.png)

1. クリックして「Analytics」タブを開きます。
1. レポートスイート名を展開して、それに対するすべての要求を表示します。
1. そのページ名変数と値で要求が実行されたことを確認します。

   ![ページヒットの検証](images/analytics-validatePageHit.png)

>[!NOTE] ページ名が表示されていない場合は、このページの手順を遡り、何もミスがないことを確認してください。

## ルールによる変数の追加

Analytics 拡張機能を設定する際、拡張機能設定に `pageName` 変数を入力しました。Launch 埋め込みコードが読み込まれる前に、ページ上で値を利用できるのであれば、ここに eVar や prop などの他のグローバル変数を入力するとよいでしょう。

変数やイベントを設定する場所は、`Set Variables` アクションを使用するルール内でより柔軟に指定できます。ルールを使用すると、異なる条件下で異なる Analytics 変数やイベントを設定できます。例えば、`prodView` は製品詳細ページでのみ、`purchase` イベントは注文確認ページでのみ設定できます。ここでは、ルールを使用して変数を設定する方法について説明します。

### 使用例

製品の詳細ページ（PDP）は、小売サイトでのデータ収集に重要なポイントです。通常は、Analytics に対し、製品の表示が発生し、どの製品が閲覧されたかを登録します。これは、顧客に人気のある製品を把握するのに役立ちます。メディアサイトでは、記事またはビデオページで、このセクションで説明する手法と似たトラッキングテクニックを使用できます。製品の詳細ページを読み込む際、その値を「ページタイプ」`eVar` に入力し、イベントと製品 ID を設定することができます。これにより、以下の内容を分析で確認できるようになります。

1. 製品の詳細ページは何回読み込まれたか。
1. どの製品が何回表示されたか。
1. その他の要因（キャンペーン、検索など）が、ユーザーが読み込む PDP にどの程度影響しているか。

### ページタイプのデータ要素の作成

まずどのページが製品の詳細ページであるかを特定する必要があります。これには、データ要素を使用します。

**ページタイプのデータ要素を作成するには、以下を実行します。**

1. 上部のナビゲーションで&#x200B;**[!UICONTROL データ要素]**&#x200B;をクリックします。
1. **[!UICONTROL データ要素を追加]**&#x200B;をクリックします。

   ![新しいデータ要素の追加](images/analytics-addDataElement.png)

1. データ要素に「`Page Type`」と名前を付けます。
1. **[!UICONTROL データ要素の種類／JavaScript 変数]**&#x200B;を選択します。
1. `digitalData.page.category.type` を `JavaScript variable name` として使用します。
1. 「`Clean text`」および「`Force Lower Case`」オプションを選択します。
1. **[!UICONTROL ライブラリに保存してビルド]**&#x200B;をクリックします。

   ![ページタイプ用の新しいデータ要素の追加](images/analytics-PageTypeDataElement.png)

### 製品 ID のデータ要素の作成

次に、現在の製品詳細ページの製品 ID をデータ要素と共に収集します。

**製品 ID のデータ要素を作成するには、以下を実行します。**

1. 上部のナビゲーションで&#x200B;**[!UICONTROL データ要素]**&#x200B;をクリックします。
1. **[!UICONTROL データ要素を追加]**&#x200B;をクリックします。

   ![新しいデータ要素の追加](images/analytics-addDataElement.png)

1. データ要素に「`Product Id`」と名前を付けます。
1. **[!UICONTROL データ要素の種類／JavaScript 変数]**&#x200B;を選択します。
1. `digitalData.product.0.productInfo.sku` を `JavaScript variable name` として使用します。
1. `Force lowercase value` オプションをオンにします。
1. `Clean text` オプションをオンにします。
1. **[!UICONTROL ライブラリに保存してビルド]**&#x200B;をクリックします。

   ![ページタイプ用の新しいデータ要素の追加](images/analytics-ProductIdDataElement.png)

### Adobe Analytics 製品文字列拡張機能の追加

Adobe Analytics の実装に詳しい方は、[products](https://docs.adobe.com/content/help/ja-JP/analytics/components/variables/dimensions-reports/reports-products.html) 変数について既に詳しいと思われます。製品変数には特定の構文があり、コンテキストに応じて使用方法は若干異なります。Launch で製品変数を生成しやすくするために、拡張機能マーケットプレイスで、さらに 3 つの拡張機能が作成されました。この節では、製品の詳細ページで使用できるよう、アドビコンサルティングによって作成された拡張機能を追加します。

**`Adobe Analytics Product String`extension** を追加するには、以下を実行します。

1. [!UICONTROL 拡張機能／カタログ]ページに移動します。
1. アドビコンサルテングサービスの `Adobe Analytics Product String` 拡張機能を探し、**[!UICONTROL インストール]**をクリックします。
   ![アドビコンサルティングによる Adobe Analytics 製品文字列拡張機能の追加](images/analytics-addProductStringExtension.png)
1. しばらく時間をかけ、手順を読んでください。
1. **[!UICONTROL ライブラリに保存してビルド]**&#x200B;をクリックします。

   ![拡張機能をライブラリに保存してビルドする](images/analytics-addProductStringExtensionSave.png)

### 製品の詳細ページ用のルールの作成

次に、新しいデータ要素と拡張機能を使用して、製品の詳細ページルールを作成します。この機能の場合は、「DOM Ready」によってトリガーされる別のページ読み込みルールを作成します。ただし、製品の詳細ページでのみ実行するよう条件を使用しビーコンを送信するルールより&#x200B;_前に_&#x200B;実行されるように順序を設定します。

**製品詳細ページのルールを作成するには、以下を実行します。**

1. 上部のナビゲーションの「**[!UICONTROL ルール]**」セクションに移動し、**[!UICONTROL ルールの追加]**&#x200B;をクリックします。

   ![ルールの追加](images/target-addRule.png)

1. ルール名を設定します。`Product Details - DOM Ready - 40`
1. **[!UICONTROL イベント／追加]**&#x200B;をクリックして、`Event Configuration` 画面を開きます。

   ![ルールに名前を付けてイベントを追加する](images/analytics-domReadyAddEvent.png)

1. **[!UICONTROL イベントタイプ／DOM Ready]**&#x200B;を選択します。
1. **[!UICONTROL 順序]** を「40」に設定し、「Analytics／ビーコンを送信」アクションを含むルールの&#x200B;*前に*&#x200B;ルールが実行されるようにします。
1. 「**[!UICONTROL 変更を保存]**」をクリックします。
   ![イベントの設定](images/analytics-configDOMReadyEvent.png)

1. **[!UICONTROL 条件]**&#x200B;で、![プラスアイコンをクリック](images/icon-plus.png)して、`Condition Configuration` 画面を開きます。
   ![「+」アイコンをクリックし、新しい条件を追加する](images/analytics-PDPRuleAddCondition.png)

   1. **[!UICONTROL 条件タイプ／値の比較]**&#x200B;を選択します。
   1. データ要素ピッカーを使用して、最初のフィールドで「`Page Type`」を選択します。
   1. 比較演算子ドロップダウンで「**[!UICONTROL 次を含む]**」を選択します。
   1. 次のフィールドで、`product-page` を入力します（PDPのデータレイヤーから取り出したページタイプ値の一意の部分）。
   1. 「**[!UICONTROL 変更を保存]**」をクリックします。

      ![条件の定義](images/analytics-PDP-condition.png)

1. 「アクション」の下の![プラスアイコン](images/icon-plus.png)をクリックして、新しいアクションを追加します。

   ![「+」アイコンをクリックし、新しいアクションを追加する](images/analytics-PDPAddAction.png)

1. **[!UICONTROL 拡張機能／Adobe Analytics]** を選択します。
1. **[!UICONTROL アクションタイプ／変数の設定]**&#x200B;を選択します。
1. **[!UICONTROL eVar1／設定]**&#x200B;を選択し、`product detail page` を入力します。
1. **[!UICONTROL イベント 1]** を設定し、オプションの値は空白のまま残します。
1. 「イベント」の下の「**[!UICONTROL その他を追加]**」ボタンをクリックします。
1. **[!UICONTROL prodView]** イベントを設定し、オプションの値は空白のまま残します。
1. 「**[!UICONTROL 変更を保存]**」をクリックします。

   ![PDP ルールでの Analytics 変数の設定](images/analytics-PDPsetVariables.png)

1. 「アクション」の下の![プラスアイコン](images/icon-plus.png)をクリックして、新しいアクションを追加します。

   ![製品文字列への別のアクションの追加](images/analytics-PDPaddProductStringAction.png)

1. **[!UICONTROL 拡張機能／Adobe Analytics 製品文字列]**&#x200B;を選択します。
1. **[!UICONTROL アクションタイプ／s.products を設定]**&#x200B;を選択します。

1. 「**[!UICONTROL Analytics E コマースイベント]**」セクションで、**[!UICONTROL prodView]** を選択します。

1. 「**[!UICONTROL 製品データのデータレイヤー変数]**」セクションで、データ要素ピッカーを使用して `Product Id` データ要素を選択します。

1. 「**[!UICONTROL 変更を保存]**」をクリックします。

   ![Adobe Analytics 製品文字列拡張機能を使用した製品文字列変数の追加](images/analytics-PDPaddProductString.png)

1. **[!UICONTROL ライブラリに保存してビルド]**&#x200B;をクリックします。

   ![ルールの保存](images/analytics-PDP-saveRule.png)

### 製品の詳細ページデータの検証

ビーコンの送信前に変数を設定するルールを作成しました。これで、Experience Cloud デバッガーでヒット内のデータを表示できるようになります。

**製品詳細ページデータを検証するには、以下を実行します。**

1. Chrome ブラウザーで [Luma サイト](https://luma.enablementadobe.com/content/luma/us/en.html)を開きます。
1. 製品の詳細ページに移動します。
1. デバッガーアイコン ![Experience Cloud デバッガーを開く](images/analytics-debuggerIcon.png)をクリックして、**[!UICONTROL Adobe Experience Cloud デバッガー]**&#x200B;を開きます。
1. 「Analytics」タブをクリックします。
1. レポートスイートを展開します。
1. 製品詳細変数がデバッガーに表示されています。`eVar1` は「製品詳細ページ」に設定されており、`Events` 変数は「event1」および「prodView」に設定されています。製品変数は表示している製品の製品 ID とともに設定されています。ページ名は引き続き Analytics 拡張機能によって設定されています。

   ![ページヒットの検証](images/analytics-validatePDPvars.png)

## トラックリンクビーコンの送信

ページが読み込まれると、通常、`s.t()` 関数によってトリガーされたページ読み込みビーコンが実行されます。これにより、`pageName` 変数に表示されるページの `page view` 指標が自動的に増分されます。

しかし、実行するアクションがページビューよりも小さい、またはページビューとは異なるという理由で、サイトのページビュー数を増加させたくない場合があります。この場合は、`s.tl()` 関数（一般的にトラックリンクリクエストと呼ばれます）を使用します。トラックリンクリクエストと呼ばれていますが、必ずしもリンクのクリックでトリガーする訳ではありません。独自のカスタム JavaScript を含め、Launch ルールビルダーで使用できる&#x200B;*任意の*&#x200B;イベントでトリガーされます。

このチュートリアルでは、最も優れた JavaScript イベントの 1 つである `s.tl()` イベントを使用して `Enters Viewport` の呼び出しをトリガーします。

### ユースケース

この使用例では、ユーザーが Luma のホームページを下にスクロールして、ページの「*新着*」セクションを表示できるかどうかを知る必要があります。ユーザーがそのセクションを閲覧できるかどうかに関して、社内で不和があるため、Analytics を使用して真実を判断する必要があります。

### Launch でのルールの作成

1. 上部のナビゲーションの「**[!UICONTROL ルール]**」セクションに移動して&#x200B;**[!UICONTROL ルールの追加]**をクリックします。
   ![ルールの追加](images/target-addRule.png)
1. ルール名を設定します。`Homepage - New Arrivals enters Viewport`
1. **[!UICONTROL イベント／追加]**&#x200B;をクリックして、`Event Configuration` 画面を開きます。

   ![新着ルールの追加](images/analytics-newArrivalsRuleAdd2.png)

1. **[!UICONTROL イベントタイプ／ビューポートに入る]**&#x200B;を選択します。表示されたフィールドに CSS セレクターを入力します。このセレクターは、ブラウザーにルールが表示されたときにルールをトリガーする必要のあるページ上の項目を識別します。
1. Luma のホームページに戻り、「新着」セクションまでスクロールします。
1. 「新着」タイトルとこのセクションの項目の間のスペースを右クリックし、右クリックメニューから `Inspect` を選択します。探しているものはすぐ近くにあります。
1. おそらく選択したセクションのすぐ下に、`class="we-productgrid aem-GridColumn aem-GridColumn--default--12"`を含む div があるはずです。この要素を見つけます。
1. 要素を右クリックして、**[!UICONTROL コピー／コピーセレクター]**&#x200B;を選択します。

   ![Enters ビューポートイベントの設定](images/analytics-copyElementSelector.png)

1. Launch に戻り、この値をクリップボードから「`Elements matching the CSS selector`」とラベリングされたフィールドに貼り付けます。
   1. CSS セレクターの識別方法はユーザーが決定します。この方法は、ページ上で特定の変更をおこなうと、このセレクターが壊れる場合があるので、少し脆弱です。Launch で任意の CSS セレクターを使用する場合は、この点を考慮してください。
1. 「**[!UICONTROL 変更を保存]**」をクリックします。
   ![Enters ビューポートイベントの設定](images/analytics-configEntersViewportEvent.png)

1. 「条件」で、![プラスアイコンをクリック](images/icon-plus.png)をクリックして、新しい条件を追加します。
1. **[!UICONTROL 条件タイプ／値の比較]**&#x200B;を選択します。
1. データ要素ピッカーを使用して、最初のフィールドで「`Page Name`」を選択します。
1. 比較演算子ドロップダウンで「**[!UICONTROL 等しい]**」を選択します。
1. 次のフィールドで、`content:we-retail:us:en` と入力します（これは、データレイヤーから取り込んだホームページのページ名です。このルールは、ホームページ上でのみ実行する必要があります）。
1. 「**[!UICONTROL 変更を保存]**」をクリックします。

   ![ホームページでの条件の設定](images/analytics-configHomepageCondition.png)

1. 「アクション」の下の![プラスアイコン](images/icon-plus.png)をクリックして、新しいアクションを追加します。
1. **[!UICONTROL 拡張機能／Adobe Analytics]** を選択します。
1. **[!UICONTROL アクションタイプ／変数の設定]**&#x200B;を選択します。
1. `eVar3` を `Home Page - New Arrivals` に設定します。
1. `prop3` を `Home Page - New Arrivals` に設定します。
1. `Events` 変数を `event3` に設定します。
1. 「**[!UICONTROL 変更を保存]**」をクリックします。

   ![Enters ビューポートイベントの設定](images/analytics-configViewportAction.png)

1. 「アクション」の下の![プラスアイコン](images/icon-plus.png)をクリックして、新しいアクションを追加します。

   ![ビーコン送信アクションの追加](images/analytics-newArrivalsSendBeacon2.png)

1. **[!UICONTROL 拡張機能／Adobe Analytics]** を選択します。
1. **[!UICONTROL アクションタイプ／ビーコンを送信]**&#x200B;を選択します。
1. **[!UICONTROL s.tl()]** トラッキングオプションを選択します。
1. **[!UICONTROL リンク名]**&#x200B;フィールドに、「`Scrolled down to New Arrivals`」と入力します。この値は、Analytics のカスタムリンクレポートに配置されます。
1. 「**[!UICONTROL 変更を保存]**」をクリックします。

   ![新着ビーコンの設定](images/analytics-configEntersViewportBeacon.png)

1. **[!UICONTROL ライブラリに保存してビルド]**&#x200B;をクリックします。

   ![ルールを保存してビルドする](images/analytics-saveCustomLinkRule.png)

### トラックリンクビーコンの検証

次に、サイトのホームページの「新着」セクションまでスクロールする場合に、このヒットが確実に含まれるようにします。最初にホームページを読み込んだときには、リクエストは実行されませんが、下にスクロールしてセクションが表示されると、新しい値でヒットが起動します。

1. Chrome ブラウザーで [Lumaサイト](https://luma.enablementadobe.com/content/luma/us/en.html)を開き、ホームページの先頭に来ていることを確認します。
1. **[!UICONTROL デバッガーアイコン]**&#x200B;をクリックして Experience Cloud デバッガー![を開き](images/analytics-debuggerIcon.png)、[!UICONTROL Adobe Experience Cloud デバッガー]を開きます。
1. 「Analytics」タブをクリックします。
1. レポートスイートのヒットを展開します。
1. ホームページの通常のページビューヒットにページ名などが付いています。（ただし、eVar3 や prop3 には何も含まれません）。

   ![デバッガーのページビュー](images/analytics-debuggerPageView.png)

1. デバッガーを開いたら、「新着」セクションが表示されるまで、サイトを下にスクロールします。
1. デバッガーを再度表示すると、別の Analytics ヒットが表示されるはずです。このヒットには、設定した s.tl() ヒットに関連付けられたパラメーターが含まれている必要があります。すなわち、
   1. `LinkType = "link_o"` となります（これは、ヒットは、ページビューヒットではなく、カスタムリンクヒットであることを意味します）。
   1. `LinkName = "Scrolled down to New Arrivals"`
   1. `prop3 = "Home Page - New Arrivals"`
   1. `eVar3 = "Home Page - New Arrivals"`
   1. `Events = "event3"`

      ![デバッガーのページビュー](images/analytics-debuggerEntersViewport.png)

## プラグインの追加

プラグインは、実装することで、製品に組み込まれていない関数を実行できる JavaScript コードです。プラグインは、ユーザー、他のアドビの顧客やパートナー、またはアドビコンサルティングによって作成できます。

プラグインを実装するには、基本的には次の 3 つの手順があります。

1. doPlugins 関数を含める（この関数では、プラグインが参照されます）。
1. プラグインのメイン関数コードを追加する。
1. 関数を呼び出し、変数などを設定するコードを含める。

### Analytics オブジェクトにグローバルでアクセスできるようにする

doPlugins 関数（以下）を追加してプラグインを使用する場合は、チェックボックスをオンにして、Analytics 実装でグローバルに Analytics の「s」オブジェクトを使用できるようにする必要があります。

1. **[!UICONTROL 拡張機能／インストール済み]**&#x200B;に移動します。

1. Adobe Analytics 拡張機能で&#x200B;**[!UICONTROL 設定]**&#x200B;をクリックします。

   ![Analytics の設定](images/analytics-configureExtension.png)

1. **[!UICONTROL ライブラリ管理]**&#x200B;で、「`Make tracker globally accessible`」とラベル付けされたボックスを選択します。ヘルプバブルに表示されるように、トラッカーは、window.s の下でグローバルにスコープされるので、顧客の JavaScript で参照する際に重要です。

### doPlugins 関数を含める

プラグインを追加するには、doPlugins と呼ばれる関数を追加する必要があります。この関数は、デフォルトでは追加されませんが、追加されると AppMeasurement ライブラリによって処理され、Adobe Analytics にヒットが送信される際に最後に呼び出されます。したがって、この関数を使用すると、JavaScript を実行して、この方法を設定するよりも簡単に変数を設定できます。

1. Analytics 拡張機能を使用している間に、下にスクロールして「`Configure Tracker Using Custom Code.`」セクションを展開します。
1. **[!UICONTROL エディターを開く]**&#x200B;をクリックします。
1. コードエディターに次のコードを貼り付けます。

   ```javascript
   /* Plugin Config */
   s.usePlugins=true
   s.doPlugins=function(s) {
   /* Add calls to plugins here */
   }
   ```

1. 次の手順で使用するので、このウィンドウを開いておきます。

### プラグインに関数コードを追加します。

このコードでは 2 つのプラグインを呼び出しますが、そのうちの 1 つは AppMeasurement ライブラリに組み込まれているので、呼び出す関数を追加する必要はありません。ただし、2 番目の関数については、関数コードを追加する必要があります。この関数は、getValOnce() と呼ばれます。

### getValOnce() プラグイン

このプラグインの目的は、訪問者がページを更新したとき、またはブラウザーの戻るボタンを使用して値が設定されたページに戻ったときに、コード内で誤って値が複製されるのを防ぐことです。このレッスンでは、`clickthrough` イベントの重複を防ぐためにこれを使用します。

このプラグインのコードは [Analytics のドキュメント](https://docs.adobe.com/content/help/ja-JP/analytics/implementation/javascript-implementation/plugins/getvalonce.html)で入手できますが、簡単にコピーして貼り付けられるよう、ここに含まれています。

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

1. Analytics 拡張機能のコードウィンドウ（まだ開いていない場合は、前の手順に従って再度開きます）にペーストします。doPlugins 関数の下に（関数の内部ではなく）**完全に**&#x200B;にペーストします。

   ![プラグインコードの追加](images/analytics-doPluginsAndGeValOnceCode.png)

これで、このプラグインを doPlugins 内から呼び出せるようになりました。

### doPlugins 内からのプラグインの呼び出し

これでコードが配置され、参照できるようになったので、doPlugins 関数内でプラグインを呼び出すことができます。

まず、AppMeasurement ライブラリに組み込まれているプラグイン（ユーティリティ）を呼び出します。これは、`s.Util.getQueryParam` と呼ばれ、s オブジェクトの一部であるため、ビルトインユーティリティであり、URL のクエリー文字列のパラメーターに基づいて値を取得します。

1. 次のコードをコピーします。

   ```javascript
   s.campaign = s.Util.getQueryParam("cid");
   ```

1. doPlugins 関数に貼り付けます。これにより、現在のページ URL で `cid` というパラメーターを探し、s.campaign 変数に配置します。
1. 次のコードをコピーして、getQueryParam への呼び出しの下に張り付け、getValOnce 関数を呼び出します。

   ```javascript
   s.campaign=s.getValOnce(s.campaign,'s_cmp',30);
   ```

   このコードを使用すると、同じ値が連続した 30 日間に 2 回以上送信されないようにすることができます（必要に応じてこのコードをカスタマイズする方法については、ドキュメントを参照してください）。

   ![doPlugins でのプラグインの呼び出し](images/analytics-doPluginsWithPlugins.png)

1. コードウィンドウを保存します。
1. **[!UICONTROL ライブラリに保存してビルド]**&#x200B;をクリックします。

   ![doPlugins でのプラグインの呼び出し](images/analytics-saveExtensionAndBuild.png)

### プラグインの検証

次に、プラグインが機能していることを確認できます。

**プラグインを検証するには、以下を実行します。**

1. Chrome ブラウザーで [Luma サイト](https://luma.enablementadobe.com/content/luma/us/en.html)を開きます。
1. デバッガーアイコン![Experience Cloud デバッガーを開く](images/analytics-debuggerIcon.png)をクリックして、**[!UICONTROL Adobe Experience Cloud デバッガー]**&#x200B;を開きます。
1. 「Analytics」タブをクリックします。
1. レポートスイートを展開します。
1. Analytics ヒットには Campaign 変数はありません。
1. デバッガーを開いたまま Luma サイトに戻り、`?cid=1234` を URL に追加してから、Enter を押してそのクエリー文字列を含むページを更新します。

   ![クエリー文字列の追加](images/analytics-cidAdded.png)

1. デバッガーを選択し、Campaign 変数が `1234` に設定された 2 回目の Analytics リクエストがあることを確認します。

   ![getQueryParam 手順 1](images/analytics-getQueryParam1.png)

1. クエリー文字列を URL 内に残したまま、戻って Luma ページをもう一度更新します。
1. デバッガーで次のヒットをチェックすると、Campaign 変数が存在&#x200B;**しない**&#x200B;はずです。getValOnce プラグインは、重複がなく、別の人がキャンペーントラッキングコードから来たように見えないように確認しているからです。

   ![getQueryParam 手順 1](images/analytics-getQueryParam2.png)

1. ボーナス：クエリー文字列内の `cid` パラメーターの値を変更することで、これを何度もテストできます。Campaign 変数は、値が含まれるページを **初めて** 実行したときにのみ表示される必要があります。デバッガーに Campaign 値が表示されていない場合は、URL のクエリー文字列にある `cid` の値を変更し、Enter を押して、デバッガーで再度表示します。

   >[!NOTE]実際には、 Analytics 拡張機能の設定など、URL のクエリー文字列からパラメーターを取得する方法はいくつかあります。ただし、これら以外のプラグインオプションでは、getValOnce プラグインを使用してここでおこなった操作と同じように、不要な重複を停止する機能は提供していません。これは著者が気に入っている方法ですが、どの方法がお客様やお客様のニーズに最適かは、お客様が判断する必要があります。

お疲れ様です。Analytics のレッスンが完了しました。もちろん、Analytics の実装を強化するためにできることは他にも多くありますが、このレッスンによって残りのニーズに対処するために必要なコアスキルが得られたなら幸いです。

[次：「Adobe Audience Manager の追加」&gt;](audience-manager.md)
