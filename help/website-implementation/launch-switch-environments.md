---
title: Adobe Experience cloudデバッガーによる起動環境の切り替え
description: Experience cloudデバッガーを使用して、様々な起動埋め込みコードを読み込む方法について説明します。 このレッスンは、「WebサイトにExperience cloudを導入する（起動）」チュートリアルの一部です。
seo-description: null
seo-title: Adobe Experience cloudデバッガーによる起動環境の切り替え
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: 79d5274d09d66b80a49aba5db3e0a997d0f0ff61

---


# Experience cloudデバッガーによる起動環境の切り替え

In this lesson you will use the [Adobe Experience Cloud Debugger extension](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj) to replace the Launch property hardcoded on the [Luma demo site](https://luma.enablementadobe.com/content/luma/us/en.html) with your own property.

この手法は環境の切り替えと呼ばれ、後で自分のWebサイトで「起動」を使用する場合に役立ちます。 実稼働環境のWebサイトをブラウザーに読み込むことができますが、開発起動環境で読み込む *ことがで* きます。 これにより、通常のコードリリースとは独立して、自信を持って起動の変更を行い、検証できます。  結局、マーケティングタグリリースを通常のコードリリースから分離することは、顧客が「起動」を最初に使用する主な理由の1つです。

## 学習目標

このレッスンを最後まで学習すると、以下の内容を習得できます。

* デバッガーを使用して代替起動環境を読み込む
* デバッガーを使用して、代替起動環境が読み込まれたことを検証します

## 開発環境のURLの取得

1. In your Launch property, open the `Environments` page

1. In the **[!UICONTROL Development]** row, click the Install icon ![Install icon](images/launch-installIcon.png) to open the modal

1. コピーアイコンコピーアイコン ![をクリックして](images/launch-copyIcon.png) 、埋め込みコードをクリップボードにコピーします

1. Click **[!UICONTROL Close]** to close the modal

   ![インストールアイコン](images/launch-copyInstallCode.png)

## ルミナンスデモサイトの起動URLを置き換える

1. Open the [Luma demo site](https://luma.enablementadobe.com/content/luma/us/en.html) in your Chrome browser

1. デバッガー [アイコンをクリックして](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj) 、 ![Experience Cloud Debugger拡張機能を](images/icon-debugger.png) 開きます。

   ![デバッガーアイコンをクリックします](images/switchEnvironments-openDebugger.png)

1. 現在実装されているLaunchプロパティは、「概要」タブに表示されます

   ![デバッガーに表示される起動環境](images/switchEnvironments-debuggerOnWeRetail-prod.png)

1. 「ツール」タブに移動します。

1. 「起動の埋め込みコードを置 **[!UICONTROL 換」セクションまでスクロールします]**

1. ルミナンスサイトのChromeタブがデバッガーの背後にあることを確認します（このチュートリアルのタブや起動インターフェイスのタブではありません）。  クリップボードにある埋め込みコードを入力フィールドに貼り付けます。

1. 「Apply across luma.enablementadobe.com」機能をオンにして、LumaサイトのすべてのページがLaunchプロパティにマッピングされるようにします。

1. 「 **[!UICONTROL Save]** 」ボタン

   ![デバッガーに表示される起動環境](images/switchEnvironments-debugger-save.png)

1. Lumaサイトを再読み込みし、デバッガーの「概要」タブを確認します。 「起動」セクションで、開発プロパティが使用されていることを確認できます。 「Name」プロパティと「Environment」プロパティの両方が一致していることを確認します。また、「Environment」に「development」と表示されていることも確認します。

   ![デバッガーに表示される起動環境](images/switchEnvironments-debuggerOnWeRetail.png)

>[!NOTE] デバッガーはこの設定を保存し、Lumaサイトに戻るたびにLaunch埋め込みコードを置き換えます。 他の開いているタブでアクセスした他のサイトには影響しません。 To stop the Debugger from replacing the embed code, click the **[!UICONTROL Remove]** button next to the embed code in the Tools tab of the Debugger.

チュートリアルを続ける際には、この方法を使用して、Lumaサイトを独自のLaunchプロパティにマッピングし、Launch実装を検証します。 実稼働用Webサイトで「起動」の使用を開始する場合、同じ方法を使用して変更を検証できます。

[次に、「Adobe Experience Platform IDサービスの追加」&gt;](id-service.md)