---
title: 儲存體遷移服務常見問題（FAQ）
description: 關於儲存體遷移服務的常見問題，例如從一部伺服器遷移至另一部伺服器時，要從傳輸中排除哪些檔案。
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 08/19/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: a1e195ab755dfd0b61cc4201f43373421ce51aa2
ms.sourcegitcommit: 86350de764b89ebcac2a78ebf32631b7b5ce409a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2019
ms.locfileid: "70923552"
---
# <a name="storage-migration-service-frequently-asked-questions-faq"></a>儲存體遷移服務常見問題（FAQ）

本主題包含有關使用[儲存體遷移服務](overview.md)來遷移伺服器的常見問題（faq）的解答。

## <a name="what-files-and-folders-are-excluded-from-transfers"></a>哪些檔案和資料夾會從傳輸中排除？

儲存體遷移服務不會傳輸我們知道可能會干擾 Windows 操作的檔案或資料夾。 具體而言，我們不會傳送或移至目的地上的 PreExistingData 資料夾：

- Windows，程式檔案，program Files （x86），程式資料，使用者
- $Recycle bin、Recycler、回收、系統磁碟區資訊、$UpgDrv $、$SysReset、$Windows。 ~ BT、$Windows. ~ LS、Windows .old、開機、復原、檔和設定
- hiberfil.sys、sys.databases、cloud-init、winpepge.sys、sys.databases、bootsect.exe、.bak、bootmgr、bootnxt
- 來源伺服器上的任何檔案或資料夾，與目的地上排除的資料夾衝突。 <br>例如，如果來源上有 N:\Windows 資料夾，而且它會對應至 C：\因為它會干擾目的地上的 C：\Windows 系統資料夾，所以目的地上的磁片區不會傳輸（不論其包含的內容為何）。

## <a name="are-domain-migrations-supported"></a>是否支援網域遷移？

儲存體遷移服務不允許在 Active Directory 網域之間進行遷移。 伺服器之間的遷移一律會將目的地伺服器加入相同的網域。 您可以從 Active Directory 樹系中的不同網域使用遷移認證。 儲存體遷移服務支援在工作組之間進行遷移。  

## <a name="are-clusters-supported-as-sources-or-destinations"></a>叢集是否支援做為來源或目的地？

儲存體遷移服務目前不會在 Windows Server 2019 中的叢集之間進行遷移。 我們計畫在未來的儲存體遷移服務版本中新增叢集支援。

## <a name="do-local-groups-and-local-users-migrate"></a>本機群組和本機使用者是否遷移？

儲存體遷移服務目前不會在 Windows Server 2019 中遷移本機使用者或本機群組。 我們計畫在未來的儲存體遷移服務版本中新增本機使用者和本機群組遷移支援。

## <a name="is-domain-controller-migration-supported"></a>是否支援網域控制站遷移？

儲存體遷移服務目前不會在 Windows Server 2019 中遷移網域控制站。 因應措施是，只要您在 Active Directory 網域中有多個網域控制站，請先將網域控制站降級，再進行遷移，然後在剪下完成後將目的地升級。 我們計畫在未來的儲存體遷移服務版本中新增網域控制站遷移支援。

## <a name="what-attributes-are-migrated-by-the-storage-migration-service"></a>儲存體遷移服務會遷移哪些屬性？

儲存體遷移服務會遷移 SMB 共用的所有旗標、設定和安全性。 儲存體遷移服務所遷移的旗標清單包括：

    - 共用狀態
    - 可用性類型
    - 共用類型
    - 資料夾列舉模式 *（也稱為以存取為基礎的列舉或 ABE）*
    - 快取模式
    - 租用模式
    - Smb 實例
    - CA 超時
    - 並行使用者限制
    - 持續可用
    - 描述           
    - 加密資料
    - 身分識別遠端
    - 基礎結構
    - Name
    - `Path`
    - 範圍
    - 領域名稱
    - 安全性描述元
    - 陰影複製
    - 特殊
    - 臨時性

## <a name="can-i-consolidate-multiple-servers-into-one-server"></a>我可以將多部伺服器合併成一部伺服器嗎？

Windows Server 2019 隨附的儲存體遷移服務版本不支援將多部伺服器合併成一部伺服器。 合併的範例是將三個不同的來源伺服器（可能具有相同的共用名稱和本機檔案路徑）遷移到單一新伺服器上，將這些路徑和共用虛擬化，以防止任何重迭或衝突，然後全部回答三個先前的伺服器名稱和 IP 位址。 我們可能會在未來的儲存體遷移服務版本中新增這項功能。 

## <a name="can-i-migrate-from-sources-other-than-windows-server"></a>我可以從 Windows Server 以外的來源進行遷移嗎？

Windows Server 2019 隨附的儲存體遷移服務版本支援從 Windows Server 2003 和更新版本的作業系統進行遷移。 您也可以從使用 Samba 的 Linux 伺服器或裝置遷移儲存體;若要這麼做，請在執行 Windows Server 1903 或更新版本的伺服器上執行儲存體遷移服務。

