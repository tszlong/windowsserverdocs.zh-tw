---
title: NTFS 概觀
description: NTFS 的說明。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/17/2019
ms.localizationpriority: medium
ms.openlocfilehash: e43d0520f97f28af54f794daf7ad263bc9928fac
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867352"
---
# <a name="ntfs-overview"></a>NTFS 概觀

>適用於：Windows 10、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

NTFS （最新版 Windows 和 Windows Server 的主要檔案系統）提供一組完整的功能，包括安全描述項、加密、磁片配額和豐富的中繼資料，並可與叢集共用磁片區（CSV）搭配使用，以持續提供可從容錯移轉叢集的多個節點同時存取的可用磁片區。

若要深入瞭解 Windows Server 2012 R2 中 NTFS 的新功能和已變更的功能，請參閱[ntfs 的新](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11))功能。 如需其他功能資訊，請參閱本主題的[其他資訊](#additional-information)一節。 若要深入瞭解較新的復原檔案系統（ReFS），請參閱[復原檔案系統（refs）總覽](../refs/refs-overview.md)。

## <a name="practical-applications"></a>實際應用

### <a name="increased-reliability"></a>提高的穩定性

NTFS 會在系統失敗後重新開機電腦時，使用其記錄檔和檢查點資訊來還原檔案系統的一致性。 在發生錯誤磁區錯誤之後，NTFS 會動態重新對應包含錯誤磁區的叢集、為數據配置新的叢集、將原始叢集標示為錯誤，而且不再使用舊的叢集。 例如，在伺服器損毀之後，NTFS 可以重新執行其記錄檔來復原資料。

NTFS 會持續監視並修正背景中的暫時性損毀問題，而不會讓磁片區離線（這項功能稱為[自我修復 NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10))，在 Windows Server 2008 中引進）。 對於較大的損毀問題，Windows Server 2012 和更新版本中的 Chkdsk 公用程式會在磁片區上線時掃描並分析磁片磁碟機，並將時間限制為在磁片區上還原資料一致性所需的時間。 搭配叢集共用磁片區使用 NTFS 時，不需要停機。 如需詳細資訊，請參閱 [NTFS 健康情況和 Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))。

### <a name="increased-security"></a>提高安全性

- **以存取控制清單（ACL）為基礎的檔案和資料夾安全性**-NTFS 可讓您設定檔案或資料夾的許可權、指定您要限制或允許其存取權的群組和使用者，以及選取存取類型。

- **BitLocker 磁碟機加密的支援**— BitLocker 磁碟機加密為重要的系統資訊和儲存在 NTFS 磁片區上的其他資料提供額外的安全性。 從 Windows Server 2012 R2 和 Windows 8.1 開始，BitLocker 可支援 x86 和 x64 型電腦上的裝置加密，以及支援連線待命的信賴平臺模組（TPM）（先前僅適用于 Windows RT 裝置）。 裝置加密可協助保護 Windows 電腦上的資料，並協助封鎖惡意使用者存取他們所依賴的系統檔案，以探索使用者的密碼，或從電腦實際移除磁片磁碟機並將它安裝在不同的。 如需詳細資訊，請參閱[BitLocker 的新功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn306081(v%3dws.11))。

- **支援大型磁片**區— NTFS 可支援的磁片區大小為 256 tb。 支援的磁片區大小受到叢集大小和叢集數目的影響。 使用（2<sup>32</sup> –1）叢集（NTFS 支援的叢集數目上限）時，會支援下列磁片區和檔案大小。

  |叢集大小|最大磁片區|最大檔案|
  |---|---|---|
  |4 KB （預設大小）|16 TB|16 TB|
  |8 KB|32 TB|32 TB|
  |16 KB|64 TB|64 TB|
  |32 KB|128 TB|128 TB|
  |64 KB （大小上限）|256 TB|256 TB|

>[!IMPORTANT]
>服務和應用程式可能會對檔案和磁片區大小施加額外的限制。 例如，如果您使用的是舊版的功能，或使用磁碟區陰影複製服務（VSS）快照集（而不是使用 SAN 或 RAID 主機殼）的備份應用程式，則磁片區大小限制為 64 TB。 不過，視您的工作負載和儲存體的效能而定，您可能需要使用較小的磁片區大小。

