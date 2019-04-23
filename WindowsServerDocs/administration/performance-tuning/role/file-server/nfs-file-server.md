---
title: NFS 檔案伺服器的效能微調
description: NFS 檔案伺服器的效能微調
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
author: phstee
ms.author: RoopeshB, NedPyle
ms.date: 10/16/2017
ms.openlocfilehash: 06a2a7206d3673046bd5a926a657bac91b02bf5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879009"
---
# <a name="performance-tuning-nfs-file-servers"></a>效能微調 NFS 檔案伺服器

## <a href="" id="servicesnfs"></a>Services for NFS 模型


下列各節提供網路檔案系統 (NFS) 模型，用戶端與伺服器通訊的 Microsoft 服務的相關資訊。 由於 NFS v2 及 NFS v3 仍最廣的通訊協定的版本，所有的登錄機碼除了 MaxConcurrentConnectionsPerIp 適用於 NFS v2 及 NFS v3 僅。

需要 NFS 4.1 版通訊協定沒有登錄微調。

### <a name="service-for-nfs-model-overview"></a>Service for NFS 模型概觀

Microsoft Services for NFS 會擁有 Windows 與 UNIX 混合的環境的企業，提供檔案共用解決方案。 這套通訊模型是由用戶端電腦和伺服器所組成。 用戶端上的應用程式要求重新導向程式 (Rdbss.sys) 和 NFS 迷你重新導向程式 (Nfsrdr.sys) 伺服器上的檔案。 迷你重新導向器會傳送其要求，透過 TCP/IP 使用 NFS 通訊協定。 伺服器接收用戶端透過 TCP/IP 來自多個要求，並將要求路由傳送至本機檔案系統 (Ntfs.sys)，用來存取儲存體堆疊。

下圖顯示 nfs 的通訊模型。

![nfs 的通訊模型](../../media/perftune-guide-nfs-model.png)

### <a name="tuning-parameters-for-nfs-file-servers"></a>調整參數 NFS 檔案伺服器

下列 REG\_DWORD 登錄設定可能會影響效能的 NFS 檔案伺服器：

-   **OptimalReads**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\OptimalReads
    ```

    預設值為 0。 此參數決定是否開啟檔案，以進行檔案\_隨機\_存取或檔案\_循序\_只，根據工作負載 I/O 特性。 將此值設定為 1 以強迫執行開啟檔案的檔案\_隨機\_存取。 檔案\_隨機\_存取可防止檔案系統和快取管理員預先擷取。

    >[!NOTE]
    > 此設定必須謹慎評估，因為它可能會增加對系統檔案快取可能的影響。


-   **RdWrHandleLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrHandleLifeTime
    ```

    預設值是 5。 此參數控制檔案控制代碼快取中的 NFS 快取項目存留的期。 參數是指處理其相關聯開啟 NTFS 檔案快取項目。 實際的存留期為大約等於 RdWrHandleLifeTime 乘以 RdWrThreadSleepTime。 最小值為 1，最大值為 60。

-   **RdWrNfsHandleLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsHandleLifeTime
    ```

    預設值是 5。 此參數控制檔案控制代碼快取中的 NFS 快取項目存留的期。 參數會參考沒有相關聯開啟 NTFS 檔案控制代碼的快取項目。 Services for NFS 會使用這些快取項目，來儲存檔案的檔案屬性，而不保留以檔案系統開啟的控制代碼。 實際的存留期為大約等於 RdWrNfsHandleLifeTime 乘以 RdWrThreadSleepTime。 最小值為 1，最大值為 60。

-   **RdWrNfsReadHandlesLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsReadHandlesLifeTime
    ```

    預設值是 5。 此參數控制 NFS 讀取快取檔案控制代碼快取中的項目存留的期。 實際的存留期為大約等於 RdWrNfsReadHandlesLifeTime 乘以 RdWrThreadSleepTime。 最小值為 1，最大值為 60。

-   **RdWrThreadSleepTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrThreadSleepTime
    ```

    預設值是 5。 此參數會控制檔案控制代碼快取上執行清除執行緒之前的等待間隔。 值是以刻度為單位，而它是不具決定性。 刻度相當於約 100 奈秒。 最小值為 1，最大值為 60。

-   **FileHandleCacheSizeinMB**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\FileHandleCacheSizeinMB
    ```

    預設值是 4。 此參數會指定要由檔案控制代碼快取項目使用的最大記憶體。 最小值為 1，而上限則為 1\*1024年\*1024年\*1024 (1073741824)。

