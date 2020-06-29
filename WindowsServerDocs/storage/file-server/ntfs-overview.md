---
title: NTFS 概觀
description: NTFS 的說明。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/17/2019
ms.localizationpriority: medium
ms.openlocfilehash: 450e5a4e800a8631adacb9826db63e747ecce4e1
ms.sourcegitcommit: 568b924d32421256f64abfee171304f1daf320d2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/18/2020
ms.locfileid: "85070536"
---
# <a name="ntfs-overview"></a>NTFS 概觀

>適用於：Windows 10, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

NTFS (最新版 Windows 和 Windows Server 的主要檔案系統) 提供一組完整的功能，包括安全性描述元、加密、磁碟配額，以及豐富的中繼資料，並且可與叢集共用磁碟區 (CSV) 搭配使用，以提供持續可用的磁碟區，讓您可以從容錯移轉叢集的多個節點同時存取。

若要深入了解 Windows Server 2012 R2 中的 NTFS 新功能和已變更的功能，請參閱 [NTFS 的新功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11))。 如需其他功能資訊，請參閱本主題的[其他資訊](#additional-information)一節。 若要深入了解較新的復原檔案系統 (ReFS)，請參閱[復原檔案系統 (ReFS) 概觀](../refs/refs-overview.md)。

## <a name="practical-applications"></a>實際應用

### <a name="increased-reliability"></a>提高的穩定性

當電腦在系統失敗後重新啟動時，NTFS 會使用其記錄檔和檢查點資訊來還原檔案系統的一致性。 在發生磁區損毀錯誤之後，NTFS 會動態地對包含磁區損毀的叢集進行重新對應、為資料配置新的叢集、將原始叢集標示為錯誤，而且不再使用舊的叢集。 例如，在伺服器損毀之後，NTFS 可以藉由重新執行其記錄檔來復原資料。

NTFS 會持續監視並修正背景中的暫時性損毀問題，而且不會讓磁碟區離線 (這項功能稱為[自我修復 NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10))，是在 Windows Server 2008 中引進的)。 對於較大的損毀問題，Windows Server 2012 和更新版本中的 Chkdsk 公用程式會在磁碟區上線時掃描並分析磁碟機，並將離線時間限制為在磁碟區上還原資料一致性所需的時間。 搭配叢集共用磁碟區使用 NTFS 時，不需要停機。 如需詳細資訊，請參閱 [NTFS 健康情況和 Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))。

### <a name="increased-security"></a>提升的安全性

- **檔案和資料夾的存取控制清單 (ACL) 安全性**—NTFS 可讓您設定檔案或資料夾的權限、指定您要限制或允許其存取權的群組和使用者，以及選取存取類型。

- **BitLocker 磁碟機加密的支援**—BitLocker 磁碟機加密可為重要的系統資訊和儲存在 NTFS 磁碟區上的其他資料提供額外的安全性。 從 Windows Server 2012 R2 和 Windows 8.1 開始，BitLocker 可透過支援連線待命的信賴平台模組 (TPM) 在 x86 型和 x64 型電腦上支援裝置加密，而此項目之前只能在 Windows RT 裝置上使用。 裝置加密會藉由實際將您的磁碟機從電腦移除並將其安裝在另一個位置，來協助保護 Windows 型電腦上的資料，以及封鎖惡意使用者，使其無法存取仰賴的系統檔案來探索使用者密碼，或存取磁碟機。 如需詳細資訊，請參閱 [BitLocker 的新功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn306081(v%3dws.11))。

- **支援大型磁碟區**—NTFS 可支援的磁碟區大小上限為 256 TB。 支援的磁碟區大小會受到叢集大小和叢集數目的影響。 若有 (2<sup>32</sup> –1) 個叢集 (NTFS 支援的叢集數目上限)，則可支援下列磁碟區和檔案的大小。

  |叢集大小|最大磁碟區|最大檔案|
  |---|---|---|
  |4 KB (預設大小)|16 TB|16 TB|
  |8 KB|32 TB|32 TB|
  |16 KB|64 TB|64 TB|
  |32 KB|128 TB|128 TB|
  |64 KB (大小上限)|256 TB|256 TB|

>[!IMPORTANT]
>服務和應用程式可能會對檔案和磁碟區大小施加額外的限制。 例如，如果您使用的是舊版功能，或是使用磁碟區陰影複製服務 (VSS) 快照集的備份應用程式 (而不是使用 SAN 或 RAID 機箱)，則磁碟區大小限制為 64 TB。 不過，視您的工作負載和儲存體效能而定，您可能需要使用較小的磁碟區大小。