### <a name="formatting-requirements-for-large-files"></a>大型檔案的格式化需求

若要允許適當的大型 .vhdx 檔案延伸模組，請提供格式化磁片區的新建議。 將用於重復資料刪除的磁片區格式化，或主控非常大型的檔案（例如大於 1 TB 的 .vhdx 檔案）時，請使用 Windows PowerShell 中的**Format-Volume** Cmdlet 搭配下列參數。

|參數|描述|
|---|---|
|-AllocationUnitSize 64KB|設定 64 KB 的 NTFS 配置單位大小。|
|-UseLargeFRS|啟用大型檔案記錄區段（FRS）的支援。 若要增加磁片區上每個檔案允許的範圍數目，這是必要的。 對於大型 FRS 記錄，限制會從大約1500000範圍增加到大約6000000個範圍。|

例如，下列 Cmdlet 會將磁片磁碟機 D 格式化為 NTFS 磁片區，並啟用 FRS 且配置單位大小為 64 KB。

```PowerShell
Format-Volume -DriveLetter D -FileSystem NTFS -AllocationUnitSize 64KB -UseLargeFRS
```

您也可以使用**format**命令。 在系統命令提示字元中，輸入下列命令，其中 **/l**會格式化大型 FRS 磁片區，而 **/a： 64k**則會設定 64 KB 配置單位大小：

```PowerShell
format /L /A:64k
```

### <a name="maximum-file-name-and-path"></a>檔案名和路徑上限

NTFS 支援長檔名和擴充長度的路徑，並具有下列最大值：

- **對長檔名的支援（具有回溯相容性），** NTFS 允許長檔名，在磁片上儲存8.3 別名（Unicode），以提供與在檔案名和副檔名上施加8.3 限制之檔案系統的相容性。 如有需要，您可以在 Windows Server 2008 R2、Windows 8 和較新版本的 Windows 作業系統中，選擇性地停用個別 NTFS 磁片區上的8.3 別名。
  在 Windows Server 2008 R2 和更新版本的系統中，當使用作業系統格式化磁片區時，預設會停用簡短名稱。 針對應用程式相容性，系統磁碟區上仍會啟用簡短名稱。

- **擴充長度路徑的支援**-許多 Windows API 函式都有 Unicode 版本，允許大約32767個字元的延伸長度路徑，超過 [最大\_路徑] 設定所定義的260個字元路徑限制。 如需詳細的檔案名和路徑格式需求，以及執行延伸長度路徑的指引，請參閱[命名檔案、路徑和命名空間](https://msdn.microsoft.com/library/windows/desktop/aa365247)。

- 叢集**存放裝置**-在容錯移轉叢集中使用時，NTFS 支援可在搭配叢集共用磁片區（CSV）檔案系統使用時，同時由多個叢集節點存取的持續可用磁片區。 如需詳細資訊，請參閱[在容錯移轉叢集中使用叢集共用磁片](../../failover-clustering/failover-cluster-csvs.md)區。

### <a name="flexible-allocation-of-capacity"></a>彈性的容量配置

如果磁片區上的空間有限，NTFS 提供下列方法來處理伺服器的儲存容量：

- 使用磁片配額來追蹤和控制個別使用者的 NTFS 磁片區上的磁碟空間使用量。
- 使用檔案系統壓縮來最大化可以儲存的資料量。
- 從相同的磁片或不同的磁片新增未配置的空間，以增加 NTFS 磁片區的大小。
- 如果您用完磁碟機號，或需要建立可從現有資料夾存取的額外空間，請在本機 NTFS 磁片區上的任何空資料夾掛接磁片區。

## <a name="additional-information"></a>其他資訊

- [ReFS 和 NTFS 的叢集大小建議](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/Cluster-size-recommendations-for-ReFS-and-NTFS/ba-p/425960)
- [復原檔案系統（ReFS）總覽](../refs/refs-overview.md)
- [NTFS 的新功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11))（Windows Server 2012 R2）
- [NTFS 的新功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff383236(v=ws.10))（Windows Server 2008 R2、Windows 7）
- [NTFS 健全狀況與 Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))
- [自我修復 NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10))（在 Windows Server 2008 中引進）
- [交易式 NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730726(v%3dws.10))（在 Windows Server 2008 中引進）
- [Windows Server 的儲存空間](../storage.md)