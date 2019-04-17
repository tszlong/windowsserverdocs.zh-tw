---
title: 在 Windows Server 中使用的 SMB 3 通訊協定的檔案共用的概觀 （英文)
description: 檔案共用及檔案提供與 Windows Server 搭配使用的 SMB 3 通訊協定的概觀。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: fc4c8b341ee78db80f862ee412400f0a930fe810
ms.sourcegitcommit: 375e94dc70c76e7aa5679c32f0f4dbc26cf7dd83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2018
ms.locfileid: "2233484"
---
# <a name="overview-of-file-sharing-using-the-smb-3-protocol-in-windows-server"></a>在 Windows Server 中使用的 SMB 3 通訊協定的檔案共用的概觀 （英文)

>適用於： Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2016

本主題說明在 Windows Server® 2012年、 Windows Server 2012 R2 和 Windows Server 2016 SMB 3.0 功能 — 實用新使用的最重要的功能或更新相較於先前版本和硬體這個版本中的功能需求。

## <a name="feature-description"></a>功能描述

伺服器訊息區 (SMB) 通訊協定是網路檔案共用通訊協定，允許電腦上的應用程式讀取和寫入檔案，以及要求來自電腦網路中伺服器程式的服務。 SMB 通訊協定可以用於其 TCP/IP 通訊協定或其他網路通訊協定的最上方。 使用 SMB 通訊協定，應用程式 (或應用程式的使用者) 可以存取遠端伺服器上的檔案或其他資源。 這允許應用程式讀取、建立及更新遠端伺服器上的檔案。 它也可以與任何設定來接收 SMB 用戶端要求的伺服器程式進行通訊。 Windows Server 2012 介紹 3.0 新版的 SMB 通訊協定。

## <a name="practical-applications"></a>實際應用

本章節將討論一些新實用方法，以使用新的 SMB 3.0 通訊協定。

* **虛擬化 (over SMB Hyper-V™) 的檔案存放區**。 HYPER-V 可儲存虛擬機器的檔案，例如設定、 虛擬硬碟 (VHD) 檔案和快照集，在一段的 SMB 3.0 通訊協定的檔案共用。 這可以使用獨立的檔案伺服器及與共用的檔案存放區一起使用 HYPER-V 叢集的叢集的檔案伺服器。
* **Over SMB Microsoft SQL Server**。 SQL Server 可儲存使用者 SMB 檔案共用上的資料庫檔案。 目前，這是獨立的 SQL server 的支援與 SQL Server 2008 R2。 即將推出版本的 SQL Server 將會新增叢集的 SQL 伺服器及系統資料庫的支援。
* **繁體中文使用者資料儲存區**。 SMB 3.0 通訊協定提供的資訊工作者 （或用戶端） 的增強工作負載。 這些增強功能包括減少透過廣域網路 (WAN) 存取資料並保護資料免於竊聽攻擊時由分公司使用者體驗的應用程式延遲。

## <a name="new-and-changed-functionality"></a>新功能和變更的功能

如需 Windows Server 2012 R2 中的最新及變更功能資訊，請參閱[在 Windows Server 中的 SMB 的新功能](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)>)。

