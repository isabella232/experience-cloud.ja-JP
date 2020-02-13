---
title: Adobe Experience Platform Launch でのライブラリ作成
description: Adobe Experience Platform Launch でライブラリを作成する方法を説明します。このレッスンは、「モバイル iOS Objective-C アプリケーションでの Experience Cloud の実装」チュートリアルの一部です。
seo-description: null
seo-title: Adobe Experience Platform Launch でのライブラリ作成
solution: Experience Cloud
translation-type: ht
source-git-commit: d166b50c6452a9a44c387bbdbfb88127a8545eda

---


# ライブラリの追加

このレッスンでは、新しい Launch プロパティ用のライブラリを作成します。ライブラリは、Launch に必要なすべての設定をまとめてパッケージ化し、モバイルアプリにライブラリをインストールするために必要な具体的な実装手順を生成します。

## 学習内容

このレッスンを最後まで学習すると、以下の内容を習得できます。

* ライブラリの作成
* ライブラリへの変更の追加
* ライブラリのビルド

## ライブラリへの変更の保存

拡張機能を設定した後、ソリューションを使用できるように、アプリケーションにデプロイできるライブラリにパッケージ化する必要があります。

**ライブラリを追加してビルドするには、以下を実行します。**

1. 「**[!UICONTROL パブリッシング]**」タブに移動します

1. **[!UICONTROL 新しいライブラリの追加]**&#x200B;をクリックします

   ![新しいライブラリの追加](images/mobile-launch-addNewLibrary.png)

1. ライブラリに「`Initial Setup`」と名前を付けます。

1. **[!UICONTROL 環境／開発]**&#x200B;を選択します。

1. **[!UICONTROL 変更されたリソースをすべて追加]**&#x200B;をクリックします。

   ![変更されたリソースをすべて追加](images/mobile-launch-addAllChangedResources.png)

1. **[!UICONTROL 変更されたリソースをすべて追加]**&#x200B;をクリックした後、Launch に拡張機能の追加時におこなった変更の概要が表示されることに注意してください。

1. **[!UICONTROL 開発用に保存およびビルド]**&#x200B;をクリックします。

   ![開発用に保存およびビルド](images/mobile-launch-saveAndBuild.png)

1. しばらくすると、ステータスドットが緑色に変わり、ライブラリが正常にビルドされたことを示します。

   ![ビルドされたライブラリ](images/mobile-launch-libraryBuilt.png)

[次：「Launch プロパティと SDK のインストール」&gt;](launch-install-the-mobile-sdk.md)
