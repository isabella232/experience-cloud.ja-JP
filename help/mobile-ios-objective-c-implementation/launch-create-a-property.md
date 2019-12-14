---
title: モバイルアプリ用の起動プロパティの作成
description: 起動インターフェイスにログインし、モバイル起動プロパティを作成する方法を説明します。 このレッスンは、「モバイルiOS Objective-CアプリケーションでのExperience cloudの実装」チュートリアルの一部です。
seo-description: null
seo-title: モバイルアプリ用の起動プロパティの作成
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# 起動プロパティの作成

Adobe Experience Platform Launchは、次世代のモバイルSDKおよびWebサイトタグ管理機能です。 Launchを使用すると、関連する顧客エクスペリエンスを強化するために必要なすべての分析、マーケティング、広告ソリューションをシンプルに導入および管理できます。 起動に追加料金はかかりません。 Adobe Experience cloudのお客様は、これを利用できます。

このレッスンでは、モバイルアプリ用の起動プロパティを作成します。

## 前提条件

次のレッスンを完了するには、起動時に環境の開発、承認、発行、管理を行う権限が必要です。 ユーザーインターフェイスオプションが利用できないためにこれらの手順を完了できない場合は、Experience cloud管理者に連絡して、アクセスをリクエストしてください。 For more information on Launch permissions, see [the documentation](https://docs.adobe.com/content/help/en/launch/using/reference/admin/user-permissions.html).

## 学習目標

このレッスンを最後まで学習すると、以下の内容を習得できます。

* Launchユーザーインターフェイスにログインします。
* 新しい起動モバイルプロパティの作成
* モバイル起動プロパティの設定

## Launch  に移動します。

**起動を開始するには**

1. [Adobe Experience cloudへのログイン](https://experiencecloud.adobe.com)

1. ソリューション切 ![り替えアイコンをクリックし](images/mobile-launch-solutionSwitcher.png) 、ソリューション切り替えボタンを開きます

1. Select **[!UICONTROL Launch]** from the menu

   ![アイコンを使用してソリューション切り替えボタンを開き、「Activation」をクリックします](images/mobile-launch-solutionSwitcherActivation.png)

1. 「 **[!UICONTROL Adobe Experience Cloud Launch]**」で、「Go to Launch **[!UICONTROL 」ボ]** タンをクリックします。

   ![「起動」ボタンをクリックします](images/mobile-launch-goToLaunch.png)

You should now see the `Properties` screen (if no properties have ever been created in the account, this screen might be empty):

![プロパティ画面](images/mobile-launch-propertiesScreen.png)

「起動」を頻繁に使用する場合は、次のURLにブックマークを付けて、直接https://launch.adobe.comにログインすることもで [きます。](https://launch.adobe.com)

## プロパティの作成

プロパティは基本的に、タグをアプリにデプロイする際に、拡張子、ルール、データ要素およびライブラリを設定するコンテナです。 1つのモバイルプロパティは、同じ機能がアプリに含まれ、同じソリューションを実装する必要がある場合、複数のアプリプラットフォーム（iOSやAndroidなど）で使用できます。  プロパティの作成について詳しくは、製品ドキュメ [ントの「モバイルプロパティの設定](https://aep-sdks.gitbook.io/docs/getting-started/create-a-mobile-property) 」を参照してください。

**プロパティを作成するには**

1. 「新しいプロパ **[!UICONTROL ティ]** 」ボタンをクリックします。

   ![「新しいプロパティ」をクリックします](images/mobile-launch-addNewProperty.png)

1. プロパティに名前を付けます(例： `Mobile Tutorial`)
1. プラットフォームとして、「 **[!UICONTROL Mobile」をクリックします]**
1. 「 **[!UICONTROL Save]** 」ボタン

   ![新しいプロパティの作成](images/mobile-launch-newProperty.png)

新しいプロパティがプロパティページに表示されます。 Note that if you check the box next to the property name, options to **[!UICONTROL Configure]** or **[!UICONTROL Delete]** the property appear above the property list. プロパティの名前(例： `Mobile Tutorial`)をクリックして、画面を開 `Overview` きます。
![プロパティの名前をクリックして開きます](images/mobile-launch-openProperty.png)

[次の「拡張子を追加」 &gt;](launch-add-extensions.md)