在 Windows Server 2012 及 Windows Server 2016 SMB 包含新的 SMB 3.0 通訊協定及說明許多新改良功能下表中。

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
<th><p>摘要</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>SMB 透明容錯移轉</p></td>
<td><p>新增</p></td>
<td><p>可讓系統管理員在叢集的檔案伺服器的硬體或軟體維護的節點執行而不會中斷儲存這些檔案共用上的資料的伺服器應用程式。 此外，如果在叢集節點上發生硬體或軟體失敗，SMB 用戶端透明重新連線至另一個叢集節點而不會中斷會儲存這些檔案共用的資料的伺服器應用程式。</p></td>
</tr>
<tr class="even">
<td><p>SMB 向外延展</p></td>
<td><p>新增</p></td>
<td><p>使用叢集共用磁碟區 (CSV) 第 2 版，管理員可以建立提供較直接 I/O、 檔案伺服器叢集裡的所有節點透過同時存取資料檔案的檔案共用。 這提供較佳的網路頻寬使用量和負載平衡的檔案伺服器用戶端，並最佳化效能的伺服器應用程式。</p></td>
</tr>
<tr class="odd">
<td><p>SMB 多重通道</p></td>
<td><p>新增</p></td>
<td><p>如果多個路徑可用 SMB 3.0 用戶端與伺服器的 SMB 3.0 之間，啟用網路頻寬和網路容錯彙的總。 這可讓善用所有可用的網路頻寬的完整並可恢復網路失敗的伺服器應用程式。</p></td>
</tr>
<tr class="even">
<td><p>SMB 直接傳輸</p></td>
<td><p>新增</p></td>
<td><p>支援使用 RDMA 功能並可以運作完整的速度非常低延遲使用非常少 CPU 時的網路介面卡。 針對如 HYPER-V 或 Microsoft SQL Server 的工作量，這可讓遠端檔案伺服器類似的本機存放區。</p></td>
</tr>
<tr class="odd">
<td><p>伺服器應用程式的效能計數器</p></td>
<td><p>新增</p></td>
<td><p>新的 SMB 效能計數器提供詳細，每個共用資訊輸送量、 延遲及 I/O 每秒 (IOPS)，讓系統管理員可以分析 SMB 3.0 檔案共用其資料儲存位置的效能。 這些計數器會特別設計的伺服器應用程式，例如 HYPER-V 與 SQL Server 儲存遠端檔案共用上的檔案。</p></td>
</tr>
<tr class="even">
<td><p>效能最佳化</p></td>
<td><p>已更新</p></td>
<td><p>SMB 3.0 用戶端和 SMB 3.0 伺服器上有已最佳化的小型隨機讀取/寫入 I/O 這是常見的伺服器應用程式，例如 SQL Server OLTP。 此外，大型的最大傳輸單位 (MTU) 會開啟依預設，大幅增強的大型循序傳輸，例如 SQL Server 資料倉儲、 資料庫備份或還原部署或複製虛擬硬碟效能。</p></td>
</tr>
<tr class="odd">
<td><p>SMB 特有的 Windows PowerShell cmdlet</p></td>
<td><p>新增</p></td>
<td><p>使用 Windows PowerShell cmdlet 的 SMB，系統管理員可以從命令列管理檔案伺服器端對端上的檔案共用。</p></td>
</tr>
<tr class="even">
<td><p>SMB 加密</p></td>
<td><p>新增</p></td>
<td><p>提供 SMB 資料的端對端加密並保護資料來自不受信任的網路上的竊聽發生次數。 需要任何新的部署成本，並不需要在網際網路通訊協定安全性 (IPsec)、 特定的硬體或 WAN 加速器。 並針對每個共用，或整個檔案伺服器，您可以設定可能會啟用各種案例資料周遊不受信任的網路的位置。</p></td>
</tr>
<tr class="odd">
<td><p>SMB 目錄租賃</p></td>
<td><p>新增</p></td>
<td><p>在分公司改善應用程式回應時間。 與使用目錄租用中，從用戶端到伺服器的往返會降低自中繼資料擷取自長活目錄快取。 因為用戶端時會通知來電者的伺服器上的目錄資訊變更維護快取一致性。 可透過案例<em>HomeFolder</em> （讀取/與任何共用的寫入） 和<em>出版物</em>（唯讀與共用）。</p></td>
</tr>
</tbody>
</table>

## <a name="hardware-requirements"></a>硬體需求

SMB 透明容錯移轉有下列需求：

* 執行 Windows Server 2012 或 Windows Server 2016 至少兩種設定的節點與容錯移轉叢集。 叢集中必須通過驗證精靈中包含的叢集驗證測試。
* 檔案共用必須建立並連續可用性 (CA) 屬性，這是預設值。
* 檔案共用必須建立 CSV 磁碟區路徑來達成 SMB 向外延展。
* 用戶端電腦必須執行 Windows® 8 或 Windows Server 2012] 兩者都包含更新的 SMB 用戶端支援連續的可用性。

>[!NOTE]
>下層用戶端可以連線至具有 [CA] 屬性中的檔案共用，但不是支援這些用戶端與容錯移轉。

SMB 多重通道有下列需求：

* 至少兩部執行 Windows Server 2012 電腦是必要的。 沒有額外功能需要安裝 — 技術會開啟預設值。
* 如需建議的網路組態資訊，請參閱本主題尾端的概觀 （英文） 請參閱 」 一節。

SMB 直接有下列需求：

* 至少兩部執行 Windows Server 2012 電腦是必要的。 沒有額外功能需要安裝 — 技術會開啟預設值。
* 網路介面卡 RDMA 項功能是必要的。 Currently, these adapters are available in three different types: iWARP, Infiniband, or RoCE (RDMA over Converged Ethernet).

## <a name="more-information"></a>更多資訊

下列清單提供有關 SMB 與相關的技術的 Windows Server 2012 R2、 Windows Server 2012 與 Windows Server 2016 網路上的其他資源。

* [Windows Server 的儲存空間](../storage.md)
* [向外延展檔案伺服器應用程式資料](../../failover-clustering/sofs-overview.md)
* [直接改善與 SMB 檔案伺服器的效能](smb-direct.md)
* [部署透過 SMB 的 Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
* [部署 SMB 多重通道](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn610980(v%3dws.11)>)
* [部署快速且高效率檔案伺服器的伺服器應用程式](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
* [SMB： 疑難排解指南](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn659439(v%3dws.11)>)