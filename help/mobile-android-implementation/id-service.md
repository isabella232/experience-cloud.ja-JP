---
title: Adobe Experience Platform Identity Serviceの起動での実装
description: Adobe Experience Platform Identity Service拡張を追加し、「顧客IDを設定」アクションを使用して顧客IDを収集する方法について説明します。 このレッスンは、「モバイルAndroidアプリケーションでのExperience cloudの実装」チュートリアルの一部です。
seo-description: null
seo-title: Adobe Experience Platform Identity Serviceの起動での実装
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: e9dee6d0aa3b775d0ce617e2b2c57b56de491b8d

---


# Adobe Experience Platform IDサービスの追加

Adobe Experience Platform IDサービス [は](https://docs.adobe.com/content/help/en/id-service/using/home.html) 、ソリューション間でのオーディエンス共有などのExperience cloud機能を強化するために、すべてのアドビソリューションに共通の訪問者IDを設定します。  独自の顧客IDをサービスに送信して、デバイス間のターゲット設定や顧客関係管理(CRM)システムとの統合を有効にすることもできます。

IDサービスはMobile Core拡張機能の一部なので、Mobile SDK ***のインストール時に既に実装済みです***。 現時点では、このチュートリアルに顧客IDの設定手順は含まれていません。 顧客IDの設定方法の詳細については、ドキュメ [ントを参照してください](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core/identity/identity-api-reference)。

## 検証手順

Identity Serviceの呼び出しを検証するには、Android studioでサンプルアプリケーションを実行し、デバッグコンソールを開いて、リクエストと応答を探します。

1. **IDサービスへの要求** (フィルターのログ `IdentityExtension`カット先)この例では、ID (`d_mid`)は既に設定済みで、再度レポートされています。

   ```java
   03-14 17:01:18.526 7743-7803/com.adobe.busbooking D/ADBMobile: IdentityExtension - Sending request (https://dpm.demdex.net/id?d_mid=59651426340521082405908216148091920022&d_ver=2&d_orgid=7ABB3E6A5A7491460A495D61%40AdobeOrg)
   ```

1. **IDサービスからの応答** (ログのフィルタ `IdentityExtension`ー)。 値が上記のリクエ `mid` スト内の値とど `d_mid` のように一致するかを確認します。

   ```java
   03-14 17:01:19.033 7743-7803/com.adobe.busbooking D/ADBMobile: IdentityExtension - Received ID response (mid: 59651426340521082405908216148091920022, blob: j8Odv6LonN4r3an7LhD3WZrU1bUpAkFkkiY1ncBR96t2PTI, hint: 9, ttl: 604800
   ```

[次の「Adobe Target VECサポートの追加」&gt;](target-vec.md)
