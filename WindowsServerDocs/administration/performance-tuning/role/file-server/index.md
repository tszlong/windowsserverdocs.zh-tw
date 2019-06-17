---
title: 檔案伺服器的效能微調
description: 執行 Windows Server 的檔案伺服器效能微調
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
author: phstee
ms.author: NedPyle; Danlo; DKruse
ms.date: 4/14/2017
ms.openlocfilehash: dc8a845a6d352fa03517e2a092c44b6d1c1def4b
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811463"
---
# <a name="performance-tuning-for-file-servers"></a>檔案伺服器的效能微調

您應該選取適當的硬體，以滿足預期的檔案伺服器負載，並考量平均負載、尖峰負載、容量、成長計畫和回應時間。 硬體瓶頸限會制軟體微調的效果。

## <a name="general-tuning-parameters-for-clients"></a>用戶端的一般微調參數

下列 REG\_DWORD 登錄設定可能會影響與 SMB 檔案伺服器互動的用戶端電腦效能：

-   **ConnectionCountPerNetworkInterface**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\ConnectionCountPerNetworkInterface
    ```

    適用於 Windows 10、Windows 8.1、Windows 8、Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012

    預設值是 1，而我們強烈建議使用預設值。 有效範圍是 1-16。 每個介面要與非 RSS 介面的伺服器建立的連線數目上限。


-   **ConnectionCountPerRssNetworkInterface**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\ConnectionCountPerRssNetworkInterface
    ```

    適用於 Windows 10、Windows 8.1、Windows 8、Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012

    預設值是 4，而我們強烈建議使用預設值。 有效範圍是 1-16。 每個介面要與 RSS 介面的伺服器建立的連線數目上限。

-   **ConnectionCountPerRdmaNetworkInterface**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\ConnectionCountPerRdmaNetworkInterface
    ```

    適用於 Windows 10、Windows 8.1、Windows 8、Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012

    預設值是 2，而我們強烈建議使用預設值。 有效範圍是 1-16。 每個介面要與 RDMA 介面的伺服器建立的連線數目上限。

-   **MaximumConnectionCountPerServer**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\MaximumConnectionCountPerServer
    ```

    適用於 Windows 10、Windows 8.1、Windows 8、Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012

    預設值是 32，有效範圍是 1-64。 在所有介面中要與執行 Windows Server 2012 的單一伺服器建立的連線數目上限。

-   **DormantDirectoryTimeout**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DormantDirectoryTimeout
    ```

    適用於 Windows 10、Windows 8.1、Windows 8、Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012

    預設值是 600 秒。 伺服器目錄控點對目錄租用保持開啟狀態的時間上限。

-   **FileInfoCacheLifetime**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileInfoCacheLifetime
    ```

    適用於 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

    預設值是 10 秒。 檔案資訊快取逾時期間。

-   **DirectoryCacheLifetime**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DirectoryCacheLifetime
    ```

    適用於 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

    預設值是 10 秒。 這是目錄快取逾時。

    > [!NOTE]
    > 此參數可控制沒有目錄租用時的目錄中繼資料快取。
     

-   **DirectoryCacheEntrySizeMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DirectoryCacheEntrySizeMax
    ```

    適用於 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

    預設值是 64 KB。 這是目錄快取項目的大小上限。

-   **FileNotFoundCacheLifetime**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileNotFoundCacheLifetime
    ```

    適用於 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

    預設值是 5 秒。 找不到檔案的快取逾時期間。

-   **CacheFileTimeout**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\CacheFileTimeout
    ```

    適用於 Windows 8.1、Windows 8、Windows Server 2012、Windows Server 2012 R2 和 Windows 7

    預設值是 10 秒。 此設定可控制在應用程式關閉檔案的最後一個控制代碼之後，重新導向器會保留檔案快取資料的時間長度 (以秒為單位)。