-   **LockFileHandleCacheInMemory**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\LockFileHandleCacheInMemory
    ```

    預設值為 0。 此參數會指定是否在記憶體中鎖定 FileHandleCacheSizeInMB 所指定的快取大小會配置實體頁面。 將此值設定為 1，可讓此活動。 頁面鎖定記憶體中 （未分頁至磁碟），可改善效能的解析檔案控制代碼，但會減少適用於應用程式的記憶體。

-   **MaxIcbNfsReadHandlesCacheSize**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\MaxIcbNfsReadHandlesCacheSize
    ```

    預設值為 64。 此參數會指定每個讀取的資料快取的磁碟區的控制代碼的最大數目。 讀取快取項目會建立只在有多個 1 GB 的記憶體的系統上。 最小值是 0，而上限則為 0xFFFFFFFF。

-   **HandleSigningEnabled**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\HandleSigningEnabled
    ```

    預設值為 1。 此參數控制所指定 NFS 檔案伺服器的控制代碼是否已以密碼編譯方式簽署。 將它設定為 0 會停用控制代碼簽署。

-   **RdWrNfsDeferredWritesFlushDelay**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsDeferredWritesFlushDelay
    ```

    預設值為 60。 此參數會控制的 NFS V3 不穩定撰寫資料快取持續時間的軟性逾時。 最小值為 1，而且最大值為 600。 實際的存留期為大約等於 RdWrNfsDeferredWritesFlushDelay 乘以 RdWrThreadSleepTime。

-   **CacheAddFromCreateAndMkDir**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\CacheAddFromCreateAndMkDir
    ```

    預設值為 1 （已啟用）。 此參數會控制在 NFS V2 和 V3 的建立和 MKDIR RPC 程序處理常式會保留在檔案中開啟的控制代碼是否處理快取。 設定此值為 0 以停用 新增項目，以建立和 MKDIR 程式碼路徑中的快取。

-   **AdditionalDelayedWorkerThreads**

    ```
    HKLM\SYSTEM\CurrentControlSet\Control\SessionManager\Executive\AdditionalDelayedWorkerThreads
    ```

    增加會針對指定的工作佇列的延遲的背景工作執行緒數目。 延遲的背景工作執行緒程序的工作項目，不會被視為為時間緊迫，且可以包含其等候的工作項目移出分頁的記憶體堆疊。 將沒有足夠的執行緒數目會減少的速率在哪一個工作項目接受服務;值太高，這個值會無謂地耗用系統資源。

-   **NtfsDisable8dot3NameCreation**

    ```
    HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreation
    ```

    Windows Server 2012 和 Windows Server 2012 R2 中的預設值為 2。 在 Windows Server 2012 之前的版本，預設值為 0。 此參數決定是否 NTFS 會產生 8.3 的簡短名稱 (MSDOS) 命名慣例的長檔名和檔案名稱包含從擴充的字元集的字元。 如果此項目的值為 0，檔案可以有兩個名稱： 指定使用者名稱和簡短名稱，NTFS 會產生。 如果使用者指定的名稱遵循 8.3 命名慣例，NTFS 不會產生簡短名稱。 值 2 表示此參數，可以設定每個磁碟區。

    >[!NOTE]
    > 系統磁碟區已預設啟用的 8.3。 Windows Server 2012 和 Windows Server 2012 R2 中的所有磁碟區有 8.3 預設停用。 變更此值不會變更檔案的內容，但它可避免建立檔案，也會變更 NTFS 的顯示方式，以及管理檔案的簡短名稱屬性。 對於大部分的檔案伺服器，建議的設定會是 1 （停用）。


-   **NtfsDisableLastAccessUpdate**

    ```
    HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableLastAccessUpdate
    ```

    預設值為 1。 此系統通用的參數會藉由停用的檔案或目錄的上次存取日期和時間戳記更新減少磁碟 I/O 負載和延遲。

-   **MaxConcurrentConnectionsPerIp**

    ```
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Rpcxdr\Parameters\MaxConcurrentConnectionsPerIp
    ```

    MaxConcurrentConnectionsPerIp 參數的預設值為 16。 您可以增加此值上限為 8192 增加每個 IP 位址的連線數目。
