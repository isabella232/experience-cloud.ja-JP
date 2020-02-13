---
title: Launch プロパティの作成
description: Launch インターフェイスにログインして Launch プロパティを作成する方法について説明します。このレッスンは、「Launch を使用した Web サイトでの Experience Cloud の実装」チュートリアルの一部です。
seo-description: null
seo-title: Launch プロパティの作成
solution: Experience Cloud
translation-type: ht
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# Launch プロパティの作成

このレッスンでは、最初の Launch プロパティを作成します。

プロパティは基本的に、サイトにタグを導入する際に拡張機能、ルール、データ要素およびライブラリを入力するコンテナです。

## 前提条件

次のレッスンを完了するには、Launch で環境の開発、承認、発行、管理をおこなう権限が必要です。ユーザーインターフェイスオプションが使用できないためにこれらの手順を完了できない場合は、Experience Cloud 管理者に連絡してアクセス権をもらってください。Launch の権限について詳しくは、[ドキュメント](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/admin/user-permissions.html)を参照してください。

## 学習内容

このレッスンを最後まで学習すると、以下の内容を習得できます。

* Launch ユーザーインターフェイスへのログイン
* 新しい Launch プロパティの作成
* Launch プロパティの設定

## Launch へのアクセス

**Launch にアクセスするには、以下を実行します。**

1. [Adobe Experience Cloud](https://experiencecloud.adobe.com) にログインします。

1. ![ソリューション切り替えアイコン](images/launch-solutionSwitcher.png)をクリックし、ソリューション切り替えツールを開きます。

1. メニューから **[!UICONTROL Launch]** を選択します。![アイコンを使用してソリューション切り替えボタンを開き、「アクティブ化」をクリックします。](images/launch-solutionSwitcherActivation.png)

1. **[!UICONTROL Adobe Experience Cloud Launch]**&#x200B;で、「**[!UICONTROL Launch にアクセス]**」ボタンをクリックします。

   ![「Launch」 ボタンをクリックする](images/launch-goToLaunch.png)

`Properties` 画面が表示されます（アカウントでプロパティを作成したことがない場合は、この画面が空になる可能性があります）。

![プロパティ画面](images/launch-propertiesScreen.png)

Launch を頻繁に使用する場合は、[https://launch.adobe.com](https://launch.adobe.com) をブックマークして、直接ログインすることもできます。

## プロパティの作成

プロパティは基本的に、サイトにタグを導入する際に拡張機能、ルール、データ要素およびライブラリを入力するコンテナです。プロパティは、1 つ以上のドメインやサブドメインをグループ化できます。それらすべてのアセットを、ほぼ共通の方法で管理および追跡できます。例えば、1 つのテンプレートに基づく複数の Web サイトがあり、それらすべてに関する共通のアセットを追跡する場合などに役立ちます。1 つのプロパティを複数のドメインに適用することもできます。プロパティの作成について詳しくは、製品ドキュメントの[企業とプロパティ](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/admin/companies-and-properties.html)を参照してください。

**プロパティを作成するには**

1. 「**[!UICONTROL 新しいプロパティ]**」ボタンをクリックします。

   ![新規プロパティをクリックする](images/launch-addNewProperty.png)

1. プロパティに名前を付けます（例：`Launch Tutorial` または `Daniel's Launch Tutorial`）
1. ドメインとして、これは Luma デモサイトがホストされているドメインなので、`enablementadobe.com` と入力します。「ドメイン」フィールドは必須ですが、Launch プロパティは、実装されている任意のドメインで機能します。このフィールドの主な目的は、ルールビルダーでメニューオプションを事前に設定することです。
1. 「**[!UICONTROL 保存]**」ボタンをクリックします。

   ![新しいプロパティの作成](images/launch-newProperty.png)

新しいプロパティが「プロパティ」ページに表示されます。プロパティ名の横にあるボックスをチェックすると、プロパティを **[!UICONTROL 設定]**&#x200B;または **[!UICONTROL 削除]**&#x200B;するオプションがプロパティリストの上に表示されます。プロパティ名（例：`Launch Tutorial`）をクリックして、`Overview`画面を開きます。
![プロパティ名をクリックして開く](images/launch-openProperty.png)

[次：「Launch 埋め込みコードの追加」&gt;](launch-add-embed.md)
