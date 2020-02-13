---
title: Launch を使用した Adobe Analytics の実装
description: Adobe Analytics Launch 拡張機能を使用した Adobe Analytics の実装方法、スクリーンビュービーコンの送信方法、変数の追加方法、イベントのトラッキング方法、プラグインの追加方法について説明します。このレッスンは、「モバイル Android アプリケーションでの Experience Cloud の実装」チュートリアルの一部です。
seo-description: null
seo-title: Launch を使用した Adobe Analytics の実装
solution: Experience Cloud
translation-type: ht
source-git-commit: e9dee6d0aa3b775d0ce617e2b2c57b56de491b8d

---


# Adobe Analytics の追加

このレッスンでは、アプリで Adobe Analytics のトラッキングを有効にします。

[Adobe Analytics](https://docs.adobe.com/content/help/ja-JP/analytics/landing/home.html) は、顧客像を把握し、顧客インテリジェンスを活用してビジネスを導く力をユーザーに提供する、業界最先端のソリューションです。

「[拡張機能の追加](launch-add-extensions.md)」と「[Mobile SDK のインストール](launch-install-the-mobile-sdk.md)」のレッスンで、Adobe Analytics 拡張機能を Launch プロパティに追加し、サンプルアプリケーションに読み込みました。後は、アプリケーション内の状態とアクションを追跡するコードを追加するだけです。

## 学習内容

このレッスンを最後まで学習すると、以下の内容を習得できます。

* ライフサイクル指標が Adobe Analytics に送信されていることを確認する
* 追加データを使用してアプリケーション内の状態を追跡するコードを追加する
* 追加データを使用してアプリケーション内のアクションを追跡するコードを追加する

Launch では、Analytics に多くの機能を実装できます。このレッスンは完全なものではありませんが、お客様独自のアプリケーションで実装するために必要となる、主な技術の概要について明確に説明します。

## 前提条件

[Launch の設定](launch-create-a-property.md)節のレッスンを既に完了していること。この節では、Analytics 拡張機能を追加し、トラッキングサーバー ID とレポートスイート ID を設定しました。

## ライフサイクル指標と Adobe Analytics

ライフサイクル指標は、Experience Platform Mobile SDK を使用してアプリケーションで簡単に有効にできる、環境ベースの指標およびディメンションです。

プロパティにコア拡張機能を追加し、インターフェイスに表示されるモバイルインストール手順に従って、既にライフサイクル指標を有効にしています。これらの指標およびディメンション（アプリケーションのバージョン、アクションを実行したユーザー数、OS のバージョン、時間分割、前回使用からの日数など、環境およびアプリケーション固有の指標を含む）は、特に、アプリケーションから Analytics セグメントを作成してすべてのレポートに適用する際に、アプリケーションの分析に非常に役立ちます。指標の完全なリストは、[ドキュメント](https://docs.adobe.com/content/help/ja-JP/mobile-services/android/metrics.html)で参照できます。

## ACPCore ライブラリの読み込み

前の[Mobile SDK のインストール](launch-install-the-mobile-sdk.md)レッスンでは、AdobeCore ライブラリを BusBookingActivity ファイルで使用できるようにする import 文を追加しました。この同じライブラリは、このレッスンのアクティビティで追加の API 呼び出しに使用されます。次の演習では、API を使用して、AdobeCore ライブラリで定義されているアプリケーション内の状態（「trackState」）とアクション（「trackAction」）を追跡します。新しい Experience Cloud Platform Mobile SDK では、trackState および trackAction API が Analytics ライブラリから Core ライブラリに移行され、Adobe Analytics トラッキング以外の目的でこれらの API を利用できるようになりました。

## 状態の追跡

アプリケーションには、ユーザーに提供する様々な画面のコンテンツが含まれる場合があります。これは、Web サイト上のページと同じです。Adobe Analytics では、これらの「ページビューヒット」を送信し、Web プロパティと同じレポートに表示するメソッドが用意されています。このメソッドは「trackState」と呼ばれます。

このチュートリアルでは、trackState 呼び出しのコードをアプリケーション内の 1 つの画面（ページ）にのみ配置します。実際には、アプリケーション内の他のすべての画面と状態にこの画像を複製します。

以下に、このチュートリアルまたは独自のアプリケーションでコピー＆ペーストできるドキュメントの構文とコード例を示します。

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

### データを伴わない状態の追跡

1. Android Studio でサンプルアプリケーションを開き、BusBookingActivity に移動し、下部付近をスクロールして onResume 関数に移動します。
1. trackState メソッド呼び出しを追加します。
1. `state name` を「Booking Screen」に設定します。
1. 追加のデータを追加する代わりに、API 呼び出しで `null` をプレースレースホルダーとして追加します。
1. または、次の場所にコピー＆ペーストします。

   ```java
   MobileCore.trackState("Booking Screen", null);
   ```

![基本的な trackState の呼び出し](images/android/mobile-analytics-basicTrackStateCall2.png)

**trackState を検証するには、以下を実行します。**

1. プロジェクトを保存、ビルド、実行します。
1. シミュレーターが実行され、アプリケーションのホーム画面が開いたら、Android Studio Logcat デバッグコンソールを表示します。
1. コンソールで `pageName=Booking%20Screen` を検索します。
1. pageName 変数は`Booking Screen` に設定され、（エンコードされたスペースとして %20 を含む）他のカスタムデータペアは存在しないことに注意してください。技術的には、「ページ名」ではなく「状態名」を設定しますが、Web サイトの実装との一貫性を保つために、パラメーター名 `pageName` が使用されます。

   ![基本的な trackState の結果](images/android/mobile-analytics-basicTrackStateResult.png)

### データを伴う状態の追跡

1. BusBookingActivity に戻り、`import java.util.HashMap;` ファイルの先頭の既存のインポートの下にインポートを追加します。
1. `onResume()` 関数で、最後の演習での基本的な trackState 呼び出しをコメントアウト（または削除）します。
1. trackState メソッドの新しい呼び出しを追加します。今回は、HashMap を作成して名前を付け、「put」コマンドを使用していくつかのキー／値のペアを組み込み、trackState の呼び出しでその HashMap を呼び出してデータを取得します。
1. `state name` は「Booking Screen」のままにします。
1. または、次の場所にコピー＆ペーストします。

   ```java
   HashMap cData = new HashMap<String, String>();
   cData.put("cd.section", "Bus Booking");
   cData.put("cd.subSection", "Booking");
   cData.put("cd.conversionType", "Landing");
   MobileCore.trackState("Booking Screen", cData);
   ```

   ![データを伴う trackState の呼び出し](images/android/mobile-analytics-trackStateWithData.png)

**データを伴う trackState を検証するには、以下を実行します。**

1. プロジェクトを再び保存、ビルド、実行します。
1. シミュレーターが実行され、アプリケーションのホーム画面が開いたら、Android Studio Logcat デバッグコンソールを表示します。
1. `subSection`（またはコードに入力したキーや値のいずれか）を検索します。
1. ここで、pageName が設定されている以外に、ヒット時に送信されたキーと値のペアもあることを確認します。

   ![trackState とデータ結果](images/android/mobile-analytics-trackStateWithDataResult.png)

>[!NOTE] Analytics の「prop と eVar」に詳しい人は、これらの変数名が SDK に含まれていないことに気が付くかもしれません。SDK からのすべてのキー／値データは [contextData](https://docs.adobe.com/content/help/ja-JP/analytics/implementation/javascript-implementation/variables-analytics-reporting/context-data-variables.html) 変数として送信されるので、Analytics UI の[処理ルール](https://docs.adobe.com/content/help/ja-JP/analytics/admin/admin-tools/processing-rules/processing-rules.html)を使用して、prop または eVar（または他の変数）にマッピングする必要があります。

## アクションの追跡

Web サイトでのページ読み込み以外のアクションの追跡と同様に、多くの場合、ユーザーがアプリケーション内でとるアクション（別の画面を読み込まないもののクリックなど）を追跡します。これは、このメソッドが `trackAction` と呼ばれている点を除いて、上で使用した trackState と非常に似ています。

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

### 目的地切り替えボタン操作の追跡

このサンプルバス予約アプリでは、これら 2 つの値の間の矢印をクリックして、出発地と目的地を切り替えることができます。Adobe Analytics でこの機能とのインタラクションを追跡することにしました。

![目的地の切り替え](images/android/mobile-analytics-destinationSwitcher.png)

この切り替えコントロールは、サンプルプロジェクトの BusBookingActivity ファイルで制御します。この演習では、クリックするたびに trackAction ヒットを送信します。

#### trackAction コードの追加。

1. Android Studio でサンプルプロジェクトを開き、BusBookingActivity に移動します。
1. 「mBtnFlip.setOnClickListener」関数を 57 行目前後で見つけます。
1. 必要に応じて関数を展開し、すべてのコードを表示します。
1. onClick 関数で、`flipSourceDesti()` の呼び出しの下で、`trackAction()` の呼び出しを追加します。
1. アクション名を「Flip Destination」に設定し、contextData パラメーターに「null」を追加します（現時点では、追加情報を送信する必要がないため）。
1. 次のコードをコピー＆ペーストできます。

   ```java
   MobileCore.trackAction("Flip Destination", null);
   ```

関数は、以下のようになります。

![目的地切り替えコード](images/android/mobile-analytics-destinationSwitcherCode.png)

#### trackAction コードを検証するには、以下を実行します。

1. コードを追加したら、プロジェクトを保存し、実行してビルドします。
1. ごみ箱アイコンをクリックして Logcat コンソールをクリアします。
1. シミュレーターの「目的地の切り替え」矢印をクリックすると、コンソールに 1 つ以上の新しいリクエストが表示されます。
1. Logcatでの `Flip%20Destination` の検索索。
1. Action パラメーターと pev2 パラメーターの両方が Flip%20Destination（エンコードされたスペース付き）であることに注意してください。
1. 同じ行の`pe=lnk_o` キー／値が、trackAction によってトリガーされた「カスタムリンク」ヒットであることに注意してください。

   <!--![trackAction Result in Debugger](images/android/mobile-analytics-trackActionResult1.png)-->

お疲れ様です。Analytics のレッスンが完了しました。もちろん、Analytics の実装を強化するためにできることは他にも多くありますが、このレッスンによって残りのニーズに対処するために必要なコアスキルが得られたなら幸いです。

## trackState と trackAction のその他の利点

この最後の演習では、trackState API と trackAction API を使用して、アプリケーションから Adobe Analytics にデータを送信しました。Experience Platform Mobile SDK は Launch に根付いているので、Launch のインターフェイスでは、先ほど追加したコードを活用して、さらに多くのことを実行できます。

Launch では、trackState API と trackAction API によってトリガーされるルールを作成し、他のアドビソリューションや外部パートナーにリクエストするなど、追加のアクションを実行させることができます。

[次：「Adobe Audience Manager の追加」&gt;](audience-manager.md)
