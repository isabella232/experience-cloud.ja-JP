---
title: Adobe AnalyticsのLaunchへの実装
description: Adobe Analytics Launch拡張機能を使用したAdobe Analyticsの実装方法、スクリーンビュービーコンの送信方法、変数の追加方法、イベントの追跡方法およびプラグインの追加方法について説明します。 このレッスンは、「モバイルAndroidアプリケーションでのExperience cloudの実装」チュートリアルの一部です。
seo-description: null
seo-title: Adobe AnalyticsのLaunchへの実装
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: e9dee6d0aa3b775d0ce617e2b2c57b56de491b8d

---


# Adobe Analytics の追加

このレッスンでは、アプリでAdobe Analyticsの追跡を有効にします。

[Adobe Analytics](https://docs.adobe.com/content/help/en/analytics/landing/home.html) は、顧客像を把握し、顧客インテリジェンスを活用してビジネスを導く力をユーザーに提供する、業界最先端のソリューションです。

「拡張機能 [の追加](launch-add-extensions.md) 」と「Mobile SDK [のインストール」のレッスンで](launch-install-the-mobile-sdk.md) 、Adobe Analytics拡張機能をLaunchプロパティに追加し、サンプルアプリケーションに読み込みました。  これで、アプリ内の状態とアクションを追跡するコードを追加するだけです。

## 学習目標

このレッスンを最後まで学習すると、以下の内容を習得できます。

* ライフサイクル指標がAdobe Analyticsに送信されていることを確認します。
* 追加のデータを使用してアプリ内の状態を追跡するコードを追加します
* 追加データを使用してアプリ内のアクションを追跡するコードを追加します

起動時にAnalytics用に実装できる多くの機能があります。 このレッスンでは、すべてを網羅したものではありませんが、独自のアプリに実装する際に必要な主なテクニックの概要を確認する必要があります。

## 前提条件

You should have already completed the lessons in the [Configure Launch](launch-create-a-property.md) section. この節では、Analytics拡張機能を追加し、トラッキングサーバーIDとレポートスイートIDを設定しました。

## ライフサイクル指標とAdobe Analytics

ライフサイクル指標は、Experience Platform Mobile SDKを使用してアプリで簡単に有効にできる、環境ベースの指標およびディメンションです。

プロパティにコア拡張機能を追加し、インターフェイスに表示されるモバイルインストール手順に従って、既にライフサイクル指標を有効にしています。 これらの指標およびディメンション（アプリのバージョン、アクションを実行したユーザー数、OSのバージョン、時間分割、前回使用からの日数など、環境およびアプリ固有の指標を含む）。 は、アプリの分析に非常に役立ちます。特に、アプリからAnalyticsセグメントを作成して、すべてのレポートに適用する際に役立ちます。 指標の完全なリストは、ドキュメントで参照でき [ます](https://docs.adobe.com/content/help/en/mobile-services/android/metrics.htmlh)。

## ACPCoreライブラリの読み込み

前のレッスン「モバイ [ルSDKのインストール」では](launch-install-the-mobile-sdk.md)、AdobeCoreライブラリをBusBookingActivityファイルで使用できるようにするインポート文を追加しました。 この同じライブラリは、このレッスンのアクティビティで追加のAPI呼び出しに使用されます。 次の練習では、APIを使用して、AdobeCoreライブラリで定義されているアプリ内の状態(「trackState」)とアクション(「trackAction」)を追跡します。  新しいExperience CloudプラットフォームモバイルSDKでは、trackStateおよびtrackAction APIがAnalyticsライブラリからコアライブラリに移行され、Adobe Analyticsトラッキング以外の目的でこれらのAPIを利用できるようになりました。

## 状態の追跡

アプリには、ユーザーに提供する様々な画面のコンテンツが含まれる場合があります。 これは、Webサイト上のページと同じです。 Adobe Analyticsでは、これらの「ページビューヒット」を送信し、Webプロパティと同じレポートに表示する方法が用意されています。 このメソッドは「trackState」と呼ばれます。

このチュートリアルでは、trackState呼び出しのコードをアプリ内の1つの画面（ページ）にのみ配置します。 実際には、アプリ内の他のすべての画面/状態にこの画像を複製します。

以下に、このチュートリアルまたは独自のアプリケーションでコピー&amp;ペーストできるドキュメントの構文とコード例を示します。

**構文：**

```java
public static void trackState(final String state, final Map<String, String> contextData)
```

**例：**

```java
HashMap cData = new HashMap<String, String>();
contextData.put("key", "value");
MobileCore.trackState("state name",contextData);
```

### データなしの状態の追跡

1. Android studioでサンプルアプリを開き、BusBookingActivityに移動し、下部付近をスクロールしてonResume関数に移動します
1. trackStateメソッド呼び出しの追加
1. を「予約 `state name` 画面」に設定します。
1. 追加のデータを追加する代わりに、API呼び出しでプ `null` レースホルダーとして追加します。
1. または、次の場所にコピーして貼り付けます。

   ```java
   MobileCore.trackState("Booking Screen", null);
   ```

![基本trackState呼び出し](images/android/mobile-analytics-basicTrackStateCall2.png)

**trackStateを検証するには**

1. プロジェクトを保存、構築、実行する
1. シミュレーターが実行され、アプリケーションのホーム画面が開いたら、Android Studio Logcatデバッグコンソールを表示します
1. コンソールで `pageName=Booking%20Screen`
1. pageName変数はに設定され、(エンコ `Booking Screen` ードされたスペースとして%20を含む)他のカスタムデータペアは存在しないことに注意してください。 技術的には、「ページ名」ではなく「状態名」を設定しますが、Webサイトの実装との一貫性を保つために `pageName` 使用されるパラメーター名が使用されます。

   ![基本trackState結果](images/android/mobile-analytics-basicTrackStateResult.png)

### データを使用した状態の追跡

1. BusBookingActivityに戻り、既存のインポートの下のファイルの先頭にインポ `import java.util.HashMap;` ートを追加します
1. この関数 `onResume()` で、最後の演習での基本的なtrackState呼び出しをコメントアウト（または削除）します
1. 新しいtrackStateメソッド呼び出しを追加します。今回は、HashMapを作成して名前を付け、「put」コマンドを使用してキーと値のペアを含め、そのHashMapをtrackStateの呼び出しで呼び出します。
1. 「予約画面」 `state name` のままにします。
1. または、次の場所にコピー&amp;ペーストします。

   ```java
   HashMap cData = new HashMap<String, String>();
   cData.put("cd.section", "Bus Booking");
   cData.put("cd.subSection", "Booking");
   cData.put("cd.conversionType", "Landing");
   MobileCore.trackState("Booking Screen", cData);
   ```

   ![trackStateデータを使用した呼び出し](images/android/mobile-analytics-trackStateWithData.png)

**trackStateをデータで検証するには**

1. プロジェクトを保存、構築、再実行する
1. シミュレーターが実行され、アプリケーションのホーム画面が開いたら、Android Studio Logcatデバッグコンソールを表示します
1. (また `subSection` はコードに入力したキーや値のいずれか)を検索します。
1. ここで、pageNameが設定されている以外に、ヒット時に送信されたキーと値のペアもあることを確認します。

   ![trackStateとデータ結果](images/android/mobile-analytics-trackStateWithDataResult.png)

>[!NOTE] Analyticsの「propとeVar」に詳しい場合は、これらの変数名がSDKに含まれていないことに注意してください。 SDKからのすべてのキー/値データは [contextData変数として送信されるので](https://docs.adobe.com/content/help/en/analytics/implementation/javascript-implementation/variables-analytics-reporting/context-data-variables.html)、Analytics UIの処理ルールを使用して、propまたはeVar（または他の変数）にマッピングする必要があります [](https://docs.adobe.com/content/help/en/analytics/admin/admin-tools/processing-rules/processing-rules.html) 。

## アクションの追跡

Webサイトでのページ読み込み以外のアクションの追跡と同様に、多くの場合、ユーザーがアプリ内でとるアクション（別の画面を読み込まないもののクリックなど）を追跡します。 これは、このメソッドが呼び出される点を除いて、上で使用したtrackStateと非常に似ています `trackAction`。

以下に、ドキュメントの構文とコード例を示します。

**構文：**

```java
public static void trackAction(final String action, final Map<String, String> contextData) data;
```

**例：**

```java
HashMap<String, String> contextData = new HashMap<String, String>();
contextData.put("key", "value");
MobileCore.trackAction("action taken", contextData);
```

### ターゲット切り替えボタンを使用した追跡操作

このサンプルバス予約アプリでは、これら2つの値の間の矢印をクリックして、出発地と目的地を切り替えることができます。 Adobe Analyticsでこの機能とのインタラクションを追跡することにしました。

![宛先切り替え](images/android/mobile-analytics-destinationSwitcher.png)

この切り替えコントロールは、サンプルプロジェクトのBusBookingActivityファイルで制御します。 この練習では、クリックするたびにtrackActionヒットを送信します。

#### trackActionコードを追加するには

1. Android studioでサンプルプロジェクトを開き、BusBookingActivityに移動します。
1. "mBtnFlip.setOnClickListener"関数を57行目前後で見つけます。
1. 必要に応じて関数を展開し、すべてのコードを表示します
1. onClick関数の呼び出し先の下で、呼び `flipSourceDesti()`出しを追加しま `trackAction()` す。
1. アクション名を「Flip Destination」に設定し、contextDataパラメーターに「null」を追加します（現時点では、追加情報を送信する必要はありません）。
1. 次のコードをコピーして貼り付けることができます

   ```java
   MobileCore.trackAction("Flip Destination", null);
   ```

関数は次のようになります。

![宛先切り替えコード](images/android/mobile-analytics-destinationSwitcherCode.png)

#### trackActionコードを検証するには

1. コードを追加したら、プロジェクトを保存し、実行してビルドします
1. ごみ箱アイコンをクリックしてLogcatコンソールをクリアします
1. シミュレーターのDestination Switcher矢印をクリックすると、コンソールに新しいリクエスト（またはそれ以上）が表示されます。
1. Logcatでの検 `Flip%20Destination` 索
1. actionパラメーターとpev2パラメーターの両方がFlip%20Destination （エンコードされたスペース付き）であることに注意してください
1. 同じ行のキ `pe=lnk_o` ー/値が、trackActionによってトリガーされた「カスタムリンク」ヒットであることを示しています。

   <!--![trackAction Result in Debugger](images/android/mobile-analytics-trackActionResult1.png)-->

いい仕事！ Analyticsのレッスンが完了しました。 もちろん、Analyticsの実装を強化するためには他にも多くのことができますが、それによって、残りのニーズに対処するために必要なコアスキルが得られれば幸いです。

## trackStateとtrackActionのその他の利点

この最後の練習では、trackState APIとtrackAction APIを使用して、アプリからAdobe Analyticsにデータを送信できました。 Experience Platform Mobile SDKは起動に根付いているので、起動インターフェイスでは、先ほど追加したコードを活用して、さらに多くのことを実行できます。

起動では、trackState APIとtrackAction APIによってトリガーされるルールを作成し、他のアドビソリューションや外部パートナーにリクエストするなど、追加のアクションを実行させることができます。

[次の「Adobe Audience Managerを追加」/](audience-manager.md)
