---
title: iSCSI 目標伺服器的擴充性限制
TOCTitle: iSCSI Target Server Scalability Limits
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 09/11/2018
ms.openlocfilehash: 3be878629d19542629cc3cbb849ac46fe14de0bd
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766831"
---
# <a name="iscsi-target-server-scalability-limits"></a>iSCSI 目標伺服器的擴充性限制

適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題提供 Windows Server 上支援和測試過的 Microsoft iSCSI 目標伺服器限制。 下表顯示已測試的支援限制，並在適用的情況下顯示是否強制執行限制。

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
<th><p>執行？</p></th>
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
<td><p>iSCSI 邏輯單元 (Lu) 或每個 iSCSI 目標伺服器的虛擬磁片</p></td>
<td><p>512</p></td>
<td><p>否</p></td>
<td><p>包含的測試設定：每個目標實例8個 Lu，平均超過64目標，以及每個目標有一個 LU 的256目標實例。</p></td>
</tr>
<tr class="odd">
<td><p>iSCSI Lu 或虛擬磁片（每個 iSCSI 目標實例）</p></td>
<td><p>256 (128 on Windows Server 2012) </p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>可同時連接到 iSCSI 目標實例的會話</p></td>
<td><p>544 (512 on Windows Server 2012) </p></td>
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
<td><p>每個儲存體設備的本機裝載虛擬磁片或快照集</p></td>
<td><p>32</p></td>
<td><p>是</p></td>
<td><p>本機掛接的虛擬磁片 don&#39;t 提供任何 iSCSI 特定功能，並已淘汰-如需詳細資訊，請參閱 <a href="/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303411(v=ws.11)">Windows Server 2012 R2 中已移除或過時的功能</a>。</p></td>
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
<th><p>執行？</p></th>
<th><p>註解</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>容錯移轉叢集節點</p></td>
<td><p>Windows Server 2012 上的 8 (5) </p></td>
<td><p>否</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>多個作用中的叢集節點</p></td>
<td><p>支援</p></td>
<td>
<p>N/A</p></td>
<td><p>容錯移轉叢集中的每個作用中節點都會擁有不同的 iSCSI 目標伺服器叢集實例，以及其他節點作為可能的擁有者節點。</p></td>
</tr>
<tr class="odd">
<td><p>錯誤復原層級 (ERL) </p></td>
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
<td><p>可同時連接到 iSCSI 目標實例的會話</p></td>
<td><p>544 (512 on Windows Server 2012) </p></td>
<td><p>否</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>多重路徑輸入/輸出 (MPIO) </p></td>
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
<td><p>ISCSI 目標實例和虛擬磁片設定資料（包括快照中繼資料）會在轉換期間遺失。</p></td>
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
<th><p>執行？</p></th>
<th><p>註解</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>作用中網路介面卡數目上限</p></td>
<td><p>8</p></td>
<td><p>否</p></td>
<td><p>適用于 iSCSI 流量專用的網路介面卡，而不是設備中的網路介面卡總數。</p></td>
</tr>
<tr class="even">
<td><p>) 支援的入口網站 (IP 位址</p></td>
<td><p>64</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>網路埠速度</p></td>
<td><p>1Gbps、10 Gbps、40Gbps、56 Gbps (Windows Server 2012 R2 和更新版本僅) </p></td>
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
<td><p>利用大型傳送 (分割) 、總和檢查碼、中斷仲裁和 RSS 卸載</p></td>
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
<th><p>執行？</p></th>
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
<td><p>.vhdx (Windows Server 2012 R2 和更新版本的) </p>
<p>.vhd</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>VHD 最小格式大小</p></td>
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
<td><p>修正 VHD 大小上限</p></td>
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
<td><p>無法針對差異 VHD 型 iSCSI 虛擬磁片建立快照集。</p></td>
</tr>
<tr class="odd">
<td><p>每個父 VHD 的差異 Vhd 數目</p></td>
<td><p>256</p></td>
<td><p>Windows Server 2012 上沒有 (Yes) </p></td>
<td><p> (孫系 .vhdx 檔案的兩個深度層級) 是 .vhdx 檔案的最大值; (子 .vhd 檔案的一層深度) 是 .vhd 檔案的最大值。</p></td>
</tr>
<tr class="even">
<td><p>VHD 動態格式</p></td>
<td><p>.vhdx：是</p>
<p>.vhd：是在 Windows Server 2012 上 (否) </p></td>
<td><p>是</p></td>
<td><p>&#39;t 支援取消對應。</p></td>
</tr>
<tr class="odd">
<td><p>VHD) 的 exFAT/FAT32/FAT (裝載磁片區</p></td>
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
<td><p>支援動態 Vhd，但不支援&#39;t 進行對應。</p></td>
</tr>
<tr class="odd">
<td><p>邏輯單元壓縮</p></td>
<td><p>是 (僅限 Windows Server 2012 R2 和更新版本) </p></td>
<td><p>N/A</p></td>
<td><p>使用重 <a href="/powershell/module/iscsitarget/resize-iscsivirtualdisk">設大小 convert-iscsivirtualdisk 功能</a> 來壓縮 LUN。</p></td>
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
<td><p>快照集-轉換成 full</p></td>
<td><p>不支援</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>快照-線上復原</p></td>
<td><p>不支援</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>快照-轉換成可寫入</p></td>
<td><p>不支援</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>快照集-重新導向</p></td>
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
<td><p>本機掛接的 iSCSI 虛擬磁片已淘汰。如需詳細資訊，請參閱 <a href="/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303411(v=ws.11)">Windows Server 2012 R2 中已移除或已淘汰的功能</a>。 無法在本機裝載動態磁碟快照集。</p></td>
</tr>
</tbody>
</table>

## <a name="iscsi-target-server-manageability-and-backup"></a>iSCSI 目標伺服器管理能力和備份

如果您想要在應用程式伺服器上建立磁片區陰影複製 (從 iSCSI 虛擬磁片上的 VSS 開啟檔案快照集) 資料，或者，您想要使用較舊的應用程式管理 iSCSI 虛擬磁片 (例如，需要虛擬磁碟服務的 Diskraid 命令)  (VDS) 硬體提供者，請在您要建立快照集的伺服器上安裝 iSCSI 目標儲存提供者，或使用 VDS 管理應用程式。

ISCSI 目標存放提供者是 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 中的角色服務;只要 iSCSI 目標伺服器是在 Windows Server 2012 上執行，您也可以針對下列作業系統上的 [下層應用程式伺服器，下載並安裝 Iscsi 目標存放裝置提供者 (VDS/VSS) ](https://www.microsoft.com/download/details.aspx?id=34759) ：

  - Windows Storage Server 2008 R2

  - Windows Server 2008 R2

  - Windows HPC Server 2008 R2

  - Windows HPC Server 2008

請注意，如果 iSCSI 目標伺服器是由執行 Windows Server 2012 R2 或更新版本的伺服器所裝載，而您想要從遠端伺服器使用 VSS 或 VDS，則遠端伺服器也必須執行相同版本的 Windows Server，並安裝 iSCSI 目標儲存提供者角色服務。 另請注意，在所有版本的 Windows 上，您應該只安裝一個版本的 iSCSI 目標儲存提供者角色服務。

如需 iSCSI 目標儲存提供者的詳細資訊，請參閱 [ (VDS/VSS) 提供者的 Iscsi 目標儲存體](/powershell/module/iscsi/?view=win10-ps)。

## <a name="tested-compatibility-with-iscsi-initiators"></a>測試與 iSCSI 啟動器的相容性

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
<td><p>必須登出會話並重新登入，以偵測已調整大小的虛擬磁片。</p></td>
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

我們也已測試下列從 iSCSI 目標伺服器裝載的虛擬磁片執行無磁片開機的 iSCSI 啟動器：

  - Windows Server 2012 R2

  - Windows Server 2012

  - 使用 iPXE 的 PCIe NIC

  - 使用 iPXE 的 CD 或 USB 磁片

## <a name="additional-references"></a>其他參考資料

下列清單提供關於 iSCSI 目標伺服器與相關技術的其他資源。

- [iSCSI 目標區塊儲存概觀](iscsi-target-server.md)

- [iSCSI Target Boot Overview](iscsi-boot-overview.md)

- [Windows Server 的儲存空間](../storage.yml)