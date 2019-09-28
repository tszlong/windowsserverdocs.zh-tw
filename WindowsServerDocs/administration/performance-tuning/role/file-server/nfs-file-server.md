---
title: NFS 檔案伺服器的效能微調
description: NFS 檔案伺服器的效能微調
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
author: phstee
ms.author: RoopeshB, NedPyle
ms.date: 10/16/2017
ms.openlocfilehash: 07e5005c1bc38e791e847c8965cbc9a6c0ac96f4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355178"
---
# <a name="performance-tuning-nfs-file-servers"></a>效能微調 NFS 檔案伺服器

## <a href="" id="servicesnfs"></a>Services for NFS 模型


下列各節提供適用于用戶端-伺服器通訊之 Microsoft Services for Network File System （NFS）模型的相關資訊。 由於 NFS v2 和 NFS v3 仍然是最廣泛部署的通訊協定版本，因此除了 MaxConcurrentConnectionsPerIp 以外的所有登錄機碼僅適用于 NFS v2 和 NFS v3。

NFS 4.1 通訊協定不需要進行任何登錄調整。

### <a name="service-for-nfs-model-overview"></a>Service for NFS 模型總覽

Microsoft Services for NFS 針對具有混合 Windows 和 UNIX 環境的企業，提供檔案共用解決方案。 此通訊模型是由用戶端電腦和伺服器所組成。 用戶端上的應用程式會透過重新導向器（Rdbss）和 NFS 迷你重新導向器（Nfsrdr）位於伺服器上的要求檔案。 迷你重新導向程式會使用 NFS 通訊協定，透過 TCP/IP 傳送其要求。 伺服器會透過 TCP/IP 接收來自用戶端的多個要求，並將要求路由傳送至本機檔案系統（Ntfs），以存取儲存體堆疊。

下圖顯示 NFS 的通訊模型。

![nfs 通訊模型](../../media/perftune-guide-nfs-model.png)

### <a name="tuning-parameters-for-nfs-file-servers"></a>調整 NFS 檔案伺服器的參數

下列 REG @ no__t-0DWORD 登錄設定可能會影響 NFS 檔案伺服器的效能：

-   **OptimalReads**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\OptimalReads
    ```

    預設值為 0。 這個參數會決定檔案 @ no__t-0RANDOM @ no__t-1ACCESS 或 FILE @ no__t-2SEQUENTIAL @ no__t-3ONLY 的檔案是否開啟，視工作負載 i/o 特性而定。 將此值設定為1，以強制開啟檔案 @ no__t-0RANDOM @ no__t-1ACCESS 的檔案。 FILE @ no__t-0RANDOM @ no__t-1ACCESS 會防止檔案系統和快取管理員進行預先提取。

    >[!NOTE]
    > 必須仔細評估這種設定，因為它可能會對系統檔案快取成長造成潛在的影響。


-   **RdWrHandleLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrHandleLifeTime
    ```

    預設值為 5。 這個參數會控制檔案控制代碼快取中 NFS 快取專案的存留期。 參數會參考具有相關聯之開啟 NTFS 檔案控制代碼的快取專案。 實際存留期大約等於 RdWrHandleLifeTime 乘以 RdWrThreadSleepTime。 最小值為1，最大值為60。

-   **RdWrNfsHandleLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsHandleLifeTime
    ```

    預設值為 5。 這個參數會控制檔案控制代碼快取中 NFS 快取專案的存留期。 參數會參考沒有相關聯之開啟的 NTFS 檔案控制代碼的快取專案。 Services for NFS 會使用這些快取專案來儲存檔案的檔案屬性，而不需要在檔案系統中保留開啟的控制碼。 實際存留期大約等於 RdWrNfsHandleLifeTime 乘以 RdWrThreadSleepTime。 最小值為1，最大值為60。

-   **RdWrNfsReadHandlesLifeTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsReadHandlesLifeTime
    ```

    預設值為 5。 這個參數會控制檔案控制代碼快取中 NFS 讀取快取專案的存留期。 實際存留期大約等於 RdWrNfsReadHandlesLifeTime 乘以 RdWrThreadSleepTime。 最小值為1，最大值為60。

-   **RdWrThreadSleepTime**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrThreadSleepTime
    ```

    預設值為 5。 這個參數會控制在檔案控制代碼快取上執行清除執行緒之前的等待間隔。 值是以刻度為單位，且不具決定性。 滴答相當於大約100毫微秒。 最小值為1，最大值為60。

-   **FileHandleCacheSizeinMB**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\FileHandleCacheSizeinMB
    ```

    預設值是 4。 這個參數會指定檔案控制代碼快取專案所使用的最大記憶體。 最小值為1，最大值為 1 @ no__t-01024 @ no__t-11024 @ no__t-21024 （1073741824）。

