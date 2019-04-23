---
title: NTFS 概觀
description: 說明的 NTFS 是什麼。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/06/2018
ms.localizationpriority: medium
ms.openlocfilehash: 556110fe7bed1aed002ef6d985324ff5171e770e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885369"
---
# <a name="ntfs-overview"></a>NTFS 概觀

>適用於：Windows 10，Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

NTFS-最新版本的 Windows 和 Windows Server 的主要檔案系統，提供一組完整的功能，包括安全性描述元、 加密、 磁碟配額，以及豐富的中繼資料，並可以持續提供使用與叢集共用磁碟區 (CSV)您可以從 容錯移轉叢集的多個節點同時存取的可用磁碟區。

若要深入了解 Windows Server 2012 R2 中 ntfs 的新的和變更功能，請參閱[What's New in NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11))。 其他功能的詳細資訊，請參閱 <<c0> [ 更多資訊](#additional-information)本主題一節。 若要深入了解較新的復原檔案系統 (ReFS)，請參閱[復原檔案系統 (ReFS) 概觀](../refs/refs-overview.md)。

## <a name="practical-applications"></a>實際應用

### <a name="increased-reliability"></a>增加的可靠性

NTFS 會使用它的記錄檔和檢查點資訊，在系統失敗後重新啟動電腦時，還原的檔案系統一致性。 不良磁區發生錯誤後 NTFS 以動態方式重新對應包含損壞的磁區，來配置新的叢集資料、 將標示為已損毀，在原始叢集中，而且不再使用舊叢集的叢集。 比方說，在伺服器當機之後 NTFS 可以復原資料的重新執行其記錄檔。

NTFS 持續監視，並修正在背景中的暫時性損毀問題，而不用讓磁碟區離線 (這項功能就所謂[自我修復 NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10))Windows Server 2008 中導入)。 對於較大的損毀問題，Chkdsk 公用程式，在 Windows Server 2012 和更新版本，會掃描並分析磁碟機，而磁碟區已上線，限制時間離線還原磁碟區上的資料一致性所需的時間。 NTFS 搭配叢集共用磁碟區，不需停機時需要。 如需詳細資訊，請參閱 [NTFS 健康情況和 Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))。

### <a name="increased-security"></a>更高的安全性

- **存取控制清單 ACL 為基礎的安全性，為檔案和資料夾**— NTFS 可讓您設定的檔案或資料夾的權限指定的群組和使用者的存取權，您想要限制或允許，然後選取 存取類型。

- **支援 BitLocker 磁碟機加密**— BitLocker 磁碟機加密提供額外的安全性，重要的系統資訊和其他儲存在 NTFS 磁碟區上的資料。 從 Windows Server 2012 R2 和 Windows 8.1，BitLocker 提供支援 x86 和 x64 型電腦使用信賴平台模組 」 (TPM) 上的裝置加密支援連線待命 （先前僅適用於 Windows RT 裝置）。 裝置加密協助保護 Windows 電腦上的資料，以及它協助封鎖惡意使用者無法存取它們來探索使用者的密碼，依賴的系統檔案，或無法存取實際從電腦移除，並安裝上的磁碟機另一個。 如需詳細資訊，請參閱 <<c0> [ 什麼是 BitLocker 的新](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn306081(v%3dws.11))。

- **支援大型磁碟區**— NTFS 可支援最大達 256 tb 的磁碟區。 支援磁碟區大小會受到叢集大小和叢集數目。 使用 (2<sup>32</sup> – 1) 支援叢集 （NTFS 支援的叢集的最大的數目），下列磁碟區和檔案大小。

  |叢集大小|最大的磁碟區|最大的檔案|
  |---|---|---|
  |4 KB （預設大小）|16 TB|16 TB|
  |8 KB|32 TB|32 TB|
  |16 KB|64 TB|64 TB|
  |32 KB|128 TB|128 TB|
  |64 KB （最大大小）|256 TB|256 TB|

>[!IMPORTANT]
>服務和應用程式可能會造成其他檔案和磁碟區大小的限制。 例如，磁碟區大小限制為 64 TB，如果您使用 [舊版] 功能或備份的應用程式，可使用的磁碟區陰影複製服務 (VSS) 快照集 （和您不使用 SAN 或 RAID 機箱）。 不過，您可能需要使用較小的磁碟區大小，視您的工作負載和您的儲存體的效能而定。

### <a name="formatting-requirements-for-large-files"></a>格式需求的大型檔案

