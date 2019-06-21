---
title: 儲存體移轉服務常見問題集 (faq)
description: 有關常見問題儲存體移轉服務，例如從一部伺服器移轉到另一個時，將會排除與傳輸的檔案。
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 06/04/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: 8f0f16f14ccf9099af8ff8bb8b27209c75c87cfc
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284463"
---
# <a name="storage-migration-service-frequently-asked-questions-faq"></a>儲存體移轉服務常見問題集 (faq)

本主題包含使用相關的常見問題 (Faq) 解答[儲存體移轉服務](overview.md)移轉伺服器。

## <a name="excluded-files"></a> 哪些檔案和資料夾會排除傳輸？

儲存體移轉服務將不會傳送檔案或資料夾，我們知道可能會干擾 Windows 作業。 具體而言，以下是什麼我們不會傳輸或移動到 PreExistingData 資料夾上的目的地：

- Windows 中，程式檔案，Program Files (x86)，Program Data，使用者
- $Recycle.bin recycler、 Recycled、 System Volume Information，$SysReset，$UpgDrv$ ~ BT、 $Windows.~ LS Windows.old、 開機、 復原、 文件和設定 $Windows。
- pagefile.sys、 hiberfil.sys、 swapfile.sys、 winpepge.sys、 config.sys、 bootsect.bak、 bootmgr、 bootnxt
- 任何檔案或與衝突的來源伺服器上的資料夾排除在目的地上的資料夾。 <br>例如，如果來源沒有 N:\Windows 資料夾，而且它取得對應至 C:\磁碟區上的目的地，它不會取得傳送 — 不論它的包含，因為它會干擾 C:\Windows 系統資料夾，在目的地上。

## <a name="domain-migration"></a> 支援網域移轉？

儲存體移轉服務不允許 Active Directory 網域之間進行移轉。 伺服器之間的移轉會一律將目的地伺服器聯結至相同的網域。 您可以使用移轉認證來自不同網域的 Active Directory 樹系。 儲存體移轉服務支援工作群組之間進行移轉。  

## <a name="cluster-support"></a> 為來源或目的地是否支援叢集？

儲存體移轉服務目前不在 Windows Server 2019 的叢集之間移轉。 我們計劃在未來的儲存體移轉服務版本中新增叢集支援。

## <a name="local-principals"></a> 本機群組和本機使用者移轉？

儲存體移轉服務目前不在本機使用者或本機群組的 Windows Server 2019 移轉。 我們計劃加入本機使用者和本機群組移轉儲存體移轉服務的未來版本的支援。

## <a name="domain-controller"></a> 支援的網域控制站移轉？

儲存體移轉服務目前不會移轉 Windows Server 2019 中的網域控制站。 因應措施，只要您在 Active Directory 網域中有一個以上的網域控制站，網域控制站降級之前移轉，在移交完成之後再升級目的地。 我們計劃在未來版本的儲存體移轉服務新增網域控制站移轉支援。

## <a name="share-attributes"></a> 儲存體移轉服務來移轉哪些屬性？

儲存體移轉服務將移轉所有的旗標、 設定和安全性的 SMB 共用。 儲存體移轉服務移轉的旗標的清單包括：

    - 共用狀態
    - 可用性類型
    - 共用類型
    - 資料夾列舉模式 *（也稱為存取型列舉或 ABE）*
    - 快取模式
    - 租用模式
    - Smb 執行個體
    - CA 逾時
    - 並行使用者限制
    - 持續可用
    - 描述           
    - 加密資料
    - 身分識別的遠端處理
    - 基礎結構
    - 名稱
    - `Path`
    - 範圍
    - 領域名稱
    - 安全性描述元
    - 陰影複製
    - 特殊
    - 暫存

## <a name="move-db"></a> 我可以移動儲存體移轉服務的資料庫嗎？

儲存體移轉服務會使用隱藏的 c:\programdata\microsoft\storagemigrationservice 資料夾中的預設安裝的可延伸儲存引擎 (ESE) 資料庫。 此資料庫會成長為作業會新增和傳輸都完成之後，在移轉之後可能會耗用大量的磁碟機空間數百萬個檔案，如果您不會刪除作業。 如果需要移動資料庫，請執行下列步驟：

1. 停止協調器電腦上的 「 儲存體移轉服務 」 服務。
2. 完整掌控`%programdata%/Microsoft/StorageMigrationService`資料夾
3. 您完整控制，共用的使用者帳戶和所有檔案和子資料夾都加入。
4. Orchestrator 電腦上將資料夾移至另一個磁碟機。
5. 設定下列登錄 REG_SZ 值：

    HKEY_Local_Machine\Software\Microsoft\SMS DatabasePath =*到不同的磁碟區上新的資料庫資料夾的路徑*。 
6. 請確定系統具有所有檔案和子資料夾，該資料夾的完整控制權
7. 移除您自己帳戶權限。
8. 啟動 「 儲存體移轉服務 」 服務。

