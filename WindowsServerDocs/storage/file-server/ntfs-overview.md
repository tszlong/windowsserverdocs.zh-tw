---
title: NTFS 概觀
description: NTFS 的是說明。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/06/2018
ms.localizationpriority: medium
ms.openlocfilehash: 556110fe7bed1aed002ef6d985324ff5171e770e
ms.sourcegitcommit: 5ed023a2ef3a9002daf41c7717feb1df186d2a14
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/02/2019
ms.locfileid: "9122101"
---
# NTFS 概觀

>適用於： Windows 10、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

NTFS — 最新版 Windows 和 Windows Server 的主要檔案系統 — 提供一組完整的功能，包括安全性描述元、 加密、 磁碟配額，以及豐富的中繼資料，並且可與叢集共用磁碟區 (CSV) 以提供持續可用的磁碟區讓您可以從容錯移轉叢集的多個節點同時存取。

若要深入了解新功能和變更 NTFS 在 Windows Server 2012 R2 中，請參閱[的新功能 NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11))。 如 」 的額外功能的詳細資訊，請參閱本主題的[其他資訊](#additional-information)一節。 若要深入了解較新的彈性檔案系統 (ReFS)，請參閱[復原檔案系統 (ReFS) 概觀](../refs/refs-overview.md)。

## 實際應用

### 更高的可靠性

NTFS 來還原檔案系統的一致性，系統失敗後重新啟動電腦時使用它的記錄檔和檢查點資訊。 壞磁區發生錯誤後 NTFS 動態重新對應包含錯誤的磁區、 資料配置新的叢集、 會標示為已損毀，原始叢集和不再使用舊的叢集中的叢集。 例如，伺服器損毀之後, NTFS 可以復原資料重新執行其記錄檔。

NTFS 持續監視並更正暫時性損毀的問題，在背景中，而不需要讓磁碟區離線 （此功能稱為 「[自我修復 NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10))，在 Windows Server 2008 導入）。 針對較大的損毀問題，Chkdsk 公用程式，在 Windows Server 2012 和更新版本，掃描，並將磁碟區仍在線上，限制離線時間來還原資料磁碟區上的一致性所需的時間時，會分析磁碟機。 使用叢集共用磁碟區使用 NTFS 時，不停機時間是必要的。 如需詳細資訊，請參閱[NTFS 健康情況和 Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))。

### 增加的安全性

- **檔案和資料夾的存取控制清單 ACL 為基礎的安全性**— NTFS 可讓您設定的權限的檔案或資料夾中，指定的群組和使用者的存取權，您想要限制或允許，然後選取存取類型。

- **BitLocker 磁碟機加密支援**— BitLocker 磁碟機加密提供額外的安全性重要的系統資訊和其他 NTFS 磁碟區上儲存的資料。 從 Windows Server 2012 R2 和 Windows 8.1，BitLocker 可提供支援 x86 和 x64 型電腦與信賴平台模組 」 (TPM) 的裝置加密可支援連線待命 （先前僅適用於 Windows RT 裝置）。 裝置加密可協助保護 Windows 電腦上的資料，以及它協助封鎖惡意使用者無法存取它們來探索使用者的密碼依賴的系統檔案或透過實際將它從電腦移除並安裝上存取磁碟機不同的其中一種。 如需詳細資訊，請參閱[什麼是 BitLocker 的新功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn306081(v%3dws.11))。

- **支援大型磁碟區**— NTFS 可支援大 256 tb 的磁碟區。 支援的磁碟區大小會受到叢集大小和叢集數目。 使用 (2<sup>32</sup> – 1) 支援叢集 （最大數目 NTFS 支援的叢集）、 下列的磁碟區和檔案大小。

  |叢集大小|最大的磁碟區|最大的檔案|
  |---|---|---|
  |4 KB （預設大小）|16 TB|16 TB|
  |8 KB|32 TB|32 TB|
  |16KB|64 TB|64 TB|
  |32 KB|128 TB|128 TB|
  |64 KB （最大大小）|256 TB|256 TB|

>[!IMPORTANT]
>服務和應用程式可能加諸其他檔案和磁碟區的大小限制。 例如，磁碟區大小上限是 64 TB，如果您使用舊版功能或備份應用程式，可讓使用磁碟區陰影複製服務 (VSS) 快照 （和您不使用 SAN 或 RAID 機殼）。 不過，您可能需要使用較小的磁碟區大小，取決於您的工作負載和存放裝置效能。

### 大型檔案格式需求

