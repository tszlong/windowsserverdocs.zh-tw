---
title: 儲存空間
description: ''
author: JasonGerend
manager: elizapo
layout: LandingPage
ms.prod: windows-server-threshold
ms.technology: storage
ms.assetid: 6b74bc7c-a58d-4915-af8e-2cc27f2c4726
ms.topic: landing-page
ms.author: jgerend
ms.localizationpriority: medium
ms.date: 03/08/2019
ms.openlocfilehash: d83d51ebf56d38f93c176d403ea5c6c14f625ee2
ms.sourcegitcommit: 419bf9d6d18e7a32cba2cbcd98587b81a79adc51
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/11/2019
ms.locfileid: "9152175"
---
# 儲存空間

>適用於：Windows Server 2019、Windows Server 2016、Windows Server (半年通道)

>[!TIP]
> 尋找舊版 Windows Server 的相關資訊嗎？ 查看我們其他位於 docs.microsoft.com 的 [Windows Server 文件庫](/previous-versions/windows/)。 您也可以[搜尋這個網站](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions)以取得特定資訊。

<hr />
Windows Server 中的儲存空間可以提供新功能和改進功能，讓軟體定義資料中心 (SDDC) 的客戶可以專注處理虛擬化工作負載。 WindowsServer 也會為使用檔案伺服器搭配現有工作負載的企業客戶提供廣泛的支援。

<hr />
<ul class="cardsF panelContent">
<li>
 <a href="whats-new-in-storage.md">
                            <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-whats-new.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                            <h2>有何新功能？</h2>
                                            <p>了解什麼是 Windows Server 儲存空間的新</p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
</ul>
<hr />
<ul class="cardsF panelContent">
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-store.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>適用於虛擬化工作負載的軟體定義存放裝置</h3>
<HR />
                        <p><h3><a href="storage-spaces/storage-spaces-direct-overview.md">儲存空間直接存取</a></h3> 直接連接的本機存放裝置，包括 SATA 和 NVME 裝置，將磁碟使用量最佳化之後新增新的實體磁碟，並更快速的虛擬磁碟修復時間。 如需有關共用 SAS 和獨立儲存空間的詳細資訊，另請參閱<a href="storage-spaces/overview.md">儲存空間</a>。</p>
<HR />
                        <p><h3><a href="storage-replica/storage-replica-overview.md">儲存體複本</a></h3> 存放裝置無關、 區塊層級的同步複寫叢集或伺服器針對災害整備與復原，以及容錯移轉叢集延伸的跨站台，針對高可用性之間。 同步複寫可在具備當機時保持一致磁碟區的實體站台中啟用資料的鏡像，確保檔案系統層級零資料遺失。</p>
<HR />
                        <p><h3><a href="storage-qos/storage-qos-overview.md">存放裝置服務品質 (QoS)</a></h3> 集中監視和管理自動使用 HYPER-V 與向外延展檔案伺服器角色的虛擬機器的存放裝置效能 improveing 存放裝置資源公平性，使用相同的檔案伺服器叢集的多個虛擬機器之間。</p>
<HR />
                        <p><h3><a href="data-deduplication/overview.md">重複資料刪除</a></h3> 藉由檢查的重複資料刪除磁碟區上的資料，可最佳化磁碟區上的可用空間。 識別完成之後，重複的磁碟區資料集部分只會儲存一次並 (選擇性) 進行壓縮，進一步節省空間。 重複資料刪除可將備援最佳化，而不必犧牲資料精確度或完整性。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-store.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>一般用途的檔案伺服器</h3>
<HR />
                        <p><h3><a href="storage-migration-service/overview.md">存放裝置移轉服務</a></h3>將伺服器移轉至較新版本的 Windows Server 使用圖形化工具，可用於清查伺服器上的資料、 將資料與設定傳輸到較新的伺服器，然後選擇性地將移動的舊伺服器的身分識別至新伺服器因此該應用程式和使用者不需要變更任何項目。</p>
<HR />
                        <p><h3><a href="work-folders/work-folders-overview.md">工作資料夾</a></h3> 儲存和存取工作檔案在個人電腦和裝置，通常稱為 「 攜帶您自己的裝置 (BYOD)，除了公司電腦上。 使用者獲得方便儲存工作檔案的位置，而他們能夠從任何地方存取這類檔案。 組織將檔案儲存在集中管理的檔案伺服器上，並選擇性地指定使用者裝置原則 (例如加密和鎖定畫面密碼)，藉此維持對公司資料的控制權。</p>