## <a name="non-windows"></a> 我可以從 Windows Server 以外的來源移轉嗎？

隨附於 Windows Server 2019 的儲存體移轉服務版本支援從 Windows Server 2003 和更新版本的作業系統移轉。 目前無法從 Linux、 Samba、 NetApp、 EMC 或其他的 SAN 和 NAS 存放裝置移轉。 我們計劃在未來版本的儲存體移轉服務，從 Linux Samba 支援允許此。

## <a name="previous-versions"></a> 可以移轉舊版的檔案嗎？

隨附於 Windows Server 2019 的儲存體移轉服務版本不支援移轉 （使用磁碟區陰影複製服務提出） 的舊版檔案。 目前的版本將會移轉。 

## <a name="ntfs-refs"></a> 我可以從移轉 NTFS 為 REFS 嗎？

隨附於 Windows Server 2019 的儲存體移轉服務版本不支援從 NTFS 移轉到 REFS 檔案系統。 您可以從 NTFS 移轉到 NTFS 和 ReFS 的 REFS。 這是根據設計，由於功能、 中繼資料，以及 ReFS 不會重複從 NTFS 其他方面的許多差異。 ReFS 是為應用程式工作負載檔案系統，不是一般的檔案系統。 如需詳細資訊，請參閱[復原檔案系統 (ReFS) 概觀](../refs/refs-overview.md)

## <a name="consolidate-servers"></a> 我是否可以將多部伺服器合併成一部伺服器？

隨附於 Windows Server 2019 的儲存體移轉服務版本不支援將多部伺服器合併到一部伺服器。 彙總的範例會移轉三個不同的來源伺服器-可能會有相同的共用名稱，且虛擬化這些路徑和以防止任何重疊或發生衝突，共用的單一新伺服器上的本機檔案路徑-然後回答這三個先前的伺服器名稱和 IP 位址。 我們可能會在儲存體移轉服務的未來版本中新增這項功能。  

## <a name="optimize"></a> 清查和傳輸的效能最佳化

儲存體移轉服務包含的多執行緒的讀取和複製引擎呼叫我們設計能快速的同時，以及加入完美的資料精確度缺乏許多檔案複製工具中的儲存體移轉服務 Proxy 服務。 雖然預設組態最適合許多客戶，有辦法改進 SMS 清查和傳輸期間的效能。

- **您可以使用 Windows Server 2019 目的地作業系統。** Windows Server 2019 包含儲存體移轉服務 Proxy 服務。 當您安裝此功能，並將移轉至 Windows Server 2019 目的地時，所有傳輸都做為來源與目的地之間直接列看到。 此服務會執行的協調器在傳輸期間如果目的地電腦是 Windows Server 2012 R2 或 Windows Server 2016 中，這表示傳輸雙躍點，而且將會變得很慢。 如果有多個工作執行 Windows Server 2012 R2 或 Windows Server 2016 的目的地，orchestrator 將會成為瓶頸。 

- **改變預設傳輸執行緒。** 儲存體移轉服務 Proxy 服務中指定的作業，請以同時複製 8 個檔案。 您可以藉由調整下列登錄 REG_DWORD 值中的名稱執行 SMS Proxy 每個節點上的小數位增加同時複製執行緒的數目：

    HKEY_Local_Machine\Software\Microsoft\SMSProxy   FileTransferThreadCount

   有效的範圍是 1 到 128 個在 Windows Server 2019。 在變更之後，您必須重新移轉所參與的所有電腦上的儲存體移轉服務 Proxy 服務。 請謹慎使用這項設定。設定較高，可能需要額外的核心、 儲存體效能和網路頻寬。 設定太高可能會導致相較於預設設定的效能降低。 較新版的 SMS 計劃啟發方式變更 CPU、 記憶體、 網路和儲存體為基礎的執行緒設定的能力。

- **新增核心和記憶體。**  我們強烈建議在來源、 協調器時，與目的地電腦有兩個以上的處理器核心或兩個 Vcpu，，和多個大幅協助清查和傳輸的效能，尤其是結合 FileTransferThreadCount （請見上方）。 傳送比平常的 Office 格式更大的檔案時 (gb 或更高) 傳輸效能將受益於更多的記憶體，預設值 2 GB 最小值。

- **建立多個作業。** 建立工作時使用多個伺服器的來源，每一部伺服器會連絡序列的方式，清查，傳輸中和完全移轉。 這表示每一部伺服器，必須完成其階段，然後才開始另一部伺服器。 若要以平行方式執行更多的伺服器，只要建立多個作業，隨著每項工作包含只有一個伺服器。 SMS 支援高達 100 同時執行的作業，這表示單一的協調器可以平行處理許多 Windows Server 2019 目的地電腦。 我們不建議執行多個平行作業，如果您的目的地電腦是 Windows Server 2016 或 Windows Server 2012 R2，而不需要執行在目的地上的 SMS proxy 服務，協調器必須執行所有傳輸本身，而且可能會變得瓶頸。 若要在單一作業內的平行執行的伺服器是我們計劃 SMS 較新版本中加入功能。

