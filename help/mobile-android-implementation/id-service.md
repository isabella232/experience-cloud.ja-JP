---
title: Launch を使用した Adobe Experience Platform ID サービスの実装
description: Adobe Experience Platform ID サービス拡張機能を追加し、「Set Customer IDs」アクションを使用して顧客 ID を収集する方法について説明します。このレッスンは、「モバイル Android アプリケーションでの Experience Cloud の実装」チュートリアルの一部です。
seo-description: null
seo-title: Launch を使用した Adobe Experience Platform ID サービスの実装
solution: Experience Cloud
translation-type: ht
source-git-commit: e9dee6d0aa3b775d0ce617e2b2c57b56de491b8d

---


# Adobe Experience Platform ID サービスの追加

[Adobe Experience Platform ID サービス](https://docs.adobe.com/content/help/ja-JP/id-service/using/home.html)は、ソリューション間でのオーディエンス共有などの Experience Cloud 機能を強化するために、すべてのアドビソリューションに共通の訪問者 ID を設定します。また、サービスに独自の顧客 ID を送信して、クロスデバイスターゲティングを有効にし、顧客関係管理（CRM）システムとのさらなる統合を可能にすることもできます。

ID サービスは Mobile Core 拡張機能の一部なので、***Mobile SDK のインストール時に既に実装済みです***。現時点では、このチュートリアルに顧客 ID の設定手順は含まれていません。[顧客 ID の設定方法](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core/identity/identity-api-reference)の詳細については、ドキュメントを参照してください。

## 検証手順

ID サービスの呼び出しを検証するには、Android Studio でサンプルアプリケーションを実行し、デバッグコンソールを開いて、要求と応答を探します。

1. **ID サービスへの要求**（Logcat を `IdentityExtension` にフィルター）。この例では、ID（`d_mid`）は既に設定済みで、再度レポートされています。

   ```java
   03-14 17:01:18.526 7743-7803/com.adobe.busbooking D/ADBMobile: IdentityExtension - Sending request (https://dpm.demdex.net/id?d_mid=59651426340521082405908216148091920022&d_ver=2&d_orgid=7ABB3E6A5A7491460A495D61%40AdobeOrg)
   ```

1. **ID サービスからの応答**（Logcat を`IdentityExtension` にフィルター）。`mid`値が上記の要求内の値`d_mid`とどのように一致するかを確認します。

   ```java
   03-14 17:01:19.033 7743-7803/com.adobe.busbooking D/ADBMobile: IdentityExtension - Received ID response (mid: 59651426340521082405908216148091920022, blob: j8Odv6LonN4r3an7LhD3WZrU1bUpAkFkkiY1ncBR96t2PTI, hint: 9, ttl: 604800
   ```

[次：「Adobe Target VEC サポートの追加」&gt;](target-vec.md)
