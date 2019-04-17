---
title: 部署 Windows Server 2019 中的檔案共用見證
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/24/2019
description: 檔案共用見證可讓您使用投票叢集仲裁中的檔案共用。 本主題描述檔案共用見證和新功能，包括使用 USB 磁碟機連接到檔案共用見證為路由器。
ms.localizationpriority: medium
ms.openlocfilehash: 1888142f96208800a0417c9caeea89e8a0472e88
ms.sourcegitcommit: d622f7af181ed0063d716b30278d41887a57db19
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/08/2019
ms.locfileid: "9150888"
---
# 部署檔案共用見證

> 適用於： Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

檔案共用見證是容錯移轉叢集使用中的叢集仲裁投票為 SMB 共用。 本主題提供技術和 Windows Server 2019，包括使用 USB 磁碟機連接到檔案共用見證為路由器中的新功能的概觀。

檔案共用見證為方便在下列情況：  

- 無法使用雲端見證，因為並非所有叢集中的伺服器都有可靠的網際網路連線
- 無法使用磁碟見證，因為沒有任何共用的磁碟機，以用於磁碟見證。 這可能是在儲存空間直接存取叢集中、 SQL Server 一律上可用性群組 (AG)，Exchange 資料庫可用性群組 (DAG)，等。 沒有任何這些類型的叢集使用共用的磁碟。

## 檔案共用見證需求

您也可以裝載檔案共用見證加入網域的 Windows 伺服器上，或如果您的叢集執行 Windows Server 2019，任何裝置，可以主機的 SMB 2 或更新版本的檔案共用。

|檔案伺服器類型                 | 支援的叢集 |
|---------------------------------|--------------------|
|任何裝置的 w/SMB 2 的檔案共用 | Windows Server 2019|
|加入網域的 Windows Server     | Windows Server 2008 和更新版本|

如果叢集中執行 Windows Server 2019，以下是需求：

- SMB 檔案共用*的 SMB 2 或更新版本的通訊協定使用的任何裝置上*，包括：
    - 網路連接儲存裝置 (NAS) 的裝置
    - Windows 電腦加入工作群組
    - 路由器以在本機連接的 USB 儲存裝置
- 用於驗證叢集在裝置上的本機帳戶
- 如果您改為使用 Active Directory 進行驗證的檔案共用的叢集，叢集名稱物件 (CNO) 必須擁有在共用的寫入權限和伺服器必須是相同的 Active Directory 樹系做為叢集
- 檔案共用已至少 5 MB 的可用空間

如果叢集中執行 Windows Server 2016 或更早版本，以下是需求：

- SMB 檔案共用*相同的 Active Directory 樹系做為叢集加入 Windows 伺服器上*
- 叢集名稱物件 (CNO) 必須有共用上的寫入權限
- 檔案共用已至少 5 MB 的可用空間

其他附註：
- 若要使用檔案共用見證主控的裝置已加入網域的 Windows server 以外，您目前必須使用**Set-clusterquorum-Credential** PowerShell cmdlet 來設定見證，稍後在本主題中所述。
- 針對高可用性，您可以個別的容錯移轉叢集上使用檔案共用見證
- 可以使用多個叢集的檔案共用
- 容錯移轉叢集的任何版本不支援使用分散式檔案系統 (DFS) 共用或已複寫存放裝置。  這些可能會造成分割大腦情況其中彼此獨立執行叢集的伺服器，而導致資料遺失。

## 建立檔案共用見證路由器上，使用 USB 裝置

在[Microsoft Ignite 2018](https://azure.microsoft.com/ignite/)， [DataOn 儲存](http://www.dataonstorage.com/)在其 kiosk 區域中所擁有儲存空間直接存取叢集。  這個叢集已連接到[NetGear](https://www.netgear.com) Nighthawk X4S WiFi 路由器類似如下的檔案共用見證為使用 USB 連接埠。

![NetGear 見證](media\File-Share-Witness\FSW1.png)

建立檔案共用見證這個特定的路由器上使用 USB 裝置的步驟如下所示。  請注意，步驟上其他路由器和 NAS 設備而有所不同，應該使用廠商來完成提供路線指引。


1. 記錄到路由器與 USB 裝置電源。

   ![NetGear 介面](media\File-Share-Witness\FSW2.png)

2. 從選項清單中，選取 [ReadySHARE 也就是建立共用的位置。

   ![NetGear ReadySHARE](media\File-Share-Witness\FSW3.png)

3. 如檔案共用見證，基本的共用 」 是所有所需。  選取 [編輯] 按鈕，就會出現一個對話方塊，其中可以建立共用 USB 裝置。

   ![NetGear 共用介面](media\File-Share-Witness\FSW4.png)

4. 一旦您選取 [套用] 按鈕，會建立共用，並可以看到清單中。

   ![NetGear 共用](media\File-Share-Witness\FSW5.png)

5. 一旦建立共用，建立叢集的檔案共用見證是使用 PowerShell 完成。

   ```PowerShell
   Set-ClusterQuorum -FileShareWitness \\readyshare\Witness -Credential (Get-Credential)
   ```

   這會顯示一個對話方塊，在裝置上輸入本機帳戶。

這些相同類似的步驟可以完成其他路由器使用 USB 功能、 NAS 裝置或其他 Windows 裝置上。