- **使用 SMB 3 與 RDMA 網路。** 如果從 Windows Server 2012 或更新版本的來源電腦、 SMB 3.x 支援 SMB 直接模式和 RDMA 網路傳輸。 RDMA 的傳輸大部分的 CPU 成本會從移颸悁蠮 Cpu 上架 NIC 的處理器，以降低延遲和伺服器的 CPU 使用率。 此外，RDMA 網路等 ROCE 和 iWARP 通常具有較高頻寬也比一般 TCP/乙太網路，包括 25、 50 和 100 Gb 速度，每個介面。 通常使用 SMB 直接傳輸時，會將傳輸速度限制移到本身的儲存體網路。   

- **使用 SMB 3 多重通道。** 如果從 Windows Server 2012 或更新版本的來源電腦傳送，SMB 3.x 支援多通路複製可大幅提升檔案的複製效能。 這項功能會自動運作，只要來源與目的地有：

   - 多個網路介面卡
   - 支援接收端調整 (RSS) 的一或多個網路介面卡
   - 其中一個更多的網路介面卡使用 NIC 小組設定
   - 一或多個支援 RDMA 的網路介面卡

- **更新驅動程式。** 視需要在來源、 目的地和 orchestrator 上安裝最新的廠商儲存體和機箱韌體與驅動程式、 最新的廠商 HBA 驅動程式、 最新的廠商 BIOS/UEFI 韌體、 最新的廠商網路驅動程式和最新的主機板晶片組驅動程式伺服器。 視需要重新啟動節點。 如需設定共用儲存體和網路硬體，請參閱硬體廠商的文件。

- **啟用高效能處理。** 確定伺服器的 BIOS/UEFI 設定能提供高效能，例如停用 C-State、設定 QPI 速度、啟用 NUMA，以及設定最高的記憶體頻率。 請確定 Windows Server 中的電源管理設定為 高效能。 視需要重新啟動。 別忘了在完成移轉後會傳回以適當的狀態。 

- **調整硬體**檢閱[效能微調指導方針的 Windows Server 2016](https://docs.microsoft.com/windows-server/administration/performance-tuning/)微調 orchestrator 和執行 Windows Server 2019 的目的地電腦和 Windows Server 2016。 [網路子系統效能調整](https://docs.microsoft.com/windows-server/networking/technologies/network-subsystem/net-sub-performance-tuning-nics)區段包含特別有用的資訊。

- **使用更快速的儲存體。** 雖然它可能難以升級來源電腦儲存體的速度，您應該確保目的地儲存體至少要一樣快速寫入 IO 效能，以確保傳輸中沒有任何不必要的瓶頸的來源是在讀取 IO 效能。 如果目的地是 VM，能確保至少為移轉目的，它會執行最快的儲存層的 hypervisor 主機，例如快閃層，或搭配使用鏡像的全快閃或混合式空間的儲存空間直接存取 HCI 叢集。 SMS 移轉完成時 VM 可以是即時移轉至速度較慢的 「 層 」 或 「 主控件。

- **更新防毒軟體。** 務必確定您的來源和目的地執行防毒軟體，以確保最少的效能負擔的最新修補的版本。 進行測試，您可以*暫時*排除掃描您清查或移轉的來源和目的地伺服器上的資料夾。 如果傳輸效能已獲得改善，請連絡您的防毒軟體廠商，如需指示，或更新的版本的防毒軟體或預期的效能降低的說明。

## <a name="give-feedback"></a> 我要提供意見反應，提報 bug，或取得支援的選項有哪些？

若要提供意見反應，對儲存體移轉服務：

- 使用包含在 Windows 10 中，按一下 「 建議功能 」，並指定 「 Windows Server"的類別和子類別目錄的 「 存放裝置移轉 」 的意見反應中樞工具
- 使用[Windows Server UserVoice](https://windowsserver.uservoice.com)站台
- 電子郵件 smsfeed@microsoft.com

若要提報 bug:

- 使用包含在 Windows 10 中，按一下 回報問題 」，並指定 「 Windows Server"的類別和子類別目錄的 「 存放裝置移轉 」 的意見反應中樞工具
- 開啟支援案例，透過[Microsoft 支援服務](https://support.microsoft.com)

若要取得支援：

 - 將問題張貼在[Windows Server 技術社群](https://techcommunity.microsoft.com/t5/Windows-Server/ct-p/Windows-Server)
 - 張貼於[Windows Server 2019 Technet 論壇](https://social.technet.microsoft.com/Forums/en-US/home?forum=ws2019&filter=alltypes&sort=lastpostdesc) 
 - 開啟支援案例，透過[Microsoft 支援服務](https://support.microsoft.com)


## <a name="see-also"></a>另請參閱

- [儲存體移轉服務概觀](overview.md)
