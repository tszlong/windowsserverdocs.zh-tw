---
title: 將非網域成員的防火牆規則設定為允許 BranchCache 流量
description: 瞭解如何設定協力廠商防火牆產品，以及如何使用防火牆規則手動設定用戶端電腦，以允許 BranchCache 在分散式快取模式中執行。
manager: brianlic
ms.topic: how-to
ms.assetid: da956be0-c92d-46ea-99eb-85e2bd67bf07
ms.author: lizross
author: eross-msft
ms.date: 01/05/2021
ms.openlocfilehash: e53601cf7c0a2cd4d88d440aea78843222881b6b
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950304"
---
# <a name="configure-firewall-rules-for-non-domain-members-to-allow-branchcache-traffic"></a>將非網域成員的防火牆規則設定為允許 BranchCache 流量

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題中的資訊來設定協力廠商防火牆產品，以及手動將用戶端電腦設定為允許 BranchCache 在分散式快取模式中執行的防火牆規則。

> [!NOTE]
> -   如果您已使用群組原則設定 BranchCache 用戶端電腦，則群組原則設定會覆寫套用原則之用戶端電腦的任何手動設定。
> -   如果您已部署 BranchCache 與 DirectAccess，您可以使用本主題中的設定來設定 IPsec 規則以允許 BranchCache 流量。

若要進行這些設定變更，至少需要 **Administrators** 的成員資格或同等許可權。

## <a name="ms-pccrd-peer-content-caching-and-retrieval-discovery-protocol"></a>[PCCRD]：對等內容快取和抓取探索通訊協定
分散式快取用戶端必須允許在 Web 服務動態探索 (WS-MANAGEMENT) 通訊協定中攜帶的輸入和輸出 MS PCCRD 流量。

除了輸入和輸出流量之外，防火牆設定還必須允許多播流量。 您可以使用下列設定來設定分散式快取模式的防火牆例外。

IPv4 多播：239.255.255.250

IPv6 多播： FF02：： C

輸入流量：本機埠：3702、遠端埠：暫時

輸出流量：本機埠：暫時、遠端埠：3702

程式：% systemroot% \system32\svchost.exe (BranchCache 服務 [PeerDistSvc] ) 

## <a name="ms-pccrr-peer-content-caching-and-retrieval-retrieval-protocol"></a>[PCCRR]：對等內容快取和抓取：抓取通訊協定
分散式快取用戶端必須允許輸入和輸出 MS PCCRR 流量，如要求 (RFC) 2616 中所述，在 HTTP 1.1 通訊協定中傳送。

防火牆設定必須允許連入和連出流量。 您可以使用下列設定來設定分散式快取模式的防火牆例外。

輸入流量：本機埠：80、遠端埠：暫時

輸出流量：本機埠：暫時、遠端埠：80



