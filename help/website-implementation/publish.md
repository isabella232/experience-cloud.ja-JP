---
title: Launch プロパティのパブリッシュ
description: 開発環境からステージングおよび実稼動環境に Launch プロパティをパブリッシュする方法について説明します。このレッスンは、「Launch を使用した Web サイトでの Experience Cloud の実装」チュートリアルの一部です。
seo-description: null
seo-title: Launch プロパティのパブリッシュ
solution: Experience Cloud
translation-type: ht
source-git-commit: aacd691c176da6ae5b247a19051de57289c128c5

---


# Launch プロパティのパブリッシュ

開発環境で Adobe Experience Cloud の主要ソリューションをいくつか実装したら、パブリッシュのワークフローについて学習しましょう。

## 学習内容

このレッスンを最後まで学習すると、以下の内容を習得できます。

1. ステージング環境へ開発ライブラリをパブリッシュする
1. デバッガーを使用してステージングライブラリを実稼動 Web サイトにマッピングする
1. ステージングライブラリを実稼動環境にパブリッシュする

## ステージングへのパブリッシュ

開発環境でライブラリを作成して検証したら、そのライブラリをステージングにパブリッシュする番です。

1. **[!UICONTROL パブリッシング]**&#x200B;ページに移動します。

1. ライブラリの横にあるドロップダウンを開き、**[!UICONTROL 承認用に送信]**&#x200B;を選択します。

   ![承認用に送信](images/publishing-submitForApproval.png)

1. ダイアログで「**[!UICONTROL 送信]**」ボタンをクリックします。

   ![モーダルの「送信」をクリックする](images/publishing-submit.png)

1. ライブラリが[!UICONTROL 送信済み]列に未ビルドの状態で表示されます。

1. ドロップダウンを開き、**[!UICONTROL ステージング用にビルド]**&#x200B;を選択します。

   ![ステージング用にビルド](images/publishing-buildForStaging.png)

1. 緑のドットアイコンが表示されると、ステージング環境でライブラリをプレビューできます。

実際のシナリオでは通常、プロセスの次のステップとして、ステージングライブラリの変更を QA チームに検証してもらいます。これをおこなうには、デバッガーを使用します。

**ステージングライブラリで変更を検証するには、以下を実行します。**

1. Launch プロパティで、[!UICONTROL 環境]ページを開きます。

1. [!UICONTROL ステージング]行で、インストールアイコン![インストールアイコン](images/launch-installIcon.png) をクリックして、モーダルを開きます。

   ![環境ページに移動し、クリックしてモーダルを開く](images/publishing-getStagingCode.png)

1. コピーアイコン![コピーアイコン](images/launch-copyIcon.png)をクリックして、埋め込みコードをクリップボードにコピーします。

1. **[!UICONTROL 閉じる]**&#x200B;をクリックしてモーダルを閉じます。

   ![インストールアイコン](images/publishing-copyStagingCode.png)

1. Chrome ブラウザーで [Luma デモサイト](https://luma.enablementadobe.com/content/luma/us/en.html)を開きます。

1. ![デバッガーアイコン](images/icon-debugger.png) アイコンをクリックして、[Experience Cloud デバッガー拡張機能](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)を開きます。

   ![デバッガーアイコンをクリックする](images/switchEnvironments-openDebugger.png)

1. 「ツール」タブに移動します。

1. **[!UICONTROL Adobe Launch／動的に Launch を挿入/コードの埋め込み]**&#x200B;ボタンをクリックして、テキスト入力フィールドを開きます（現在、開発埋め込みコードのURLが含まれている場合があります）。

   ![「Adobe Launch／動的に Launch を挿入／コードの埋め込み」ボタンをクリックする](images/switchEnvironments-debugger-editEmbedCode.png)

1. クリップボードにあるステージング埋め込みコードをペーストします。

1. ディスクアイコンをクリックして保存します。

   ![デバッガーに表示される Launch 環境](images/switchEnvironments-debugger-save.png)

1. デバッガーの「概要」タブをリロードして確認します。「Launch」セクションの下で、ステージングプロパティが実装され、プロパティ名（例：「Launch Tutorial」またはプロパティに指定した名前）が表示されていることを確認します。

   ![デバッガーに表示される Launch 環境](images/publishing-debugger-staging.png)

実際には、QA チームがステージング環境の変更を確認してサインオフしたら、実稼動環境にパブリッシュします。

## 実稼動環境へのパブリッシュ

1. [!UICONTROL パブリッシング]ページに移動します。

1. ドロップダウンで&#x200B;**[!UICONTROL パブリッシュの承認]**&#x200B;をクリックします。

   ![パブリッシュの承認](images/publishing-approveForPublishing.png)

1. ダイアログボックスで&#x200B;**[!UICONTROL 承認]**&#x200B;ボタンをクリックします。

   ![「承認」をクリックする](images/publishing-approve.png)

1. ライブラリが[!UICONTROL 承認済み]列に未ビルド（黄色の点）として表示されます。

1. ドロップダウンを開き、「**[!UICONTROL ビルドして実稼動環境にパブリッシュ]**」を選択します。

   ![「ビルドして実稼動環境にパブリッシュ」をクリックする](images/publishing-buildAndPublishToProduction.png)

1. ダイアログボックスで&#x200B;**[!UICONTROL パブリッシュ]**&#x200B;をクリックします。

   ![「パブリッシュ」をクリックする](images/publishing-publish.png)

1. ライブラリが[!UICONTROL パブリッシュ済み]列に表示されます。

   ![パブリッシュ済み](images/publishing-published.png)

これで全部です。チュートリアルを完了し、Launch で最初のプロパティをパブリッシュしました。