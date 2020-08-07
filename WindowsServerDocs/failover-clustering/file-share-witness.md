---
title: 在 Windows Server 2019 中部署檔案共用見證
description: 檔案共用見證可讓您使用檔案共用，在叢集仲裁中進行投票。 本主題描述檔案共用見證和新功能，包括使用連接到路由器的 USB 磁片磁碟機做為檔案共用見證。
manager: eldenc
ms.topic: article
author: johnmarlin-msft
ms.author: johnmar
ms.date: 01/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: ea9dd3f79576048a57c85e879daf86567d325046
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87945831"
---
# <a name="deploy-a-file-share-witness"></a>部署檔案共用見證

> 適用於：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

「檔案共用見證」是一個 SMB 共用，容錯移轉叢集會在叢集仲裁中用來做為投票。 本主題提供 Windows Server 2019 中技術和新功能的總覽，包括使用連接到路由器的 USB 磁片磁碟機做為檔案共用見證。

在下列情況下，檔案共用見證很方便：

- 無法使用雲端見證，因為並非叢集中的所有伺服器都有可靠的網際網路連線
- 無法使用磁片見證，因為沒有任何共用磁片磁碟機可用於磁片見證。 這可能是儲存空間直接存取叢集、SQL Server Always On 可用性群組 (AG) 、Exchange 資料庫可用性群組 (DAG) 等等。 這些類型的叢集都不會使用共用磁片。

## <a name="file-share-witness-requirements"></a>檔案共用見證需求

您可以在已加入網域的 Windows 伺服器上裝載檔案共用見證，或如果您的叢集執行 Windows Server 2019，則可裝載 SMB 2 或更新版本之檔案共用的任何裝置。

|檔案伺服器類型                 | 支援的叢集 |
|---------------------------------|--------------------|
|任何具有 SMB 2 檔案共用的裝置 | Windows Server 2019|
|已加入網域的 Windows Server     | Windows Server 2008 及更新版本|

如果叢集正在執行 Windows Server 2019，以下是需求：

- *任何使用 smb 2 或更新版本通訊協定的裝置上*的 SMB 檔案共用，包括：
    -  (NAS) 裝置的網路連接存放裝置
    - 加入工作組的 Windows 電腦
    - 具有本機連線 USB 存放裝置的路由器
- 用於驗證叢集的裝置上的本機帳戶
- 如果您改為使用 Active Directory 來驗證具有檔案共用的叢集，則叢集名稱物件 (CNO) 必須擁有共用的寫入權限，且伺服器必須與叢集位於相同的 Active Directory 樹系中。
- 檔案共用至少有 5 MB 的可用空間

如果叢集執行的是 Windows Server 2016 或更早版本，以下是需求：

- Windows server 上的 SMB 檔案共用，已加入與叢集*相同的 Active Directory 樹*系
-  (CNO) 的叢集名稱物件必須擁有共用的寫入權限
- 檔案共用至少有 5 MB 的可用空間

其他注意事項：
- 若要使用裝置所裝載的檔案共用見證，而非已加入網域的 Windows server，您目前必須使用**set-clusterquorum-Credential** PowerShell Cmdlet 來設定見證，如本主題稍後所述。
- 如需高可用性，您可以在個別的容錯移轉叢集上使用檔案共用見證
- 檔案共用可供多個叢集使用
- 任何版本的容錯移轉叢集都不支援使用分散式檔案系統 (DFS) 共用或複寫的存放裝置。  這些可能會導致分割的大腦狀況，其中叢集伺服器彼此獨立執行，而且可能會造成資料遺失。

## <a name="creating-a-file-share-witness-on-a-router-with-a-usb-device"></a>在具有 USB 裝置的路由器上建立檔案共用見證

在[Microsoft Ignite 2018](https://azure.microsoft.com/ignite/)， [DataOn 儲存體](http://www.dataonstorage.com/)在其 kiosk 區域中有儲存空間直接存取叢集。  此叢集已連線到[NetGear](https://www.netgear.com) Nighthawk X4S WiFi 路由器，其使用 USB 埠作為檔案共用見證，如下所示。

![NetGear 見證](media/File-Share-Witness/FSW1.png)

以下列出在此特定路由器上使用 USB 裝置來建立檔案共用見證的步驟。  請注意，其他路由器和 NAS 裝置上的步驟會有所不同，而且應該使用廠商提供的指示來完成。


1. 登入已插入 USB 裝置的路由器。

   ![NetGear 介面](media/File-Share-Witness/FSW2.png)

2. 從選項清單中，選取 [ReadySHARE]，這是可以建立共用的位置。

   ![NetGear ReadySHARE](media/File-Share-Witness/FSW3.png)

3. 若為檔案共用見證，則需要基本共用。  選取 [編輯] 按鈕將會顯示一個對話方塊，讓您可以在 USB 裝置上建立共用。

   ![NetGear 共用介面](media/File-Share-Witness/FSW4.png)

4. 一旦選取 [套用] 按鈕，就會建立共用並顯示在清單中。

   ![NetGear 共用](media/File-Share-Witness/FSW5.png)

5. 建立共用之後，就可以使用 PowerShell 來建立叢集的檔案共用見證。

   ```PowerShell
   Set-ClusterQuorum -FileShareWitness \\readyshare\Witness -Credential (Get-Credential)
   ```

   這會顯示一個對話方塊，以在裝置上輸入本機帳戶。

這些相同的步驟也可以在具有 USB 功能、NAS 裝置或其他 Windows 裝置的其他路由器上執行。
