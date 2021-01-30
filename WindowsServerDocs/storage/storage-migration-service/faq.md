---
title: '儲存體遷移服務的常見問題 (常見問題) '
description: 儲存體遷移服務的常見問題，例如，從一部伺服器遷移至另一部伺服器時，哪些檔案會從傳輸中排除。
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 01/29/2021
ms.topic: article
ms.openlocfilehash: 48dd8848f401556dc005727484006a95bf1f51b9
ms.sourcegitcommit: f89c1bc137ff92eeca2499131854287f28851f63
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2021
ms.locfileid: "99084956"
---
# <a name="storage-migration-service-frequently-asked-questions-faq"></a>儲存體遷移服務的常見問題 (常見問題) 

本主題包含常見問題的解答 (常見問題) 有關使用 [儲存體遷移服務](overview.md) 來遷移伺服器的問題。

## <a name="what-files-and-folders-are-excluded-from-transfers"></a>哪些檔案和資料夾已從傳輸中排除？

儲存體遷移服務不會傳送我們知道可能會干擾 Windows 作業的檔案或資料夾。 具體而言，我們將不會傳送或移至目的地上的 PreExistingData 資料夾：

- Windows、Program Files、Program Files (x86) 、程式資料、使用者
- $Recycle bin、Recycler、回收、系統磁碟區資訊、$UpgDrv $、$SysReset、$Windows. ~ BT、$Windows. ~ LS、Windows old、開機、修復、檔和設定
- pagefile.sys、hiberfil.sys、swapfile.sys、winpepge.sys、config.sys、bootsect.exe .bak、bootmgr、bootnxt
- 來源伺服器上的任何檔案或資料夾，與目的地上的排除資料夾發生衝突。 <br>例如，如果來源上有 N:\Windows 資料夾，並將它對應至 C：\因為它會干擾目的地的 C：\Windows 系統資料夾，所以不會傳輸目的地的磁片區，而不論它包含什麼內容。

## <a name="are-locked-files-migrated"></a>是否已遷移鎖定的檔案？

儲存體遷移服務不會遷移應用程式獨佔鎖定的檔案。 服務會自動重試三次，並在嘗試之間有60秒的延遲，而且您可以控制嘗試次數和延遲。 您也可以重新執行傳輸，只複製先前因共用違規而略過的檔案。

## <a name="are-domain-migrations-supported"></a>是否支援網域遷移？

儲存體遷移服務不允許在 Active Directory 網域之間進行遷移。 伺服器之間的遷移一律會將目的地伺服器加入相同的網域。 您可以使用 Active Directory 樹系中不同網域的遷移認證。 儲存體遷移服務支援在工作組之間進行遷移。

## <a name="are-clusters-supported-as-sources-or-destinations"></a>叢集是否支援作為來源或目的地？

儲存體遷移服務支援在安裝累計更新 [KB4513534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) 或後續更新之後，從叢集遷移到叢集。 這包括從來源叢集遷移至目的地叢集，以及從獨立來源伺服器遷移至目的地叢集，以進行裝置合併用途。 但是，您無法將叢集遷移到獨立伺服器。

## <a name="do-local-groups-and-local-users-migrate"></a>本機群組和本機使用者是否遷移？

儲存體遷移服務支援在安裝累計更新 [KB4513534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) 或後續更新之後，遷移本機使用者和群組。

## <a name="is-domain-controller-migration-supported"></a>是否支援網域控制站遷移？

儲存體遷移服務目前無法在 Windows Server 2019 中遷移網域控制站。 因應措施是，只要您在 Active Directory 網域中有多個網域控制站，請先將網域控制站降級後再進行遷移，然後在切換完成後升級目的地。 如果您選擇遷移網域控制站來源或目的地，您將無法進行切換。 從網域控制站遷移或移至網域控制站時，您絕不能遷移使用者和群組。

## <a name="what-attributes-are-migrated-by-the-storage-migration-service"></a>儲存體遷移服務會遷移哪些屬性？

儲存體遷移服務會遷移所有的 SMB 共用旗標、設定和安全性。 儲存體遷移服務所遷移的旗標清單包括：

- 共用狀態
- 可用性類型
- 共用類型
- 資料夾列舉模式 *(也稱為 Access-Based 列舉或 ABE)*
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
- 路徑
- 具範圍
- 領域名稱
- 安全性描述元
- 陰影複製
- 特殊
- 暫存

## <a name="can-i-consolidate-multiple-servers-into-one-server"></a>我可以將多部伺服器合併成一部伺服器嗎？