-   **DisableBandwidthThrottling**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DisableBandwidthThrottling
    ```

    適用於 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

    預設值為 0。 根據預設，在某些情況下，SMB 重新導向器會對高延遲網路連線的輸送量進行節流，以避免網路相關逾時。 若將此登錄值設為 1 則會停用此節流，進而透過高延遲網路連接來達到更高的檔案轉送輸送量。

-   **DisableLargeMtu**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DisableLargeMtu
    ```

    適用於 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

    預設值是 0 (僅適用於 Windows 8)。 在 Windows 8 中，SMB 重新導向器會轉送每個要求 1 MB 的承載，以提升檔案轉送速度。 若將此登錄值設定為 1，則要求大小限制為 64 KB。 套用此設定之前，您應該評估其影響。

-   **RequireSecuritySignature**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\RequireSecuritySignature
    ```

    適用於 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

    預設值是 0，並停用 SMB 簽署。 將此值變更為 1 可啟用所有 SMB 通訊的 SMB 簽署，以防止 SMB 與停用 SMB 簽署的電腦通訊。 SMB 簽署可能增加 CPU 成本和網路往返，但可協助封鎖中間人攻擊。 如果 SMB 簽署並非必要，請確定所有用戶端和伺服器上的此登錄值均為 0。 
    
    如需詳細資訊，請參閱 [SMB 簽署的基本概念](https://blogs.technet.microsoft.com/josebda/2010/12/01/the-basics-of-smb-signing-covering-both-smb1-and-smb2/)。

-   **FileInfoCacheEntriesMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileInfoCacheEntriesMax
    ```

    適用於 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

    預設值是 64，有效範圍是 1 至 65536。 此值用來決定用戶端可以快取的檔案中繼資料數量。 提高此值可降低網路流量，並且在存取大量檔案時提升效能。

-   **DirectoryCacheEntriesMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DirectoryCacheEntriesMax
    ```

    適用於 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

    預設值是 16，有效範圍是 1 至 4096。 此值用來決定用戶端可以快取的目錄資訊數量。 提高此值可降低網路流量，並且在存取大量目錄時提升效能。

-   **FileNotFoundCacheEntriesMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileNotFoundCacheEntriesMax
    ```

    適用於 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

    預設值是 128，有效範圍是 1 至 65536。 此值用來決定用戶端可以快取的檔案名稱資訊數量。 提高此值可降低網路流量，並且在存取大量檔案名稱時提升效能。

-   **MaxCmds**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\MaxCmds
    ```

    適用於 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

    預設值是 15。 此參數可限制一個工作階段的未處理要求數目。 提高此值可使用更多記憶體，但可藉由啟用更深的要求管線來提升效能。 搭配 MaxMpxCt 提高此值，也可以排除由於大量未處理長期檔案要求所造成的錯誤，例如 FindFirstChangeNotification 呼叫。 此參數不會影響與 SMB 2.0 伺服器的連線。

-   **DormantFileLimit**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DormantFileLimit
    ```

    適用於 Windows 10、Windows 8.1、Windows 8、Windows 7、Windows Vista、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008

    預設值是 1023。 此參數可指定在應用程式關閉檔案之後，共用資源上應該保持開啟的檔案數目上限。

### <a name="client-tuning-example"></a>用戶端微調範例

用戶端電腦的一般微調參數可將電腦最佳化，以便存取遠端檔案共用，尤其是透過某些高延遲網路 (例如分公司、跨資料中心通訊、家庭辦公室，以及行動寬頻) 存取。 在所有電腦上，這些設定並不是最佳或適當的。 套用個別設定之前，您應該評估其影響。

| 參數                   | 值 | 預設值 |
|-----------------------------|-------|---------|
| DisableBandwidthThrottling  | 1     | 0       |
| FileInfoCacheEntriesMax     | 32768 | 64      |
| DirectoryCacheEntriesMax    | 4096  | 16      |
| FileNotFoundCacheEntriesMax | 32768 | 128     |
| MaxCmds                     | 32768 | 15      |

 

從 Windows 8 開始，您可以使用 **Set-SmbClientConfiguration** 和 **Set-SmbServerConfiguration** Windows PowerShell Cmdlet 來設定其中許多 SMB 設定。 使用 Windows PowerShell 也可以設定僅限登錄的設定。

```
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters" RequireSecuritySignature -Value 0 -Force
```