若要允許適當的擴充功能的大型的.vhdx 檔案，有新建議的磁碟區格式化。 格式化將用於重複資料刪除，或將主機超大型的檔案，例如.vhdx 檔案大於 1 TB，磁碟區時使用**格式化磁碟區**Windows PowerShell 中使用下列參數的 cmdlet。

|參數|描述|
|---|---|
|-AllocationUnitSize 64 KB|設定 64 KB NTFS 配置單位大小。|
|-UseLargeFRS|啟用支援大型檔案記錄區段 (FRS)。 這需要增加每個磁碟區上的檔案所允許的範圍數目。 對於大型 FRS 記錄，限制從 15 萬範圍增加至大約 6 百萬個範圍。|

例如，下列 cmdlet 格式 D 磁碟機為 NTFS 磁碟區與啟用的 FRS 和 64 KB 配置單位大小。

```PowerShell
Format-Volume -DriveLetter D -FileSystem NTFS -AllocationUnitSize 64KB -UseLargeFRS
```

您也可以使用**格式**命令。 在系統命令提示字元中，輸入下列命令，其中 **/L**格式的大型 FRS 磁碟區並 **/A:64 k**設定 64 KB 配置單位大小：

```PowerShell
format /L /A:64k
```

### <a name="maximum-file-name-and-path"></a>最大的檔案名稱和路徑

NTFS 支援長檔名和擴充長度路徑，使用下列的最大值：

- **長檔名，回溯相容性支援**— NTFS 允許長檔名，以提供與加諸的限制 8.3 檔案名稱和副檔名的檔案系統的相容性 8.3 別名儲存在磁碟上 （Unicode)。 如有需要 （基於效能考量），您可以選擇性地停用個別的 NTFS 磁碟區，在 Windows Server 2008 R2、 Windows 8 和較新版本的 Windows 作業系統上的 8.3 別名。
  在 Windows Server 2008 R2 和更新版本的系統中，簡短名稱預設為停用格式化磁碟區時使用的作業系統。 應用程式相容性，簡短名稱仍然會啟用在系統磁碟區。

- **擴充長路徑支援**— 許多 Windows API 函式具有允許的長度擴充路徑大約 32,767 個字元的 Unicode 版本 — 超過最大值所定義的路徑超過 260 個字元限制\_路徑設定。 詳細的檔案名稱和路徑格式需求，以及實作擴充長路徑的指引，請參閱[命名檔案、 路徑與命名空間](https://msdn.microsoft.com/library/windows/desktop/aa365247)。

- **叢集儲存體**— NTFS 中容錯移轉叢集使用時，支援持續可用的磁碟區，可由多個叢集節點與叢集共用磁碟區 (CSV) 檔案系統搭配使用時，同時存取。 如需詳細資訊，請參閱 <<c0> [ 使用容錯移轉叢集的叢集共用磁碟區](../../failover-clustering/failover-cluster-csvs.md)。

### <a name="flexible-allocation-of-capacity"></a>彈性的配置的容量

如果磁碟區上的空間有限，NTFS 提供下列方式來使用伺服器的儲存體容量：

- 您可以使用 磁碟配額來追蹤及控制個別使用者的 NTFS 磁碟區上的磁碟空間使用量。
- 您可以使用檔案系統壓縮最大化的可以儲存的資料量。
- 藉由新增未配置的空間，從相同的磁碟或不同的磁碟增加 NTFS 磁碟區的大小。
- 如果您用盡磁碟機代號或需要建立額外的空間可從現有的資料夾，請在本機的 NTFS 磁碟區掛接磁碟區任何空白資料夾。

## <a name="additional-information"></a>其他資訊

|內容類型|參考|
|---|---|
|評估|- [在 NTFS 中最新消息](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11))(Windows Server 2012 R2)<br>- [在 NTFS 中最新消息](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff383236(v=ws.10))(Windows 7 中的 Windows Server 2008 R2）<br>- [NTFS 健康情況和 Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))<br>- [自我修復 NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)) （Windows Server 2008 中導入）<br>- [交易式 NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730726(v%3dws.10)) （Windows Server 2008 中導入）|
|社群資源|- [Windows 儲存體團隊部落格](https://blogs.msdn.microsoft.com/san/)|
|相關技術|- [Windows Server 中的儲存體](../storage.md)<br>- [使用叢集共用磁碟區，在容錯移轉叢集中](../../failover-clustering/failover-cluster-csvs.md)<br>-[叢集共用磁碟區](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)#cluster-shared-volumes>)並[儲存體設計](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)#storage-design>)區段[設計雲端基礎結構](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)) <br>- [儲存空間](../storage-spaces/overview.md)<br>- [彈性檔案系統 (ReFS) 概觀](../refs/refs-overview.md)
