---
title: 起動プロパティの作成
description: 起動インターフェイスにログインし、起動プロパティを作成する方法について説明します。 このレッスンは、「WebサイトにExperience cloudを導入する（起動）」チュートリアルの一部です。
seo-description: null
seo-title: 起動プロパティの作成
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# 起動プロパティの作成

このレッスンでは、最初の起動プロパティを作成します。

プロパティは基本的に、サイトにタグを導入する際に拡張機能、ルール、データ要素およびライブラリを入力するコンテナです。

## 前提条件

次のレッスンを完了するには、起動時に環境の開発、承認、発行、管理を行う権限が必要です。 ユーザーインターフェイスオプションが利用できないためにこれらの手順を完了できない場合は、Experience cloud管理者に連絡して、アクセスをリクエストしてください。 For more information on Launch permissions, see [the documentation](https://docs.adobe.com/content/help/en/launch/using/reference/admin/user-permissions.html).

## 学習目標

このレッスンを最後まで学習すると、以下の内容を習得できます。

* Launchユーザーインターフェイスにログインします。
* 新しい起動プロパティの作成
* Launchプロパティの設定

## Launch  に移動します。

**起動を開始するには**

1. [Adobe Experience cloudへのログイン](https://experiencecloud.adobe.com)

1. ソリューション切 ![り替えアイコンをクリックし](images/launch-solutionSwitcher.png) 、ソリューション切り替えボタンを開きます

1. メニューか **[!UICONTROL ら「起動]** 」を選択します。ア ![イコンを使用してソリューション切り替えボタンを開き、「Activation」をクリックします。](images/launch-solutionSwitcherActivation.png)

1. 「 **[!UICONTROL Adobe Experience Cloud Launch]**」で、「Go to Launch **[!UICONTROL 」ボ]** タンをクリックします。

   ![「起動」ボタンをクリックします](images/launch-goToLaunch.png)

You should now see the `Properties` screen (if no properties have ever been created in the account, this screen might be empty):

![プロパティ画面](images/launch-propertiesScreen.png)

「起動」を頻繁に使用する場合は、次のURLにブックマークを付けて、直接https://launch.adobe.comにログインすることもで [きます。](https://launch.adobe.com)

## プロパティの作成

プロパティは基本的に、サイトにタグを導入する際に拡張機能、ルール、データ要素およびライブラリを入力するコンテナです。 プロパティは、1 つ以上のドメインやサブドメインをグループ化できます。それらすべてのアセットを、ほぼ共通の方法で管理および追跡できます。例えば、1 つのテンプレートに基づく複数の Web サイトがあり、それらすべてに関する共通のアセットを追跡する場合などに役立ちます。1 つのプロパティを複数のドメインに適用することもできます。プロパティの作成の詳細については、製品ドキュメ [ントの「会社とプロパティ](https://docs.adobe.com/content/help/en/launch/using/reference/admin/companies-and-properties.html) 」を参照してください。

**プロパティを作成するには**

1. 「新しいプロパ **[!UICONTROL ティ]** 」ボタンをクリックします。

   ![「新しいプロパティ」をクリックします](images/launch-addNewProperty.png)

1. プロパティに名前を付けます(例： `Launch Tutorial` または `Daniel's Launch Tutorial`)
1. ドメインとして、これはルミ `enablementadobe.com` ナンスデモサイトがホストされているドメインなので、と入力します。 「Domain」フィールドは必須ですが、Launchプロパティは、実装されている任意のドメインで機能します。 このフィールドの主な目的は、ルールビルダーでメニューオプションを事前に設定することです。
1. 「 **[!UICONTROL Save]** 」ボタン

   ![新しいプロパティの作成](images/launch-newProperty.png)

新しいプロパティがプロパティページに表示されます。 Note that if you check the box next to the property name, options to **[!UICONTROL Configure]** or **[!UICONTROL Delete]** the property appear above the property list. プロパティの名前(例： `Launch Tutorial`)をクリックして、画面を開 `Overview` きます。
![プロパティの名前をクリックして開きます](images/launch-openProperty.png)

[次の「開始埋め込みコードを追加」&gt;](launch-add-embed.md)
