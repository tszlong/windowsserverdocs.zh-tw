---
title: AD FS 中的用戶端存取宣告類型
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d73995b118ec41ffc892700858d20798f637d83b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857481"
---
# <a name="client-access-policy-claim-types-in-ad-fs"></a>AD FS 中的用戶端存取原則宣告類型

為了提供額外的要求內容資訊，用戶端存取原則會使用下列宣告類型，AD FS 從要求標頭資訊產生以進行處理。  如需詳細資訊，請參閱[宣告引擎的角色](../technical-reference/the-role-of-the-claims-engine.md)。

## <a name="x-ms-forwarded-client-ip"></a>X-毫秒-轉送-用戶端 IP

宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`

此 AD FS 宣告代表在查明使用者的 IP 位址（例如 Outlook 用戶端）提出要求時的「最佳嘗試」。 此宣告可以包含多個 IP 位址，包括轉送要求的每個 proxy 的位址。  此宣告是從目前僅由 Exchange Online 設定的 HTTP 標頭填入，在將驗證要求傳遞至 AD FS 時，會填入標頭。 宣告的值可以是下列其中一項：


- 單一 IP 位址-直接連線至 Exchange Online 之用戶端的 IP 位址

    >!下公司網路上用戶端的 IP 位址將會顯示為組織的輸出 proxy 或閘道的外部介面 IP 位址。

- 一或多個 IP 位址
  - 如果 Exchange Online 無法判斷連線用戶端的 IP 位址，它會根據 x 轉送的標頭值來設定值，這是一個非標準標頭，可包含在 HTTP 要求中，並由市場上的許多用戶端、負載平衡器和 proxy 支援。
  - 指出用戶端 IP 位址和每個傳遞要求之 proxy 位址的多個 IP 位址，會以逗號分隔。

    >!下與 Exchange Online 基礎結構相關的 IP 位址將不會出現在清單中。


>!WarningExchange Online 目前僅支援 IPV4 位址;它不支援 IPV6 位址。 


## <a name="x-ms-client-application"></a>X-MS-用戶端-應用程式

宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`

此 AD FS 宣告代表終端用戶端所使用的通訊協定，其對應鬆散于所使用的應用程式。  此宣告是從目前僅由 Exchange Online 設定的 HTTP 標頭填入，在將驗證要求傳遞至 AD FS 時，會填入標頭。 根據應用程式而定，此宣告的值將會是下列其中一項：



- 如果是使用 Exchange Active Sync 的裝置，則值為 [Microsoft. Exchange ActiveSync]。 
- 使用 Microsoft Outlook 用戶端可能會產生下列任何值：
    - Microsoft. Exchange. 自動探索
    - OfflineAddressBook
    - Microsoft. Exchange RPC
    - WebServices
    - Microsoft. Exchange Mapi
- 此標頭的其他可能值包括下列各項：
    - Microsoft. Exchange Powershell
    - Microsoft. Exchange SMTP
    - PopImap
    - Microsoft. Exchange Pop
    - Microsoft。

## <a name="x-ms-client-user-agent"></a>X-MS-用戶端-使用者-代理程式

宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

此 AD FS 宣告會提供字串，以代表用戶端用來存取服務的裝置類型。 當客戶想要防止特定裝置（例如特定類型的智慧型手機）的存取權時，可以使用此方法。  此宣告是從目前僅由 Exchange Online 設定的 HTTP 標頭填入，在將驗證要求傳遞至 AD FS 時，會填入標頭。 此宣告的範例值包括（但不限於）以下的值。
>!下以下範例為 x-ms-用戶端-應用程式的用戶端---------------------------------

- Vortex/1。0
- Apple-iPad1C1/812。1
- Apple-iPhone3C1/811。2
- Apple-iPhone/704.11
- Moto-DROID2/4.5。1
- SAMSUNGSPHD700/100.202
- Android/0。3

>!下這個值也可能是空的。


## <a name="x-ms-proxy"></a>X-MS-Proxy

宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

此 AD FS 宣告表示要求已通過同盟伺服器 proxy。  同盟伺服器 proxy 會填入此宣告，在將驗證要求傳遞給後端同盟服務時，會填入標頭。 AD FS 然後將它轉換為宣告。 

宣告的值是通過要求的同盟伺服器 proxy 的 DNS 名稱。

## <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-端點-絕對路徑（主動與被動）

宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

此宣告類型可用來判斷源自「作用中」（豐富）用戶端與「被動」（以 web 瀏覽器為基礎）用戶端的要求。 這可讓來自瀏覽器型應用程式（例如 Outlook Web 存取、SharePoint Online 或 Office 365 入口網站）的外部要求允許，而源自豐富用戶端（例如 Microsoft Outlook）的要求會遭到封鎖。

宣告的值是收到要求的 AD FS 服務名稱。
