---
title: iSCSI 目標伺服器的擴充性限制
TOCTitle: iSCSI Target Server Scalability Limits
ms.prod: windows-server
ms.technology: storage-iscsi
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 09/11/2018
ms.openlocfilehash: 6799e0e3b47d6cc98cbb42407ffbed1a9578675a
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85473435"
---
# <a name="iscsi-target-server-scalability-limits"></a>iSCSI 目標伺服器的擴充性限制

適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題提供在 Windows Server 上支援和測試的 Microsoft iSCSI 目標伺服器限制。 下表顯示已測試的支援限制，並在適用的情況下，決定是否強制執行限制。

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
<th><p>強行?</p></th>
<th><p>註解</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>每一 iSCSI 目標伺服器的 iSCSI 目標實例</p></td>
<td><p>256</p></td>
<td><p>否</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>iSCSI 邏輯單元（Lu）或每一 iSCSI 目標伺服器的虛擬磁片</p></td>
<td><p>512</p></td>
<td><p>否</p></td>
<td><p>包含的測試設定：平均超過64個目標的每個目標實例8個 Lu，以及每個目標有一個 LU 的256目標實例。</p></td>
</tr>
<tr class="odd">
<td><p>iSCSI Lu 或每個 iSCSI 目標實例的虛擬磁片</p></td>
<td><p>256（128 on Windows Server 2012）</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>可以同時連接至 iSCSI 目標實例的會話</p></td>
<td><p>544（512 on Windows Server 2012）</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>每個 LU 的快照集</p></td>
<td><p>512</p></td>
<td><p>是</p></td>
<td><p>每個獨立 iSCSI 應用程式磁片區有512個快照集的限制。</p></td>
</tr>
<tr class="even">
<td><p>每個儲存體應用裝置的本機掛接虛擬磁片或快照集</p></td>
<td><p>32</p></td>
<td><p>是</p></td>
<td><p>本機掛接的虛擬磁片&#39;t 提供任何 iSCSI 特定功能，並已淘汰-如需詳細資訊，請參閱<a href="https://technet.microsoft.com/library/dn303411.aspx">Windows Server 2012 R2 中已移除或過時的功能</a>。</p></td>
</tr>
</tbody>
</table>

## <a name="fault-tolerance-limits"></a>容錯限制

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
<th><p>強行?</p></th>
<th><p>註解</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>容錯移轉叢集節點</p></td>
<td><p>8（Windows Server 2012 上的5）</p></td>
<td><p>否</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>多個作用中叢集節點</p></td>
<td><p>支援</p></td>
<td>
<p>N/A</p></td>
<td><p>容錯移轉叢集中的每個使用中節點，都擁有不同的 iSCSI 目標伺服器叢集實例，而其他節點則擔任可能的擁有者節點。</p></td>
</tr>
<tr class="odd">
<td><p>錯誤恢復層級（ERL）</p></td>
<td><p>0</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>每個會話的連線數</p></td>
<td><p>1</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>可以同時連接至 iSCSI 目標實例的會話</p></td>
<td><p>544（512 on Windows Server 2012）</p></td>
<td><p>否</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>多重路徑輸入/輸出（MPIO）</p></td>
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
<td><p>將獨立 iSCSI 目標伺服器轉換成叢集 iSCSI 目標伺服器，反之亦然</p></td>
<td><p>不支援</p></td>
<td><p>否</p></td>
<td><p>轉換期間會遺失 iSCSI 目標實例和虛擬磁片設定資料（包括快照中繼資料）。</p></td>
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
<th><p>強行?</p></th>
<th><p>註解</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>作用中網路介面卡的數目上限</p></td>
<td><p>8</p></td>
<td><p>否</p></td>
<td><p>適用于專用於 iSCSI 流量的網路介面卡，而不是設備中的網路介面卡總數。</p></td>
</tr>
<tr class="even">
<td><p>支援的入口網站（IP 位址）</p></td>
<td><p>64</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>網路埠速度</p></td>
<td><p>1Gbps、10 Gbps、40Gbps、56 Gbps （僅限 Windows Server 2012 R2 和更新版本）</p></td>
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
<td><p>運用大型傳送（分割）、總和檢查碼、中斷仲裁和 RSS 卸載</p></td>
</tr>
<tr class="odd">
<td><p>iSCSI 卸載</p></td>
<td><p>不支援</p></td>
<td><br/><p>N/A</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>大型訊框</p></td>
<td><p>支援</p></td>
<td><p>N/A</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>IPsec</p></td>
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

