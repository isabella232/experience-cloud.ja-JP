---
title: モバイルアプリケーション用の Launch プロパティの作成
description: Launch インターフェイスにログインし、モバイル Launch プロパティを作成する方法を説明します。このレッスンは、「モバイル Android アプリケーションでの Experience Cloud の実装」チュートリアルの一部です。
seo-description: null
seo-title: モバイルアプリケーション用の Launch プロパティの作成
solution: Experience Cloud
translation-type: ht
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# Launch プロパティの作成

Adobe Experience Platform Launch は、次世代のモバイル SDK および Web サイトタグ管理機能です。Launch は、顧客体験の実現に必要なすべての分析、マーケティングおよび広告のソリューションをデプロイおよび管理するためのシンプルな手段を提供します。Launch では、追加料金はかかりません。Adobe Experience Cloud のお客様は、Launch を利用できます。

このレッスンでは、モバイルアプリ用の Launch プロパティを作成します。

## 前提条件

次のレッスンを完了するには、Launch で環境の開発、承認、発行、管理をおこなう権限が必要です。ユーザーインターフェイスオプションが使用できないためにこれらの手順を完了できない場合は、Experience Cloud 管理者に連絡してアクセス権をもらってください。Launch の権限について詳しくは、[ドキュメント](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/admin/user-permissions.html)を参照してください。

## 学習内容

このレッスンを最後まで学習すると、以下の内容を習得できます。

* Launch ユーザーインターフェイスへのログイン
* 新しい Launch モバイルプロパティの作成
* Launch モバイルプロパティの設定

## Launch へのアクセス

**Launch にアクセスするには、以下を実行します。**

1. [Adobe Experience Cloud ](https://experiencecloud.adobe.com)にログインします。

1. ![ソリューション切り替えアイコン](images/mobile-launch-solutionSwitcher.png)をクリックし、ソリューション切り替えツールを開きます。

1. メニューから **[!UICONTROL Launch]** を選択します。

   ![アイコンを使用してソリューション切り替えツールを開き、「アクティブ化」をクリックする](images/mobile-launch-solutionSwitcherActivation.png)

1. **[!UICONTROL Adobe Experience Cloud Launch]**&#x200B;で、「**[!UICONTROL Launch にアクセス]**」ボタンをクリックします。

   ![「Launch」ボタンをクリックする](images/mobile-launch-goToLaunch.png)

`Properties` 画面が表示されます（アカウントでプロパティを作成したことがない場合は、この画面が空になる可能性があります）。

![プロパティ画面](images/mobile-launch-propertiesScreen.png)

Launch を頻繁に使用する場合は、[https://launch.adobe.com](https://launch.adobe.com) をブックマークして、直接ログインすることもできます。

## プロパティの作成

プロパティは基本的に、アプリケーションにタグを導入する際に拡張機能、ルール、データ要素およびライブラリを入力するコンテナです。同じ機能がアプリに含まれ、同じソリューションを実装する必要がある場合、1 つのモバイルプロパティを複数のアプリプラットフォーム（iOS や Android など）で使用できます。プロパティの作成について詳しくは、製品ドキュメントの[モバイルプロパティの設定](https://aep-sdks.gitbook.io/docs/getting-started/create-a-mobile-property)を参照してください。

**プロパティを作成するには**

1. 「**[!UICONTROL 新しいプロパティ]**」ボタンをクリックします。

   ![新規プロパティをクリックする](images/mobile-launch-addNewProperty.png)

1. プロパティに名前を付けます（例：`Mobile Tutorial`）。
1. プラットフォームとして、**[!UICONTROL モバイル]**&#x200B;をクリックします。
1. 「**[!UICONTROL 保存]**」ボタンをクリックします。

   ![新しいプロパティの作成](images/mobile-launch-newProperty.png)

新しいプロパティが「プロパティ」ページに表示されます。プロパティ名の横にあるボックスをチェックすると、プロパティを&#x200B;**[!UICONTROL 設定]**&#x200B;または&#x200B;**[!UICONTROL 削除]**&#x200B;するオプションがプロパティリストの上に表示されます。プロパティ名（例：`Mobile Tutorial`）をクリックして、`Overview` 画面を開きます。
![プロパティ名をクリックして開く](images/mobile-launch-openProperty.png)

[次：「拡張子の追加」&gt;](launch-add-extensions.md)
