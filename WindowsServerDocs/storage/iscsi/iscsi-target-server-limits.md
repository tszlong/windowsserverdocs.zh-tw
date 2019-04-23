---
title: iSCSI 目標伺服器的延展性限制
TOCTitle: iSCSI Target Server Scalability Limits
ms.prod: windows-server-threshold
ms.technology: storage-iscsi
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 09/11/2018
ms.openlocfilehash: 9514392da133911c900f68fc8f1be260b6c91138
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873029"
---
# <a name="iscsi-target-server-scalability-limits"></a>iSCSI 目標伺服器的延展性限制

適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

本主題提供在 Windows Server 上的支援並經過測試的 Microsoft iSCSI 目標伺服器的限制。 下表顯示已測試的支援限制和情況適用時，是否會強制執行限制。

## <a name="general-limits"></a>一般限制

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>項目</p></th>
<th><p>支援限制</p></th>
<th><p>強制執行？</p></th>
<th><p>註解</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>每個 iSCSI 目標伺服器的 iSCSI 目標執行個體</p></td>
<td><p>256</p></td>
<td><p>否</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>iSCSI 邏輯單元 (Lu) 或每個 iSCSI 目標伺服器的虛擬磁碟</p></td>
<td><p>512</p></td>
<td><p>否</p></td>
<td><p>包含的測試組態：每個目標執行個體，平均超過 6.4 目標，以及與每個目標的一個 LU 256 的目標執行個體的 8 Lu。</p></td>
</tr>
<tr class="odd">
<td><p>iSCSI Lu 或虛擬磁碟，每個 iSCSI 目標執行個體</p></td>
<td><p>256 (Windows Server 2012 上的 128)</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>可以同時連線至 iSCSI 目標執行個體的工作階段</p></td>
<td><p>544 (Windows Server 2012 上的 512)</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>快照集每 LU</p></td>
<td><p>512</p></td>
<td><p>是</p></td>
<td><p>沒有限制為 512 個快照集，每個獨立的 iSCSI 應用程式磁碟區。</p></td>
</tr>
<tr class="even">
<td><p>在本機掛接的虛擬磁碟或快照集，每個儲存體應用裝置</p></td>
<td><p>32</p></td>
<td><p>是</p></td>
<td><p>在本機掛接的虛擬磁碟不會提供任何 iSCSI 特有的功能，且已過時-如需詳細資訊，請參閱 < <a href="https://technet.microsoft.com/library/dn303411.aspx">Features Removed or Deprecated in Windows Server 2012 R2</a>。</p></td>
</tr>
</tbody>
</table>

## <a name="fault-tolerance-limits"></a>容錯移轉限制

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>項目</p></th>
<th><p>支援限制</p></th>
<th><p>強制執行？</p></th>
<th><p>註解</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>容錯移轉叢集節點</p></td>
<td><p>8 (Windows server 2012 的 5)</p></td>
<td><p>否</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>多個作用中的叢集節點</p></td>
<td><p>支援</p></td>
<td> 
<p>N/A</p></td>
<td><p>容錯移轉叢集中每個作用中的節點的其他節點，做為可能的擁有者節點都擁有不同的 iSCSI 目標伺服器叢集執行個體。</p></td>
</tr>
<tr class="odd">
<td><p>錯誤修復層級 (ERL)</p></td>
<td><p>0</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>每個工作階段的連線</p></td>
<td><p>1</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>可以同時連線至 iSCSI 目標執行個體的工作階段</p></td>
<td><p>544 (Windows Server 2012 上的 512)</p></td>
<td><p>否</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>多重路徑 I/o (MPIO)</p></td>
<td><p>支援</p></td>
<td><p>N/A</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>MPIO 路徑</p></td>
<td><p>4</p></td>
<td><p>否</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>將獨立的 iSCSI 目標伺服器轉換為叢集的 iSCSI 目標伺服器，反之亦然</p></td>
<td><p>不支援</p></td>
<td><p>否</p></td>
<td><p>ISCSI 目標執行個體和虛擬磁碟設定資料，包括快照集的中繼資料，在轉換期間將會遺失。</p></td>
</tr>
</tbody>
</table>

