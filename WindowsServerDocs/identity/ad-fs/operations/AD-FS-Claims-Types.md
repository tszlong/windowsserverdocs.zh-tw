---
description: 深入瞭解： AD FS 中的用戶端存取原則宣告類型
title: AD FS 中的用戶端存取宣告類型
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: b76b386f9c79e28ea9c97ba71bb58e09f984aed6
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97040566"
---
# <a name="client-access-policy-claim-types-in-ad-fs"></a>AD FS 中的用戶端存取原則宣告類型

為了提供其他要求內容資訊，用戶端存取原則會使用下列宣告類型，AD FS 從要求標頭資訊產生以進行處理。  如需詳細資訊，請參閱 [宣告引擎的角色](../technical-reference/the-role-of-the-claims-engine.md)。

## <a name="x-ms-forwarded-client-ip"></a>X-毫秒轉送-用戶端-IP

宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`

此 AD FS 宣告代表在查明 (使用者 IP 位址時的「最佳嘗試」，例如，) 提出要求的 Outlook 用戶端。 此宣告可以包含多個 IP 位址，包括轉送要求的每個 proxy 的位址。  此宣告會從目前僅由 Exchange Online 設定的 HTTP 標頭填入，而此標頭會在將驗證要求傳遞至 AD FS 時填入標頭。 宣告的值可以是下列其中一項：


- 單一 IP 位址：直接連線至 Exchange Online 之用戶端的 IP 位址

    >!記公司網路上用戶端的 IP 位址會顯示為組織輸出 proxy 或閘道的外部介面 IP 位址。

- 一或多個 IP 位址
  - 如果 Exchange Online 無法判斷連線用戶端的 IP 位址，它將會根據 x 轉寄的標頭值（可包含在 HTTP 型要求中的非標準標頭）來設定值，而且市場上的許多用戶端、負載平衡器和 proxy 都支援此標頭。
  - 指出用戶端 IP 位址的多個 IP 位址，以及通過要求的每個 proxy 的位址，將會以逗號分隔。

    >!記與 Exchange Online 基礎結構相關的 IP 位址不會出現在清單中。


>!條Exchange Online 目前僅支援 IPV4 位址;它不支援 IPV6 位址。


## <a name="x-ms-client-application"></a>X-MS-用戶端應用程式

宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`

此 AD FS 宣告代表終端用戶端所使用的通訊協定，此通訊協定與所使用的應用程式相當鬆散。  此宣告會從目前僅由 Exchange Online 設定的 HTTP 標頭填入，而此標頭會在將驗證要求傳遞至 AD FS 時填入標頭。 視應用程式而定，此宣告的值將會是下列其中一項：



- 如果是使用 Exchange Active Sync 的裝置，則值為 [中]。
- 使用 Microsoft Outlook 用戶端可能會產生下列任何值：
    - Microsoft. Exchange 自動探索
    - OfflineAddressBook
    - Microsoft. RPC
    - WebServices
    - Microsoft。
- 此標頭的其他可能值包括下列各項：
    - Microsoft. Powershell
    - Microsoft 的 SMTP
    - PopImap
    - Microsoft. Exchange Pop
    - Microsoft 的 Imap

## <a name="x-ms-client-user-agent"></a>X-MS-用戶端-使用者-代理程式

宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

此 AD FS 宣告會提供字串來代表用戶端用來存取服務的裝置類型。 當客戶想要防止特定裝置的存取 (例如特定的智慧型手機) 類型時，就可以使用這種方式。  此宣告會從目前僅由 Exchange Online 設定的 HTTP 標頭填入，而此標頭會在將驗證要求傳遞至 AD FS 時填入標頭。 此宣告的範例值包含 (但不限於) 以下值。
>!記以下是 x-ms-用戶端應用程式的用戶端---ms--------

- Vortex/1。0
- Apple-iPad1C1/812。1
- Apple-iPhone3C1/811。2
- Apple-iPhone/704.11
- Moto-DROID2/4.5。1
- SAMSUNGSPHD700/100.202
- Android/0。3

>!記此值也可能是空的。


## <a name="x-ms-proxy"></a>X-MS-Proxy

宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

此 AD FS 宣告指出已透過同盟伺服器 proxy 傳遞要求。  此宣告是由同盟伺服器 proxy 填入，它會在將驗證要求傳遞至後端同盟服務時填入標頭。 AD FS 然後將它轉換為宣告。

宣告的值是通過要求之同盟伺服器 proxy 的 DNS 名稱。

## <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-端點-絕對路徑 (主動與被動) 

宣告類型： `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

此宣告類型可以用來判斷源自「作用中」的要求 (豐富的) 用戶端與「被動」 (以網頁瀏覽器為基礎的) 用戶端。 這可讓您從瀏覽器應用程式（例如 Outlook Web 存取、SharePoint Online 或 Office 365 入口網站）允許外部要求，而從豐富用戶端（例如 Microsoft Outlook）發出的要求會遭到封鎖。

宣告的值是接收要求的 AD FS 服務名稱。
