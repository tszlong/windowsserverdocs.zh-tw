---
title: Windows Server 中 DNS 用戶端的新功能
description: 本主題提供 Windows Server 和 Windows 10 中 DNS 用戶端的新功能總覽
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 6edaba84-4595-4fd8-95d7-64d4d975a38a
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f94f4dc5c85ba4f80b45e3c406c1e11d2da05d65
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85473925"
---
# <a name="whats-new-in-dns-client-in-windows-server-2016"></a>Windows Server 2016 中 DNS 用戶端的新功能

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題說明在 Windows 10 和 Windows Server 2016 和更新版本的作業系統中，新的或已變更的網域名稱系統（DNS）用戶端功能。

## <a name="updates-to-dns-client"></a>DNS 用戶端的更新

**Dns 用戶端服務**系結：在 Windows 10 中，Dns 用戶端服務會針對具有多個網路介面的電腦提供增強的支援。 對於多重主目錄電腦，DNS 解析會以下列方式進行優化：

-   當使用特定介面上設定的 DNS 伺服器來解析 DNS 查詢時，DNS 用戶端服務會先系結至此介面，然後再傳送 DNS 查詢。

    藉由系結至特定的介面，DNS 用戶端可以清楚地指定進行名稱解析的介面，讓應用程式可以透過此網路介面來優化與 DNS 用戶端的通訊。

-   如果所使用的 DNS 伺服器是由名稱解析原則資料表（NRPT）中的群組原則設定所指定，DNS 用戶端服務就不會系結至特定的介面。

> [!NOTE]
> Windows 10 中的 DNS 用戶端服務的變更也會出現在執行 Windows Server 2016 和更新版本的電腦上。

## <a name="additional-references"></a>其他參考

-   [Windows Server 2016 中 DNS 伺服器的新功能](What-s-New-in-DNS-Server.md)


