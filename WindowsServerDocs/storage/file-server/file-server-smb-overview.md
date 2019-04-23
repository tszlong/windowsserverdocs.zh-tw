---
title: 檔案共用在 Windows Server 中使用 SMB 3 通訊協定概觀
description: 使用 SMB 3 通訊協定的檔案共用和 Windows server 檔案服務的概觀。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: fc4c8b341ee78db80f862ee412400f0a930fe810
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845049"
---
# <a name="overview-of-file-sharing-using-the-smb-3-protocol-in-windows-server"></a>檔案共用在 Windows Server 中使用 SMB 3 通訊協定概觀

>適用於：Windows Server 2012 R2 中，Windows Server 2012 中，Windows Server 2016

本主題說明 Windows Server® 2012年、 Windows Server 2012 R2 和 Windows Server 2016 中的 SMB 3.0 功能 — 實際使用的功能，最重要的新功能或更新這個版本相較於舊的版本及硬體的功能需求。

## <a name="feature-description"></a>功能描述

伺服器訊息區 (SMB) 通訊協定是網路檔案共用通訊協定，允許電腦上的應用程式讀取和寫入檔案，以及要求來自電腦網路中伺服器程式的服務。 SMB 通訊協定可以用於其 TCP/IP 通訊協定或其他網路通訊協定的最上方。 使用 SMB 通訊協定，應用程式 (或應用程式的使用者) 可以存取遠端伺服器上的檔案或其他資源。 這允許應用程式讀取、建立及更新遠端伺服器上的檔案。 它也可以與任何設定來接收 SMB 用戶端要求的伺服器程式進行通訊。 Windows Server 2012 引進了新的 3.0 版 SMB 通訊協定。

## <a name="practical-applications"></a>實際應用

本節討論一些使用新 SMB 3.0 通訊協定的新實用方法。

* **適用於虛擬化的檔案存放裝置 (透過 SMB 的 Hyper-V™)**。 Hyper-V 可以透過 SMB 3.0 通訊協定，在檔案共用中儲存虛擬機器檔案，例如設定、虛擬硬碟 (VHD) 檔案及快照。 這適用於獨立式檔案伺服器和叢集檔案伺服器，這些伺服器會將 Hyper-V 與叢集的共用檔案存放裝置搭配使用。
* **透過 SMB 的 Microsoft SQL Server**。 SQL Server 可以在 SMB 檔案共用上儲存使用者資料庫檔案。 目前適用於獨立式 SQL Server 的 SQL Server 2008 R2 支援此項。 即將推出的 SQL Server 版本將新增對於叢集 SQL 伺服器和系統資料庫的支援。
* **適用於使用者資料的傳統存放裝置**。 SMB 3.0 通訊協定會為資訊工作者 (或用戶端) 工作負載提供增強功能。 這些增強功能包括透過廣域網路 (WAN) 存取資料和保護資料免於遭受竊聽攻擊時，減少分公司使用者感受到應用程式延遲。

## <a name="new-and-changed-functionality"></a>新功能和變更的功能

