---
title: Launch を使用した Adobe Analytics の実装
description: Adobe Analytics Launch 拡張機能を使用した Adobe Analytics の実装方法、スクリーンビュービーコンの送信方法、変数の追加方法、イベントのトラッキング方法、プラグインの追加方法について説明します。このレッスンは、「モバイル iOS Objective-C アプリケーションでの Experience Cloud の実装」チュートリアルの一部です。
seo-description: null
seo-title: Launch を使用した Adobe Analytics の実装
solution: Experience Cloud
translation-type: ht
source-git-commit: e9dee6d0aa3b775d0ce617e2b2c57b56de491b8d

---


# Adobe Analytics の追加

このレッスンでは、アプリケーションで Adobe Analytics のトラッキングを有効にします。

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

ライフサイクル指標は、Experience Platform Mobile SDK を使用してアプリケーションで簡単に有効にできる、環境ベースの指標およびディメンションです。実際、既に追加されています。

プロパティにコア拡張機能を追加し、インターフェイスに表示されるモバイルインストール手順に従って、既にライフサイクル指標を有効にしています。これらの指標およびディメンション（アプリのバージョン、アクションを実行したユーザー数、OS のバージョン、時間分割、前回使用からの日数など、環境およびアプリ固有の指標を含む）は、特に、アプリから Analytics セグメントを作成してすべてのレポートに適用する際に、アプリの分析に非常に役立ちます。指標の完全なリストは、[ドキュメント](https://docs.adobe.com/content/help/ja-JP/mobile-services/ios/metrics.html)で参照できます。

### Analytics のライフサイクルヒットの表示

ライフサイクルヒットは、任意のデバッグプログラムやパケットスニファーで確認できますが、ここでは単に Xcode デバッグコンソールで表示します。

1. Xcode でプロジェクトをビルドして実行し、シミュレーターを起動します。
1. Xcode デバッグコンソールで、下部のフィルターに `lifecycle` と入力して表示内容を制限し、一番下までスクロールします。
1. 「`Analytics request was sent with body`」セクションに注意してください。
1. ライフサイクル指標には、AppID、CarrierName、DayOfWeek、DaysSinceFirstUse、および[ドキュメント](https://docs.adobe.com/content/help/ja-JP/mobile-services/ios/metrics.html)に記載されているその他の指標／ディメンションが含まれます。

   ![ライフサイクルヒットのデバッグ](images/ios/objective-c/mobile-analytics-lifecycleHitDebugging.png)

## ACPCore ライブラリの読み込み

次の演習では、API を使用して、アプリケーション内の状態（「trackState」）とアクション（「trackAction」）を追跡します。これらの API を使用するには、それらを含むライブラリを読み込む必要があります。新しい Experience Cloud Platform Mobile SDK では、trackState および trackAction API が Analytics ライブラリから Core ライブラリに移行され、Adobe Analytics トラッキング以外の目的でこれらの API を利用できるようになりました。

このチュートリアルでは、1 つの状態のみを追跡しますが、実際のアプリでは、複数の状態を追跡する必要があります。

**ACPCore ライブラリを読み込むには、以下を実行します。**

1. Xcode で BookingViewController.m ファイルを開きます。
1. ファイルの先頭（通常は他のインポート文と並べて）に、`#import "ACPCore.h"` を追加します。
1. 保存します。
1. これで、このファイルで trackState API と trackAction API を使用する準備が整いました。

   <!--![Import ACPCore to File](images/ios/objective-c/mobile-analytics-importACPCoreToFile.png)-->

## 状態の追跡

アプリケーションには、ユーザーに提供する様々な画面のコンテンツが含まれる場合があります。これは、Web サイト上のページと同じです。Adobe Analytics では、これらの「ページビューヒット」を送信し、Web プロパティと同じレポートに表示するメソッドが用意されています。このメソッドは「trackState」と呼ばれます。

このチュートリアルでは、trackState 呼び出しのコードをアプリケーション内の 1 つの画面（ページ）にのみ配置します。実際には、アプリケーション内の他のすべての画面と状態にこの画像を複製します。また、ヒットと共にデータを送信するいくつかの異なる方法（キーと値のペア）を調べます。

以下に、このチュートリアルまたは独自のアプリケーションでコピー＆ペーストできるドキュメントの構文とコード例を示します。

**構文：**

```objective-c
+ (void) trackState: (nullable NSString*) state data: (nullable NSDictionary*) data;
```

**例：**

```objective-c
[ACPCore trackState:@"state name" data:@{@"key":@"value"}];
```

### データを伴わない状態の追跡

1. Xcode でサンプルアプリを開き、BookingViewController.m に移動し、関数 `viewDidLoad()` に trackState メソッド呼び出しを追加します。
1. `state name` を「Home Screen」に設定します。
1. さらにデータを追加する代わりに、メソッド呼び出しでプレースホルダーとして `null` を追加します。
1. または、次の場所にコピー＆ペーストします。

   ```objective-c
   [ACPCore trackState:@"Home Screen" data:nil];
   ```

   ![基本的な trackState の呼び出し](images/ios/objective-c/mobile-analytics-basicTrackState2.png)

>[!NOTE] Target の VEC を実装するためのレッスンを完了している場合、viewDidLoad() 関数には、この演習のスクリーンショットには示されていない、追加のコードが含まれています。これは、目の前のタスクに焦点を当てるためのものです。

**trackState を検証するには、以下を実行します。**

1. プロジェクトを保存、ビルド、実行します。
1. シミュレーターが実行され、アプリケーションのホーム画面が開いたら、Xcode コンソールを表示します。
1. コンソールを「home」を含むエントリにフィルターし、下部のエントリを見ると、`Analytics request was sent with body` が示されています。
1. pageName 変数は `Home Screen` に設定され、他のカスタムデータペアは存在しません。技術的には、「ページ名」ではなく「状態名」を設定しますが、Web サイトの実装との一貫性を保つために、パラメーター名 `pageName` が使用されます。

   ![基本的な trackState の結果](images/ios/objective-c/mobile-analytics-basicTrackStateResult1.png)

### データを伴う状態の追跡

1. BookingViewController.m に戻り、`viewDidLoad()`関数内で、最後の演習からの基本的な（データが追加されていない）trackState の呼び出しをコメントアウト（または削除）します。
1. キーとして `key1`、値として `value1` を使用して、今回は新しい trackState メソッドの呼び出しを追加します。
1. `state name` を「Home Screen」のままにします。
1. または、次の場所にコピー＆ペーストします。

   ```objective-c
   [ACPCore trackState:@"Home Screen" data:@{@"key1":@"value1"}];
   ```

   ![基本的な trackState の呼び出し](images/ios/objective-c/mobile-analytics-trackStateWithData2.png)

**データを伴う trackState を検証するには、以下を実行します。**

1. プロジェクトを再び保存、ビルド、実行します。
1. シミュレーターが実行され、アプリケーションのホーム画面が開いたら、Xcode コンソールを表示します。
1. フィルターを「home」のままにして下部のエントリを見ると、`Analytics request was sent with body` が示されています。
1. ここで、pageName が設定されている以外に、ヒット時に送信されたキーと値のペアもあることを確認します。

   ![基本的な trackState の結果](images/ios/objective-c/mobile-analytics-trackStateWithDataResult1.png)

>[!NOTE] Analytics の「prop と eVar」に詳しい人は、これらの変数名が SDK に含まれていないことに気が付くかもしれません。SDK からのすべてのキー／値データは [contextData](https://docs.adobe.com/content/help/ja-JP/analytics/implementation/javascript-implementation/variables-analytics-reporting/context-data-variables.html) 変数として送信されるので、Analytics UI の[処理ルール](https://docs.adobe.com/content/help/ja-JP/analytics/admin/admin-tools/processing-rules/processing-rules.html)を使用して、prop または eVar（または他の変数）にマッピングする必要があります。

### その他のデータ送信オプション

前の 2 つの演習では、2 つの要求を実行しました。1 つは追加のデータを持ち、もう 1 つは追加のデータを持ちませんでした。しかし、画面または状態の読み込みを使用して複数のデータポイントを Analytics に送信する場合はどうなるのでしょうか。次に 2 つのオプションを示します。

#### オプション 1：複数のキーと値のペア

trackState の呼び出しでは、複数のキーと値のペアを、データセット内でコンマで区切って送信するオプションがあります。以下に例を示します。

```objective-c
[ACPCore trackState:@"Home Screen" data:@{@"key1":@"value1",@"key2":@"value2",@"key3":@"value3"}];
```

#### オプション 2：Dictionary オブジェクト

また、コード内に辞書を定義し、trackState と共にその辞書を送信することもできます。もちろん、コードに既にいくつかの辞書オブジェクトを定義しており Analytics に送信している場合は、これが最適なオプションとなります。以下に例を示します。

```objective-c
NSDictionary *theStuff = @{@"key1": @"value1",@"key2": @"value2"};
[ACPCore trackState:@"Home Screen" data:theStuff];
```

**ボーナスポイント**
コードでこれら 2 つのオプションを試してください。Xcode デバッグコンソールで結果を表示し、コードでこれら 2 つのオプションを試してください。以前と同じフィルターを使用し、結果を見てし、変数と値が取得されていることを確認できます。

## アクションの追跡

Web サイトでのページ読み込み以外のアクションの追跡と同様に、多くの場合、ユーザーがアプリケーション内でとるアクション（別の画面を読み込まないもののクリックなど）を追跡します。これは、このメソッドが `trackAction` と呼ばれている点を除いて、上で使用した trackState と非常に似ています。

以下に、このチュートリアルまたは独自のアプリケーションでコピー＆ペーストできるドキュメントの構文とコード例を示します。

**構文：**

```objective-c
+ (void) trackAction: (nullable NSString*) action data: (nullable NSDictionary*) data;
```

**例：**

```objective-c
[ACPCore trackAction:@"action name" data:@{@"key":@"value"}];
```

### 「停止なし」チェックボックスを使用したインタラクションの追跡

このサンプルバス予約アプリケーションには、検索結果をオプションに限定するかどうかをユーザーが決定できるチェックボックスがあります。Adobe Analytics で、そのチェックボックスを使用したインタラクションを追跡することにしたとします。

![ノンストップチェックボックス](images/mobile-analytics-nonstopCheckbox.png)

このチェックボックスは、サンプルプロジェクトの BookingViewController.m ファイルで制御します。この演習では、ユーザーがチェックボックスをオンまたはオフにするたびに trackAction ヒットを送信します。

#### trackAction コードの設定

1. Xcode でサンプルプロジェクトを開き、BookingViewController.m に移動し、「nonStopButtonToggled」関数を探します。
1. `if` 文の最初の節では、ボックスが既に選択されている場合にその選択を解除します。このシナリオでは、次のコードを使用して、値が「off」のヒットを送信します。

   ```objective-c
   [ACPCore trackAction:@"NonStop Button Interaction" data:@{@"NonStop":@"off"}];
   ```

1. 次の節（「else」節）では、まだオンになっていない場合にチェックボックスをオンにします。このシナリオでは、次のコードを使用して、値が「on」のヒットを送信します。

   ```objective-c
   [ACPCore trackAction:@"NonStop Button Interaction" data:@{@"NonStop":@"on"}];
   ```

コード内のその他のカスタマイズに注意してください。

* `action name`を「NonStop Button Interaction」 に設定しています。この値は、要求の「アクション」パラメーターと、Adobe Analytics のカスタムリンクレポートやディメンションに入力されます。
* 使用している `key` の名前は「NonStop」です。これは、Analytics Admin Console の処理ルールで検索できるキー名です。これらの値を prop または eVar にマップできます。

関数は、以下のようになります。

![ノンストップチェックボックス](images/ios/objective-c/mobile-analytics-nonStopButtonCode2.png)

#### trackAction コードを検証するには、以下を実行します。

1. コードを追加したら、プロジェクトを保存し、実行してビルドします。
1. ごみ箱アイコンをクリックしてコンソールをクリアします。
1. シミュレーターのチェックボックスをオンにすると、コンソールに 2 つの要求が表示されます。最後の 1 つでは、先ほど追加したコードから Adobe Analytics にデータを送信します。
1. action パラメーターと pev2 パラメーターの両方が「NonStop Button Interaction」（エンコードされたスペースを含む）に設定されています。
1. 「NonStop=on」キー／値ペアが存在し、処理ルールでprop／eVarに割り当てることができます。
1. 「Pe=lnk_o」キー／値は trackAction によってトリガーされた「カスタムリンク」ヒットです。

   ![デバッガーでの trackAction の結果](images/ios/objective-c/mobile-analytics-trackActionResult1.png)

お疲れ様です。Analytics のレッスンが完了しました。もちろん、Analytics の実装を強化するためにできることは他にも多くありますが、このレッスンによって残りのニーズに対処するために必要なコアスキルが得られたなら幸いです。

## trackState と trackAction のその他の利点

この最後の演習では、trackState API と trackAction API を使用して、アプリケーションから Adobe Analytics にデータを送信しました。Experience Platform Mobile SDK は Launch に根付いているので、Launch のインターフェイスでは、先ほど追加したコードを活用して、さらに多くのことを実行できます。

Launch では、trackState API と trackAction API によってトリガーされるルールを作成し、他のアドビソリューションや外部パートナーにリクエストするなど、追加のアクションを実行させることができます。

[次：「Adobe Audience Manager の追加」&gt;](audience-manager.md)