## <a name="network-limits"></a>網路限制

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>項目</p></th>
<th><p>支援限制</p></th>
<th><p>強制執行？</p></th>
<th><p>註解</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>作用中的網路介面卡數目上限</p></td>
<td><p>8</p></td>
<td><p>否</p></td>
<td><p>適用於網路介面卡專用於 iSCSI 流量，而不是設備中的網路介面卡的總數。</p></td>
</tr>
<tr class="even">
<td><p>入口網站 （IP 位址） 支援</p></td>
<td><p>64</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>網路連接埠速度</p></td>
<td><p>1Gbps，10 Gbps，40Gbps，56 Gbps (Windows Server 2012 R2 及更新版本只)</p></td>
<td><p>否</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>IPv4</p></td>
<td><p>支援</p></td>
<td><p>N/A</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>IPv6</p></td>
<td><p>支援</p></td>
<td><p>N/A</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>TCP 卸載</p></td>
<td><p>支援</p></td>
<td><p>N/A</p></td>
<td><p>運用大型傳送 （分割）、 總和檢查碼、 插斷仲裁和 RSS 卸載</p></td>
</tr>
<tr class="odd">
<td><p>iSCSI 卸載</p></td>
<td><p>不支援</p></td>
<td>              
<p>N/A</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>大型訊框</p></td>
<td><p>支援</p></td>
<td><p>N/A</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>IPSec</p></td>
<td><p>支援</p></td>
<td><p>N/A</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>CRC 卸載</p></td>
<td><p>支援</p></td>
<td><p>N/A</p></td>
<td></td>
</tr>
</tbody>
</table>

## <a name="iscsi-virtual-disk-limits"></a>iSCSI 虛擬磁碟限制

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>項目</p></th>
<th><p>支援限制</p></th>
<th><p>強制執行？</p></th>
<th><p>註解</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>從 iSCSI 啟動器將虛擬磁碟從基本磁碟轉換成動態磁碟 </p></td>
<td><p>是</p></td>
<td><p>否</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>虛擬硬碟格式</p></td>
<td><p>.vhdx (Windows Server 2012 R2 及更新版本只)</p>
<p>.vhd</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>最小格式的 VHD 大小</p></td>
<td><p>.vhdx:3 MB</p>
<p>.vhd:8 MB</p></td>
<td><p>是</p></td>
<td><p>適用於所有支援的 VHD 類型： 父項、 差異和修正。</p></td>
</tr>
<tr class="even">
<td><p>父代 VHD 大小上限</p></td>
<td><p>.vhdx:64 TB</p>
<p>.vhd:2 TB</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>固定的 VHD 大小上限</p></td>
<td><p>.vhdx:64 TB</p>
<p>.vhd:16 TB</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>差異 VHD 的大小上限</p></td>
<td><p>.vhdx:64 TB</p>
<p>.vhd:2 TB</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>固定格式 VHD</p></td>
<td><p>支援</p></td>
<td><p>否</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>差異 VHD 的格式</p></td>
<td><p>支援</p></td>
<td><p>否</p></td>
<td><p>無法產生的差異 VHD 為基礎的 iSCSI 虛擬磁碟的快照。</p></td>
</tr>
<tr class="odd">
<td><p>每個父 VHD 差異 Vhd 的數目</p></td>
<td><p>256</p></td>
<td><p>否 （可以在 Windows Server 2012)</p></td>
<td><p>兩個層級 （孫系.vhdx 檔案） 是深度的.vhdx 檔案; 的最大值一個層級 （子系.vhd 檔案） 是深度的.vhd 檔案的最大值。</p></td>
</tr>
<tr class="even">
<td><p>VHD 動態格式</p></td>
<td><p>.vhdx:是</p>
<p>.vhd:是 （Windows Server 2012 上的 [否]）</p></td>
<td><p>是</p></td>
<td><p>取消對應不支援。</p></td>
</tr>
<tr class="odd">
<td><p>exFAT/FAT32/FAT （裝載 VHD 的磁碟區）</p></td>
<td><p>不支援</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>CSV v2</p></td>
<td><p>不支援</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>ReFS</p></td>
<td><p>支援</p></td>
<td><p>N/A</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>NTFS</p></td>
<td><p>支援</p></td>
<td><p>N/A</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Non-Microsoft CFS</p></td>
<td><p>不支援</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>精簡佈建</p></td>
<td><p>否</p></td>
<td><p>N/A</p></td>
<td><p>支援動態 Vhd，但 Unmap 不受支援。</p></td>
</tr>
<tr class="odd">
<td><p>邏輯單元壓縮</p></td>
<td><p>[是] (Windows Server 2012 R2 及更新版本只)</p></td>
<td><p>N/A</p></td>
<td><p>使用<a href="https://docs.microsoft.com/powershell/module/iscsitarget/resize-iscsivirtualdisk">調整大小 iSCSIVirtualDisk</a>壓縮 LUN。</p></td>
</tr>
<tr class="even">
<td><p>複製的邏輯單元</p></td>
<td><p>不支援</p></td>
<td><p>N/A</p></td>
<td><p>您可以快速複製磁碟的資料，使用差異 Vhd。</p></td>
</tr>
</tbody>
</table>

