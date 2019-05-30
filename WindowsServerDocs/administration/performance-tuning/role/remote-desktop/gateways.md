---
title: 效能微調遠端桌面閘道
description: 效能微調建議，以遠端桌面閘道
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: HammadBu; VladmiS
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 70b27d45acbfb046d52271a50ca7deffb226b8d0
ms.sourcegitcommit: d84dc3d037911ad698f5e3e84348b867c5f46ed8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/28/2019
ms.locfileid: "66266723"
---
# <a name="performance-tuning-remote-desktop-gateways"></a>效能微調遠端桌面閘道

> [!Note]
> 在 Windows 8 及更新版本和 Windows Server 2012 R2 +，遠端桌面閘道 （RD 閘道） 支援 TCP、 UDP 和舊版的 RPC 傳輸。 大部分的下列資料有關舊版 RPC 傳輸。 如果未使用舊版的 RPC 傳輸，不適用這一節。

本主題說明的效能相關的參數，協助改善客戶部署的效能及仰賴客戶的網路使用量模式 tunings。

基本上，RD 閘道會執行許多的封包轉送遠端桌面連線執行個體與客戶的網路中的 RD 工作階段主機伺服器執行個體之間的作業。

> [!Note]
> RPC 傳輸只適用於下列的參數。

Internet Information Services (IIS) 和 RD 閘道匯出下列的登錄參數，以協助改善在 RD 閘道的系統效能。

**執行緒 tunings**

-   **Maxiothreads**

    ``` syntax
    HKLM\Software\Microsoft\Terminal Server Gateway\Maxiothreads (REG_DWORD)
    ```

    此應用程式專屬執行緒集區指定 RD 閘道會建立處理傳入要求的執行緒的數目。 如果此登錄設定存在，就會生效。 執行緒數目等於邏輯的處理序數目。 如果邏輯處理器的數目小於 5 時，預設值是 5 個執行緒。

-   **MaxPoolThreads**

    ``` syntax
    HKLM\System\CurrentControlSet\Services\InetInfo\Parameters\MaxPoolThreads (REG_DWORD)
    ```

    此參數指定 IIS 集區的執行緒建立每個邏輯處理器的數目。 IIS 集區執行緒觀賞網路上的要求，並處理所有的連入要求。 **MaxPoolThreads**計數不包括 RD 閘道使用的執行緒。 預設值為 4。

**遠端程序呼叫 tunings RD 閘道**

下列參數以協助調整遠端程序呼叫 (RPC) 所接收的遠端桌面連線和 RD 閘道的電腦。 變更 windows，可協助進行節流處理多少資料流經每個連線，並可改善 rpc over HTTP v2 案例的效能。

-   **ServerReceiveWindow**

    ``` syntax
    HKLM\Software\Microsoft\Rpc\ServerReceiveWindow (REG_DWORD)
    ```

    預設值為 64 KB。 這個值指定視窗中，伺服器使用的 RPC proxy 從收到的資料。 最小值設為 8 KB，和最大值設為 1 GB。 如果值不存在，則會使用預設值。 此值變更時，則必須重新啟動 IIS，變更才會生效。

-   **ServerReceiveWindow**

    ``` syntax
    HKLM\Software\Microsoft\Rpc\ServerReceiveWindow (REG_DWORD)
    ```

    預設值為 64 KB。 這個值會指定用戶端使用的資料從 RPC proxy 接收到的視窗。 最小值為 8 KB，和最大值為 1 GB。 如果值不存在，則會使用預設值。

## <a name="monitoring-and-data-collection"></a>監視和資料收集


下列效能計數器的清單會被視為一組基底計數器，當您監視在 RD 閘道上的資源使用量：

-   \\終端機服務閘道\\\*

-   \\RPC/HTTP Proxy\\\*

-   \\每一部伺服器的 RPC/HTTP Proxy\\\*

-   \\Web 服務\\\*

-   \\W3SVC\_W3WP\\\*

-   \\IPv4\\\*

-   \\記憶體\\\*

-   \\Network Interface(\*)\\\*

-   \\Process(\*)\\\*

-   \\處理器資訊 (\*)\\\*

-   \\Synchronization(\*)\\\*

-   \\系統\\\*

-   \\TCPv4\\\*

只適用於舊版的 RPC 傳輸的下列效能計數器︰

-   \\RPC/HTTP Proxy\\ \* RPC

-   \\每一部伺服器的 RPC/HTTP Proxy\\ \* RPC

-   \\Web 服務\\\* RPC

-   \\W3SVC\_W3WP\\\* RPC

**附註**  如果適用的話，新增\\IPv6\\ \*並\\tcpv6-已\\\*物件。ReplaceThisText

 
