---
title: 起動プロパティの発行
description: 開発環境からステージングおよび実稼働環境に起動プロパティを発行する方法について説明します。 このレッスンは、「起動を使用するモバイルAndroidアプリケーションにExperience cloudを実装する」チュートリアルの一部です。
seo-description: null
seo-title: 起動プロパティの発行
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# 起動プロパティの発行

これで、Adobe Experience cloudの主要なソリューションが開発環境に実装されたので、投稿ワークフローを学ぶべきです。

## 前提条件

このレッスンを完了するには、起動ユーザーアカウントで「承認」と「公開」を行う権限が必要です。 ユーザーインターフェイスオプションが使用できないためにこれらの手順を完了できない場合は、Experience cloud管理者にアクセス権を問い合わせてください。 For more information on Launch permissions, see [the documentation](https://docs.adobe.com/content/help/en/launch/using/reference/admin/user-permissions.html).

## 学習目標

このレッスンを最後まで学習すると、以下の内容を習得できます。

1. ステージング環境へ開発ライブラリをパブリッシュする
1. 異なる起動環境を読み込むようにアプリを更新する
1. ステージングライブラリを実稼働環境にパブリッシュする

## ステージングへのパブリッシュ

これで、開発環境でライブラリを作成し、検証したので、ステージングに発行する時間です。

1. Go to the **[!UICONTROL Publishing]** page

1. ライブラリの横にあるドロップダウンを開き、「承認用に送信」 **[!UICONTROL を選択します。]**

   ![Submit for Approval](images/mobile-publishing-submitForApproval.png)

1. ダイアログで **[!UICONTROL 「送信]** 」ボタンをクリックします。

   ![モーダルの「送信」をクリックします。](images/mobile-publishing-submit.png)

1. ライブラリが「送信済み  」列に未構築状態で表示されます。

1. ドロップダウンを開き、「 **[!UICONTROL Build for Staging]**:

   ![Build for Staging](images/mobile-publishing-buildForStaging.png)
1. 緑のドットアイコンが表示されると、ステージング環境でライブラリをプレビューできます。

実際のシナリオでは通常、プロセスの次のステップとして、ステージングライブラリの変更を QAチームに検証してもらいます。

**ステージング用ライブラリの変更を検証するには**

1. Launchプロパティで、 [!UICONTROL Environmentsページを開きます] 。

1. 「 [!UICONTROL Staging] 」行で、「Install」アイコンをクリックします。

   ![モーダルを開くため](images/mobile-launch-installIcon.png) 、インストールアイコン

![環境ページに移動し、をクリックしてモーダルを開きます](images/android/mobile-publishing-getStagingCode.png)

この時点で、開発環境とステージング環境でのインストール手順の違いは、上のスクリーンショットで示した初期化コードの起動リファレンスだけです。   必要なのは、DemoApplicationファイルの対応する行を更新して、アプリを再構築することだけです。 ステージングアプリに別のプロジェクトを使用する場合は、このチュートリアル全体で行った他のアプリの更新をすべてこのプロジェクトに含める必要があります。

実際には、QAチームがステージング環境の変更を確認してサインオフしたら、実稼働環境に発行する時間です。

## 実稼動環境へのパブリッシュ

1. Go to the [!UICONTROL Publishing] page

1. ドロップダウンで[投稿の承 **[!UICONTROL 認]をクリックします]**。

   ![Approve for Publishing](images/mobile-publishing-approveForPublishing.png)

1. ダイアログボッ **[!UICONTROL クスで]** 「承認」ボタンをクリックします。

   ![「承認」をクリックします](images/mobile-publishing-approve.png)

1. ライブラリが「 [!UICONTROL Approved] 」列の未構築状態（黄色の点）で表示されます。

1. ドロップダウンを開き、「ビル **[!UICONTROL ドして実稼働環境に発行]**」を選択します。

   ![「ビルドして実稼働環境に公開」をクリックします](images/mobile-publishing-buildAndPublishToProduction.png)

1. ダイアログボッ **[!UICONTROL クスで]** 「公開」をクリックします。

   ![「公開」をクリックします](images/mobile-publishing-publish.png)

1. ライブラリが「発行済み  」列に表示されます。

   ![Published](images/mobile-publishing-published.png)

実稼働環境では、下のスクリーンショットでハイライト表示されているように、コア設定で起動参照が使用されています。  実稼働版アプリに異なるプロジェクトを使用する場合は、このチュートリアル全体で行ったすべてのアプリの更新をこのプロジェクトに含める必要があります。

![環境ページに移動し、をクリックしてモーダルを開きます](images/android/mobile-publishing-getProductionCode.png)

>[!IMPORTANT] 次回起動設定を変更する際は、開発環境に新しいライブラリを作成する必要があります。 拡張機能の追加と削除を行う場合は、アプリケーション自体を更新する必要があることに注意してください。 問題を回避するため、Launch環境とアプリコードの同期を維持するように注意してください。

それだ！ このチュートリアルが完了し、Launch！の最初のモバイルプロパティが公開されました。