## <a name="iscsi-virtual-disk-limits"></a>iSCSI 虛擬磁片限制

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
<th><p>強行?</p></th>
<th><p>註解</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>從將虛擬磁片從基本磁碟轉換為動態磁碟的 iSCSI 啟動器 </p></td>
<td><p>是</p></td>
<td><p>否</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>虛擬硬碟格式</p></td>
<td><p>.vhdx （僅限 Windows Server 2012 R2 和更新版本）</p>
<p>.vhd</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>VHD 的最小格式大小</p></td>
<td><p>.vhdx： 3 MB</p>
<p>.vhd： 8 MB</p></td>
<td><p>是</p></td>
<td><p>適用于所有支援的 VHD 類型：父系、差異和固定。</p></td>
</tr>
<tr class="even">
<td><p>父 VHD 大小上限</p></td>
<td><p>.vhdx： 64 TB</p>
<p>.vhd： 2 TB</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>已修正 VHD 大小上限</p></td>
<td><p>.vhdx： 64 TB</p>
<p>.vhd： 16 TB</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>差異 VHD 大小上限</p></td>
<td><p>.vhdx： 64 TB</p>
<p>.vhd： 2 TB</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>VHD 固定格式</p></td>
<td><p>支援</p></td>
<td><p>否</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>VHD 差異格式</p></td>
<td><p>支援</p></td>
<td><p>否</p></td>
<td><p>快照集不能用在差異 VHD 型 iSCSI 虛擬磁片上。</p></td>
</tr>
<tr class="odd">
<td><p>每個父 VHD 的差異 Vhd 數目</p></td>
<td><p>256</p></td>
<td><p>否（在 Windows Server 2012 上為是）</p></td>
<td><p>兩層深度（孫系 .vhdx 檔案）是 .vhdx 檔案的最大值;一層深度（子 .vhd 檔案）是 .vhd 檔案的最大值。</p></td>
</tr>
<tr class="even">
<td><p>VHD 動態格式</p></td>
<td><p>.vhdx：是</p>
<p>.vhd：是（在 Windows Server 2012 上為否）</p></td>
<td><p>是</p></td>
<td><p>不&#39;t 支援取消對應。</p></td>
</tr>
<tr class="odd">
<td><p>exFAT/FAT32/FAT （VHD 的裝載磁片區）</p></td>
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
<td><p>非 Microsoft CFS</p></td>
<td><p>不支援</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>精簡佈建</p></td>
<td><p>否</p></td>
<td><p>N/A</p></td>
<td><p>支援動態 Vhd，但不&#39;t 支援取消對應。</p></td>
</tr>
<tr class="odd">
<td><p>邏輯單元壓縮</p></td>
<td><p>是（僅限 Windows Server 2012 R2 和更新版本）</p></td>
<td><p>N/A</p></td>
<td><p>使用 [重<a href="https://docs.microsoft.com/powershell/module/iscsitarget/resize-iscsivirtualdisk">設大小-convert-iscsivirtualdisk 功能</a>] 來壓縮 LUN。</p></td>
</tr>
<tr class="even">
<td><p>邏輯單元複製</p></td>
<td><p>不支援</p></td>
<td><p>N/A</p></td>
<td><p>您可以使用差異 Vhd 快速複製磁片資料。</p></td>
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
<td><p>快照集建立</p></td>
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
<td><p>快照集–轉換成完整</p></td>
<td><p>不支援</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>快照集–線上復原</p></td>
<td><p>不支援</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>快照集–轉換成可寫入</p></td>
<td><p>不支援</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>快照-重新導向</p></td>
<td><p>不支援</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>快照集-釘選</p></td>
<td><p>不支援</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>本機掛接</p></td>
<td><p>支援</p></td>
<td><p>本機掛接的 iSCSI 虛擬磁片已淘汰-如需詳細資訊，請參閱<a href="https://technet.microsoft.com/library/dn303411.aspx">Windows Server 2012 R2 中已移除或過時的功能</a>。 無法在本機掛接動態磁碟快照集。</p></td>
</tr>
</tbody>
</table>