## <a name="can-i-migrate-previous-file-versions"></a>我可以遷移先前的檔案版本嗎？

Windows Server 2019 隨附的儲存體遷移服務版本不支援將檔案的先前版本（與磁片區陰影複製服務一起建立）進行遷移。 只會遷移目前的版本。 

## <a name="optimizing-inventory-and-transfer-performance"></a>優化清查和傳輸效能

儲存體遷移服務包含多執行緒讀取和複製引擎，稱為儲存體遷移服務 Proxy 服務，其設計目的是要快速，並在許多檔案複製工具中提供完美的資料精確度。 雖然對許多客戶而言，預設設定都是最佳的，但在清查和傳輸期間，有一些方法可以改善 SMS 的效能。

- **使用適用于目的地作業系統的 Windows Server 2019。** Windows Server 2019 包含儲存體遷移服務 Proxy 服務。 當您安裝這項功能並遷移至 Windows Server 2019 目的地時，所有傳輸都會以直接在來源與目的地之間的可見方式運作。 如果目的地電腦是 Windows Server 2012 R2 或 Windows Server 2016，此服務就會在 orchestrator 中執行，這表示傳輸雙躍點，速度會變得很慢。 如果有多個工作在 Windows Server 2012 R2 或 Windows Server 2016 目的地執行，協調器就會成為瓶頸。 

- **改變預設傳送執行緒。** 儲存體遷移服務 Proxy 服務會在指定的作業中同時複製8個檔案。 您可以藉由在每個執行儲存體遷移服務 Proxy 的節點上調整下列登錄 REG_DWORD 值名稱，來增加同時複製執行緒的數目：

    HKEY_Local_Machine\Software\Microsoft\SMSProxy
    
    FileTransferThreadCount

   在 Windows Server 2019 中，有效範圍是1到128。 變更之後，您必須在參與遷移的所有電腦上重新開機儲存體遷移服務 Proxy 服務。 請謹慎使用此設定;將它設定為較高可能需要額外的核心、儲存體效能和網路頻寬。 若設定太高，可能會導致效能降低，相較于預設設定。

- **新增核心和記憶體。**  我們強烈建議來源、orchestrator 和目的地電腦至少要有兩個處理器核心或兩個個 vcpu，而更多可以大幅協助清查和傳輸效能，特別是在與 FileTransferThreadCount （上方）結合時。 當傳輸的檔案大於一般的 Office 格式（gb 或以上）時，傳輸效能會從比預設2GB 最小值更多的記憶體獲益。

- **建立多個作業。** 建立具有多個伺服器來源的作業時，會以序列方式來連接每部伺服器，以進行清查、傳輸和轉換。 這表示每一部伺服器都必須在另一部伺服器啟動之前完成其階段。 若要平行執行更多伺服器，只需建立多個作業，每個作業只包含一部伺服器。 SMS 最多支援100同時執行作業，這表示單一協調器可以平行處理許多 Windows Server 2019 目的地電腦。 如果您的目的地電腦是 Windows Server 2016 或 Windows Server 2012 R2，而不是在目的地上執行 SMS proxy 服務，則不建議執行多個並行作業，協調器必須自行執行所有傳輸，而且可能會變成成為. 在單一作業內平行執行伺服器的功能，是我們打算在較新版本的 SMS 中新增的功能。

- **搭配使用 SMB 3 與 RDMA 網路。** 如果是從 Windows Server 2012 或更新版本的來源電腦傳輸，SMB 3.x 支援 SMB 直接傳輸模式和 RDMA 網路。 RDMA 會將大部分的 CPU 成本從主機板 Cpu 轉移到上架 NIC 處理器，以降低延遲和伺服器 CPU 使用率。 此外，ROCE 和 iWARP 這類 RDMA 網路的頻寬通常會比一般 TCP/ethernet 高，包括25、50，以及每個介面100Gb 的速度。 使用 SMB 直接傳輸時，通常會將從網路到儲存體本身的傳送速率限制往上移動。   

- **使用 SMB 3 多重通道。** 如果是從 Windows Server 2012 或更新版本的來源電腦傳輸，SMB 3.x 支援可大幅提升檔案複製效能的多重通道複本。 只要來源和目的地都有下列內容，此功能就會自動運作：

   - 多個網路介面卡
   - 一或多個支援接收端調整（RSS）的網路介面卡
   - 使用 NIC 小組設定的其他網路介面卡之一
   - 一或多個支援 RDMA 的網路介面卡

- **更新驅動程式。** 視情況而定，安裝最新的廠商存放裝置和主機殼固件和驅動程式、最新廠商 HBA 驅動程式、最新廠商 BIOS/UEFI 固件、最新廠商網路驅動程式，以及來源、目的地和協調器上最新的主機板晶片組驅動程式伺服器. 視需要重新啟動節點。 如需設定共用儲存體和網路硬體，請參閱硬體廠商的文件。

