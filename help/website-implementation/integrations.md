---
title: Experience cloud統合のLaunchへの実装
description: Adobe Experience cloud実装でオーディエンス、A4Tおよび顧客属性の統合を検証する方法について説明します。 このレッスンは、「WebサイトにExperience cloudを導入する（起動）」チュートリアルの一部です。
seo-description: null
seo-title: Experience cloud統合のLaunchへの実装
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: e9dee6d0aa3b775d0ce617e2b2c57b56de491b8d

---


# Experience Cloud の統合

このレッスンでは、実装したソリューション間の主要な統合を確認します。 良い知らせは、前のレッスンを完了することで、統合のコード面をすでに実装していることです。 このレッスンでは、読み取りと検証以外の追加作業を行う必要はありません。

## 学習目標

このレッスンを最後まで学習すると、以下の内容を習得できます。

1. オーディエンス共有、Analytics for Target(A4T)および顧客属性統合の基本的な使用例の説明
1. これらの統合の基本的なクライアント側実装の側面を検証する

## 前提条件

このレッスンの手順に従う前に、このチュートリアルの前のレッスンをすべて完了する必要があります。

>[!NOTE] これらの統合を最大限活用するためには多くのユーザー権限要件、アカウント設定、プロビジョニング手順がありますが、これらはこのチュートリアルの範囲外です。現在のExperience cloudの実装で、これらの統合をまだ使用していない場合は、以下の点を考慮する必要があります。
>
> * [コアサービス統合](https://docs.adobe.com/content/help/en/core-services/interface/about-core-services/core-services.html)の完全な要件の確認
> * [Analytics for Target 統合](https://docs.adobe.com/content/help/en/target/using/integrate/a4t/before-implement.html)の完全な要件の確認
> * Have an Administrator of your Experience Cloud Organization [request provisioning of these integrations](https://www.adobe.com/go/audiences)


## オーディエンス

[オーディエンスは](https://docs.adobe.com/content/help/en/core-services/interface/audiences/audience-library.htm) 、Peopleコアサービスの一部であり、ソリューション間でオーディエンスを共有できます。 例えば、Audience Managerでオーディエンスを作成し、それを使用して、Targetでパーソナライズされたコンテンツを配信できます。

A4Tの実装に必要な主な要件は、次のとおりです（既に済んでいます）。

1. Adobe Experience Platform IDサービスの実装
1. Audience Managerの実装
1. TargetやAnalyticsなど、オーディエンスを受け取る、または作成する他のソリューションの実装

### オーディエンス統合の検証

オーディエンス統合を検証する最善の方法は、実際にオーディエンスを作成し、他のソリューションと共有して、それを他のソリューションで完全に使用することです（例えば、AAMセグメントに適格な訪問者が、そのセグメントをターゲットにしたTargetアクティビティに資格を得られることを確認します）。 ただし、これはこのチュートリアルの範囲外です。

これらの検証手順では、クライアント側の実装（訪問者ID）に表示される重要な部分に焦点を当てます。

1. Open the [Luma site](https://luma.enablementadobe.com/content/luma/us/en.html)

1. Make sure the Debugger is mapping the Launch property to *your* Development environment, as described in the [earlier lesson](launch-switch-environments.md)

   ![デバッガーに表示される起動開発環境](images/switchEnvironments-debuggerOnWeRetail.png)

1. デバッガーの「ネットワーク」タブに移動します。

1. 「すべての **[!UICONTROL リクエストをクリア]** 」をクリックして、クリーンアップを行います

1. ルミナンスページを再読み込みし、デバッガーにTargetとAnalyticsの両方のリクエストが表示されていることを確認します

1. ルミナンスページを再度読み込む

1. これで、デバッガーの「ネットワーク」タブに4つのリクエストが表示されます。2つはTargetのリクエスト、2つはAnalyticsのリクエストです。

1. 「Experience cloud訪問者ID」というラベルの付いた行を確認します。 各ソリューションのすべてのリクエストの ID は常に同じにする必要があります。

   ![一致するSDIDの確認](images/integrations-matchingECIDs.png)

1. ID は訪問者ごとに一意であり、これらの手順を繰り返すことによって、共同作業者に確認できます。

## Analytics for Target（A4T）

The [Analytics for Target (A4T)](https://docs.adobe.com/content/help/en/target/using/integrate/a4t/a4t.html) integration allows you to leverage your Analytics data as the source for reporting metrics in Target.

A4Tの実装に必要な主な要件は、次のとおりです（既に済んでいます）。

1. Adobe Experience Platform IDサービスの実装
1. Analyticsページビュービーコンの前にTargetページ読み込みリクエストを実行する

A4Tは、TargetからAnalyticsへのサーバー側リクエストを、Analyticsページビュービーコンと組み合わせて、「ヒットステッチ」と呼びます。  ヒットのステッチでは、アクティビティを配信する（またはTargetベースの目標指標を増分する）Targetリクエストに、Analyticsページビュービーコンのパラメーターと一致するパラメーターが必要です。 このパラメーターは、追加のデータID(SDID)と呼ばれます。

### A4T の実装の検証

A4T統合を検証する最善の方法は、A4Tを使用して実際にTargetアクティビティを構築し、レポートデータを検証することですが、これはこのチュートリアルの範囲外です。 このチュートリアルでは、追加のデータIDがソリューションの呼び出し間で一致することを確認する方法を示します。

**SDIDを検証するには**

1. Open the [Luma site](https://luma.enablementadobe.com/content/luma/us/en.html)

1. Make sure the Debugger is mapping the Launch property to *your* Development environment, as described in the [earlier lesson](launch-switch-environments.md)

   ![デバッガーに表示される起動開発環境](images/switchEnvironments-debuggerOnWeRetail.png)

1. デバッガーの「ネットワーク」タブに移動します。

1. 「すべての **[!UICONTROL リクエストをクリア]** 」をクリックして、クリーンアップを行います

1. ルミナンスページを再読み込みし、デバッガーにTargetとAnalyticsの両方のリクエストが表示されていることを確認します

1. ルミナンスページを再度読み込む

1. これで、デバッガーの「ネットワーク」タブに4つのリクエストが表示されます。2つはTargetのリクエスト、2つはAnalyticsのリクエストです。

1. 「Supplemental Data ID」というラベルの付いた行を見つけます。最初のページ読み込みからのIDは、TargetとAnalyticsの間で一致する必要があります。 2 番目のページ読み込みからの ID も一致する必要がありますが、最初のページ読み込みの ID とは異なります。

   ![一致するSDIDの確認](images/integrations-matchingSDIDs.png)

A4Tアクティビティの一部であるページ読み込みの範囲で追加のTargetリクエストを行う場合は、そのリクエストが最初のTargetおよびAnalyticsリクエストと同じSDIDを継続するように（target-global-mboxではなく）一意の名前を付けることをお勧めします。

## 顧客属性

[顧客属性は](https://docs.adobe.com/content/help/en/core-services/interface/customer-attributes/attributes.html) 、顧客関係管理(CRM)データベースからデータをアップロードし、Adobe AnalyticsとAdobe targetで活用できるPeopleコアサービスの一部です。

顧客属性を実装する主な要件は、次のとおりです（既に行った要件）。

1. Adobe Experience Platform IDサービスの実装
1. Set Customer Ids via the Id Service *before* Target and Analytics fire their requests (which you accomplished using the rule ordering feature in Launch)

### 顧客属性の実装の検証

以前のレッスンで、顧客IDがIDサービスとTargetの両方に渡されることを既に検証済みである。 また、Analyticsヒットで顧客IDを検証することもできます。
現時点では、顧客IDはExperience cloudデバッガーに表示されないいくつかのパラメーターの1つなので、ブラウザーのJavaScriptコンソールを使用して表示します。

1. ルミナンスサイトを開く
1. ブラウザーの開発者ツールを開く
1. [ネットワーク]タブに移動します。
1. フィルターフィールドに、表 `b/ss` 示する内容をAdobe Analyticsリクエストに制限する値を入力します

   ![開発者ツールを開き、「ネットワーク」タブをフィルターして、Analyticsリクエストのみを表示します](images/aam-openTheJSConsole.png)

1. サイトの **[!UICONTROL 右上隅にある]** 「LOGIN」リンクをクリックします。

   ![上部のナビゲーションで「ログイン」をクリックします。](images/idservice-loginNav.png)

1. ユーザ `test@adobe.com` ー名として入力
1. パスワ `test` ードとして入力
1. 「 **[!UICONTROL LOGIN]** 」ボタン

   ![資格情報を入力し、「ログイン」をクリックします](images/idservice-login.png)

1. ホームページに戻り、開発者ツールに表示されるビーコンもトリガーされます。 アカウント情報ページに移動した場合は、WE.RETAILロゴをクリックしてホームページに戻ります。
1. リクエストをクリックし、「ヘッダー」タブを選択します
1. ネストされたパラメーターが表示されるまで下にスクロールします
   1. cid — リクエストの顧客ID部分の標準の区切り文字です。
   1. crm_id — これは、IDサービスのレッスンで指定したカスタム統合コードです。
   1. id — データ要素から取得される顧客ID `Email (Hashed)` 値。
   1. as — 認証状態。「1」はログインを意味します。
   ![Analytics顧客IDの検証](images/integrations-analyticsCustomerIDValidation.png)

[次の「プロパティを公開」&gt;](publish.md)