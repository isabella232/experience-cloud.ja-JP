---
title: Adobe Experience Platform Identity Serviceの起動での実装
description: Adobe Experience Platform Identity Service拡張を追加し、「顧客IDを設定」アクションを使用して顧客IDを収集する方法について説明します。 このレッスンは、「WebサイトにExperience cloudを導入する（起動）」チュートリアルの一部です。
seo-description: null
seo-title: Adobe Experience Platform Identity Serviceの起動での実装
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# Adobe Experience Platform IDサービスの追加

このレッスンでは、 [Adobe Experience Platform Identity Service Extensionの実装と顧客IDの送信に必要な手順を](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/id-service-extension/overview.html) 、手順を説明します。

Adobe Experience Platform IDサービス [は](https://docs.adobe.com/content/help/en/id-service/using/home.html) 、ソリューション間でのオーディエンス共有などのExperience cloud機能を強化するために、すべてのアドビソリューションに共通の訪問者IDを設定します。  独自の顧客IDをサービスに送信して、デバイス間のターゲット設定や顧客関係管理(CRM)システムとの統合を有効にすることもできます。

## 学習目標

このレッスンを最後まで学習すると、以下の内容を習得できます。

* IDサービス拡張の追加
* 顧客IDを収集するデータ要素を作成します
* 「顧客IDを設定」アクションを使用して顧客IDをアドビに送信するルールを作成します
* ルールの順序機能を使用して、同じイベントで実行されるルールの順序を設定する

## 前提条件

You should have already completed the lessons in the [Configure Launch](launch.md) section.

## IDサービス拡張の追加

これは最初に追加する拡張機能なので、次に拡張機能の概要を示します。 拡張機能は、Launchの中核機能の1つです。 拡張機能とは、アドビ、アドビパートナーまたはアドビのお客様が構築した統合で、Webサイトに導入できるタグの新しい無限のオプションを追加します。 「起動」をオペレーティングシステムと考える場合、拡張機能とは、Launchが必要な操作を行えるようにインストールしたアプリケーションのことです。

**IDサービス拡張を追加するには**

1. In the top navigation, click **[!UICONTROL Extensions]**

1. 「カタロ **[!UICONTROL グ」をクリックし]** 、拡張機能カタログページに移動します。

   ![拡張機能カタログに移動](images/extensions-goToExtensionsCatalog.png)

1. カタログに含まれる様々な拡張子に注意してください。

1. 上部のフィルターで、「id」と入力してカタログをフィルターします

1. Adobe Experience Platform IDサービスのカード上で、「インストール」をクリックし **[!UICONTROL ます]**

   ![IDサービス拡張のインストール](images/idservice-install.png)

1. Experience cloud組織IDが自動的に検出されました。

1. すべてのデフォルト設定をそのままにし、「ライブラリに保存」 **[!UICONTROL と「ビルド」をクリックします]**

   ![拡張機能の保存](images/idservice-save.png)

>[!NOTE] IDサービス拡張の各バージョンには、VisitorAPI.jsの特定のバージョンが付属しています。このバージョンは、拡張の説明に記載されています。 VisitorAPI.jsのバージョンを更新するには、Identity Service拡張を更新します。

### 拡張機能の検証

IDサービス拡張は、ルールアクションを使用せずにリクエストを行う、いくつかの起動拡張の1つです。 拡張機能は、Webサイトへの初回訪問の最初のページ読み込み時に、IDサービスに自動的にリクエストを行います。 リクエストされたIDは、「AMCV_」で始まるファーストパーティcookieに保存されます。

**IDサービス拡張を検証するには**

1. Open the [Luma site](https://luma.enablementadobe.com/content/luma/us/en.html)

1. Make sure the Debugger is mapping the Launch property to *your* Development environment, as described in the [earlier lesson](launch-switch-environments.md).

1. デバッガーの「概要」タブで、「起動」セクションにAdobe Experience Platform Identity Service拡張が実装されていることが示されます。

1. また、「概要」タブで、「IDサービス」セクションに、起動インターフェイスの拡張設定画面と同じ組織IDが入力されます。

   ![Adobe Experience Platform Identity Service拡張機能が実装されていることを確認します](images/idservice-debugger-summary.png)

1. 訪問者IDを取得するための最初のリクエストは、デバッガーの「IDサービス」タブに表示される場合があります。 既に要求されていた可能性がありますので、表示されない場合は心配しないでください。
   ![組織IDを持つIDサービスに対するリクエストがあるかどうかを確認します](images/idservice-idRequest.png)

1. 訪問者 IDを取得する最初のリクエストの後、ID は名前が `AMCV_` で始まる cookie に保存されます。次の手順を実行して、cookie が設定されていることを確認します。
   1. ブラウザーの開発者ツールを開く
   1. Go to the `Application` tab
   1. Expand `Cookies` on the left side
   1. Click on the domain `https://luma.enablementadobe.com`
   1. 右側のAMCV_ cookieを探します。 LumaサイトがハードコードされたLaunchプロパティと独自のLaunchプロパティの両方を使用して読み込まれた後に、いくつか表示される場合があります。
      ![AMCV_ cookieの検証](images/idservice-AMCVCookie.png)

これで作業は完了です。最初の拡張子が追加されました。 IDサービスの設定オプションについて詳しくは、ドキュメントを参照 [してください](https://docs.adobe.com/content/help/en/id-service/using/id-service-api/configurations/function-vars.html)。

## 顧客IDの送信

次に、顧客IDをID [サービス](https://docs.adobe.com/content/help/en/id-service/using/reference/authenticated-state.html) に送信します。 This will allow you to [integrate your CRM](https://docs.adobe.com/content/help/en/core-services/interface/customer-attributes/attributes.html) with the Experience Cloud as well as track visitors across devices.

In the earlier lesson, [Add Data Elements, Rules, and Libraries](launch-data-elements-rules.md) you created a data element and used it in a rule. 次に、これらの同じ方法を使用して、訪問者が認証されたときに顧客IDを送信します。

### 顧客 ID のデータ要素を作成する

まず、2 つのデータ要素を作成します。

1. `Authentication State`— 訪問者がログインしているかどうかをキャプチャします。
1. `Email (Hashed)`— ハッシュ化されたバージョンの電子メールアドレス（顧客IDとして使用）をデータレイヤーから取り込む

**認証状態用のデータ要素を作成するには**

1. 上部ナビゲ **[!UICONTROL ーションで]** 「データ要素」をクリックします。
1. 「データ要素 **[!UICONTROL を追加」ボタンをクリック]**

   ![「データ要素を追加」をクリックします。](images/idservice-addDataElement1.png)

1. Name the data element `Authentication State`
1. 「データ要素 **[!UICONTROL のタイプ」で]**、「カスタムコ **[!UICONTROL ード」を選択します]**
1. 「 **[!UICONTROL Open Editor]** 」ボタン

   ![エディターを開き、データ要素のカスタムコードを追加します](images/idservice-authenticationState.png)

1. コード [!UICONTROL の編集ウィンドウで] 、次のコードを使用して、Lumaサイトのデータレイヤーの属性に基づいて「logged in」または「logged out」の値を返します。

   ```javascript
   if (digitalData.user[0].profile[0].attributes.loggedIn)
       return "logged in"
   else
       return "logged out"
   ```

1. Click **[!UICONTROL Save]** to save the custom code

   ![カスタムコードの保存](images/idservice-authenticationCode.png)

1. その他の設定はすべてデフォルト値のまま残します。
1. 「ライブ **[!UICONTROL ラリおよびビルドに保存]** 」をクリックしてデータ要素を保存し、データ要素ページに戻ります

   ![データ要素を保存します](images/idservice-authenticationStateFinalSave.png)

ユーザーの認証状態を知ることで、Idサービスに送信するページに顧客IDがいつ存在する必要があるかを知ることができます。 次の手順は、顧客ID自体のデータ要素を作成することです。 ルミナンスデモサイトでは、訪問者の電子メールアドレスのハッシュ化バージョンを使用します。

**ハッシュ化された電子メールのデータ要素を追加するには**

1. 「データ要素 **[!UICONTROL を追加」ボタンをクリック]**

   ![データ要素を追加する](images/idservice-addDataElement2.png)

1. Name the data element `Email (Hashed)`
1. 「データ要 **[!UICONTROL 素の種類]**」で「 **[!UICONTROL JavaScript変数」を選択します]**
1. JavaScript変数名 **[!UICONTROL として]**、ルミナンスサイトのデータレイヤー内の変数への次のポインターを使用します。 `digitalData.user.0.profile.0.attributes.username`
1. その他の設定はすべてデフォルト値のまま残します。
1. 「ライブ **[!UICONTROL ラリおよびビルドに保存]** 」をクリックして、データ要素を保存します

   ![データ要素を保存します](images/idservice-emailHashed.png)

### 顧客 ID を送信するルールを追加する

Adobe Experience Platform IDサービスは、「顧客IDの設定」と呼ばれるアクションを使用して、顧客IDをルールで渡します。次に、訪問者が認証されたときにこのアクションをトリガーするルールを作成します。

**顧客IDを送信するルールを作成するには**

1. In the top navigation, click **[!UICONTROL Rules]**
1. 「ルール **[!UICONTROL の追加]** 」をクリックして、ルールビルダーを開きます

   ![ルールを追加](images/idservice-addRule.png)

1. Name the rule `All Pages - Library Loaded - Authenticated - 10`

   >[!TIP] この命名規則は、ユーザーが認証され、「10」の順序でこのルールが実行される場合に、すべてのページの先頭でこのルールを実行することを示します。 アクションでトリガーされるソリューションに対して、このような命名規則を使用すると、実装で必要とされるルールの全体的な数を最小限に抑えることができます。

1. Under **[!UICONTROL Events]** click **[!UICONTROL Add]**

   ![イベントの追加](images/idservice-customerId-addEvent.png)

   1. For the **[!UICONTROL Event Type]** select **[!UICONTROL Library Loaded (Page Top)]**
   1. For the  **[!UICONTROL Order]** enter `10`. 「Order」は、同じイベントによってトリガーされるルールのシーケンスを制御します。下位のルールは、上位のルールよりも前に実行されます。 In this case, you want to set the customer ID before you fire the Target request, which you will do in the next lesson with a rule with an order of `50` .
   1. 「変更を保持 **[!UICONTROL 」ボタンをクリックし]** 、ルールビルダーに戻ります
   ![イベントの保存](images/idservice-customerId-saveEvent.png)

1. 「条件」で **[!UICONTROL 「追加]** 」をクリ **[!UICONTROL ックします]**

   ![ルールへの条件の追加](images/idservice-customerId-addCondition.png)

   1. 「条件タイプ」 **[!UICONTROL で「値の比較]** 」を **[!UICONTROL 選択します。]**
   1. Click the ![data element icon](images/icon-dataElement.png) icon to open the Data Element modal

      ![データ要素モーダルを開く](images/idservice-customerId-valueComparison.png)

   1. 「データ要素モーダル」で、「認証状態」をク **[!UICONTROL リックし]** 、「選択」をクリックし **[!UICONTROL ます]**

      ![認証状態の設定](images/idservice-customerId-authStateCondition.png)

1. Make sure `Equals` is the operator
1. テキストフィールドに「logged in」と入力すると、データ要素「Authentication State」の値が「logged in」の場合にルールが起動します

1. Click **[!UICONTROL Keep Changes]**

   ![条件の保存](images/idservice-customerId-loggedIn.png)

1. 「アクショ **[!UICONTROL ン」で]** 「追加」をクリ **[!UICONTROL ックします]**

   ![新しいアクションの追加](images/idservice-customerId-addAction.png)

   1. 拡張機能に **[!UICONTROL は]** 、「 **[!UICONTROL Adobe Experience Platform Identity Service」を選択します。]**
   1. 「アクションタ **[!UICONTROL イプ」で]** 「顧客IDを **[!UICONTROL 設定」を選択します]**
   1. 統合コ **[!UICONTROL ード]** : `crm_id`
   1. 「値」に **[!UICONTROL 対して]** 、データ要素セレクターモーダルを開き、 `Email (Hashed)`
   1. 「 **[!UICONTROL Auth State]** 」で「 **[!UICONTROL Authenticated」を選択します]**
   1. Click the **[!UICONTROL Keep Changes]** button to save the action and return to the Rule Builder

      ![アクションを設定し、変更を保存します](images/idservice-customerId-action.png)

1. 「ライブラリ **[!UICONTROL とビルドに保存」ボタンをクリックし]** 、ルールを保存します

   ![ルールの保存](images/idservice-customerId-saveRule.png)

これで、訪問者が認証されたときに顧客IDを変数として送信するル `crm_id` ールが作成されました。 このルールは、デフォルトの順序値を使用する「データ要素 `10` 、ルール、ライブラリを追加 `All Pages - Library Loaded` 」レッスンで作成したルールの前に実行される順序を指定したので [、このルールが実行されま](launch-data-elements-rules.md)`50`す。

### 顧客IDの検証

作業内容を検証するには、Lumaサイトにログインし、新しいルールの動作を確認します。

**Lumaサイトにログインするには**

1. Open the [Luma site](https://luma.enablementadobe.com/content/luma/us/en.html)

1. Make sure the Debugger is mapping the Launch property to *your* Development environment, as described in the [earlier lesson](launch-switch-environments.md)

   ![デバッガーに表示される起動開発環境](images/switchEnvironments-debuggerOnWeRetail.png)

1. ルミナン **[!UICONTROL スサイトの]** 右上隅にある「LOGIN」リンクをクリックします。

   ![上部のナビゲーションで「ログイン」をクリックします。](images/idservice-loginNav.png)

1. ユーザ `test@adobe.com` ー名として入力
1. パスワ `test` ードとして入力
1. 「 **[!UICONTROL LOGIN]** 」ボタン

   ![資格情報を入力し、「ログイン」をクリックします](images/idservice-login.png)

1. ホームページに戻る

次に、Debugger拡張を使用して、顧客IDがサービスに送信されたことを確認します。

**IDサービスが顧客IDを渡していることを検証するには**

1. ルミナンスサイトのタブにフォーカスがあることを確認します。
1. デバッガーで、「Adobe Experience Platform Identity Service」タブに移動します。
1. 組織IDを展開します。
1. Click on the cell with the `Customer ID - crm_id` value
1. In the modal, note the customer id value and that the `AUTHENTICATED` state is reflected:

   ![デバッガーでの顧客IDの確認](images/idservice-debugger-confirmCustomerId.png)

1. ハッシュ化された電子メール値は、Lumaページのソースコードを表示し、usernameプロパティを確認することで確認できます。 この値は、デバッガーに表示される値と一致する必要があります。

   ![ソースコード内のハッシュ化された電子メール](images/idservice-customerId-inSourceCode.png)

### その他の検証に関するヒント

また、起動には、豊富なコンソールログ機能も備えています。 有効にするには、デバッガーの「ツ **[!UICONTROL ール]** 」タブに移動し、「コンソールログの起動」 **[!UICONTROL 切り替えをオンにします]** 。

![起動のコンソールログの切り替え](images/idservice-debugger-logging.png)

これにより、ブラウザーコンソールとデバッガーの「ログ」タブの両方でコンソールログが有効になります。 これまでに作成したすべてのルールのログが表示されます。 新しいログエントリがリストの先頭に追加されるので、「All Pages - Library Loaded - Authenticated - 10」のルールは、「All Pages - Library Loaded」のルールの前に実行され、デバッガのコンソールログの下に表示されます。

![デバッガーの「ログ」タブ](images/idservice-debugger-loggingStatements.png)

[次の「Adobe targetの追加」&gt;](target.md)
