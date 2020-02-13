---
title: Launch プロパティのパブリッシュ
description: 開発環境からステージングおよび実稼動環境に Launch プロパティをパブリッシュする方法について説明します。このレッスンは、「Launch を使用したモバイル iOS Objective-C アプリケーションでの Experience Cloud の実装」チュートリアルの一部です。
seo-description: null
seo-title: Launch プロパティのパブリッシュ
solution: Experience Cloud
translation-type: ht
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# Launch プロパティのパブリッシュ

開発環境で Adobe Experience Cloud の主要ソリューションをいくつか実装したら、パブリッシュのワークフローについて学習しましょう。

## 前提条件

このレッスンを完了するには、Launch ユーザーアカウントで「承認」と「パブリッシュ」をおこなう権限が必要です。ユーザーインターフェイスオプションが使用できないためにこれらの手順を完了できない場合は、Experience Cloud 管理者に連絡してアクセス権をもらってください。Launch の権限について詳しくは、[ドキュメント](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/admin/user-permissions.html)を参照してください。

## 学習内容

このレッスンを最後まで学習すると、以下の内容を習得できます。

1. ステージング環境へ開発ライブラリをパブリッシュする
1. 異なる Launch 環境を読み込むようにアプリケーションを更新する
1. ステージングライブラリを実稼動環境にパブリッシュする

## ステージングへのパブリッシュ

開発環境でライブラリを作成して検証したら、そのライブラリをステージングにパブリッシュする番です。

1. **[!UICONTROL パブリッシング]**&#x200B;ページに移動します。

1. ライブラリの横にあるドロップダウンを開き、**[!UICONTROL 承認用に送信]**&#x200B;を選択します。

   ![承認用に送信](images/mobile-publishing-submitForApproval.png)

1. ダイアログで「**[!UICONTROL 送信]**」ボタンをクリックします。

   ![モーダルの「送信」をクリックする](images/mobile-publishing-submit.png)

1. ライブラリが[!UICONTROL 送信済み]列に未ビルドの状態で表示されます。

1. ドロップダウンを開き、**[!UICONTROL ステージング用にビルド]**&#x200B;を選択します。

   ![ステージング用にビルド](images/mobile-publishing-buildForStaging.png)
1. 緑のドットアイコンが表示されると、ステージング環境でライブラリをプレビューできます。

実際のシナリオでは通常、プロセスの次のステップとして、ステージングライブラリの変更を QA チームに検証してもらいます。

**ステージングライブラリで変更を検証するには、以下を実行します。**

1. Launch プロパティで、[!UICONTROL 環境]ページを開きます。

1. [!UICONTROL ステージング]行で、インストールアイコン

   ![インストールアイコン](images/mobile-launch-installIcon.png) をクリックしてモーダルを開きます。
   ![環境ページに移動し、クリックしてモーダルを開く](images/ios/objective-c/mobile-publishing-getStagingCode.png)

ステージングアプリケーションに別のワークスペースを使用する場合は、このチュートリアル全体でおこなったポッドおよびアプリケーションの更新をすべてこのワークスペースに含める必要があります。この時点で、開発環境とのインストール手順の違いは、上のスクリーンショットでハイライト表示された Launch へのリファレンスだけです。AppDelegate.h ファイルの対応する行を更新して、アプリケーションを再構築する必要があります。

実際には、QA チームがステージング環境の変更を確認してサインオフしたら、実稼動環境にパブリッシュします。

## 実稼動環境へのパブリッシュ

1. [!UICONTROL パブリッシング]ページに移動します。

1. ドロップダウンで&#x200B;**[!UICONTROL パブリッシュの承認]**&#x200B;をクリックします。

   ![パブリッシュの承認](images/mobile-publishing-approveForPublishing.png)

1. ダイアログボックスで&#x200B;**[!UICONTROL 承認]**&#x200B;ボタンをクリックします。

   ![「承認」をクリックする](images/mobile-publishing-approve.png)

1. ライブラリが[!UICONTROL 承認済み]列に未ビルド（黄色の点）として表示されます。

1. ドロップダウンを開き、**[!UICONTROL ビルドして実稼動環境にパブリッシュ]**&#x200B;を選択します。

   ![「ビルドして実稼動環境にパブリッシュ」をクリックする](images/mobile-publishing-buildAndPublishToProduction.png)

1. ダイアログボックスで&#x200B;**[!UICONTROL パブリッシュ]**&#x200B;をクリックします。

   ![「パブリッシュ」をクリックする](images/mobile-publishing-publish.png)

1. ライブラリが[!UICONTROL パブリッシュ済み]列に表示されます。

   ![パブリッシュ済み](images/mobile-publishing-published.png)

実稼動環境では、下のスクリーンショットでハイライト表示されているように、コア設定で Launch 参照が使用されています。ステージングアプリケーションに別のワークスペースを使用する場合は、このチュートリアル全体でおこなったポッドおよびアプリケーションの更新をすべてこのワークスペースに含める必要があります。![環境ページに移動し、クリックしてモーダルを開く](images/ios/objective-c/mobile-publishing-getProductionCode.png)

>[!WARNING] 次回 Launch 設定を変更する際は、開発環境に新しいライブラリを作成する必要があります。拡張機能の追加と削除をおこなう場合は、アプリケーション自体を更新する必要があることに注意してください。問題を回避するため、Launch 環境とアプリコードの同期を維持するように注意してください。

これで全部です。チュートリアルを完了し、Launch で最初のモバイルプロパティをパブリッシュしました。