- **啟用高效能處理。** 確定伺服器的 BIOS/UEFI 設定能提供高效能，例如停用 C-State、設定 QPI 速度、啟用 NUMA，以及設定最高的記憶體頻率。 確保 Windows Server 中的電源管理設定為高效能。 視需要重新啟動。 在完成遷移之後，別忘了將這些回復到適當的狀態。 

- **微調硬體**請參閱[Windows server 2016 的效能微調指導方針](https://docs.microsoft.com/windows-server/administration/performance-tuning/)，以微調執行 Windows Server 2019 和 Windows server 2016 的 orchestrator 和目的地電腦。 [網路子系統效能調整](https://docs.microsoft.com/windows-server/networking/technologies/network-subsystem/net-sub-performance-tuning-nics)一節包含特別有用的資訊。

- **使用更快速的儲存空間。** 雖然可能難以升級來源電腦儲存速度，但您應該確保目的地儲存體的寫入 IO 效能至少會快上一倍，因為來源會處於讀取 IO 效能，以確保傳輸中沒有任何不必要的瓶頸。 如果目的地是 VM，請確定至少有遷移的目的，它會在您的管理元件主機最快速的儲存層中執行，例如在 flash 層或使用鏡像全部-flash 或混合式空間的儲存空間直接存取 HCI 叢集。 當 SMS 遷移完成時，VM 可以即時移轉至較慢的層或主機。

- **更新防毒軟體。** 請務必確定您的來源和目的地正在執行防毒軟體的最新修補版本，以確保最小的效能負擔。 作為測試，您可以*暫時*排除在來源和目的地伺服器上清查或遷移的資料夾掃描。 如果您的傳輸效能已獲得改善，請洽詢您的防毒軟體廠商，以取得防毒軟體的更新版本或預期效能降低的說明。

## <a name="can-i-migrate-from-ntfs-to-refs"></a>我可以從 NTFS 遷移到 REFS 嗎？

Windows Server 2019 隨附的儲存體遷移服務版本不支援從 NTFS 遷移到 REFS 檔案系統。 您可以從 NTFS 遷移至 NTFS 和 REFS 至 ReFS。 這是設計的，因為功能、中繼資料及 ReFS 的其他方面有許多差異，而不是從 NTFS 複製。 ReFS 的目的是做為應用程式工作負載檔案系統，而不是一般檔案系統。 如需詳細資訊，請參閱[復原檔案系統（ReFS）總覽](../refs/refs-overview.md) 

## <a name="can-i-move-the-storage-migration-service-database"></a>我可以移動儲存體遷移服務資料庫嗎？

儲存體遷移服務會使用依預設安裝在隱藏的 c:\programdata\microsoft\storagemigrationservice 資料夾中的可擴充儲存引擎（ESE）資料庫。 此資料庫會隨著作業的新增和傳輸完成而成長，如果您未刪除作業，則在遷移數百萬個檔案之後，可能會耗用大量的磁碟空間。 如果需要移動資料庫，請執行下列步驟：

1. 停止 orchestrator 電腦上的「儲存體遷移服務」服務。
2. 取得`%programdata%/Microsoft/StorageMigrationService`資料夾的擁有權
3. 新增您的使用者帳戶，以對該共用及其所有檔案和子資料夾擁有完整控制權。
4. 將資料夾移至 orchestrator 電腦上的另一個磁片磁碟機。
5. 設定下列登錄 REG_SZ 值：

    HKEY_Local_Machine\Software\Microsoft\SMS DatabasePath =*不同磁片區上新資料庫檔案夾的路徑*。 
6. 確定系統對該資料夾的所有檔案和子資料夾具有完全控制權
7. 移除您自己的帳戶許可權。
8. 啟動「儲存體遷移服務」服務。

## <a name="give-feedback"></a>我有哪些選項可以提供意見反應、提出 bug，或取得支援？

若要提供儲存體遷移服務的意見反應：

- 使用包含在 Windows 10 中的意見反應中樞工具，按一下 [建議功能]，並指定「Windows Server」的類別和「儲存體遷移」的子類別
- 使用[Windows Server UserVoice](https://windowsserver.uservoice.com)網站
- 電子郵件smsfeed@microsoft.com

若要提出錯誤：

- 使用包含在 Windows 10 中的意見反應中樞工具，按一下 [回報問題]，並指定「Windows Server」的類別和「儲存體遷移」的子類別
- 透過[Microsoft 支援服務](https://support.microsoft.com)開啟支援案例

若要取得支援：

 - 在[Windows Server 技術小組](https://techcommunity.microsoft.com/t5/Windows-Server/ct-p/Windows-Server)張貼問題
 - [Windows Server 2019 Technet 論壇](https://social.technet.microsoft.com/Forums/en-US/home?forum=ws2019&filter=alltypes&sort=lastpostdesc)上的文章 
 - 透過[Microsoft 支援服務](https://support.microsoft.com)開啟支援案例

## <a name="see-also"></a>另請參閱

- [儲存體遷移服務總覽](overview.md)