## <a name="snapshot-limits"></a>快照集限制

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><p>項目</p></th>
<th><p>支援限制</p></th>
<th><p>註解</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>建立快照集</p></td>
<td><p>支援</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>還原快照</p></td>
<td><p>支援</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>可寫入快照集</p></td>
<td><p>不支援</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>快照集 – 轉換成完整</p></td>
<td><p>不支援</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>快照集 – 線上復原</p></td>
<td><p>不支援</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>快照集 – 將轉換為可寫入</p></td>
<td><p>不支援</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>快照集-重新導向</p></td>
<td><p>不支援</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>快照集釘選</p></td>
<td><p>不支援</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>本機掛接</p></td>
<td><p>支援</p></td>
<td><p>掛接於本機的 iSCSI 虛擬磁碟已被取代-如需詳細資訊，請參閱 < <a href="https://technet.microsoft.com/library/dn303411.aspx">Features Removed or Deprecated in Windows Server 2012 R2</a>。 無法在本機裝載的動態磁碟的快照集。</p></td>
</tr>
</tbody>
</table>

## <a name="iscsi-target-server-manageability-and-backup"></a>iSCSI 目標伺服器可管理性和備份

如果您想要從應用程式伺服器的 iSCSI 虛擬磁碟上建立磁碟區陰影複製 （VSS 開放檔案快照集） 的資料，或您想要管理 iSCSI 虛擬磁碟的舊版應用程式 （例如 Diskraid 命令），需要虛擬磁碟服務 (VDS) 硬體提供者，您想要建立快照集，或使用 VDS 管理應用程式在伺服器上安裝 iSCSI 目標存放提供者。

ISCSI 目標存放提供者是 Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012; 中的角色服務您也可以下載並安裝[針對舊版應用程式伺服器的 iSCSI 目標存放裝置提供者 (VDS/VSS)](http://www.microsoft.com/download/details.aspx?id=34759)下列作業系統上只要 iSCSI 目標伺服器 Windows Server 2012 上執行：

  - Windows Storage Server 2008 R2

  - Windows Server 2008 R2

  - Windows HPC Server 2008 R2

  - Windows HPC Server 2008

請注意，是否 iSCSI 目標伺服器是執行 Windows Server 2012 R2 之伺服器所裝載或更新版本，您要從遠端伺服器使用 VSS 或 VDS 的遠端伺服器必須也執行相同版本的 Windows Server，並讓 iSCSI 目標存放提供者角色服務安裝 e。 也請注意，所有版本的 Windows 上您應該安裝只有一個版本的 iSCSI 目標存放提供者角色服務。

如需 iSCSI 目標存放提供者的詳細資訊，請參閱[iSCSI 目標儲存體 (VDS/VSS) 提供者](http://blogs.technet.com/b/filecab/archive/2012/10/08/iscsi-target-storage-vds-vss-provider.aspx)。

## <a name="tested-compatibility-with-iscsi-initiators"></a>ISCSI 啟動器與測試的相容性

我們已測試過的 iSCSI 目標伺服器的下列的 iSCSI 啟動器軟體：

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>啟動者</p></td>
<td><p>Windows Server 2012 R2</p></td>
<td><p>Windows Server 2012</p></td>
<td><p>註解</p></td>
</tr>
<tr class="even">
<td><p>Windows Server 2012 R2</p></td>
<td><p>驗證</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2012 中，Windows Server 2008 R2 及 Windows Server 2008 中，Windows Server 2003</p></td>
<td><p>驗證</p></td>
<td><p>驗證</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>VMWare vSphere 5</p></td>
<td></td>
<td><p>驗證</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>VMWare ESXi 5.0</p></td>
<td><p>驗證</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>VMWare ESX 4.1</p></td>
<td><p>驗證</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>CentOS 6.x</p></td>
<td><p>驗證</p></td>
<td></td>
<td><p>必須先登出工作階段，並重新登入來偵測已調整大小的虛擬磁碟。</p></td>
</tr>
<tr class="even">
<td><p>Red Hat Enterprise Linux 6</p></td>
<td><p>驗證</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>RedHat Enterprise Linux 5 和 5</p></td>
<td><p>驗證</p></td>
<td><p>驗證</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>SUSE Linux Enterprise Server 10</p></td>
<td></td>
<td><p>驗證</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Oracle Solaris 11.x</p></td>
<td><p>驗證</p></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

我們也已測試過下列的 iSCSI 啟動器的 iSCSI 目標伺服器裝載的虛擬磁碟從執行無磁碟開機：

  - Windows Server 2012 R2

  - Windows Server 2012

  - IPXE PCIe NIC

  - CD 或 USB 磁碟 iPXE

## <a name="see-also"></a>另請參閱

下列清單提供關於 iSCSI 目標伺服器與相關技術的其他資源。

  - [iSCSI 目標區塊儲存概觀](iscsi-target-server.md)

  - [iSCSI 目標開機概觀](iscsi-boot-overview.md)

  - [Windows Server 中的儲存體](..\storage.md)

