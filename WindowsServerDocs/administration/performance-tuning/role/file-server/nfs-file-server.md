---
title: NFS 檔案伺服器的效能微調
description: NFS 檔案伺服器的效能微調
ms.topic: article
author: phstee
ms.author: roopeshb
ms.date: 10/16/2017
ms.openlocfilehash: dfa16581f0849b792f679c3fde31e390a623a133
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077255"
---
# <a name="performance-tuning-nfs-file-servers"></a>效能微調 NFS 檔案伺服器

## <a name="services-for-nfs-model"></a><a href="" id="servicesnfs"></a>Services for NFS 模型


下列各節提供 Microsoft Services for Network File System (NFS) 模型的相關資訊，以進行用戶端-伺服器通訊。 由於 NFS v2 和 NFS v3 仍是最廣泛部署的通訊協定版本，因此除了 MaxConcurrentConnectionsPerIp 以外的所有登錄機碼只適用于 NFS v2 和 NFS v3。

NFS 4.1 通訊協定不需要進行登錄微調。

### <a name="service-for-nfs-model-overview"></a>Service for NFS 模型總覽

Microsoft Services for NFS 針對具有混合 Windows 和 UNIX 環境的企業，提供檔案共用解決方案。 此通訊模型包含用戶端電腦和伺服器。 透過重新導向器 ( # A0) 和 NFS 迷你重新導向程式在伺服器上的用戶端要求檔案上的應用程式 ( # A1) 。 迷你重新導向程式會使用 NFS 通訊協定，透過 TCP/IP 傳送其要求。 伺服器會透過 TCP/IP 接收來自用戶端的多個要求，並將要求路由傳送至本機檔案系統 ( # A0) ，以存取儲存體堆疊。

下圖顯示 NFS 的通訊模型。

![nfs 通訊模型](../../media/perftune-guide-nfs-model.png)

### <a name="tuning-parameters-for-nfs-file-servers"></a>調整 NFS 檔案伺服器的參數

下列 REG \_ DWORD 登錄設定可能會影響 NFS 檔案伺服器的效能：

-   **OptimalReads**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\OptimalReads
    ```

    預設值是 0。 此參數會 \_ \_ \_ \_ 根據工作負載 i/o 特性，決定是否要開啟檔案以進行檔案隨機存取或僅限檔案順序。 將此值設定為1，以強制開啟檔案以進行 \_ 隨機 \_ 存取。 檔案 \_ 隨機 \_ 存取會防止檔案系統和快取管理員進行預先提取。

    >[!NOTE]
    > 必須仔細評估此設定，因為它可能會對系統檔案快取成長造成影響。


-   **RdWrHandleLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrHandleLifeTime
    ```

    預設值為 5。 此參數可控制檔案控制代碼快取中 NFS 快取專案的存留期。 參數指的是具有相關聯開啟 NTFS 檔案控制代碼的快取專案。 實際存留期大約等於 RdWrHandleLifeTime 乘以 RdWrThreadSleepTime。 最小值為1，最大值為60。

-   **RdWrNfsHandleLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsHandleLifeTime
    ```

    預設值為 5。 此參數可控制檔案控制代碼快取中 NFS 快取專案的存留期。 參數會參考沒有相關聯之開啟 NTFS 檔案控制代碼的快取專案。 Services for NFS 會使用這些快取專案來儲存檔案的檔案屬性，而不需保留檔案系統的開啟控制碼。 實際存留期大約等於 RdWrNfsHandleLifeTime 乘以 RdWrThreadSleepTime。 最小值為1，最大值為60。

-   **RdWrNfsReadHandlesLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsReadHandlesLifeTime
    ```

    預設值為 5。 此參數可控制檔案控制代碼快取中 NFS 讀取快取專案的存留期。 實際存留期大約等於 RdWrNfsReadHandlesLifeTime 乘以 RdWrThreadSleepTime。 最小值為1，最大值為60。

-   **RdWrThreadSleepTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrThreadSleepTime
    ```

    預設值為 5。 此參數會控制在檔案控制代碼快取上執行清除執行緒之前的等候間隔。 值是以刻度為單位，且不具決定性。 刻度相當於大約100的毫微秒。 最小值為1，最大值為60。

-   **FileHandleCacheSizeinMB**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\FileHandleCacheSizeinMB
    ```

    預設值為 4。 此參數會指定檔案控制代碼快取專案所要使用的最大記憶體。 最小值為1，最大值為 1 \* 1024 \* 1024 \* 1024 (1073741824) 。

-   **LockFileHandleCacheInMemory**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\LockFileHandleCacheInMemory
    ```

    預設值是 0。 此參數會指定針對 FileHandleCacheSizeInMB 指定的快取大小所配置的實體頁面是否會在記憶體中鎖定。 將此值設定為1會啟用此活動。 頁面會在記憶體中鎖定 (不會分頁至磁片) ，這樣可改善解析檔案控制代碼的效能，但會減少應用程式可用的記憶體。

-   **MaxIcbNfsReadHandlesCacheSize**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\MaxIcbNfsReadHandlesCacheSize
    ```

    預設值為 64。 此參數會指定讀取資料快取之每個磁片區的控制碼數目上限。 讀取快取專案只會在記憶體超過 1 GB 的系統上建立。 最小值為0，最大值為0xFFFFFFFF。

-   **HandleSigningEnabled**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\HandleSigningEnabled
    ```

    預設值是 1。 此參數控制是否以密碼編譯方式簽署 NFS 檔案伺服器所指定的處理常式。 將它設定為0會停用控制碼簽署。

-   **RdWrNfsDeferredWritesFlushDelay**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsDeferredWritesFlushDelay
    ```

    預設值是 60。 此參數是一個軟超時，可控制 NFS V3 不穩定寫入資料快取的持續時間。 最小值為1，最大值為600。 實際存留期大約等於 RdWrNfsDeferredWritesFlushDelay 乘以 RdWrThreadSleepTime。

-   **CacheAddFromCreateAndMkDir**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\CacheAddFromCreateAndMkDir
    ```

    預設值為 1 (已啟用) 。 此參數控制在 NFS V2 和 V3 建立期間開啟的控制碼，以及在檔案控制代碼快取中保留的 MKDIR RPC 程式處理常式。 將此值設定為0，以停用在 CREATE 和 MKDIR 程式碼路徑中將專案加入至快取。

-   **AdditionalDelayedWorkerThreads**

    ```
    HKLM\SYSTEM\CurrentControlSet\Control\SessionManager\Executive\AdditionalDelayedWorkerThreads
    ```

    增加為指定工作佇列建立的延遲工作者執行緒數目。 延遲工作者執行緒的工作專案不被視為時間緊迫，而且在等候工作專案時，可能會將其記憶體堆疊分頁。 沒有足夠的執行緒數目可減少工作專案的維修率;太高的值會不必要地耗用系統資源。

-   **NtfsDisable8dot3NameCreation**

    ```
    HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreation
    ```

    Windows Server 2012 和 Windows Server 2012 R2 中的預設值為2。 在 Windows Server 2012 之前的版本中，預設值為0。 此參數會判斷 NTFS 是否會在 8.3 (MSDOS.SYS) 命名慣例中，為長檔名和包含擴充字元集字元的檔案名指定名稱。 如果這個專案的值為0，則檔案可以有兩個名稱：使用者指定的名稱，以及 NTFS 產生的簡短名稱。 如果使用者指定的名稱遵循8.3 命名慣例，則 NTFS 不會產生簡短名稱。 值為2表示可以針對每個磁片區設定此參數。

    >[!NOTE]
    > 系統磁碟區預設會啟用8.3。 Windows Server 2012 和 Windows Server 2012 R2 中的所有其他磁片區依預設會停用8.3。 變更這個值並不會變更檔案的內容，但可避免檔案的簡短名稱屬性建立，而這也會變更 NTFS 顯示和管理檔案的方式。 對於大部分的檔案伺服器，建議的設定為 1 (停用) 。


-   **NtfsDisableLastAccessUpdate**

    ```
    HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableLastAccessUpdate
    ```

    預設值是 1。 這個系統全域交換器會藉由停用最後一個檔案或目錄存取的日期和時間戳記更新，來減少磁片 i/o 負載和延遲。

-   **MaxConcurrentConnectionsPerIp**

    ```
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Rpcxdr\Parameters\MaxConcurrentConnectionsPerIp
    ```

    MaxConcurrentConnectionsPerIp 參數的預設值為16。 您可以將此值增加到最大值8192，以增加每個 IP 位址的連接數目。
