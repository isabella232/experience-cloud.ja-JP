---
title: Adobe AnalyticsのLaunchへの実装
description: Adobe Analytics Launch拡張機能を使用したAdobe Analyticsの実装方法、スクリーンビュービーコンの送信方法、変数の追加方法、イベントの追跡方法およびプラグインの追加方法について説明します。 このレッスンは、「モバイルiOS Objective-CアプリケーションでのExperience cloudの実装」チュートリアルの一部です。
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

ライフサイクル指標は、Experience Platform Mobile SDKを使用してアプリで簡単に有効にできる、環境ベースの指標およびディメンションです。 実際、既に追加されています。

プロパティにコア拡張機能を追加し、インターフェイスに表示されるモバイルインストール手順に従って、既にライフサイクル指標を有効にしています。 これらの指標およびディメンション（アプリのバージョン、アクションを実行したユーザー数、OSのバージョン、時間分割、前回使用からの日数など、環境およびアプリ固有の指標を含む）。 は、アプリの分析に非常に役立ちます。特に、アプリからAnalyticsセグメントを作成して、すべてのレポートに適用する際に役立ちます。 指標の完全なリストは、ドキュメントで参照でき [ます](https://docs.adobe.com/content/help/en/mobile-services/ios/metrics.html)。

### Analyticsのライフサイクルヒットの表示

ライフサイクルヒットは、任意のデバッグプログラム/パケットスニファーで確認できますが、Xcodeデバッグコンソールで表示するだけです。

1. Xcodeでプロジェクトを構築して実行し、シミュレーターを起動します
1. Xcodeデバッグコンソールで、下部 `lifecycle` のフィルターに入力して表示内容を制限し、エントリの一番下までスクロールします
1. セクションに注意し `Analytics request was sent with body` てください
1. ライフサイクル指標には、AppID、CarrierName、DayOfWeek、DaysSinceFirstUse、およびドキュメントに記載されているその他の指標/ディメンションが含ま [れます](https://docs.adobe.com/content/help/en/mobile-services/ios/metrics.html)

   ![ライフサイクルヒットのデバッグ](images/ios/objective-c/mobile-analytics-lifecycleHitDebugging.png)

## ACPCoreライブラリの読み込み

次の演習では、APIを使用して、アプリ内の状態(「trackState」)とアクション(「trackAction」)を追跡します。 これらのAPIを使用するには、それらを含むライブラリを読み込む必要があります。  新しいExperience CloudプラットフォームモバイルSDKでは、trackStateおよびtrackAction APIがAnalyticsライブラリからコアライブラリに移行され、Adobe Analyticsトラッキング以外の目的でこれらのAPIを利用できるようになりました。

このチュートリアルでは、1つの状態のみを追跡しますが、実際のアプリでは、複数の状態を追跡する必要があります。

**ACPCoreライブラリを読み込むには**

1. XcodeでBookingViewController.mファイルを開く
1. ファイルの先頭（通常は他のインポート文と並べて）に、 `#import "ACPCore.h"`
1. 保存
1. これで、このファイルでtrackState APIまたはtrackAction APIを使用する準備が整いました

   <!--![Import ACPCore to File](images/ios/objective-c/mobile-analytics-importACPCoreToFile.png)-->

## 状態の追跡

アプリには、ユーザーに提供する様々な画面のコンテンツが含まれる場合があります。 これは、Webサイト上のページと同じです。 Adobe Analyticsでは、これらの「ページビューヒット」を送信し、Webプロパティと同じレポートに表示する方法が用意されています。 このメソッドは「trackState」と呼ばれます。

このチュートリアルでは、trackState呼び出しのコードをアプリ内の1つの画面（ページ）にのみ配置します。 実際には、アプリ内の他のすべての画面/状態にこの画像を複製します。 また、ヒットと共にデータを送信するいくつかの異なる方法（キー/値のペア）を調べます。

以下に、このチュートリアルまたは独自のアプリケーションでコピー&amp;ペーストできるドキュメントの構文とコード例を示します。

**構文：**

```objective-c
+ (void) trackState: (nullable NSString*) state data: (nullable NSDictionary*) data;
```

**例：**

```objective-c
[ACPCore trackState:@"state name" data:@{@"key":@"value"}];
```

### データなしの状態の追跡

1. Xcodeでサンプルアプリを開き、BookingViewController.mに移動し、関数 `viewDidLoad()` にtrackStateメソッド呼び出しを追加します
1. を「ホーム `state name` 画面」に設定します。
1. 追加のデータを追加する代わりに、メソッド呼び出し `null` でプレースホルダーとして追加します
1. または、次の場所にコピーして貼り付けます。

   ```objective-c
   [ACPCore trackState:@"Home Screen" data:nil];
   ```

   ![基本trackState呼び出し](images/ios/objective-c/mobile-analytics-basicTrackState2.png)

>[!NOTE] TargetのVECを実装するためのレッスンを完了した場合、viewDidLoad()関数に追加のコードが含まれ、この実習のスクリーンショットには示されていません。 これは、目の前のタスクに焦点を当てるためのものです。

**trackStateを検証するには**

1. プロジェクトを保存、構築、実行する
1. シミュレーターが実行され、アプリケーションのホーム画面が開いたら、Xcodeコンソールを表示します
1. コンソールを「home」を含むエントリにフィルターし、下部のエントリを見ると、 `Analytics request was sent with body`
1. pageName変数はに設定され、他のカスタムデ `Home Screen`ータペアは存在しないことに注意してください。 技術的には、「ページ名」ではなく「状態名」を設定しますが、Webサイトの実装との一貫性を保つために `pageName` 使用されるパラメーター名が使用されます。

   ![基本trackState結果](images/ios/objective-c/mobile-analytics-basicTrackStateResult1.png)

### データを使用した状態の追跡

1. BookingViewController.mに戻り、関数内で、最後の演習 `viewDidLoad()` からの基本的な（データが追加されていない）trackState呼び出しをコメントアウト（または削除）します
1. キーと値を使用して、今回は新しいtrackStateメソッド呼び出し `key1` を追加し `value1` ます。
1. 「ホーム画 `state name` 面」のままにします。
1. または、次の場所にコピー&amp;ペーストします。

   ```objective-c
   [ACPCore trackState:@"Home Screen" data:@{@"key1":@"value1"}];
   ```

   ![基本trackState呼び出し](images/ios/objective-c/mobile-analytics-trackStateWithData2.png)

**trackStateをデータで検証するには**

1. プロジェクトを保存、構築、再実行する
1. シミュレーターが実行され、アプリケーションのホーム画面が開いたら、Xcodeコンソールを表示します
1. フィルターを「home」のままにし、下のエントリを見ると、 `Analytics request was sent with body`
1. ここで、pageNameが設定されている以外に、ヒット時に送信されたキーと値のペアもあることを確認します。

   ![基本trackState結果](images/ios/objective-c/mobile-analytics-trackStateWithDataResult1.png)

>[!NOTE] Analyticsの「propとeVar」に詳しい場合は、これらの変数名がSDKに含まれていないことに注意してください。 SDKからのすべてのキー/値データは [contextData変数として送信されるので](https://docs.adobe.com/content/help/en/analytics/implementation/javascript-implementation/variables-analytics-reporting/context-data-variables.html)、Analytics UIの処理ルールを使用して、propまたはeVar（または他の変数）にマッピングする必要があります [](https://docs.adobe.com/content/help/en/analytics/admin/admin-tools/processing-rules/processing-rules.html) 。

### その他のデータ送信オプション

前の2つの練習では、2つのリクエストを行いました。1つは追加のデータを持ち、もう1つはリクエストを行わなかったものです。 ただし、画面または状態の読み込みを使用して複数のデータポイントをAnalyticsに送信する場合はどうなりますか。 次に2つのオプションを示します。

#### オプション1:複数のキー/値のペア

trackState呼び出しでは、複数のキー/値のペアを、データセット内でコンマで区切って送信するオプションがあります。 以下に例を示します。

```objective-c
[ACPCore trackState:@"Home Screen" data:@{@"key1":@"value1",@"key2":@"value2",@"key3":@"value3"}];
```

#### オプション2:辞書オブジェクト

また、コード内にディクショナリを定義し、trackStateと共にそのディクショナリを送信することもできます。 もちろん、コードに既にいくつかの辞書オブジェクトを定義しており、それらをAnalyticsに送信する場合は、これが最適なオプションとなります。 以下に例を示します。

```objective-c
NSDictionary *theStuff = @{@"key1": @"value1",@"key2": @"value2"};
[ACPCore trackState:@"Home Screen" data:theStuff];
```

**Extra Credit** Go aget and try the the the options out in your code, viewing the results in the Xcode debugging console. 以前と同じフィルターを使用し、結果を確認して、変数と値が取得されていることを確認できます

## アクションの追跡

Webサイトでのページ読み込み以外のアクションの追跡と同様に、多くの場合、ユーザーがアプリ内でとるアクション（別の画面を読み込まないもののクリックなど）を追跡します。 これは、このメソッドが呼び出される点を除いて、上で使用したtrackStateと非常に似ています `trackAction`。

以下に、このチュートリアルまたは独自のアプリケーションでコピー&amp;ペーストできるドキュメントの構文とコード例を示します。

**構文：**

```objective-c
+ (void) trackAction: (nullable NSString*) action data: (nullable NSDictionary*) data;
```

**例：**

```objective-c
[ACPCore trackAction:@"action name" data:@{@"key":@"value"}];
```

### 「停止なし」チェックボックスを使用したインタラクションの追跡

このサンプルバス予約アプリには、検索結果をオプションに限定するかどうかをユーザーが決定できるチェックボックスがあります。 Adobe Analyticsで、そのチェックボックスを使用したインタラクションを追跡することを決定しました。

![NonStopチェックボックス](images/mobile-analytics-nonstopCheckbox.png)

このチェックボックスは、サンプルプロジェクトのBookingViewController.mファイルで制御します。 この練習では、ユーザーがチェックボックスをオンまたはオフにするたびにtrackActionヒットを送信します。

#### trackActionコードの設定

1. Xcodeでサンプルプロジェクトを開き、BookingViewController.mに移動し、「nonStopButtonToggled」関数を探します。
1. ステートメン `if` トで、最初のセクションで既に選択されている場合は、そのボックスの選択を解除します。 このシナリオでは、次のコードを使用して、値が「off」のヒットを送信します。

   ```objective-c
   [ACPCore trackAction:@"NonStop Button Interaction" data:@{@"NonStop":@"off"}];
   ```

1. 次のセクション（「else」セクション）で、チェックボックスがまだオンになっていない場合は、チェックボックスをオンにします。 このシナリオでは、次のコードを使用して、値が「on」のヒットを送信します。

   ```objective-c
   [ACPCore trackAction:@"NonStop Button Interaction" data:@{@"NonStop":@"on"}];
   ```

コード内のその他のカスタマイズに注意してください。

* を「NonStop Button Interaction」 `action name` に設定している。 この値は、リクエストの「アクション」パラメーターと、Adobe Analyticsのカスタムリンクレポート/ディメンションに入力されます
* 使用してい `key` るのは「NonStop」です。 これは、Analytics管理コンソールの処理ルールで検索できるキー名です。これらの値をpropまたはeVarにマップできます。

関数は次のようになります。

![NonStopチェックボックス](images/ios/objective-c/mobile-analytics-nonStopButtonCode2.png)

#### trackActionコードを検証するには

1. コードを追加したら、プロジェクトを保存し、実行してビルドします
1. ごみ箱アイコンをクリックしてコンソールをクリアします
1. シミュレーターのチェックボックスをオンにすると、コンソールに2つのリクエストが表示されます。 最後の1つは、先ほど追加したコードからAdobe Analyticsにデータを送信することです。
1. actionパラメーターとpev2パラメーターの両方が「NonStop Button Interaction」（エンコードされたスペースを含む）に設定されています。
1. 「NonStop=on」キー/値のペアが存在し、処理ルールでprop/eVarに割り当てることができます。
1. 「pe=lnk_o」キー/値が、trackActionによってトリガーされた「カスタムリンク」ヒットであることを示しています。

   ![trackAction結果（デバッガー）](images/ios/objective-c/mobile-analytics-trackActionResult1.png)

いい仕事！ Analyticsのレッスンが完了しました。 もちろん、Analyticsの実装を強化するためには他にも多くのことができますが、それによって、残りのニーズに対処するために必要なコアスキルが得られれば幸いです。

## trackStateとtrackActionのその他の利点

この最後の練習では、trackState APIとtrackAction APIを使用して、アプリからAdobe Analyticsにデータを送信できました。 Experience Platform Mobile SDKは起動に根付いているので、起動インターフェイスでは、先ほど追加したコードを活用して、さらに多くのことを実行できます。

起動では、trackState APIとtrackAction APIによってトリガーされるルールを作成し、他のアドビソリューションや外部パートナーにリクエストするなど、追加のアクションを実行させることができます。

[次の「Adobe Audience Managerを追加」/](audience-manager.md)