### <a name="formatting-requirements-for-large-files"></a>大型檔案的格式化需求

若要允許適當增加大型 .vhdx 檔案的數目，以下是適用於格式化磁碟區的新建議。 若要格式化的磁碟區將與「重複資料刪除」搭配使用或將裝載非常大型的檔案 (例如大於 1 TB 的 .vhdx 檔案)，請在 Windows PowerShell 中使用 **Format-Volume** Cmdlet，並搭配下列參數。

|參數|說明|
|---|---|
|-AllocationUnitSize 64KB|設定 64 KB 的 NTFS 配置單位大小。|
|-UseLargeFRS|啟用大型檔案記錄區段 (FRS) 的支援。 若要增加磁碟區上每個檔案允許的延伸數目，這是必要參數。 對於大型 FRS 記錄，限制會從大約 150 萬個延伸數目增加到大約 600 萬個延伸數目。|

例如，下列 Cmdlet 會將磁碟機 D 格式化為 NTFS 磁碟區，並啟用 FRS 及搭配 64 KB 的配置單位大小。

```PowerShell
Format-Volume -DriveLetter D -FileSystem NTFS -AllocationUnitSize 64KB -UseLargeFRS
```

您也可以使用 **format** 命令。 在系統命令提示字元中輸入下列命令，其中 **/L** 會格式化大型 FRS 磁碟區，而 **/A:64k** 會設定 64 KB 的配置單位大小：

```PowerShell
format /L /A:64k
```

### <a name="maximum-file-name-and-path"></a>檔案名稱和路徑的長度上限

NTFS 支援長檔名和擴充長度的路徑，最大值如下所示：

- **支援長檔名且具有回溯相容性**—NTFS 允許使用長檔名，並可在磁碟上儲存 8.3 別名 (使用 Unicode)，以相容於在檔案名稱和副檔案名稱上施加 8.3 限制的檔案系統。 如有需要 (基於效能需要)，您可以在 Windows Server 2008 R2、Windows 8 和較新版本的 Windows 作業系統中，選擇性地停用個別 NTFS 磁碟區上的 8.3 別名。
  在 Windows Server 2008 R2 和更新版本的系統中，當使用作業系統格式化磁碟區時，預設會停用簡短名稱。 針對應用程式相容性，系統磁碟區上仍會啟用簡短名稱。

- **支援擴充長度的路徑**—許多 Windows API 函式具有 Unicode 版本，其允許大約 32,767 個字元的擴充長度路徑，也就是超過 MAX\_PATH 設定所定義的 260 個字元路徑限制。 如需詳細的檔案名稱和路徑格式需求，以及實作擴充長度路徑的指引，請參閱[命名檔案、路徑和命名空間](https://msdn.microsoft.com/library/windows/desktop/aa365247)。

- **叢集存放區**—在容錯移轉叢集中使用時，NTFS 支援持續可用磁碟區，在與叢集共用磁碟區 (CSV) 檔案系統搭配使用時，這些磁碟區可同時由多個叢集節點存取。 如需有關詳細資訊，請參閱[使用容錯移轉叢集的叢集共用磁碟區](../../failover-clustering/failover-cluster-csvs.md)。

### <a name="flexible-allocation-of-capacity"></a>彈性的容量配置

如果磁碟區上的空間有限，NTFS 會提供下列方法來處理伺服器的儲存容量：

- 使用磁碟配額為個別使用者追蹤和控制 NTFS 磁碟區上的磁碟空間使用量。
- 使用檔案系統壓縮來最大化可以儲存的資料量。
- 從相同磁碟或不同磁碟新增未配置的空間，以增加 NTFS 磁碟區大小。
- 如果您用完磁碟機代號，或需要建立可從現有資料夾存取的額外空間，請在本機 NTFS 磁碟區上的任何空資料夾掛接磁碟區。

## <a name="additional-information"></a>其他資訊

- [ReFS 和 NTFS 的叢集大小建議](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/Cluster-size-recommendations-for-ReFS-and-NTFS/ba-p/425960)
- [復原檔案系統 (ReFS) 概觀](../refs/refs-overview.md)
- [NTFS 的新功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)) (Windows Server 2012 R2)
- [NTFS 的新功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff383236(v=ws.10)) (Windows Server 2008 R2、Windows 7)
- [NTFS 健康情況和 Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))
- [自我修復 NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)) (於 Windows Server 2008 中引入)
- [交易式 NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730726(v%3dws.10)) (於 Windows Server 2008 中引入)
- [Windows Server 的儲存空間](../storage.yml)