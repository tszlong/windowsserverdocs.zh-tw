---
description: 深入瞭解：如何在 AD FS 中使用 Uri
ms.assetid: 53ee93e2-09ea-4f8b-adb7-c24c59f055ea
title: AD FS 中的 URI 使用方式
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 236f75152c64cf841ce1196415d6f95e92a252ca
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97045336"
---
# <a name="how-uris-are-used-in-ad-fs"></a>AD FS 中的 URI 使用方式
統一資源識別項 \( URI \) 是用來做為唯一識別碼的字元字串。  在 AD FS 中，URI 可用來識別合作夥伴網路位址和設定物件。  用來識別合作夥伴網路位址時，URI 一律是 URL。  用來識別設定物件時，URI 可能是 URN 或 URL。  如需更多關於 URI 的一般資訊，請參閱 [RFC 2396](https://go.microsoft.com/fwlink/?LinkId=48289) 和 [RFC 3986](https://go.microsoft.com/fwlink/?LinkId=90453)。

## <a name="uris-as-partner-network-addresses"></a>URI 做為合作夥伴網路位址
以下是 AD FS 中最常由系統管理員處理的網路位址 URL。

-   同盟服務的 Url，包括 WS \- 同盟、SAML、WS \- 信任、同盟中繼資料、WS \- 透過 ws-metadataexchange、隱私權和組織 url

-   信賴憑證者信任的 Url，包括 WS \- 同盟、SAML 和同盟中繼資料 url

-   宣告提供者信任的 Url，包括 WS \- 同盟、SAML 和同盟中繼資料 url

## <a name="uris-as-object-identifiers"></a>URI 做為物件識別碼
下表說明 AD FS 中最常由系統管理員處理的識別碼。

|識別碼名稱|描述|比較|
|-------------------|---------------|---------------|
|Federation Service 識別碼|這個識別碼用來識別同盟服務。  它是由信賴憑證者 (使用來自此同盟服務的宣告) 和宣告提供者 (將宣告發行給此同盟服務) 使用。|當使用者向宣告提供者要求此同盟服務的宣告時，同盟服務識別碼將會用來識別宣告的目標。<p>當此同盟服務收到來自宣告提供者的宣告時，就會檢查以確保宣告是適用的，方法是尋找其同盟服務識別碼。<p>當信賴憑證者收到來自此同盟服務的宣告時，信賴憑證者會檢查宣告的簽發者與同盟服務識別碼相符。|
|信賴憑證者識別碼|這個識別碼是用來識別此同盟服務的信賴憑證者。  它是在將宣告發行給信賴憑證者時使用。|當使用者向此同盟服務要求信賴憑證者的宣告時，信賴憑證者識別碼將會用來識別應該做為宣告目標的信賴憑證者。  這項比較是使用前置詞比對來完成， \( 請參閱下面 \) 的。<p>當信賴憑證者收到宣告時，它會檢查其安全性權杖中的識別碼以確保這些宣告的目標是它。|
|宣告提供者識別碼|這個識別碼是用來識別此同盟服務的宣告提供者。  它是在接收來自宣告提供者的宣告時使用。|當此同盟服務收到來自此宣告提供者的宣告時，此同盟服務會檢查宣告的簽發者與宣告提供者識別碼相符。|
|宣告類型|這個識別碼是用來定義宣告的類型。  它是由這個同盟服務、宣告提供者和信賴憑證者在傳送和接收宣告時使用。|當同盟服務收到來自宣告提供者的宣告時，與對應的宣告提供者信任相關聯的宣告規則可以讓系統管理員比較宣告類型和處理宣告。  與信賴憑證者信任相關聯的的宣告規則也允許系統管理員比較宣告類型與來自宣告提供者信任規則的宣告，並決定要發行哪個宣告。|

## <a name="uri-prefix-matching-for-relying-party-identifiers"></a>符合信賴憑證者識別碼的 URI 首碼
URI 的路徑語法會以階層方式組織，並以所有 " \/ " 字元或所有 "：" 字元分隔。  因此路徑可能會根據分隔字元分割為路徑區段。  前置詞比對時，每個區段都必須是完全相符的，因為 \( 這些規則會管理相符專案的大小寫 \) 。 如需有關比對規則的詳細資訊，請參閱上面所述的 RFC。

在對於同盟服務的要求中已識別信賴憑證者時，AD FS 會使用首碼比對邏輯來判斷 AD FS 設定資料庫中是否有相符的信賴憑證者信任。

例如，如果 AD FS 設定資料庫 URI1 中的信賴憑證者識別碼 \( \) 是連入要求 URI2 中信賴憑證者識別碼的前置詞 \( ，則下列條件 \) 必須成立：

-   \( \) 必須忽略路徑區段或授權單位的尾端分隔符號斜線和冒號

-   URI1 與 URI2 的配置和授權部分必須是不區分大小寫完全相符

-   URI1 的每個路徑區段都必須是完全相符的，其 \( 依據是針對 \) URI2 的對應路徑區段選擇的大小寫敏感度。

-   URI2 可能會有比 URI1 更多的路徑區段，但是 URI1 不能有比 URI2 更多的路徑區段

-   URI1 不能有比 URI2 更多的路徑區段

-   如果 URI1 具有片段，它必須完全符合 URI2 片段

 >[!NOTE]
 > 不支援查詢字串參數，且將會忽略信賴憑證者識別碼中的查詢字串參數。

下表提供其他範例。

|AD FS 設定資料庫中的信賴憑證者識別碼|要求訊息中的信賴憑證者識別碼|要求識別碼是否與設定識別項相符？|原因|
|------------------------------------------------------------|-----------------------------------------------|------------------------------------------------------------|----------|
|HTTP： \/ \/ contoso.com|HTTP： \/ \/ contoso.com|true|完全相符|
|HTTP： \/ \/ contoso.com\/|HTTP： \/ \/ contoso.com|true|會忽略尾端斜線|
|HTTP： \/ \/ contoso.com|HTTP： \/ \/ contoso.com\/|true|會忽略尾端斜線|
|HTTP： \/ \/ contoso.com|HTTP： \/ \/ contoso.com \/ hr|true|URI1 沒有與 URI2 相符的路徑和配置及授權|
|HTTP： \/ \/ contoso.com \/ hr|HTTP： \/ \/ contoso.com \/ hr \/ web|true|第一個路徑區段相符，URI1 沒有第二個路徑區段|
|HTTP： \/ \/ contoso.com \/ hr\/|HTTP： \/ \/ contoso.com \/ hrw \/ main|FALSE|URI1 路徑區段 1 與 URI2 路徑區段 1 不相符|
|HTTP： \/ \/ contoso.com \/ hr|HTTP： \/ \/ contoso.com|FALSE|URI1 有比 URI2 更多的路徑區段|
|HTTP： \/ \/ contoso.com \/ hr|HTTP： \/ \/ contoso.com \/ hrweb|FALSE|第一個路徑區段不相符|
|HTTPs： \/ \/ contoso.com|HTTP： \/ \/ contoso.com|FALSE|配置組件不相符|
|HTTP： \/ \/ sts.contoso.com|HTTP： \/ \/ contoso.com|FALSE|授權組件不相符|
|HTTP： \/ \/ contoso.com|HTTP： \/ \/ sts.contoso.com|FALSE|授權組件不相符|