-   **LockFileHandleCacheInMemory**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\LockFileHandleCacheInMemory
    ```

    預設值為 0。 這個參數會指定配置給 FileHandleCacheSizeInMB 所指定快取大小的實體頁面是否會在記憶體中鎖定。 將此值設定為1可啟用此活動。 分頁會在記憶體中鎖定（未分頁到磁片），這可改善解析檔案控制代碼的效能，但會減少應用程式可用的記憶體。

-   **MaxIcbNfsReadHandlesCacheSize**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\MaxIcbNfsReadHandlesCacheSize
    ```

    預設值為64。 此參數會指定讀取資料快取的每個磁片區最大控制碼數目。 讀取快取專案只會在具有超過 1 GB 記憶體的系統上建立。 最小值為0，而最大值為0xFFFFFFFF。

-   **HandleSigningEnabled**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\HandleSigningEnabled
    ```

    預設值為 1。 此參數控制由 NFS 檔案伺服器提供的控制碼是否已簽署密碼編譯。 將它設定為0會停用控制碼簽署。

-   **RdWrNfsDeferredWritesFlushDelay**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\RdWrNfsDeferredWritesFlushDelay
    ```

    預設值為60。 此參數是一種軟性的時間，可控制 NFS V3 不穩定寫入資料快取的持續時間。 最小值為1，最大值為600。 實際存留期大約等於 RdWrNfsDeferredWritesFlushDelay 乘以 RdWrThreadSleepTime。

-   **CacheAddFromCreateAndMkDir**

    ```
    HKLM\System\CurrentControlSet\Services\NfsServer\Parameters\CacheAddFromCreateAndMkDir
    ```

    預設值為1（已啟用）。 這個參數會控制在 NFS V2 和 V3 建立期間開啟的控制碼，以及在檔案控制代碼快取中保留的程式。 將此值設定為0，以停用在 CREATE 和 MKDIR 程式碼路徑中將專案新增至快取。

-   **AdditionalDelayedWorkerThreads**

    ```
    HKLM\SYSTEM\CurrentControlSet\Control\SessionManager\Executive\AdditionalDelayedWorkerThreads
    ```

    增加針對指定工作佇列所建立的延遲工作者執行緒數目。 延遲的工作者執行緒的工作專案不會被視為時間緊迫，而且可以在等候工作專案時，將其記憶體堆疊分頁出來。 執行緒數目不足可減少工作專案的服務速率;太高的值會不必要地耗用系統資源。

-   **NtfsDisable8dot3NameCreation**

    ```
    HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreation
    ```

    Windows Server 2012 和 Windows Server 2012 R2 中的預設值為2。 在 Windows Server 2012 之前的版本中，預設值為0。 此參數會決定 NTFS 是否會針對長檔名和包含擴充字元集之字元的檔案名，產生8.3 （MSDOS.SYS）命名慣例中的簡短名稱。 如果此專案的值為0，檔案可以有兩個名稱：使用者指定的名稱和 NTFS 產生的簡短名稱。 如果使用者指定的名稱遵循8.3 命名慣例，則 NTFS 不會產生簡短名稱。 值為2表示可以為每個磁片區設定此參數。

    >[!NOTE]
    > 系統磁碟區預設已啟用8.3。 Windows Server 2012 和 Windows Server 2012 R2 中的所有其他磁片區預設都會停用8.3。 變更此值不會變更檔案的內容，但它會避免檔案的簡短名稱屬性建立，這也會變更 NTFS 顯示和管理檔案的方式。 對於大部分的檔案伺服器而言，建議的設定為1（停用）。


-   **NtfsDisableLastAccessUpdate**

    ```
    HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableLastAccessUpdate
    ```

    預設值為 1。 此系統全域交換器會藉由停用最後一個檔案或目錄存取的日期和時間戳記更新，來減少磁片 i/o 負載和延遲。

-   **MaxConcurrentConnectionsPerIp**

    ```
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Rpcxdr\Parameters\MaxConcurrentConnectionsPerIp
    ```

    MaxConcurrentConnectionsPerIp 參數的預設值為16。 您可以將此值增加到最多8192，以增加每個 IP 位址的連接數目。
