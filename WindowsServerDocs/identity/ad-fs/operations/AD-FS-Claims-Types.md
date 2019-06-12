---
ms.assetid: ''
title: 用戶端存取宣告的 AD FS 中的類型
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0ffa4273a2c776a16f3ea0ce77d1b3a528481468
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445167"
---
# <a name="client-access-policy-claim-types-in-ad-fs"></a>用戶端存取原則的宣告在 AD FS 中的類型

若要提供額外的要求內容資訊，用戶端存取原則會使用 AD FS 會產生從處理的要求標頭資訊的下列宣告類型。  如需詳細資訊，請參閱[宣告引擎的角色](../technical-reference/the-role-of-the-claims-engine.md)。

## <a name="x-ms-forwarded-client-ip"></a>X-MS-Forwarded-Client-IP

宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`

此 AD FS 宣告表示 「 地嘗試 「 查明提出要求的使用者 （例如 Outlook 用戶端） 的 IP 位址。 此宣告可以包含多個 IP 位址，包括將要求轉送給每個 proxy 的位址。  此宣告會從目前的 HTTP 標頭填入只能設定 Exchange online，會將驗證要求傳遞至 AD FS 時填入標頭。 宣告的值可以是下列其中一項：


- 單一 IP 位址直接連線到 Exchange Online 的用戶端的 IP 位址

    >![注意]公司網路上的用戶端的 IP 位址會顯示為組織的連出 proxy 或閘道的外部介面 IP 位址。

- 一或多個 IP 位址
  - 如果 Exchange Online 無法判斷連線的用戶端的 IP 位址，它會根據設定值 x 轉送的標頭值，可以包含在 HTTP 型的非標準標頭要求，並支援許多用戶端，負載平衡器和市面上的 proxy。
  - 指出用戶端 IP 位址和每個傳遞要求的 proxy 位址的多個 IP 位址會以逗號分隔。

    >![注意]Exchange Online 的基礎結構相關的 IP 位址將不會出現在清單中。


>![警告]Exchange Online 目前支援的只有 IPV4 位址;它不支援 IPV6 位址。 


## <a name="x-ms-client-application"></a>X-MS-Client-Application

宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`

此 AD FS 宣告表示結束用戶端，鬆散對應至所使用的應用程式所使用的通訊協定。  此宣告會從目前的 HTTP 標頭填入只能設定 Exchange online，會將驗證要求傳遞至 AD FS 時填入標頭。 根據應用程式，此宣告的值會是下列其中一項：



- 在使用 Exchange Active Sync 的裝置中，這個值會是 Microsoft.Exchange.ActiveSync。 
- 使用 Microsoft Outlook 用戶端可能會導致下列值之一：
    - Microsoft.Exchange.Autodiscover
    - Microsoft.Exchange.OfflineAddressBook
    - Microsoft.Exchange.RPC
    - Microsoft.Exchange.WebServices
    - Microsoft.Exchange.Mapi
- 其他可能的值，此標頭包括下列各項：
    - Microsoft.Exchange.Powershell
    - Microsoft.Exchange.SMTP
    - Microsoft.Exchange.PopImap
    - Microsoft.Exchange.Pop
    - Microsoft.Exchange.Imap

## <a name="x-ms-client-user-agent"></a>X-MS-Client-User-Agent

宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

此 AD FS 宣告提供一個字串，代表用戶端用來存取服務的裝置類型。 這可以在客戶想要防止存取某些裝置 （例如智慧型手機的特殊類型） 時使用。  此宣告會從目前的 HTTP 標頭填入只能設定 Exchange online，會將驗證要求傳遞至 AD FS 時填入標頭。 此宣告的範例值包括 （但不限於） 以下的值。
>![注意]以下是範例的 x-ms-使用者-代理程式值可能會包含其 x-ms-用戶端-應用程式是 「 Microsoft.Exchange.ActiveSync"的用戶端

- 旋風/1.0
- Apple-iPad1C1/812.1
- Apple-iPhone3C1/811.2
- Apple-iPhone/704.11
- Moto-DROID2/4.5.1
- SAMSUNGSPHD700/100.202
- Android/0.3

>![注意]此外，也可以是空的這個值。


## <a name="x-ms-proxy"></a>X-MS-Proxy

宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

此 AD FS 宣告表示要求已通過的同盟伺服器 proxy。  此宣告會填入同盟伺服器 proxy，將驗證要求傳遞至後端 Federation Service 時，會填入標頭。 AD FS 然後將它轉換成宣告。 

宣告的值是同盟伺服器 proxy 傳遞要求的 DNS 名稱。

## <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-端點-絕對-路徑 (使用中 vs 被動)

宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

這種宣告類型可以用於判斷發出的 「 作用中 」 或 「 被動 」 （web 瀏覽器型） 的用戶端的 （豐富） 用戶端要求。 這可讓來自瀏覽器型應用程式，例如 Outlook Web Access、 SharePoint Online 或 Office 365 入口網站會封鎖來自 如 Microsoft Outlook 的豐富型用戶端的要求時允許的外部要求。

宣告的值是所接收到要求的 AD FS 服務名稱。
