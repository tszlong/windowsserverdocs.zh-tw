---
title: 儲存體移轉服務常見問題集 (faq)
description: 有關常見問題儲存體移轉服務，例如從一部伺服器移轉到另一個時，將會排除與傳輸的檔案。
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 11/06/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: df03f722b7b36a163693f675a2eaade2fabeb82f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860909"
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
    - 路徑
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

## <a name="transfer-threads"></a> 我可以增加同時複製的檔案的數目嗎？

儲存體移轉服務 Proxy 服務中指定的作業，請以同時複製 8 個檔案。 這項服務會在執行協調器在傳輸期間的目的地電腦是 Windows Server 2012 R2 或 Windows Server 2016 中，但也會執行所有的 Windows Server 2019 目的地節點上。 您可以藉由調整下列登錄 REG_DWORD 值中的名稱執行 SMS Proxy 每個節點上的小數位增加同時複製執行緒的數目：

    HKEY_Local_Machine\Software\Microsoft\SMSProxy
    FileTransferThreadCount

 有效的範圍是 1 到 128 個在 Windows Server 2019。 

 變更之後您必須重新啟動儲存體移轉服務 Proxy 服務上所有電腦 partipating 在移轉。 我們計劃會引發這個儲存體移轉服務的未來版本中的數字。

## <a name="non-windows"></a> 我可以從 Windows Server 以外的來源移轉嗎？

隨附於 Windows Server 2019 的儲存體移轉服務版本支援從 Windows Server 2003 和更新版本的作業系統移轉。 目前無法從 Linux、 Samba、 NetApp、 EMC 或其他的 SAN 和 NAS 存放裝置移轉。 我們計劃在未來版本的儲存體移轉服務，從 Linux Samba 支援允許此。

## <a name="previous-versions"></a> 可以移轉舊版的檔案嗎？

隨附於 Windows Server 2019 的儲存體移轉服務版本不支援移轉 （使用磁碟區陰影複製服務提出） 的舊版檔案。 目前的版本將會移轉。 

## <a name="ntfs-refs"></a> 我可以從移轉 NTFS 為 REFS 嗎？

隨附於 Windows Server 2019 的儲存體移轉服務版本不支援從 NTFS 移轉到 REFS 檔案系統。 您可以從 NTFS 移轉到 NTFS 和 ReFS 的 REFS。 這是根據設計，由於功能、 中繼資料，以及 ReFS 不會重複從 NTFS 其他方面的許多差異。 ReFS 是為應用程式工作負載檔案系統，不是一般的檔案系統。 如需詳細資訊，請參閱[復原檔案系統 (ReFS) 概觀](../refs/refs-overview.md)

## <a name="consolidate-servers"></a> 我是否可以將多部伺服器合併成一部伺服器？

隨附於 Windows Server 2019 的儲存體移轉服務版本不支援將多部伺服器合併到一部伺服器。 彙總的範例會移轉三個不同的來源伺服器-可能會有相同的共用名稱，且虛擬化這些路徑和以防止任何重疊或發生衝突，共用的單一新伺服器上的本機檔案路徑-然後回答這三個先前的伺服器名稱和 IP 位址。 我們可能會在儲存體移轉服務的未來版本中新增這項功能。  


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