若要允許適當的擴充功能的大型.vhdx 檔案，有新的建議格式化磁碟區。 格式化磁碟區與重複資料刪除搭配使用，或將裝載非常大的檔案，例如.vhdx 檔案大於 1 TB，使用 Windows PowerShell 中的**磁碟區格式化**cmdlet 與下列參數。

|參數|描述|
|---|---|
|-AllocationUnitSize 64 KB|設定 64 KB NTFS 配置單位大小。|
|-UseLargeFRS|啟用支援大型檔案記錄區段 (FRS)。 這被需要增加允許每個磁碟區上檔案的範圍的數量。 對於大型 FRS 記錄，限制 15 萬範圍從增加到 6 百萬個範圍。|

例如，下列 cmdlet 格式磁碟機 D 為 NTFS 磁碟區，使用 FRS 啟用和 64 KB 的配置單位大小。

```PowerShell
Format-Volume -DriveLetter D -FileSystem NTFS -AllocationUnitSize 64KB -UseLargeFRS
```

您也可以使用**格式**命令。 系統命令提示字元中，輸入下列命令，其中 **/L**格式的大型 FRS 磁碟區，而 **/A:64 k**設定 64 KB 配置單位大小：

```PowerShell
format /L /A:64k
```

### 最大的檔名和路徑

NTFS 支援較長的檔案名稱和延伸長度路徑，具有下列的最大值：

- **長的檔案名稱，回溯相容性的支援**— NTFS 允許長檔名，8.3 別名磁碟上儲存 （在 Unicode) 來提供與加諸 8.3 限制檔名和副檔名的檔案系統的相容性。 如有需要 （基於效能考量），您可以選擇性地停用 Windows Server 2008 R2、 Windows 8 及較新版本的 Windows 作業系統中的個別 NTFS 磁碟區上的 8.3 鋸齒化。
  在 Windows Server 2008 R2 和更新版本的系統，簡短名稱預設會停用時使用的作業系統格式化磁碟區。 應用程式相容性，簡短名稱仍會啟用系統磁碟區上。

- **支援延伸長度路徑**— 許多 Windows API 函式中都有允許延伸長度的路徑大約 32767 個字元的 Unicode 版本 — 超出 MAX\_PATH 設定所定義的路徑 260 字元限制。 詳細的檔名和路徑格式需求，以及實作延伸長度路徑的指導方針，請參閱[命名檔案、 路徑及命名空間](https://msdn.microsoft.com/library/windows/desktop/aa365247)。

- **叢集儲存體**— NTFS 時使用，請在容錯移轉叢集支援持續可用的磁碟區可以由多個叢集節點的叢集共用磁碟區 (CSV) 檔案系統搭配使用時，同時存取。 如需詳細資訊，請參閱[使用容錯移轉叢集的叢集共用磁碟區](../../failover-clustering/failover-cluster-csvs.md)。

### 有彈性的配置的容量

如果磁碟區上的空間有限，NTFS 會提供可搭配使用的伺服器的儲存容量下列方式：

- 使用磁碟配額，來追蹤和控制針對個別使用者 NTFS 磁碟區上的磁碟空間使用量。
- 您可以使用檔案系統壓縮來獲得最佳的可能會儲存的資料量。
- 從相同的磁碟或不同的磁碟新增未配置的空間，提高 NTFS 磁碟區的大小。
- 本機 NTFS 磁碟區上掛接的磁碟區，任何空白的資料夾，如果您執行退出磁碟機代號，或需要建立額外的空間可提供無障礙功能，從現有的資料夾。

## 其他資訊

|內容類型|參考|
|---|---|
|評估|- [什麼是 NTFS 的新功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11))(Windows Server 2012 R2)<br>- [什麼是 NTFS 的新功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff383236(v=ws.10))(Windows Server 2008 R2、 Windows 7)<br>- [NTFS 健康情況和 Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))<br>- [自我修復 NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10))（在 Windows Server 2008 導入）<br>- [交易式 NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730726(v%3dws.10))（在 Windows Server 2008 導入）|
|社群資源|- [Windows 儲存體小組部落格](https://blogs.msdn.microsoft.com/san/)|
|相關技術|- [Windows Server 的儲存空間](../storage.md)<br>- [使用叢集共用磁碟區容錯移轉叢集](../../failover-clustering/failover-cluster-csvs.md)<br>-[叢集共用磁碟區](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)#cluster-shared-volumes>)和[儲存體設計](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)#storage-design>)區段的[設計您的雲端基礎結構](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)) <br>- [儲存空間](../storage-spaces/overview.md)<br>- [復原檔案系統 (ReFS) 概觀](../refs/refs-overview.md)