Windows Server 2019 中隨附的儲存體遷移服務版本不支援將多部伺服器合併至一部伺服器。 合併的範例將會遷移三個不同的來源伺服器（可能會有相同的共用名稱和本機檔案路徑）到單一新的伺服器，該伺服器會將這些路徑和共用虛擬化，以避免發生重迭或衝突，然後回答所有三個先前的伺服器名稱和 IP 位址。 不過，您可以將獨立伺服器遷移到單一叢集上的多個檔案伺服器資源。

## <a name="can-i-migrate-from-sources-other-than-windows-server"></a>我可以從 Windows Server 以外的來源進行遷移嗎？

儲存體遷移服務支援在安裝累計更新 [KB4513534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) 或後續更新之後，從 Samba Linux 伺服器進行遷移。 請參閱支援的 Samba 版本和 Linux 散發版本清單的需求。

## <a name="can-i-migrate-previous-file-versions"></a>我可以遷移先前的檔案版本嗎？

Windows Server 2019 中隨附的儲存體遷移服務版本不支援遷移舊版 (與磁片區陰影複製服務) 的舊版。 只有目前的版本會遷移。

## <a name="optimizing-inventory-and-transfer-performance"></a>將清查和傳輸效能優化

儲存體遷移服務包含多執行緒讀取和複製引擎（稱為「儲存體遷移服務 Proxy 服務」），其設計為可快速，並在許多檔案複製工具中提供完全資料的精確度。 雖然許多客戶的預設設定都是最佳的，但還是有方法可以改善清查和傳輸期間的 SMS 效能。

- **針對目的地作業系統使用 Windows Server 2019。** Windows Server 2019 包含儲存體遷移服務 Proxy 服務。 當您安裝此功能並遷移至 Windows Server 2019 目的地時，所有傳輸都會在來源與目的地之間以直接的方式運作。 如果目的地電腦是 Windows Server 2012 R2 或 Windows Server 2016，此服務會在 orchestrator 期間執行，這表示傳輸雙躍點的速度會變慢。 如果有多個工作與 Windows Server 2012 R2 或 Windows Server 2016 目的地一起執行，協調器就會變成瓶頸。

