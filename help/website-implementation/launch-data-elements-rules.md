---
title: データ要素、ルールおよびライブラリの追加
description: 起動でデータ要素、ルールおよびライブラリを作成する方法を説明します。 このレッスンは、「WebサイトにExperience cloudを導入する（起動）」チュートリアルの一部です。
seo-description: null
seo-title: データ要素、ルールおよびライブラリの追加
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: e9dee6d0aa3b775d0ce617e2b2c57b56de491b8d

---


# データ要素、ルールおよびライブラリの追加

このレッスンでは、最初のデータ要素、ルール、ライブラリを作成します。

データ要素とルールは、起動の基本要素です。 データ要素には、マーケティングソリューションと広告ソリューションに送信する属性が格納され、ルールは、適切な条件下でこれらのソリューションに対する要求を実行します。  ライブラリは、すべての作業を行うためにページに読み込まれるJavaScriptファイルです。 このレッスンでは、3つすべてを使用して、サンプルページで何らかの処理を行うようにします。

## 学習目標

このレッスンを最後まで学習すると、以下の内容を習得できます。

* データ要素の作成
* ルールの作成
* ライブラリの作成
* ライブラリへの変更の追加
* ライブラリが Web ブラウザーに読み込まれていることを検証する
* 「作業用ライブラリ」機能を使用して、より効率的に作業を行う

## ページ名のデータ要素の作成

データ要素は、Launchのバージョンのデータレイヤーです。 独自のデータレイヤーオブジェクト、cookie、ローカルストレージオブジェクト、クエリ文字列パラメーター、ページ要素、メタタグなどから値を格納できます。 この演習では、ページ名のデータ要素を作成し、TargetおよびAnalyticsの実装で後から使用します。

**データ要素を作成するには**

1. 上部のナビゲーションで、「データ要素」を **[!UICONTROL クリックします]**

1. このプロパティにまだデータ要素を作成していないので、このトピックに関する詳細情報を含む短いビデオが表示されます。 よろしければこのビデオをご覧ください。

1. Click the **[!UICONTROL Create New Data Element]** button:

   ![「新規データ要素を作成」ボタンをクリックします](images/launch-newDataElement.png)

1. Name the data element, e.g. `Page Name`

1. Use the [!UICONTROL JavaScript Variable] Data Element type to point to a value in your sample page's data layer: `digitalData.page.pageInfo.pageName`

1. Use "`not available`" as the [!UICONTROL Default Value]. The [!UICONTROL Default Value] tells Launch what value to use for the data element if your JavaScript Variable specified above is not found.

1. Check the boxes for **[!UICONTROL Force lowercase value]** and **[!UICONTROL Clean text]** to standardize the case and remove extraneous spaces

1. 「な **[!UICONTROL し」は]** 「 **[!UICONTROL Storage Duration]** 」設定のままにします。これは、通常、この値が各ページで異なるためです

1. 「 **[!UICONTROL Save]** 」ボタンをクリックして、データ要素を保存します

   ![ページ名データ要素を作成します](images/launch-dataElement.png)。

>[!NOTE]**** DTM Migrators:DTMに存在しない新しいデータ要素タイプが「起動」に追加されました。 新しいデータ要素のタイプには、ローカルストレージ、セッションストレージ、ページ情報、乱数などがあります
<!-- -->
>[!NOTE]データ要素の機能 _は、拡張機能を使用して拡張できます_。 例えば、ContextHub拡張機能を使用すると、拡張機能の機能を使用してデータ要素を追加できます。

## ルールの作成

次に、このデータ要素を単純なルールで使用します。 ルールは、起動で最も強力な機能の1つで、訪問者がWebサイトとやり取りする際の動作を指定できます。 ルールで概要が示されている条件が満たされると、指定したアクションがルールによってトリガーされます。

次に、ページ名データ要素の値をブラウザーコンソールに出力するルールを作成します。

**ルールを作成するには**

1. In the top navigation, click **[!UICONTROL Rules]**

1. このプロパティにはまだルールを作成していないので、トピックに関する追加情報を含む短いビデオが表示されます。 よろしければこのビデオをご覧ください。

1. Click the **[!UICONTROL Create New Rule]** button:

   ![「新規ルールを作成」ボタンをクリックします](images/launch-newRule.png)