<HR />
                        <p><h3><a href="folder-redirection/folder-redirection-rup-overview.md">離線檔案和資料夾重新導向</a></h3> 同時快取的內容，以提升的速度及可用性，在本機，本機資料夾 （例如文件] 資料夾） 的路徑重新導向到網路位置。</p>
<HR />
                        <p><h3><a href="folder-redirection/deploy-roaming-user-profiles.md">漫遊使用者設定檔</a></h3> 將使用者設定檔重新導向到網路位置。</p>
<HR />
                        <p><h3><a href="dfs-namespaces/dfs-overview.md">DFS 命名空間</a></h3> 共用的資料夾，群組成一或多個邏輯結構命名空間位於不同伺服器上。 每個命名空間對使用者顯示為含有一系列子資料夾的單一共用資料夾。 不過，命名空間的基本結構可以包含多個檔案共用，而它們位於不同伺服器與多個站台。</p>
<HR />
                        <p><h3><a href="dfs-replication/dfsr-overview.md">DFS 複寫</a></h3> 在多個伺服器與網站上複寫資料夾 （包括 DFS 命名空間路徑參照的）。 DFS 複寫使用的壓縮演算法稱為遠端差異壓縮 (RDC)。 RDC 會偵測到檔案中資料的變更，然後讓 DFS 複寫僅複寫已變更的檔案區塊，而不是複寫整個檔案。</p>
<HR />
                        <p><h3><a href="fsrm/fsrm-overview.md">檔案伺服器資源管理員</a></h3> 管理和分類儲存在檔案伺服器上的資料。<p>
<HR />
                        <p><h3><a href="iscsi/iscsi-target-server.md">iSCSI 目標伺服器</a></h3> 使用網際網路 SCSI (iSCSI) 標準，為網路上提供區塊儲存區其他伺服器與應用程式。</p>
<HR />
                       <p><h3><a href="iscsi/iscsi-boot-overview.md">iSCSI 目標伺服器</a></h3> 可開機數百個會儲存在集中位置的單一作業系統映像的電腦。 這個做法可以提升效率、管理性、可用性以及安全性。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-store.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>檔案系統、通訊協定等。</h3>
<HR />
                        <p><h3><a href="refs/refs-overview.md">ReFS</a></h3> 彈性檔案系統，將資料可用性最大化、 在各種工作負載，間有效率地調整為非常大型資料集，以及透過對於毀損的彈性 （不論是軟體或硬體失敗） 提供資料完整性。<p>
<HR />
                        <p><h3><a href="file-server/file-server-smb-overview.md">伺服器訊息區 (SMB) 通訊協定</a></h3> 網路檔案共用通訊協定，允許的應用程式來讀取和寫入檔案，以及要求來自電腦網路中伺服器程式服務的電腦上。 SMB 通訊協定可以用於其 TCP/IP 通訊協定或其他網路通訊協定的最上方。 使用 SMB 通訊協定，應用程式 (或應用程式的使用者) 可以存取遠端伺服器上的檔案或其他資源。 這允許應用程式讀取、建立及更新遠端伺服器上的檔案。 它也可以與任何設定來接收 SMB 用戶端要求的伺服器程式進行通訊。<p>
<HR />
                        <p><h3><a href="storage-spaces/Storage-class-memory-health.md">存放裝置類別記憶體</a></h3> 提供效能類似電腦記憶體 （高速），但是具備一般存放磁碟機的資料保留。 存放裝置類別記憶體對於 Windows 就如同一般的磁碟機 (只是速度更快)，但其裝置健全狀況的管理方式有一些差異。<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/cc766295(v=ws.10).aspx">BitLocker 磁碟機加密</a></h3> 即使電腦遭到篡改，或者當作業系統未執行時，將資料儲存於磁碟區以加密格式。 這有助於防範離線攻擊，攻擊是藉由停用或規避已安裝的作業系統，或是透過實際取出硬碟以個別攻擊資料。<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/dn466522(v=ws.11).aspx">NTFS</a></h3> 最新版 Windows 和 Windows Server 的主要檔案系統 — 提供一組完整的功能，包括安全性描述元、 加密、 磁碟配額，以及豐富的中繼資料，並且可與叢集共用磁碟區 (CSV) 以提供持續可用的磁碟區讓您可以從容錯移轉叢集的多個節點同時存取。<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/jj592688(v=ws.11).aspx">網路檔案系統 (NFS)</a></h3> 提供檔案共用解決方案，適用於具有 Windows 和非 Windows 電腦所組成之異質環境的企業。<p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

---


## 在 Azure 中

* [Azure 儲存體](https://azure.microsoft.com/documentation/services/storage/)
* [Azure StorSimple](https://www.microsoft.com/en-us/cloud-platform/azure-storsimple)