- **安裝最新的每月累計更新。** 我們已改善數個更新中的儲存體遷移服務 Proxy 服務，以獲得更好的傳輸和重新傳輸效能，以及清查效能。 安裝 [KB4580390 2020 年10月累積更新](https://support.microsoft.com/help/4580390/windows-10-update-kb4580390) 或更新版本，以獲得顯著的速度改進。

- **改變預設傳輸執行緒。** 儲存體遷移服務 Proxy 服務會在指定的作業中同時複製8個檔案。 您可以在執行儲存體遷移服務 Proxy 的每個節點上，以 decimal 調整下列登錄 REG_DWORD 值名稱，以增加同時複製執行緒的數目：

    HKEY_Local_Machine \Software\Microsoft\SMSProxy

    FileTransferThreadCount

   Windows Server 2019 中的有效範圍是1至512。 只要您建立新的作業，就不需要重新開機服務就可以開始使用此設定。 請謹慎使用此設定;設定較高的值可能需要額外的核心、儲存體效能和網路頻寬。 如果設定太高，可能會導致效能降低，相較于預設設定。

- **變更預設的平行共用執行緒。** 儲存體遷移服務 Proxy 服務會在指定的作業中同時從8個共用複製。 您可以調整儲存體遷移服務協調器伺服器上的下列登錄 REG_DWORD 值名稱，以增加同時共用執行緒的數目：

    HKEY_Local_Machine \Software\Microsoft\SMS

    EndpointFileTransferTaskCount

   Windows Server 2019 中的有效範圍是1至512。 只要您建立新的作業，就不需要重新開機服務就可以開始使用此設定。 請謹慎使用此設定;設定較高的值可能需要額外的核心、儲存體效能和網路頻寬。 如果設定太高，可能會導致效能降低，相較于預設設定。

    FileTransferThreadCount 和 EndpointFileTransferTaskCount 的總和是儲存體遷移服務可以同時從作業中的一個來源節點複製的檔案數目。 若要新增更多平行來源節點，請建立並執行更多的並行作業。

- **新增核心和記憶體。**  強烈建議來源、協調器和目的地電腦至少有兩個處理器核心或兩個個 vcpu，而且更多可大幅協助清查和傳輸效能，特別是與) 上述的 FileTransferThreadCount (結合時。 傳送大於一般 Office 格式的檔案時 (gb 或更大) 傳輸效能將會比預設2GB 的最小值更多的記憶體。

- **建立多個作業。** 建立具有多個伺服器來源的作業時，每一部伺服器都會以序列方式連接，以進行清查、傳輸和轉換。 這表示每一部伺服器都必須在另一部伺服器啟動之前完成其階段。 若要平行執行更多伺服器，只需建立多個作業，每個作業只包含一部伺服器。 SMS 支援最多100同時執行的作業，這表示單一協調器可以平行處理許多 Windows Server 2019 目的地電腦。 如果您的目的地電腦是 Windows Server 2016 或 Windows Server 2012 R2 （如同沒有在目的地上執行的 SMS proxy 服務），則不建議您執行多個平行作業。協調器必須執行所有的傳輸作業，而且可能會變成瓶頸。 在單一作業內平行執行伺服器的能力，是我們計畫在較新版本的 SMS 中新增的功能。

- **使用具有 RDMA 網路的 SMB 3。** 如果從 Windows Server 2012 或更新版本的來源電腦傳輸，SMB 3.x 支援 SMB Direct 模式和 RDMA 網路功能。 RDMA 會移動從主機板 Cpu 傳輸的大部分 CPU 成本以將 NIC 處理器上線，以降低延遲和伺服器 CPU 使用率。 此外，ROCE 和 iWARP 等 RDMA 網路的頻寬通常會高於一般的 TCP/ethernet，包括每個介面25、50和100Gb 的速度。 使用 SMB 直接存取時，通常會將傳送速率限制從網路向下移動到儲存體本身。

- **使用 SMB 3 多重通道。** 如果從 Windows Server 2012 或更新版本的來源電腦傳輸，SMB 3.x 支援多重通道複製，可大幅改善檔案複製效能。 只要來源和目的地都有下列各項功能，這項功能就會自動運作：

   - 多個網路介面卡
   - 一或多個支援接收端調整 (RSS 的網路介面卡) 
   - 使用 NIC 小組設定的其中一個網路介面卡
   - 一或多個支援 RDMA 的網路介面卡

- **更新驅動程式。** 如有需要，請安裝最新的廠商存放裝置和主機殼固件和驅動程式、最新的廠商 HBA 驅動程式、最新的廠商 BIOS/UEFI 固件、最新的廠商網路驅動程式，以及來源、目的地及 orchestrator 伺服器上的最新主機板晶片驅動程式 視需要重新啟動節點。 如需設定共用儲存體和網路硬體，請參閱硬體廠商的文件。

- **啟用高效能處理。** 確定伺服器的 BIOS/UEFI 設定能提供高效能，例如停用 C-State、設定 QPI 速度、啟用 NUMA，以及設定最高的記憶體頻率。 確定 Windows Server 中的電源管理設定為 [高效能]。 視需要重新啟動。 請記得在完成遷移之後，將這些狀態傳回適當的狀態。

- **調整硬體** 請參閱 [Windows server 2016 的效能微調指導方針](../../administration/performance-tuning/index.md) ，以微調執行 Windows server 2019 和 Windows server 2016 的 orchestrator 和目的地電腦。 [網路子系統效能微調](../../networking/technologies/network-subsystem/net-sub-performance-tuning-nics.md)區段包含特別有用的資訊。

- **使用更快速的儲存體。** 雖然可能很難升級來源電腦的儲存速度，但您應該確保目的地儲存體至少能達到寫入 IO 效能的速度，因為來源處於讀取 IO 效能，以確保傳輸中沒有任何不必要的瓶頸。 如果目的地是 VM，請務必至少針對遷移的目的，在您的執行程式主機的最快儲存層中執行，例如在 flash 層或使用鏡像全部-flash 或混合式空格儲存空間直接存取 HCI 叢集。 當 SMS 遷移完成時，VM 可以即時移轉至較慢的層級或主機。

- **更新防毒軟體。** 請務必確定您的來源和目的地正在執行防毒軟體的最新修補版本，以確保效能的最低負擔。 作為測試，您可以 *暫時* 排除要在來源與目的地伺服器上清查或遷移的資料夾掃描。 如果您的傳輸效能已獲得改善，請洽詢您的防毒軟體廠商以取得相關指示或更新版本的防毒軟體，或說明預期的效能降低。

## <a name="can-i-migrate-from-ntfs-to-refs"></a>我可以從 NTFS 遷移至 REFS 嗎？

Windows Server 2019 中隨附的儲存體遷移服務版本不支援從 NTFS 遷移至 REFS 檔案系統。 您可以從 NTFS 遷移至 NTFS，將 REFS 遷移至 ReFS。 這是設計的原因，是因為功能、中繼資料以及 ReFS 不會與 NTFS 重複的其他層面有許多差異。 ReFS 的目的是做為應用程式工作負載檔案系統，而不是一般檔案系統。 如需詳細資訊，請參閱 [復原檔案系統 (ReFS) 總覽](../refs/refs-overview.md)

## <a name="can-i-move-the-storage-migration-service-database"></a>我可以移動儲存體遷移服務資料庫嗎？

儲存體遷移服務會使用可延伸的儲存引擎 (在隱藏的 c:\programdata\microsoft\storagemigrationservice 資料夾中預設安裝的 ESE) 資料庫。 如果您未刪除作業，此資料庫將會隨著作業新增和傳輸而增加，並且在遷移數百萬個檔案之後，可能會耗用大量磁碟空間。 如果資料庫需要移動，請執行下列步驟：

1. 停止 orchestrator 電腦上的「儲存體遷移服務」服務。
2. 取得資料夾的擁有權 `%programdata%/Microsoft/StorageMigrationService`
3. 新增您的使用者帳戶，以擁有該共用及其所有檔案和子資料夾的完整控制權。
4. 將資料夾移至 orchestrator 電腦上的另一個磁片磁碟機。
5. 設定下列登錄 REG_SZ 值：

    HKEY_Local_Machine \Software\Microsoft\SMS DatabasePath = *指向不同磁片區上新資料庫檔案夾的路徑*

6. 確定系統對該資料夾的所有檔案和子資料夾具有完整控制權
7. 移除您自己的帳戶許可權。
8. 啟動「儲存體遷移服務」服務。

## <a name="does-the-storage-migration-service-migrate-locally-installed-applications-from-the-source-computer"></a>儲存體遷移服務是否會從來源電腦遷移本機安裝的應用程式？

否，儲存體遷移服務不會遷移本機安裝的應用程式。 完成遷移之後，請將任何應用程式重新安裝到來源電腦上執行的目的地電腦上。 不需要重新設定任何使用者或其應用程式;儲存體遷移服務的設計目的是要讓用戶端看不到伺服器變更。

## <a name="what-happens-with-existing-files-on-the-destination-server"></a>目的地伺服器上的現有檔案會發生什麼事？

執行傳輸時，儲存體遷移服務會搜尋來源伺服器的鏡像資料。 目的地伺服器不應包含任何實際執行資料或連接的使用者，因為該資料可能會遭到覆寫。 依預設，第一次傳輸會在目的地伺服器上建立任何資料的備份複本作為保護。 在所有後續的傳輸中，儲存體遷移服務預設會將資料鏡像至目的地;這表示不只是加入新的檔案，也會任意覆寫任何現有的檔案，以及刪除來源上不存在的任何檔案。 這是故意的行為，可讓來源電腦得到完美的精確度。

## <a name="what-do-the-error-numbers-mean-in-the-transfer-csv"></a>傳輸 CSV 中的錯誤號碼是什麼意思？

在傳輸 CSV 檔案中找到的大部分錯誤都是 Windows 系統錯誤碼。 您可以查看 [Win32 錯誤碼檔](/windows/win32/debug/system-error-codes)，瞭解每個錯誤的意義。

## <a name="are-existing-certificates-updated-on-the-destination-server-during-cutover"></a>在轉換期間，目的地伺服器上是否已更新現有憑證？

目的地伺服器可能包含在轉換之前發出的憑證-在其本機憑證存放區中，以伺服器的名稱作為主體、主體替代名稱或其他欄位的一部分。 當進行切換時，如果重新命名伺服器，就不會更新這些憑證。 您必須使用目前的部署方法（例如群組原則或網頁註冊），將憑證重新發出至新重新命名的伺服器。    

## <a name="what-are-my-options-to-give-feedback-file-bugs-or-get-support"></a><a name="give-feedback"></a> 有哪些選項可以提供意見反應、提出 bug 或取得支援？

若要提供關於儲存體遷移服務的意見反應：

- 使用 Windows 10 中包含的意見反應中樞工具，然後按一下 [建議功能]，並指定 "Windows Server" 的類別和「儲存體遷移」的子類別
- 使用 [Windows Server UserVoice](https://windowsserver.uservoice.com) 網站
- 電子郵件 smsfeed@microsoft.com

若要提出 bug：

- 使用 Windows 10 中包含的意見反應中樞工具，然後按一下 [回報問題]，並指定 "Windows Server" 的類別和「儲存體遷移」的子類別
- 透過[Microsoft 支援服務](https://support.microsoft.com)開啟支援案例

若要取得支援：

 - 在[Windows Server 技術小組](https://techcommunity.microsoft.com/t5/Windows-Server/ct-p/Windows-Server)張貼問題
 - 在[Windows Server 2019 論壇](/answers/topics/windows-server-2019.html)張貼文章
 - 透過[Microsoft 支援服務](https://support.microsoft.com)開啟支援案例

## <a name="additional-references"></a>其他參考

- [儲存體遷移服務總覽](overview.md)