1. Name the Rule `All Pages - Library Loaded`. この命名規則は、ルールが起動する場所とタイミングを示し、Launchプロパティの成熟度に合わせて識別し、再利用しやすくします。

1. 「イベント」で、「**[!UICONTROL 追加]**」をクリックします。このイベントは、ルールがいつ起動するかを「起動」に知らせ、ページ読み込み、クリック、カスタムJavaScriptイベントなど、様々な要素を含めます。

   ![ルールに名前を付けてイベントを追加する](images/launch-addEventToRule.png)

   1. 「イベントタイプ」として、「**[!UICONTROL 読み込まれたライブラリ（ページ上部）]**」を選択します。「Event Type」を選択すると、「Launch」は選択した内容を使用してイベントの名前を事前設定します。 また、イベントのデフォルトの順序は50です。 順序付けは、起動の強力な機能で、同じイベントでトリガーされる複数のルールがある場合に、アクションの順序を正確に制御できます。 この機能は、チュートリアルの後半で使用します。

   1. 「変更を保持」ボ **[!UICONTROL タンをクリックします]** 。
   ![イベントの選択](images/launch-ruleSelectEvent.png)

1. このルールはすべてのページで実行されるので、「条件」は空白の **[!UICONTROL まま]** 。 条件モーダルを開くと、URL、データ要素の値、日付範囲など、幅広いオプションに基づき、条件で制限と除外の両方を追加できます。

1. Under Actions, click **[!UICONTROL Add]**

1. アクシ **[!UICONTROL ョンタイプ/カスタムコード]**（この時点でのみ選択可能）を選択します。 チュートリアルの後半で、拡張機能を追加すると、より多くのオプションが利用できるようになります。

1. 「 **[!UICONTROL &lt;/&gt; Open Editor]** 」を選択して、コードエディターを開きます。

   ![アクションの選択](images/launch-selectAction.png)

1. コードエディターに次を追加します。 次のコードは、ページ名データ要素の値をブラウザーコンソールに出力し、機能していることを確認できます。

   ```javascript
   console.log('The page name is '+_satellite.getVar('Page Name'));
   ```

1. コードエディターの保存

   ![カスタムコードの入力](images/launch-customCodeAction.png)

1. On the Action configuration screen click **[!UICONTROL Keep Changes]**

1. Click **[!UICONTROL Save]** to save the rule

>[!NOTE]**** DTM Migrators:「起動」では、ほとんどのマーケティングピクセルを起動するためにルールが必要です。 例えば、Adobe Analyticsビーコンを起動するには、ルールを使用してLaunchに指示する必要があります。
>
> ルールビルダーのデザインが大幅に変更され、起動時に再構築されました。
> 主な変更点は次のとおりです。
>
> * ルールビルダーは 1 つだけです。「ページの下部」、「クリック」、「ダイレクト型」などのDTMルールタイプは、すべてルールビルダーのイベントタイプです。 これにより、トリガーを DOM Ready イベントからカスタムイベントへと変更する必要があるので、ルールを更新しやすくなります。
> * 新しい「カスタムコード」イベントタイプ
> * 拡張機能では、新しいイベントタイプをルールビルダーに追加できます。例えば、Target 拡張機能により最終的に [at.js カスタムイベント](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/functions-overview/atjs-custom-events.html) のビルトインサポートが追加されるので、この機能を使用するのにカスタムコードは必要ありません。
> * 拡張機能を使用すると、ルールビルダーに新しいアクションを追加でき、カスタムコードを使用しない方が問題を減らすことができます。 このチュートリアルでは、これらの拡張機能アクションの多くを使用します。
> * ほとんどのマーケティングツールに関連するリクエストを実行するには、ルールが必要です。これには、特に顧客IDの設定、Analyticsビーコンの実行、Targetリクエストの実行などの場合に、マインドセットの調整が必要です。


## ライブラリへの変更の保存

起動インターフェイスで拡張機能、データ要素およびルールのコレクションを設定した後、訪問者がサイトに来訪したときにマーケティングタグが実行されるように、これらの機能とロジックをWebサイトにデプロイできるJavaScriptコードのセットにパッケージ化する必要があります。 ライブラリとは、これを行うJavaScriptコードのセットです。

