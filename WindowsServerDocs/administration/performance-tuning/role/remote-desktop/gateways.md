---
title: 效能調整遠端桌面閘道
description: 遠端桌面閘道的效能微調建議
ms.topic: article
ms.author: hammadbu
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 34305b94ff431f90d152bfd987c827b990fb0e74
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078165"
---
# <a name="performance-tuning-remote-desktop-gateways"></a>效能調整遠端桌面閘道

> [!NOTE]
> 在 Windows 8 + 和 Windows Server 2012 R2 + 中，遠端桌面閘道 (RD 閘道) 支援 TCP、UDP 和舊版 RPC 傳輸。 下列大部分資料都與舊版 RPC 傳輸有關。 如果未使用舊版 RPC 傳輸，則不適用此區段。

本主題說明效能相關的參數，這些參數有助於改善客戶部署的效能，以及依賴客戶網路使用模式的並。

在其核心中，RD 閘道會在遠端桌面連線實例和客戶網路內 RD 工作階段主機伺服器實例之間執行許多封包轉送作業。

> [!NOTE]
> 下列參數僅適用于 RPC 傳輸。

Internet Information Services (IIS) 並 RD 閘道匯出下列登錄參數，以協助改善 RD 閘道中的系統效能。

**執行緒並**

-   **Maxiothreads**

    ``` syntax
    HKLM\Software\Microsoft\Terminal Server Gateway\Maxiothreads (REG_DWORD)
    ```

    此應用程式專用的執行緒集區會指定 RD 閘道建立來處理傳入要求的執行緒數目。 如果有此登錄設定，它會生效。 執行緒數目等於邏輯進程的數目。 如果邏輯處理器的數目小於5，預設值為5個執行緒。

-   **MaxPoolThreads**

    ``` syntax
    HKLM\System\CurrentControlSet\Services\InetInfo\Parameters\MaxPoolThreads (REG_DWORD)
    ```

    此參數會指定要為每個邏輯處理器建立的 IIS 集區執行緒數目。 IIS 集區執行緒會監看網路中的要求，並處理所有傳入的要求。 **MaxPoolThreads**計數不包含 RD 閘道耗用的執行緒。 預設值為 4。

**RD 閘道的遠端程序呼叫並**

下列參數有助於微調遠端程序呼叫， (RPC) 由遠端桌面連線和 RD 閘道電腦接收。 變更 windows 有助於節流流經每個連接的資料量，並可改善 RPC over HTTP v2 案例的效能。

-   **ServerReceiveWindow**

    ``` syntax
    HKLM\Software\Microsoft\Rpc\ServerReceiveWindow (REG_DWORD)
    ```

    預設值為 64 KB。 此值會指定伺服器用於從 RPC proxy 接收之資料的視窗。 最小值會設定為 8 KB，而最大值會設定為 1 GB。 如果值不存在，則會使用預設值。 變更此值時，必須重新開機 IIS，變更才會生效。

-   **ServerReceiveWindow**

    ``` syntax
    HKLM\Software\Microsoft\Rpc\ServerReceiveWindow (REG_DWORD)
    ```

    預設值為 64 KB。 此值會指定用戶端用來接收自 RPC proxy 之資料的視窗。 最小值為 8 KB，最大值為 1 GB。 如果值不存在，則會使用預設值。

## <a name="monitoring-and-data-collection"></a>監視和資料收集

當您監視 RD 閘道上的資源使用量時，會將下列效能計數器清單視為一組基本的計數器：

-   \\終端機服務閘道\\\*

-   \\RPC/HTTP Proxy\\\*

-   \\每一伺服器的 RPC/HTTP Proxy\\\*

-   \\Web 服務\\\*

-   \\W3SVC \_ w3wp.exe\\\*

-   \\IPv4\\\*

-   \\記憶\\\*

-   \\網路介面 (\*) \\\*

-   \\進程 (\*) \\\*

-   \\處理器資訊 (\*) \\\*

-   \\同步處理 (\*) \\\*

-   \\系統\\\*

-   \\TCPv4\\\*

下列效能計數器僅適用于舊版 RPC 傳輸：

-   \\RPC/HTTP Proxy \\ \* rpc

-   \\每一伺服器 rpc 的 RPC/HTTP Proxy \\ \*

-   \\Web 服務 \\ \* RPC

-   \\W3SVC \_ W3wp.exe \\ \* RPC

> [!NOTE]
> 如果適用的話，請新增 \\ IPv6 \\ \* 和 \\ TCPv6 \\ \* 物件。ReplaceThisText

