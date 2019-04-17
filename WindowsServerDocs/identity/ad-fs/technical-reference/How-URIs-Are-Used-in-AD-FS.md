---
ms.assetid: 53ee93e2-09ea-4f8b-adb7-c24c59f055ea
title: "AD FS 中使用 Uri 的方式"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 305bf0cece742c961604dacda7e27b8eac8065e5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
>適用於：Windows Server 2016、Windows Server 2012 R2

# <a name="how-uris-are-used-in-ad-fs"></a>AD FS 中使用 Uri 的方式
統一資源識別碼 \(URI\) 是用來做的唯一的字元字串。  AD FS，在 Uri 用來找出合作夥伴網路地址和設定物件。  當您用來找出合作夥伴網路位址，URI 都 URL。  當您用來找出組態物件，URI 可能 URN 或 URL。  有關更一般 Uri，請查看[RFC 2396](https://go.microsoft.com/fwlink/?LinkId=48289)和[RFC 3986](https://go.microsoft.com/fwlink/?LinkId=90453)。  
  
## <a name="uris-as-partner-network-addresses"></a>為合作夥伴網路位址 Uri  
以下是最常由 AD FS 中的系統管理員的網路位址 Url。  
  
-   聯盟服務，包括 WS\-同盟、 SAML、 WS\ 信任、 聯盟中繼資料、 WS\-MetadataExchange 隱私權和組織 Url 的 Url  
  
-   信賴的派對信任，包括 WS\-同盟、 SAML，以及聯盟中繼資料 Url 的 Url  
  
-   宣告提供者信任，包括 WS\-同盟、 SAML，以及聯盟中繼資料 Url 的 Url  
  
## <a name="uris-as-object-identifiers"></a>做為物件識別碼 Uri  
下表描述通常由 AD FS 管理員識別碼。  
  
|識別碼名稱|描述|比較|  
|-------------------|---------------|---------------|  
|聯盟服務識別碼|找出同盟服務使用此識別碼。  使用這個同盟服務，以及宣告提供者，此同盟服務宣告問題宣告信賴派對使用它。|當使用者向宣告服務提供者，此聯盟要求宣告時，同盟服務識別碼將會用來找出所宣告的目標。<br /><br />當此同盟服務接收宣告宣告提供者時，就會檢查以確保宣告範圍它所尋找的項目同盟服務。<br /><br />信賴從這個同盟服務接收宣告，當信賴會檢查所宣告的發行者符合同盟服務識別碼。|  
|可以廠商識別碼|找出信賴此同盟服務使用此識別碼。  發行宣告信賴來時使用它。|當使用者向此同盟服務要求宣告信賴的時信賴的派對識別碼將會用來找出的信賴宣告應該會對應的。  完成此比較使用前置詞 \(see below\) 相符。<br /><br />當信賴收到宣告時，就會檢查以確保宣告針對安全性權杖中的項目。|  
|宣告提供者識別碼|使用這個識別碼找出宣告此同盟服務提供者。  它是用來接收宣告宣告提供者。|當此同盟服務接收宣告宣告提供者時，此同盟服務會檢查所宣告的發行者符合宣告提供者識別碼。|  
|宣告類型|使用這個識別碼來定義理賠要求的類型。  它會使用此同盟服務、 宣告提供者，以及時傳送和接收宣告信賴的派對。|當同盟服務接收宣告宣告提供者時，對應的宣告提供者信任相關聯的理賠要求規則允許比較宣告類型及處理宣告的系統管理員。  信賴的派對信任相關聯的理賠要求規則也允許比較宣告類型退出宣告提供者信任規則，提供宣告的系統管理員，並選擇的發行宣告。|  
  
## <a name="uri-prefix-matching-for-relying-party-identifiers"></a>符合信賴派對識別碼 URI 前置詞  
URI 的路徑語法階層組織和由所有分隔 」 \ 日 」 的字元或所有 」: 「 字元。  因此路徑可分成根據分隔字元路徑區段。  當前置詞比對，每個區段必須符合規則根據完整相符項目 \ （這些規則管理 matches\ 的大小寫）。 如需符合規則，查看 RFC 的上述。  
  
當信賴辨識要求同盟服務中時，AD FS 使用前置詞相符邏輯操作，判斷是否符合信賴廠商信任 AD FS 設定資料庫中。  
  
例如是否 AD FS 設定資料庫中的依賴派對識別碼 \(URI1\) 連入信賴派對識別碼前置詞要求 \(URI2\)，接著必須符合下列：  
  
-   結尾分隔字元 \(slashes and colons\) 路徑的區段或授權必須忽略  
  
-   必須區分大小寫的完全符合 URI1 和 URI2 的配置，並授權的部分。  
  
-   每個路徑部分 URI1 必須完全符合 \ （根據區分大小寫 chosen\） URI2 相對路徑一節  
  
-   URI2 可能會有更多的路徑區段比 URI1，但 URI1 不能有更多的路徑區段 URI2 比  
  
-   URI1 不會有更多的路徑區段 URI2 比  
  
-   如果 URI1 查詢字串，必須符合完全為 URI2 查詢字串  
  
-   如果 URI1 片段，必須符合完全到 URI2 片段  
  
下表會提供額外的範例。  
  
|可以廠商識別碼 AD FS 設定資料庫中|可以廠商識別碼接受要求訊息中|組態識別字要求識別碼相符項目嗎？|原因|  
|------------------------------------------------------------|-----------------------------------------------|------------------------------------------------------------|----------|  
|http:///\/contoso.com|http:///\/contoso.com|為 TRUE|相符|  
|http:///\/contoso.com\/|http:///\/contoso.com|為 TRUE|忽略行尾斜線|  
|http:///\/contoso.com|http:///\/contoso.com\/|為 TRUE|忽略行尾斜線|  
|http:///\/contoso.com|http:///\/contoso.com\/hr|為 TRUE|URI1 已經不會路徑和相符項目配置和 URI2 權限|  
|http:///\/contoso.com\/hr|http:///\/contoso.com\/hr\/web|為 TRUE|第一次路徑區段符合、 URI1 有無第二個路徑一節|  
|http:///\/contoso.com\/hr|http:///\/contoso.com\/hr\/web\/?m\=t|為 TRUE|上述相同的原因，查詢字串未變更任何項目|  
|http:///\/contoso.com\/hr\/|http:///\/contoso.com\/hrw\/main|FALSE|URI1 路徑區段 1 不符合 URI2 路徑區段 1|  
|http:///\/contoso.com\/hr|http:///\/contoso.com|FALSE|URI1 有更多的路徑區段 URI2 比|  
|http:///\/contoso.com\/hr|http:///\/contoso.com\/hrweb|FALSE|第一次路徑章節不相符|  
|http:///\/contoso.com\/?m\=t|http:///\/contoso.com\/?m\=f|FALSE|查詢字串組件不相符|  
|https:\/\/contoso.com|http:///\/contoso.com|FALSE|部分配置不相符|  
|http:///\/sts.contoso.com|http:///\/contoso.com|FALSE|授權單位部分不符|  
|http:///\/contoso.com|http:///\/sts.contoso.com|FALSE|授權單位部分不符|  
  