以前のレッスンでは、開発環境の埋め込みコードをサンプルページに実装しました。サンプルページを読み込むと、起動ライブラリがまだ構築されておらず、環境に割り当てられていないので、埋め込みコードURLに対して404エラーが返されました。 次に、新しいデータ要素とルールをライブラリに配置し、サンプルページで何かを行えるようにします。

**ライブラリを追加して構築するには**

1. Go to the [!UICONTROL Publishing] tab

1. Click **[!UICONTROL Add New Library]**

   ![新しいライブラリの追加](images/launch-addNewLibrary.png)

1. ライブラリに「初期設定」という名前を付けます。

1. 「 **[!UICONTROL Environment」&gt;「Development」を選択します。]**

1. Click **[!UICONTROL Add All Changed Resources]**

   ![変更されたすべてのリソースを追加](images/launch-addAllChangedResources.png)

1. 「変更されたすべてのリソ **[!UICONTROL ースの起動を追加」をクリックした後に]** 、行った変更の概要が表示されます。

1. Click **[!UICONTROL Save &amp; Build for Development]**

   ![開発用に保存およびビルド](images/launch-saveAndBuild.png)

しばらくすると、ステータスドットが緑色に変わり、ライブラリが正常に構築されたことを示します。

![ライブラリ構築](images/launch-libraryBuilt.png)

## 作業の検証

次に、ルールが期待どおりに動作していることを検証します。

サンプルページを再度読み込みます。開発者ツール/「ネットワーク」タブを見ると、起動ライブラリに対する200回の応答が表示されます。

![応答が200のライブラリ読み込み](images/samplepage-200.png)

Developer Tools -&gt; Consoleを見ると、「The page name is home」というテキストが表示されます。

![コンソールメッセージ](images/samplepage-console.png)

初めてのデータ要素とルールを作成し、初回起動ライブラリを構築しました。

## 作業用ライブラリ機能の使用

「起動」で多くの変更を行う場合、結果を表示するたびに「発行」タブに移動し、変更を追加し、ライブラリを構築する必要があるのは不便です。  「初期セットアップ」ライブラリを作成したら、「作業用ライブラリ」と呼ばれる機能を使用して、変更をすばやく保存し、ライブラリを1回の手順で再構築できます。

「すべてのページ — ライブラリ読み込み済み」のルールを少し変更します。 In the top navigation, click **[!UICONTROL Rules]** and then click on the `All Pages - Library Loaded` rule to open it up.

![ルールを再度開く](images/launch-reopenRule.png)

ページで、「作 `Edit Rule` 業用ライブラリ」ド ***[!UICONTROL ロップダウンをクリックし]*** 、ライブラリを選択 `Initial Setup` します。

![作業ライブラリとして初期設定を選択](images/launch-setWorkingLibrary.png)

ライブラリを選択すると、「 **[!UICONTROL Save]** 」ボタンが「Save to Library and Build」にデフォルト **[!UICONTROL 設定されます]**。 「起動」で変更を行う場合、このオプションを使用して、変更を作業ライブラリに直接追加し、再構築することができます。

テストします。 「カスタムコード」アクションを開き、「ページ名は次のとおりです」というテキストの後にコロンを追加すると、コードブロック全体が次のように表示されます。

```javascript
console.log('The page name is: '+_satellite.getVar('Page Name'));
```

コードを保存し、アクションの変更を保持したまま、「ライブラリとビルドに保存 **[!UICONTROL 」ボタンをクリックします]** 。

![「保存とビルド」オプションが存在するようになりました](images/launch-workingLibrary-saveAndBuild.png)

[作業ライブラリ]ドロップダウンの横に緑色の点が再び表示されるま [!UICONTROL でしばらく待ち] ます。 次に、サンプルページを再読み込みし、変更がコンソールメッセージに反映されていることを確認します（ページの変更を確認するには、ブラウザーのキャッシュをクリアして再読み込みする必要がある場合があります）。

![コロン付きコンソールメッセージ](images/samplepage-consoleWithColon.png)

これは、作業がはるかに高速になり、この方法をチュートリアルの残りの部分に使用します。

[次の「Experience cloudデバッガーを使用した環境の切り替え」&gt;](launch-switch-environments.md)
