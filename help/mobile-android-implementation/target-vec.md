---
title: Adobe Target の Visual Experience Composer（VEC）の追加
description: Target の Visual Experience Composer（VEC）をカスタムパラメーターで実装し、デバイスとペアリングして、アクティビティを構築する方法について説明します。このレッスンは、「モバイル Android アプリケーションでの Experience Cloud の実装」チュートリアルの一部です。
seo-description: null
seo-title: Adobe Target の Visual Experience Composer（VEC）の追加
solution: Experience Cloud
translation-type: ht
source-git-commit: 79d5274d09d66b80a49aba5db3e0a997d0f0ff61

---


# Adobe Target の Visual Experience Composer（VEC）の追加

このレッスンでは、モバイルアプリケーションに対して Target Visual Experience Composer（VEC） を有効にします。

[Adobe Target](https://docs.adobe.com/content/help/ja-JP/target/using/target-home.html) は、顧客のエクスペリエンスのカスタマイズやパーソナライズに必要なすべてのツールを提供する Adobe Experience Cloud ソリューションです。Web サイト、モバイルサイト、アプリケーション、ソーシャルメディアおよびその他のデジタルチャネルでの収益性を最大化することができます。

ネイティブモバイルアプリ向け Visual Experience Composer（VEC）を使用すると、開発やリリースのサイクルに依存することなく、マーケターが自らアクティビティを作成したりコンテンツをパーソナライズすることができます。

レッスン「[拡張機能の追加](launch-add-extensions.md)」では、Launch プロパティに Target VEC 拡張機能を追加しました。レッスン「[Mobile SDK のインストール](launch-install-the-mobile-sdk.md)」では、サンプルアプリケーションに拡張機能を読み込みました。Target の Mobile Visual Experience Composer でアクティビティの設定を開始するには、若干のマイナーアップデートのみが必要です。

>[!IMPORTANT] モバイルアプリケーションで Target VEC を使用するには、Target と Target VEC の両方の Launch 拡張機能が必要です。

## 学習内容

このレッスンを最後まで学習すると、以下の内容を習得できます。

* Target VEC サンプルアプリケーションの有効化
* Target VEC 要求へのパラメーターの追加
* デバイスと VEC のペアリング
* VEC を使用したアクティビティの作成

## 前提条件

この節のレッスンを完了するには、次が必要です。

* 「[Launch の設定](launch-create-a-property.md)」節のレッスンを完了していること
* Adobe Target インターフェイスへの承認者レベルのアクセス権を持っていること

## アプリケーションの読み込み要求

Target は、Target VEC 拡張機能の設定時に選択した設定により、アプリケーションが最初に読み込まれる際に「アプリケーションの読み込み」要求を実行します。この要求は、アプリケーション用に作成したすべての Target VEC アクティビティを事前取得します。

Android Studio で、Logcat を「Target r」にフィルターして、Target のリクエストと応答を表示します。アプリケーション名とバージョンのパラメーターに注意してください。作成するすべての Target VEC アクティビティは、自動的にこれらのプロパティをターゲットにします。

![Target VEC 要求の表示](images/android/mobile-targetvec-viewLogs.png)

## パラメーターの追加

前の演習で見たように、アプリケーションのライフサイクル指標は Target VEC 要求パラメーターとして自動的に含まれます。また、要求に対して、グローバルに、またはアプリケーション内の特定のビューに対して、カスタムパラメーターを追加することもできます。

**カスタムパラメーターをグローバルに追加するには、以下を実行します。**

1. Android Studio で、`DemoApplication` ファイルを開きます。
1. 既存のインポートの下に `import ACPTargetVEC` を追加して、Target VEC 拡張機能をインポートします。
1. 拡張機能が登録される前に、`onCreate()` 関数に次のサンプルコードを追加します。次のコード例は、標準パラメーター、プロファイルパラメーター、製品（またはエンティティ）パラメーター、注文パラメーターを TargetVEC 要求に追加する方法を示しています。この例では静的な値を使用しますが、実際のアプリケーションでは動的変数を使用して値を入力すると考えられます。もちろん、すべてのビューに関連するパラメーターのみを設定する必要があります。

   ```java
   Map<String, String>targetParams = new HashMap<>(); //params
   targetParams.put( "param1", "value1");
   Map<String, String>taregtProfileParams = new HashMap<>(); //profile params
   taregtProfileParams.put("profilekey1","profilevalue1");
   
   TargetVEC.setGlobalRequestParameters(new TargetParameters.Builder()
            .parameters(targetParams)
            .profileParameters(taregtProfileParams)
            .product(new TargetProduct("1234", "furniture"))
            .order(new TargetOrder("12343", 123.45, Arrays.asList("100", "200")))
            .build());
   ```

1. 上記のパラメーターコードでは次のインポートが必要で、ファイルに追加する必要があるので、Android Studio でエラーが発生する場合があります。

   ```java
   import com.adobe.marketing.mobile.TargetOrder;
   import com.adobe.marketing.mobile.TargetProduct;
   import com.adobe.marketing.mobile.TargetParameters;
   import java.util.Arrays;
   import java.util.Map;
   import java.util.HashMap;
   ```

   ![TargetVEC 要求へのパラメーターの追加](images/android/mobile-targetvec-addParameters.png)

アプリケーションにパラメーターが追加されたので、次に要求でパラメーターが渡されていることを確認します。

**パラメーターを検証するには、以下を実行します。**

1. Android Studio プロジェクトを保存します。
1. アプリケーションを再構築し、エミュレーターでアプリケーションが再び開くまで待ちます。
1. Android Studio の Logcat ペインを開きます。
1. 「Target r」を含むすべての文を表示するようにフィルターします。
1. 追加したカスタムパラメーターは、要求に表示されるはずです。

   ![TargetVEC 要求へのパラメーターの検証](images/android/mobile-targetvec-verifyParams.png)

特定のビューでパラメーターを渡す方法の詳細および詳細については、[ドキュメント](https://docs.adobe.com/content/help/ja-JP/target/using/implement-target/mobile-apps/composer/mobile-visual-experience-composer-android.html#parameters)を参照してください。

## モバイルアプリケーションと Target インターフェイスのペアリング

Target インターフェイスで VEC アクティビティを作成するには、まず Target とアプリケーションをペアにする必要があります。このペアリングは、ディープリンクを使用して実現されます。

### ディープリンクの作成

Android では、[ディープリンクと Android アプリケーションリンク](https://developer.android.com/training/app-links/deep-linking)を使用して、アプリケーション内の特定の場所に直接移動する URL を作成できます。これらはおそらく、既にアプリケーションで使用されています。その場合は、既存の URL 構造を使用して Target とペアリングできます。このチュートリアルでは、バス予約アプリで事前に定義されたディープリンクを確認し、それが機能していることを確認してから、アプリをモバイルアプリ用の Target VEC とペアリングします。

**ディープリンクの設定を確認するには、以下を実行します。**

1. Android Studio で、AndroidManifest.xml ファイルを開きます。
1. バス予約アプリケーションのディープリンクスキームに対して、既にインテントフィルターが設定されています。
1. `Host` と `Scheme` は、それぞれ既に`com.adobe.example.busbooking` と `http` に設定されています。つまり、`http://com.adobe.example.busbooking` のような URL は、エミュレーターで開いたときにサンプルアプリケーションを自動的に開く必要があります。

   ![URL スキームの登録](images/android/mobile-targetvec-registerScheme.png)

次の手順で、ディープリンクスキームが機能していることを確認します。

### ディープリンクの確認

次に、ディープリンクがエミュレーターでアプリを開くことを確認します。「adb」コマンドは確実に使用できるので、好きなように実行できます。

**adb（Mac®）を使用してディープリンクを検証するには、以下を実行します。**

1. Android エミュレーターが実行されていることを確認します。
1. 既にバス予約アプリケーションが開いている場合は閉じます。
1. ターミナルウィンドウを開きます。
1. Android プラットフォームツールディレクトリに移動します。`cd Library/Android/sdk/platform-tools/`
1. エミュレーターが接続されていることを確認します。`./adb devices`
1. adb シェルを開きます。`./adb shell`
1. adb シェルで、ディープリンクをテストします。`am start -W -a android.intent.action.VIEW -d "http://com.adobe.example.busbooking" "com.adobe.busbooking"`
1. バス予約アプリケーションがエミュレーターで起動したことを確認します。

   ![ディープリンクの確認](images/android/mobile-targetvec-verifyDeepLink.png)

これで、ディープリンク構造が設定され、Target VEC を使用してアクティビティを設定する準備が整いました。

## Mobile VEC でのアクティビティの作成

次に、Target インターフェイスでアクティビティを作成します。

**Target VEC を使用してアクティビティを作成するには、以下を実行します。**

1. [Adobe Experience Cloud](https://experiencecloud.adobe.com) にログインします。
1. ソリューション切り替えボタンを使用して Target に移動します。

   ![ディープリンクの確認](images/mobile-targetvec-goToTarget.png)

1. Target を起動します。

   ![ディープリンクの確認](images/mobile-targetvec-launchTarget.png)

1. 「**[!UICONTROL アクティビティを作成]**」ボタンをクリックして、「**[!UICONTROL A/B テスト]**」を選択します。
1. 「**[!UICONTROL モバイルアプリケーション]**」を選択します。
1. 「**[!UICONTROL Experience Composer を選択]**」で「**[!UICONTROL ビジュアル]**」が選択されていることを確認します。
1. 「**[!UICONTROL 次へ]**」ボタンをクリックします。

   ![Mobile VEC を使用した A/B アクティビティの作成](images/mobile-targetvec-createActivity.png)

1. 「**[!UICONTROL 使用するアプリを選択]**」画面で、「**[!UICONTROL 新しいアプリケーションを追加]**」をクリックします。

   ![新しいアプリケーションの追加](images/mobile-targetvec-addNewApp.png)

1. 定義したばかりの URL スキームを「**[!UICONTROL URL スキームを入力]**」フィールドに入力します（例：`http://com.adobe.example.busbooking/`）。
1. 「**[!UICONTROL ディープリンクを作成]**」をクリックします。

   ![URL スキームを入力し、ディープリンクを作成する](images/android/mobile-targetvec-enterURLScheme.png)

   >[!NOTE] アプリケーションにディープリンクを送信する方法はいくつかあります。次のことができます。
   >
   >   1. 有効な電子メールアドレスにディープリンクを電子メールで送信し、デバイス上の電子メールアプリケーションでリンクを開く。
   >   1. Android デバイスから QR コードの写真を撮影する（このチュートリアルでは、デバイスを Android Studio にリンクする必要があります）。
   >   1. Target インターフェイスからディープリンクをコピーし、デバイスに送信します。


1. 「**[!UICONTROL リンクをコピーして送信]**」タブをクリックします。
1. 生成された URL をクリックします（URL をクリックすると、クリップボードに自動的にコピーされます）。

   ![URL をコピーする](images/android/mobile-targetvec-copyURL.png)

1. ターミナルウィンドウを開きます（または、まだ開いている場合はターミナルウィンドウに戻ります）。
1. Android プラットフォームツールディレクトリに移動します（既にそこにいる場合もあります）。`cd Library/Android/sdk/platform-tools/`
1. エミュレーターが接続されていることを確認します。`./adb devices`
1. adb シェルを開きます。`./adb shell`
1. adb シェルで、次のコマンドの [YOUR_TARGET_URL_WITH_TOKEN] を、クリップボードにコピーしたURLに置き換えます：`am start -W -a android.intent.action.VIEW -d "[YOUR_TARGET_URL_WITH_TOKEN]" "com.adobe.busbooking"`
1. アプリケーションが読み込まれたら、Target を開いたブラウザータブに戻ります。アプリケーションが VEC に読み込まれていることを確認します。
1. アプリケーション内のテキストおよび画像アセットをクリックすると、編集および置き換えのオプションが表示されます。

   ![VEC でのアプリケーションの読み込み](images/android/mobile-targetvec-devicePaired.png)

   > [!TIP] モバイルデバイスでディープリンクを開いた後に、Mobile VEC が Target インターフェイスで自動的に開かない場合は、以下を試してみてください。
   >
   >   1. Target インターフェイスで同じ URL を使用していることを確認する（誤って文字が切り取られていないようにする）。adb シェルでコマンドを実行する場合、URL が引用符で囲まれていることを確認する。
      >
      >
      >   

   1. Target VEC で必要な build.gradle ファイルに、追加の依存関係が追加されたことを確認する。これらの依存関係は、レッスン「 [Mobile SDK のインストール](https://docs.adobe.com/content/help/ja-JP/experience-cloud/implementing-in-mobile-android-apps-with-launch/configure-launch/launch-install-the-mobile-sdk.html#update-the-buildgradle-file)」で追加されているはずです。
      >
      >
      >   

   1. 下の図で詳細に説明されているように、アプリケーションに保存されているデータを消去してみる。
      >
      >       
      ![VEC でのアプリケーションの読み込み](images/android/mobile-targetvec-pairing-clearData.png)


1. アプリケーションの最初の画面に変更を加えます。
1. 次に、VEC を開いた状態でブラウザーの横にエミュレーターを配置します。
1. アプリケーション内の別の画面に移動し、エミュレーターで VEC がどのように更新されたかを確認します。
1. 1 つのアクティビティで、アプリケーション内の複数のビューを更新できます。

   ![VEC でのアプリケーションの読み込み](images/android/mobile-targetvec-navigateTheApp.png)

1. クリックトラッキング指標を視覚的に追加することもできます。
1. アクティビティを保存して承認し、サンプルアプリケーションでアクティビティが表示されることを確認します。

デバイスと VEC のペアリングは 1 回限りの操作です。同じデバイス上に、今後さらに多くのアクティビティを作成する場合、次の図に示すように、デバイスをリストから選択できます。

![保存されたデバイスの使用](images/android/mobile-targetvec-useSavedApp.png)

>[!TIP] デバイスを開いているが、選択メニューで「使用不可」になっている場合は、エミュレーターまたはデバイスでアプリケーションを閉じてから再度開いてみてください。

## ライフサイクル指標に基づくオーディエンスの作成

ライフサイクル指標は、Adobe Mobile SDK による呼び出しに自動的に含まれる、訪問者のアプリケーションの使用状況に関する組み込み指標です。これらの指標に基づいて、Target で簡単にオーディエンスを構築できます。

**オーディエンスを作成するには、以下を実行します。**

1. Target インターフェイスで、上部のナビゲーションで「**オーディエンス**」をクリックします。
1. 「**オーディエンスを作成**」ボタンをクリックします。

   ![新しいオーディエンスを作成する](images/mobile-targetvec-audienceList.png)

1. オーディエンスに名前を付けます。`Launches < 5`
1. **ルールを追加／ネットワーク**&#x200B;をクリックします。

   ![カスタムオプションを選択する](images/mobile-targetvec-custom.png)

1. 最初のドロップダウンで、**a.Launches** パラメーターを選択します。すべてのライフサイクル指標パラメーターはプレフィックス「a」で始まります。ユーザーが持つアプリケーションの起動数に基づいてコンテンツをターゲット設定します。これは、ユーザーに説明的な初回ユーザーエクスペリエンス（FTUE）を提供し、アプリケーションの初回ユーザーをターゲットにする優れた方法です。
1. 次のドロップダウンで、**次より小さい**&#x200B;を選択します。
1. 3 番目のドロップダウンに、**5** と入力します。
1. **保存**&#x200B;をクリックします。

   ![Launches のオーディエンス &lt;= 4](images/mobile-targetvec-LaunchesLessThan5.png)

Target には、あらかじめ用意されている様々なオーディエンス作成オプションがあります。また、Target のオーディエンス作成要求でカスタムデータを送信したり、Audience Manager や Analytics など他の Experience Cloud ソリューションから共有されたオーディエンスを使用したり、People コアサービスの顧客属性機能を使用して Target に共有された CRM データを使用したりできます。

[次：「Adobe Target の追加」&gt;](target.md)