如需 Windows Server 2012 R2 中的新增和變更功能的資訊，請參閱[What's New in Windows Server 中的 SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)>)。

Windows Server 2012 和 Windows Server 2016 中的 SMB 包含新的 SMB 3.0 通訊協定和所述的許多新改良下, 表中。

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><p>特色/功能</p></th>
<th><p>新功能或更新功能</p></th>
<th><p>總結</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>SMB 透明容錯移轉</p></td>
<td><p>新的</p></td>
<td><p>讓系統管理員能夠在叢集檔案伺服器中執行節點的硬體或軟體維護，而不需中斷正在這些檔案共用上儲存資料的伺服器應用程式。 此外，如果叢集節點上發生硬體或軟體失敗，SMB 用戶端會以透明的方式重新連線到其他叢集節點，而不需中斷正在這些檔案共用上儲存資料的伺服器應用程式。</p></td>
</tr>
<tr class="even">
<td><p>SMB 向外延展</p></td>
<td><p>新的</p></td>
<td><p>使用叢集共用磁碟區 (CSV) 版本 2，系統管理員可以建立檔案共用，透過檔案伺服器叢集中的所有節點，利用直接 I/O 來提供資料檔案的同時存取。 這樣可以提供較佳的網路頻寬使用率，以及檔案伺服器用戶端的負載平衡，並將伺服器應用程式的效能最佳化。</p></td>
</tr>
<tr class="odd">
<td><p>SMB 多重通道</p></td>
<td><p>新的</p></td>
<td><p>如果 SMB 3.0 用戶端與 SMB 3.0 伺服器之間有多個路徑可供使用，則能彙總網路頻寬和網路容錯性。 這樣能讓伺服器應用程式可以充分利用所有可用的網路頻寬，並能彈性處理網路失敗。</p></td>
</tr>
<tr class="even">
<td><p>SMB 直接傳輸</p></td>
<td><p>新的</p></td>
<td><p>支援使用含有 RDMA 功能的網路介面卡，而且在使用非常少量的 CPU 時，以非常低的延遲全速運作。 對於像是 Hyper-V 或 Microsoft SQL Server 的工作負載，這讓遠端檔案伺服器能夠類似本機存放裝置。</p></td>
</tr>
<tr class="odd">
<td><p>伺服器應用程式的效能計數器</p></td>
<td><p>新的</p></td>
<td><p>新的 SMB 效能計數器可以針對每個共用提供有關輸送量、延遲及每秒 I/O (IOPS) 的詳細資訊，讓系統管理員能夠分析儲存資料所在的 SMB 3.0 檔案共用的效能。 這些計數器是特別針對伺服器應用程式而設計 (例如 Hyper-V 和 SQL Server)，它們可以在遠端檔案共用上儲存檔案。</p></td>
</tr>
<tr class="even">
<td><p>效能最佳化</p></td>
<td><p>已更新</p></td>
<td><p>SMB 3.0 用戶端和 SMB 3.0 伺服器都已針對小型隨機讀取/寫入 I/O 進行最佳化，這在像是 SQL Server OLTP 的伺服器應用程式中很常見。 此外，預設會開啟大型傳輸單元最大值 (MTU)，明顯增強大型循序傳輸的效能，例如 SQL Server 資料倉儲、資料庫備份或還原、部署或複製虛擬硬碟。</p></td>
</tr>
<tr class="odd">
<td><p>SMB 特定的 Windows PowerShell Cmdlet</p></td>
<td><p>新的</p></td>
<td><p>利用適用於 SMB 的 Windows PowerShell Cmdlet，系統管理員可以從命令列，以端對端方式，在檔案伺服器上管理檔案共用。</p></td>
</tr>
<tr class="even">
<td><p>SMB 加密</p></td>
<td><p>新的</p></td>
<td><p>提供 SMB 資料的端對端加密，並保護資料以免在不受信任的網路上遭到竊聽。 不需要新的部署成本，也不需要網際網路通訊協定安全性 (IPsec)、特定的硬體或 WAN 加速器。 這可能會以每個共用為基礎進行設定，或者針對整個檔案伺服器進行設定，而且可能會針對資料周遊不受信任網路的各種案例加以啟用。</p></td>
</tr>
<tr class="odd">
<td><p>SMB 目錄租用</p></td>
<td><p>新的</p></td>
<td><p>增強分公司的應用程式回應時間。 使用目錄租用，從用戶端到伺服器的來回時間會降低，因為中繼資料是從存留期間較長的目錄快取中所擷取。 快取的一致性可以維持，因為當伺服器上的目錄資料變更時，用戶端會收到通知。 使用 <em>主資料夾</em> (讀取/寫入且無共用) 和 <em>發行集</em> (唯讀且有共用) 的案例。</p></td>
</tr>
</tbody>
</table>

## <a name="hardware-requirements"></a>硬體需求

SMB 透明容錯移轉具備下列需求：

* 容錯移轉叢集，且已設定至少兩個節點執行 Windows Server 2012 或 Windows Server 2016。 這個叢集必須傳遞驗證精靈中所含的叢集驗證測試。
* 檔案共用必須利用持續可用性 (CA) 屬性來建立，這是預設值。
* 檔案共用必須建立於 CSV 磁碟區路徑上，以達到 SMB 向外延展。
* 用戶端電腦必須執行 Windows® 8 或 Windows Server 2012，這兩者都包含支援持續可用性的更新的 SMB 用戶端。

>[!NOTE]
>下層用戶端可以連線到檔案共用，含有 CA 屬性，但這些用戶端，不支援透明的容錯移轉。

SMB 多重通道具備下列需求：

* 需要至少兩部執行 Windows Server 2012 的電腦。 不需要安裝任何其他的功能 - 預設會啟用這項技術。
* 如需所建議網路設定的相關資訊，請參閱這個概觀主題最後的＜另請參閱＞一節。

SMB 直接傳輸具備下列需求：

* 需要至少兩部執行 Windows Server 2012 的電腦。 不需要安裝任何其他的功能 - 預設會啟用這項技術。
* 需要含有 RDMA 功能的網路介面卡。 這些介面卡目前可用於三種不同類型：iWARP、Infiniband 或 RoCE (透過聚合式乙太網路的 RDMA)。

## <a name="more-information"></a>詳細資訊

下列清單提供網路上有關 SMB 及相關的技術，在 Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2016 中的其他資源。

* [Windows Server 中的儲存體](../storage.md)
* [用於應用程式資料的向外延展檔案伺服器](../../failover-clustering/sofs-overview.md)
* [直接改善的 SMB 檔案伺服器的效能](smb-direct.md)
* [部署透過 SMB 的 HYPER-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
* [部署 SMB 多重通道](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn610980(v%3dws.11)>)
* [伺服器應用程式的部署快速又有效率的檔案伺服器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
* [SMB:疑難排解指南](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn659439(v%3dws.11)>)