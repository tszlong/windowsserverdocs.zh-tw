---
title: Windows Server 中 DNS 用戶端的新功能
description: 本主題概要說明 Windows Server 中 DNS 用戶端的新功能和 Windows 10
manager: brianlic
ms.topic: article
ms.assetid: 6edaba84-4595-4fd8-95d7-64d4d975a38a
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 0ad32c51b526cdead4b8b2b785dff9040ab1ef37
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949134"
---
# <a name="whats-new-in-dns-client-in-windows-server-2016"></a>Windows Server 2016 中 DNS 用戶端的新功能

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題說明 Windows 10 和 Windows Server 2016 和更新版本的這些作業系統中，新的或變更的網域名稱系統 (DNS) 用戶端功能。

## <a name="updates-to-dns-client"></a>DNS 用戶端的更新

**Dns 用戶端服務** 系結：在 Windows 10 中，Dns 用戶端服務為具有多個網路介面的電腦提供增強的支援。 針對多重主目錄電腦，DNS 解析會以下列方式進行優化：

-   使用特定介面上設定的 DNS 伺服器來解析 DNS 查詢時，DNS 用戶端服務會先系結至此介面，再傳送 DNS 查詢。

    藉由系結至特定介面，DNS 用戶端可以明確地指定名稱解析所在的介面，讓應用程式透過此網路介面優化與 DNS 用戶端的通訊。

-   如果使用的 DNS 伺服器是由名稱解析原則表中的群組原則設定所指定 (NRPT) ，則 DNS 用戶端服務不會系結至特定的介面。

> [!NOTE]
> 針對 Windows 10 中的 DNS 用戶端服務所做的變更也會出現在執行 Windows Server 2016 和更新版本的電腦上。

## <a name="additional-references"></a>其他參考資料

-   [Windows Server 2016 中 DNS 伺服器的新功能](What-s-New-in-DNS-Server.md)


