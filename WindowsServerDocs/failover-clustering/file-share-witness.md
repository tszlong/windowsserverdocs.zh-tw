---
title: 部署 Windows Server 2019 中的檔案共用見證
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/24/2019
description: 檔案共用見證可讓您使用檔案共用，以在叢集仲裁投票。 本主題說明檔案共用見證和新功能，包括使用 USB 磁碟機連線到路由器，以作為檔案共用見證。
ms.localizationpriority: medium
ms.openlocfilehash: 47371be946c08cac2f271138d701922fc340a89d
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453042"
---
# <a name="deploy-a-file-share-witness"></a>部署檔案共用見證

> 適用於：Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

檔案共用見證是容錯移轉叢集做為叢集仲裁投票的 SMB 共用。 本主題提供技術和 Windows Server 2019，包括使用 USB 磁碟機連線到作為檔案共用見證的路由器中的新功能的概觀。

檔案共用見證是在下列情況下方便：  

- 無法使用的雲端見證，因為並非所有的伺服器叢集中有可靠的網際網路連線
- 無法使用磁碟見證，因為沒有任何共用的磁碟機，若要使用的磁碟見證。 這可能是儲存空間直接存取叢集中，SQL Server Alwayson 可用性群組 (AG)，Exchange 資料庫可用性群組 (DAG)，等等。沒有任何這些類型的叢集使用共用的磁碟。

## <a name="file-share-witness-requirements"></a>檔案共用見證需求

您也可以裝載檔案共用見證的已加入網域的 Windows 伺服器上，或者如果您的叢集執行 Windows Server 2019，任何裝置，可以主控件 SMB 2 或更新版本的檔案共用。

|檔案伺服器類型                 | 支援的叢集 |
|---------------------------------|--------------------|
|任何裝置的 w/SMB 2 的檔案共用 | Windows Server 2019|
|已加入網域的 Windows Server     | Windows Server 2008 及更新版本|

如果叢集正在執行 Windows Server 2019，需求如下：

- SMB 檔案共用*SMB 2 或更新版本的通訊協定會使用任何裝置上*，包括：
    - 網路連接存放 (NAS) 裝置
    - Windows 電腦加入工作群組
    - 使用本機連接的 USB 儲存裝置的路由器
- 驗證叢集的裝置上的本機帳戶
- 如果您改為使用 Active Directory 來驗證與檔案共用叢集，叢集名稱物件 (CNO) 必須在共用上，具有寫入權限和伺服器必須位在與叢集相同的 Active Directory 樹系
- 檔案共用至少為 5 MB 的可用空間

如果叢集正在執行 Windows Server 2016 或更早版本，需求如下：

- SMB 檔案共用*聯結至與叢集相同的 Active Directory 樹系的 Windows 伺服器上*
- 叢集名稱物件 (CNO) 必須在共用上具有寫入權限
- 檔案共用至少為 5 MB 的可用空間

其他注意事項：
- 若要使用已加入網域的 Windows server 以外的裝置所裝載的檔案共用見證，您目前必須使用**Set-clusterquorum-認證**PowerShell cmdlet 來設定見證，請在本主題稍後所述。
- 高可用性，您可以使用檔案共用見證的個別的容錯移轉叢集
- 檔案共用可供多個叢集
- 使用分散式檔案系統 (DFS) 共用或複寫的儲存體不支援任何版本的容錯移轉叢集。  這些可能會導致叢集的伺服器執行彼此獨立，而可能造成資料遺失的分割核心分裂情況。

## <a name="creating-a-file-share-witness-on-a-router-with-a-usb-device"></a>使用 USB 裝置，在路由器上建立檔案共用見證

在[Microsoft Ignite 2018](https://azure.microsoft.com/ignite/)， [DataOn 儲存體](http://www.dataonstorage.com/)必須在其 [kiosk] 區域的儲存空間直接存取叢集。  此叢集已連線到[NetGear](https://www.netgear.com) Nighthawk X4S WiFi 路由器使用的 USB 連接埠為檔案共用見證如下所示。

![NetGear 見證](media/File-Share-Witness/FSW1.png)

建立檔案共用見證，在這個特定的路由器上使用 USB 裝置的步驟如下所示。  請注意，在其他路由器和 NAS 裝置上的步驟會有所不同，而且應該使用廠商來完成提供指示。


1. 登入路由器插入 USB 裝置。

   ![NetGear 介面](media/File-Share-Witness/FSW2.png)

2. 從選項清單中，選取 ReadySHARE 也就是建立共用的位置。

   ![NetGear ReadySHARE](media/File-Share-Witness/FSW3.png)

3. 針對檔案共用見證，基本的共用是所需要的全部。  選取 [編輯] 按鈕，就會出現一個對話方塊，其中建立共用在 USB 裝置。

   ![NetGear 共用介面](media/File-Share-Witness/FSW4.png)

4. 一旦選取 [套用] 按鈕，共用會建立，並可以在清單中看到。

   ![NetGear 共用](media/File-Share-Witness/FSW5.png)

5. 一旦建立共用之後，建立叢集的檔案共用見證是使用 PowerShell 完成。

   ```PowerShell
   Set-ClusterQuorum -FileShareWitness \\readyshare\Witness -Credential (Get-Credential)
   ```

   這會顯示對話方塊，以在裝置上輸入本機帳戶。

這些相同的類似步驟可以完成其他路由器與 USB 功能、 NAS 裝置或其他 Windows 裝置上。
