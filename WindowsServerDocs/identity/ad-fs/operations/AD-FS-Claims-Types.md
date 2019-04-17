---
ms.assetid: 
title: "Client 存取宣告 AD FS 中的類型"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1e37aded450555d293806d1ed8903a51e3df9424
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
#<a name="client-access-policy-claim-types-in-ad-fs"></a>Client 存取原則宣告 AD FS 中的類型

若要提供其他要求操作資訊，Client 存取原則，請使用下列理賠要求類型，負責要求標頭處理資訊的 AD FS。  如需詳細資訊請查看[的角色宣告引擎的](../technical-reference/the-role-of-the-claims-engine.md)。

##<a name="x-ms-forwarded-client-ip"></a>X MS-轉送-Client-IP

宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`

此 AD FS 宣告代表「盡量嘗試」，請確實提出要求的使用者 (例如，Outlook client) 的 IP 位址。 此宣告可能包含以多個 IP 位址，包括每個 proxy 轉寄要求的位址。  此宣告會填入 HTTP 標頭目前僅限設定換貨 Online，以 AD FS 傳遞驗證要求時，會填入標頭。 宣告值可以下列其中一個動作：


- 單一 IP 位址直接連接至換貨 Online client 的 IP 位址

    >![筆記]Client 公司網路上的 IP 位址會出現外部介面組織的輸出 proxy 或閘道 IP 位址。

- 一或多個 IP 位址
    - 如果換貨 Online 無法判斷連接 client 的 IP 位址，它將會設定 x 轉送的標頭的值為基礎的可以根據 http 包含非標準標頭要求和支援許多戶端、負載平衡器，與市面上的 proxy 的值。
    - 將會以逗號分隔指出 client IP 位址和每個 proxy 傳遞要求的地址多個 IP 位址。

    >![筆記]將不會出現在清單中相關 Exchange Online 基礎結構的 IP 位址。


>![警告]換貨 Online 目前支援只 IPV4 位址。不支援 IPV6 位址。 


## <a name="x-ms-client-application"></a>X MS-Client 的應用程式

宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`

此 AD FS 宣告代表結束 client，彈性對應至所使用的應用程式使用的通訊協定。  此宣告會填入 HTTP 標頭目前僅限設定換貨 Online，以 AD FS 傳遞驗證要求時，會填入標頭。 而定，應用程式的此宣告值將下列其中一個動作：



- 如果使用 Exchange 使用同步的裝置的值為 Microsoft.Exchange.ActiveSync。 
- 使用 Microsoft Outlook client 可能會導致任何下列值：
    - Microsoft.Exchange.Autodiscover
    - Microsoft.Exchange.OfflineAddressBook
    - Microsoft.Exchange.RPC
    - Microsoft.Exchange.WebServices
    - Microsoft.Exchange.Mapi
- 下列其他此標頭可能的值：
    - Microsoft.Exchange.Powershell
    - Microsoft.Exchange.SMTP
    - Microsoft.Exchange.PopImap
    - Microsoft.Exchange.Pop
    - Microsoft.Exchange.Imap

## <a name="x-ms-client-user-agent"></a>X-MS-Client-使用者代理程式

宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

此 AD FS 理賠要求提供代表 client 存取服務使用的裝置類型的字串。 這可以針對想要避免存取特定裝置 （例如智慧型手機的特定類型） 時使用。  此宣告會填入 HTTP 標頭目前僅限設定換貨 Online，以 AD FS 傳遞驗證要求時，會填入標頭。 此宣告值範例包括 （但不是限於） 下列值。
>![筆記]以下是範例 x ms-使用者代理值可能會包含對其 x ms-client 的應用程式是 「 Microsoft.Exchange.ActiveSync 「 client

- 1.0 漩渦日
- 蘋果-iPad1C1 日 812.1
- 蘋果-iPhone3C1 日 811.2
- 蘋果-iPhone 日 704.11
- Moto-DROID2/4.5.1
- 100.202 SAMSUNGSPHD700 日
- Android 0.3 日

>![筆記]它也可是空的這個值。


## <a name="x-ms-proxy"></a>X-MS-Proxy

宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

此 AD FS 宣告指示要求已經通過聯盟 proxy 伺服器。  此宣告後端服務聯盟傳遞驗證要求時，會填入標頭聯盟伺服器 proxy 會填入。 AD FS 再將它轉換為理賠要求。 

宣告的值為傳遞要求聯盟伺服器 proxy 的 DNS 名稱。

## <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X MS-端點-絕對值-路徑 （作用中與被動式）

宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

此宣告類型可用來判斷來自從 「 作用中的 「 （進階） 與 「 被動式 」 （web-瀏覽器為基礎） 戶端要求。 這可讓外部瀏覽器為基礎的應用程式例如 Outlook Web Access、 SharePoint Online 或 Office 365 入口網站時，會被封鎖來自從豐富例如 Microsoft Outlook 要求允許要求。

宣告的值為收到要求 AD FS 服務的名稱。
