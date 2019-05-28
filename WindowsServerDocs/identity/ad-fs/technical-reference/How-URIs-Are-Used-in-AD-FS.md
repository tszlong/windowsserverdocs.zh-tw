---
ms.assetid: 53ee93e2-09ea-4f8b-adb7-c24c59f055ea
title: AD FS 中的 URI 使用方式
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 677d3136305cbddd29f2fd782be33ae1e824d096
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188546"
---
# <a name="how-uris-are-used-in-ad-fs"></a>AD FS 中的 URI 使用方式
統一資源識別項\(URI\)是用來當做唯一識別碼的字元字串。  在 AD FS 中，URI 可用來識別合作夥伴網路位址和設定物件。  用來識別合作夥伴網路位址時，URI 一律是 URL。  用來識別設定物件時，URI 可能是 URN 或 URL。  如需更多關於 URI 的一般資訊，請參閱 [RFC 2396](https://go.microsoft.com/fwlink/?LinkId=48289) 和 [RFC 3986](https://go.microsoft.com/fwlink/?LinkId=90453)。  
  
## <a name="uris-as-partner-network-addresses"></a>URI 做為合作夥伴網路位址  
以下是 AD FS 中最常由系統管理員處理的網路位址 URL。  
  
-   包括 WS 同盟服務的 Url\-同盟、 SAML、 WS\-信任，同盟中繼資料、 WS\-MetadataExchange、 隱私權和組織 Url  
  
-   信賴憑證者的合作對象信任，包括 WS 的 Url\-同盟、 SAML 和同盟中繼資料 Url  
  
-   宣告提供者信任，包括 WS Url\-同盟、 SAML 和同盟中繼資料 Url  
  
## <a name="uris-as-object-identifiers"></a>URI 做為物件識別碼  
下表說明 AD FS 中最常由系統管理員處理的識別碼。  
  
|識別碼名稱|描述|比較|  
|-------------------|---------------|---------------|  
|Federation Service 識別碼|這個識別碼用來識別同盟服務。  它是由信賴憑證者 (使用來自此同盟服務的宣告) 和宣告提供者 (將宣告發行給此同盟服務) 使用。|當使用者向宣告提供者要求此同盟服務的宣告時，同盟服務識別碼將會用來識別宣告的目標。<br /><br />當此同盟服務收到來自宣告提供者的宣告時，就會檢查以確保宣告是適用的，方法是尋找其同盟服務識別碼。<br /><br />當信賴憑證者收到來自此同盟服務的宣告時，信賴憑證者會檢查宣告的簽發者與同盟服務識別碼相符。|  
|信賴憑證者識別碼|這個識別碼是用來識別此同盟服務的信賴憑證者。  它是在將宣告發行給信賴憑證者時使用。|當使用者向此同盟服務要求信賴憑證者的宣告時，信賴憑證者識別碼將會用來識別應該做為宣告目標的信賴憑證者。  完成這項比較使用前置詞比對\(如下所示\)。<br /><br />當信賴憑證者收到宣告時，它會檢查其安全性權杖中的識別碼以確保這些宣告的目標是它。|  
|宣告提供者識別碼|這個識別碼是用來識別此同盟服務的宣告提供者。  它是在接收來自宣告提供者的宣告時使用。|當此同盟服務收到來自此宣告提供者的宣告時，此同盟服務會檢查宣告的簽發者與宣告提供者識別碼相符。|  
|宣告類型|這個識別碼是用來定義宣告的類型。  它是由這個同盟服務、宣告提供者和信賴憑證者在傳送和接收宣告時使用。|當同盟服務收到來自宣告提供者的宣告時，與對應的宣告提供者信任相關聯的宣告規則可以讓系統管理員比較宣告類型和處理宣告。  與信賴憑證者信任相關聯的的宣告規則也允許系統管理員比較宣告類型與來自宣告提供者信任規則的宣告，並決定要發行哪個宣告。|  
  
## <a name="uri-prefix-matching-for-relying-party-identifiers"></a>符合信賴憑證者識別碼的 URI 首碼  
URI 的路徑語法以階層方式組織，並分隔所有 「\/"字元或所有":"字元。  因此路徑可能會根據分隔字元分割為路徑區段。  當首碼相符時，每個區段必須根據比對規則完全相符\(這些規則會控制大小寫的相符項目\)。 如需比對規則的詳細資訊，請參閱上述的 RFC。  
  
在對於同盟服務的要求中已識別信賴憑證者時，AD FS 會使用首碼比對邏輯來判斷 AD FS 設定資料庫中是否有相符的信賴憑證者信任。  
  
比方說，如果 AD FS 組態資料庫中的信賴憑證者合作對象識別項\(URI1\)是傳入要求中的信賴憑證者的合作對象識別碼的前置詞\(URI2\)，則下列必須為 true:  
  
-   尾端分隔符號\(斜線和冒號\)的路徑區段或授權必須忽略  
  
-   URI1 與 URI2 的配置和授權部分必須是不區分大小寫完全相符  
  
-   URI1 的每個路徑區段必須是完全相符\(根據選擇的區分大小寫\)URI2 的對應路徑區段  
  
-   URI2 可能會有比 URI1 更多的路徑區段，但是 URI1 不能有比 URI2 更多的路徑區段  
  
-   URI1 不能有比 URI2 更多的路徑區段  
  
-   如果 URI1 具有查詢字串，它必須完全符合 URI2 查詢字串  
  
-   如果 URI1 具有片段，它必須完全符合 URI2 片段  
  
下表提供其他範例。  
  
|AD FS 設定資料庫中的信賴憑證者識別碼|要求訊息中的信賴憑證者識別碼|要求識別碼是否與設定識別項相符？|`Reason`|  
|------------------------------------------------------------|-----------------------------------------------|------------------------------------------------------------|----------|  
|http:\/\/contoso.com|http:\/\/contoso.com|TRUE|完全相符|  
|http:\/\/contoso.com\/|http:\/\/contoso.com|TRUE|會忽略尾端斜線|  
|http:\/\/contoso.com|http:\/\/contoso.com\/|TRUE|會忽略尾端斜線|  
|http:\/\/contoso.com|http:\/\/contoso.com\/hr|TRUE|URI1 沒有與 URI2 相符的路徑和配置及授權|  
|http:\/\/contoso.com\/hr|http:\/\/contoso.com\/hr\/web|TRUE|第一個路徑區段相符，URI1 沒有第二個路徑區段|  
|http:\/\/contoso.com\/hr|http:\/\/contoso.com\/hr\/web\/?m\=t|TRUE|與上述原因相同，查詢字串不會變更任何項目|  
|http:\/\/contoso.com\/hr\/|http:\/\/contoso.com\/hrw\/main|FALSE|URI1 路徑區段 1 與 URI2 路徑區段 1 不相符|  
|http:\/\/contoso.com\/hr|http:\/\/contoso.com|FALSE|URI1 有比 URI2 更多的路徑區段|  
|http:\/\/contoso.com\/hr|http:\/\/contoso.com\/hrweb|FALSE|第一個路徑區段不相符|  
|http:\/\/contoso.com\/?m\=t|http:\/\/contoso.com\/?m\=f|FALSE|查詢字串組件不相符|  
|https:\/\/contoso.com|http:\/\/contoso.com|FALSE|配置組件不相符|  
|http:\/\/sts.contoso.com|http:\/\/contoso.com|FALSE|授權組件不相符|  
|http:\/\/contoso.com|http:\/\/sts.contoso.com|FALSE|授權組件不相符|  
  