## <a name="iscsi-target-server-manageability-and-backup"></a>iSCSI 目標伺服器管理能力和備份

如果您想要從應用程式伺服器建立 iSCSI 虛擬磁片上的磁片區陰影複製（VSS 開啟檔案快照集），或者，您想要使用需要虛擬磁碟服務（VDS）硬體提供者的繼承應用程式（例如 Diskraid 命令）來管理 iSCSI 虛擬磁片，請在您要建立快照集的伺服器上安裝 iSCSI 目標儲存提供者，或使用 VDS 管理應用程式。

ISCSI 目標儲存提供者是 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 中的角色服務。只要 iSCSI 目標伺服器是在 Windows Server 2012 上執行，您也可以在下列作業系統上下載並安裝[適用于下層應用程式伺服器的 Iscsi 目標儲存提供者（VDS/VSS）](https://www.microsoft.com/download/details.aspx?id=34759) ：

  - Windows Storage Server 2008 R2

  - Windows Server 2008 R2

  - Windows HPC Server 2008 R2

  - Windows HPC Server 2008

請注意，如果 iSCSI 目標伺服器是由執行 Windows Server 2012 R2 或更新版本的伺服器所主控，而您想要從遠端伺服器使用 VSS 或 VDS，則遠端伺服器也必須執行相同版本的 Windows Server 並安裝 iSCSI 目標儲存提供者角色服務。 另請注意，在所有版本的 Windows 上，您應該只安裝一個版本的 iSCSI 目標儲存提供者角色服務。

如需 iSCSI 目標儲存提供者的詳細資訊，請參閱[Iscsi 目標儲存體（VDS/VSS）提供者](https://blogs.technet.com/b/filecab/archive/2012/10/08/iscsi-target-storage-vds-vss-provider.aspx)。

## <a name="tested-compatibility-with-iscsi-initiators"></a>已測試與 iSCSI 啟動器的相容性

我們已使用下列 iSCSI 啟動器測試 iSCSI 目標伺服器軟體：

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Initiator</p></td>
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
<td><p>Windows Server 2012、Windows Server 2008 R2、Windows Server 2008、Windows Server 2003</p></td>
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
<td><p>VMWare ESXi 5。0</p></td>
<td><p>驗證</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>VMWare ESX 4。1</p></td>
<td><p>驗證</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>CentOS 6.x</p></td>
<td><p>驗證</p></td>
<td></td>
<td><p>必須登出會話再重新登入，以偵測已調整大小的虛擬磁片。</p></td>
</tr>
<tr class="even">
<td><p>Red Hat Enterprise Linux 6</p></td>
<td><p>驗證</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>RedHat Enterprise Linux 5 和5</p></td>
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
<td><p>Oracle Solaris 11. x</p></td>
<td><p>驗證</p></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

我們也已測試下列 iSCSI 啟動器，從 iSCSI 目標伺服器所裝載的虛擬磁片執行無磁片開機：

  - Windows Server 2012 R2

  - Windows Server 2012

  - 使用 iPXE 的 PCIe NIC

  - 具有 iPXE 的 CD 或 USB 磁片

## <a name="additional-references"></a>其他參考

下列清單提供關於 iSCSI 目標伺服器與相關技術的其他資源。

- [iSCSI 目標區塊儲存概觀](iscsi-target-server.md)

- [iSCSI Target Boot Overview](iscsi-boot-overview.md)

- [Windows Server 的儲存空間](../storage.yml)

