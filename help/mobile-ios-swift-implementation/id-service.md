---
title: Launch を使用した Adobe Experience Platform ID サービスの実装
description: Adobe Experience Platform ID サービス拡張機能を追加し、「Set Customer IDs」アクションを使用して顧客 ID を収集する方法について説明します。このレッスンは、「モバイル iOS Swift アプリケーションでの Experience Cloud の実装」チュートリアルの一部です。
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

ID サービスの呼び出しを検証するには、Xcode または Developer Studio でサンプルアプリケーションを実行し、デバッグコンソールを開いて、要求と応答を探します。

1. **ID サービスへの要求**（コンソールを `demdex.net` にフィルター）。この例では、ID（`d_mid`）は既に設定済みで、再度レポートされています。

   ```swift
   2019-01-15 12:11:45.164590-0500 BusDemoSwift[52399:5056322] [AMSDK DEBUG <com.adobe.module.identity>]: Sending request (https://dpm.demdex.net/id?d_rtbd=json&d_ver=2&d_orgid=7ABB3E6A5A7491460A495D61@AdobeOrg&d_mid=17179986463578698626041670574784107777&d_blob=j8Odv6LonN4r3an7LhD3WZrU1bUpAkFkkiY1ncBR96t2PTI&dcs_region=9)
   ```

1. **ID サービスからの応答**（コンソールを `ID Service` にフィルター）。`mid`値が上記の要求内の値`d_mid`とどのように一致するかを確認します。

   ```swift
   2019-01-15 12:11:45.681821-0500 BusDemoSwift[52399:5056322] [AMSDK DEBUG <com.adobe.module.identity>]: ID Service - Got ID Response (mid: 17179986463578698626041670574784107777, blob: j8Odv6LonN4r3an7LhD3WZrU1bUpAkFkkiY1ncBR96t2PTI, hint: 9, ttl: "604800000 ms")
   
[次：「Adobe Target VEC サポートの追加」&gt;](target-vec.md)
