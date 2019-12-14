---
title: 起動プロパティの発行
description: 開発環境からステージングおよび実稼働環境に起動プロパティを発行する方法について説明します。 このレッスンは、「WebサイトにExperience cloudを導入する（起動）」チュートリアルの一部です。
seo-description: null
seo-title: 起動プロパティの発行
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: aacd691c176da6ae5b247a19051de57289c128c5

---


# 起動プロパティの発行

これで、Adobe Experience cloudの主要なソリューションが開発環境に実装されたので、投稿ワークフローを学ぶべきです。

## 学習目標

このレッスンを最後まで学習すると、以下の内容を習得できます。

1. ステージング環境へ開発ライブラリをパブリッシュする
1. デバッガーを使用した実稼働用Webサイトへのステージングライブラリのマッピング
1. ステージングライブラリを実稼働環境にパブリッシュする

## ステージングへのパブリッシュ

これで、開発環境でライブラリを作成し、検証したので、ステージングに発行する時間です。

1. Go to the **[!UICONTROL Publishing]** page

1. ライブラリの横にあるドロップダウンを開き、「承認用に送信」 **[!UICONTROL を選択します。]**

   ![Submit for Approval](images/publishing-submitForApproval.png)

1. ダイアログで **[!UICONTROL 「送信]** 」ボタンをクリックします。

   ![モーダルの「送信」をクリックします。](images/publishing-submit.png)

1. ライブラリが「送信済み  」列に未構築状態で表示されます。

1. ドロップダウンを開き、「 **[!UICONTROL Build for Staging]**:

   ![Build for Staging](images/publishing-buildForStaging.png)

1. 緑のドットアイコンが表示されると、ステージング環境でライブラリをプレビューできます。

実際のシナリオでは通常、プロセスの次のステップとして、ステージングライブラリの変更を QAチームに検証してもらいます。これはデバッガを使用して行うことができます。

**ステージング用ライブラリの変更を検証するには**

1. Launchプロパティで、 [!UICONTROL Environmentsページを開きます] 。

1. 「 [!UICONTROL Staging] 」行で、「Install」アイコン「 ![Install](images/launch-installIcon.png) 」アイコンをクリックして、モーダルを開きます。

   ![環境ページに移動し、をクリックしてモーダルを開きます](images/publishing-getStagingCode.png)

1. コピーアイコンコピーアイコン ![をクリックして](images/launch-copyIcon.png) 、埋め込みコードをクリップボードにコピーします

1. Click **[!UICONTROL Close]** to close the modal

   ![インストールアイコン](images/publishing-copyStagingCode.png)

1. Open the [Luma demo site](https://luma.enablementadobe.com/content/luma/us/en.html) in your Chrome browser

1. デバッガー [アイコンをクリックして](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj) 、 ![Experience Cloud Debugger拡張機能を](images/icon-debugger.png) 開きます。

   ![デバッガーアイコンをクリックします](images/switchEnvironments-openDebugger.png)

1. 「ツール」タブに移動します。

1. Adobe Launch/ **[!UICONTROL Dynamically Insert Launch/Embed Code]** （埋め込みコードを動的に挿入）ボタンをクリックして、テキスト入力フィールドを開きます（現在、開発埋め込みコードのURLが含まれている場合があります）。

   ![Adobe Launch/Dynamically Insert Launch/Embed Codeボタンをクリックします](images/switchEnvironments-debugger-editEmbedCode.png)

1. クリップボードにあるステージング埋め込みコードを貼り付けます

1. ディスクアイコンをクリックして保存します

   ![デバッガーに表示される起動環境](images/switchEnvironments-debugger-save.png)

1. 再読み込みして、デバッガーの「概要」タブを確認します。 「起動」セクションで、ステージングプロパティが実装され、プロパティ名(「Launch Tutorial」など、プロパティ名を指定します。

   ![デバッガーに表示される起動環境](images/publishing-debugger-staging.png)

実際には、QAチームがステージング環境の変更を確認してサインオフしたら、実稼働環境に発行する時間です。

## 実稼動環境へのパブリッシュ

1. Go to the [!UICONTROL Publishing] page

1. ドロップダウンで[投稿の承 **[!UICONTROL 認]をクリックします]**。

   ![Approve for Publishing](images/publishing-approveForPublishing.png)

1. ダイアログボッ **[!UICONTROL クスで]** 「承認」ボタンをクリックします。

   ![「承認」をクリックします](images/publishing-approve.png)

1. ライブラリが「 [!UICONTROL Approved] 」列の未構築状態（黄色の点）で表示されます。

1. ドロップダウンを開き、「 **[!UICONTROL **Build and Publish to Production]**:

   ![「ビルドして実稼働環境に公開」をクリックします](images/publishing-buildAndPublishToProduction.png)

1. ダイアログボッ **[!UICONTROL クスで]** 「公開」をクリックします。

   ![「公開」をクリックします](images/publishing-publish.png)

1. ライブラリが「発行済み  」列に表示されます。

   ![Published](images/publishing-published.png)

それだ！ このチュートリアルが完了し、「起動」の最初のプロパティが公開されました。