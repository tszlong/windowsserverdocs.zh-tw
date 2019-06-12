---
title: 部署兩個節點儲存空間直接存取 SOFS 針對 UDP 儲存體，在 Azure 中
description: 了解如何使用儲存空間直接存取 rds。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1099f21d-5f07-475a-92dd-ad08bc155da1
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
manager: scottman
ms.openlocfilehash: 792c9320f6976a4fc7f2ccd235f66daa0cb19b19
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805195"
---
# <a name="deploy-a-two-node-storage-spaces-direct-scale-out-file-server-for-upd-storage-in-azure"></a>部署在 Azure 中的 UDP 儲存體的兩個節點儲存空間直接存取向外延展檔案伺服器

>適用於：Windows Server （半年通道），Windows Server 2019，Windows Server 2016

遠端桌面服務 (RDS) 需要已加入網域的檔案伺服器的使用者設定檔磁碟 (Upd)。 若要在 Azure 中部署高可用性已加入網域的向外延展檔案伺服器 (SOFS)，使用儲存空間直接存取 Windows Server 2016。 如果您不熟悉 Upd 或遠端桌面服務，請參閱[歡迎使用遠端桌面服務](welcome-to-rds.md)。

> [!NOTE] 
> 剛發行的 Microsoft[部署儲存空間直接存取的向外延展檔案伺服器的 Azure 範本](https://azure.microsoft.com/documentation/templates/301-storage-spaces-direct/)！ 您可以使用範本來建立您的部署，或使用本文中的步驟。 

我們建議您部署您的 SOFS，使用 DS 系列 Vm 和進階儲存體資料磁碟，其中有相同數目和每個 VM 上的資料磁碟的大小。 您必須至少有兩個儲存體帳戶。 

針對小型部署，建議的 2 個節點叢集搭配雲端見證，具有 2 份鏡像磁碟區的所在。 藉由新增資料磁碟中擴大小型部署。 藉由新增節點 (Vm) 中擴大較大型的部署。 

這些指示是針對 2 個節點部署。 下表顯示的 VM 和磁碟大小，您必須在您企業中儲存的使用者數目的 Upd。 

| 使用者 | 總計 (GB) | VM | # 磁碟 | 磁碟類型 | 磁碟大小 (GB) | 組態   |
|-------|------------|----|---------|-----------|----------------|-----------------|
| 10    | 50         | DS1 | 2       | P10       | 128            | 2 倍 （DS1 + 2 P10）  |
| 25    | 125        | DS1 | 2       | P10       | 128            | 2 倍 （DS1 + 2 P10）  |
| 50    | 250        | DS1 | 2       | P10       | 128            | 2 倍 （DS1 + 2 P10）  |
| 100   | 500        | DS1 | 2       | P20       | 512            | 2 倍 （DS1 + 2 P20）  |
| 250   | 1250       | DS1 | 2       | P30       | 1024           | 2 倍 （DS1 + 2 P30）  |
| 500   | 2500       | DS2 | 3       | P30       | 1024           | 2 倍 （DS2 + 3 P30）  |
| 1000  | 5000       | DS3 | 5       | P30       | 1024           | 2 倍 （DS3 + 5 P30）  |
| 2500  | 12500      | DS4 | 13      | P30       | 1024           | 2 倍 （DS4 + 13 P30） |
| 5000  | 25000      | DS5 | 25      | P30       | 1024           | 2 倍 （DS5 + 25 P30） | 

使用下列步驟來建立網域控制站 （我們稱為我們"my dc"下方） 和兩個節點 （"my fsn1 」 和 「 我 fsn2"） 的 Vm 並設定為 2 個節點儲存空間直接存取 SOFS 的 Vm。

1. 建立[Microsoft Azure 訂用帳戶](https://azure.microsoft.com)。
2. 登入 [Azure 入口網站](https://ms.portal.azure.com)。
3. 建立[Azure 儲存體帳戶](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account)在 Azure Resource Manager。 建立新的資源群組中，並使用下列設定：
   - 部署模型：Resource Manager
   - 儲存體帳戶類型：一般用途
   - 效能層級：Premium
   - 複寫選項：LRS
4. 若要設定 Active Directory 樹系，可使用 快速入門範本或以手動方式部署樹系。 
   - 部署使用 Azure 快速入門範本：
      - [與新的 AD 樹系中建立 Azure VM](https://azure.microsoft.com/documentation/templates/active-directory-new-domain/)
      - [使用 2 個網域控制站中建立新的 AD 網域](https://azure.microsoft.com/documentation/templates/active-directory-new-domain-ha-2-dc/)（適用於高可用性）
   - 以手動方式[部署樹系](https://azure.microsoft.com/documentation/articles/active-directory-new-forest-virtual-machine/)具備下列組態：
      - 建立虛擬網路中的儲存體帳戶相同的資源群組。
      - 建議的大小：DS2 （如果網域控制站會裝載多個網域物件，請增加大小）
      - 使用自動產生的 VNet。
      - 請依照下列步驟來安裝 AD DS。
5. 設定檔案伺服器叢集節點。 您可以藉由部署[Windows Server 2016 儲存空間直接存取 SOFS 叢集的 Azure 範本](https://azure.microsoft.com/resources/templates/301-storage-spaces-direct/)或依照下列步驟 6-11，以手動方式部署。
6. 若要以手動方式設定檔案伺服器叢集節點：
   1. 建立第一個節點： 
      1. 建立新的虛擬機器使用 Windows Server 2016 映像。 (按一下**新增 > 虛擬機器 > Windows Server 2016。** 選取**Resource Manager**，然後按一下**建立**。)
      2. 設定基本的組態如下所示：
         - -Fsn1 名稱： my
         - SSD 的 VM 磁碟類型
         - 使用現有的資源群組，您在步驟 3 中建立一個。 
      3. 大小:DS1、 DS2、 DS3，DS4 或根據您的使用者 DS5 需要 （請參閱這些指示的開頭的資料表）。 請確定已選取 高階磁碟支援。
      4. 設定： 
         - 儲存體帳戶：選擇您在步驟 3 中建立的儲存體帳戶。
         - 高可用性-建立新的可用性設定組。 (按一下**高可用性 > 建立新**，然後輸入名稱 （例如，叢集 s2d）。 使用的預設值**更新網域**並**容錯網域**。)
   2. 建立第二個節點。 重複上述步驟，以下列變更：
      - -Fsn2 名稱： my
      - 高可用性-上面建立的可用性設定組，您的選取。  
7. [將資料磁碟連結](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-attach-disk-portal/)到叢集節點 Vm，以根據您的使用者需要 （如同上表中所見）。 之後資料磁碟建立並連結至 VM，設定**主機快取**要**無**。
8. 設定所有 Vm 的 IP 位址**靜態**。 
   1. 在資源群組中，選取 VM，然後**網路介面**(底下**設定**)。 選取列出的網路介面，然後按一下**IP 組態**。 選取列出的 IP 組態，請選取**靜態**，然後按一下**儲存**。
   2. 請注意網域控制站 (my-dc 我們的範例) 私人 IP 位址 (10.x.x.x)。
9. 設定 Nic 的叢集節點 Vm 上的主要 DNS 伺服器位址，為我 dc 伺服器。 選取 VM，然後按一下**網路介面 > DNS 伺服器 > 自訂 DNS**。 輸入您先前所述，私人 IP 位址，然後按一下**儲存**。
10. 建立[Azure 儲存體帳戶，為您的雲端見證](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness)。 （如果您使用連結的指示，停止您前往 「 設定雲端見證與容錯移轉叢集管理員 GUI"-我們將執行該步驟。）
11. 設定儲存空間直接存取的檔案伺服器。 連線到 VM 的節點，然後執行下列 Windows PowerShell cmdlet。
    1. 兩個檔案伺服器叢集節點 Vm 上安裝容錯移轉叢集功能和檔案伺服器功能：

       ```powershell
       $nodes = ("my-fsn1", "my-fsn2")
       icm $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools} 
       icm $nodes {Install-WindowsFeature FS-FileServer} 
       ```
    2. 驗證叢集節點 Vm，並建立 2 個節點在 SOFS 叢集：

       ```powershell
       Test-Cluster -node $nodes
       New-Cluster -Name MY-CL1 -Node $nodes –NoStorage –StaticAddress [new address within your addr space]
       ``` 
    3. 設定雲端見證。 使用雲端見證儲存體帳戶名稱和存取金鑰。

       ```powershell
       Set-ClusterQuorum –CloudWitness –AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> 
       ```
    4. 啟用儲存空間直接存取。

       ```powershell
       Enable-ClusterS2D 
       ```
      
    5. 建立虛擬磁碟的磁碟區。

       ```powershell
       New-Volume -StoragePoolFriendlyName S2D* -FriendlyName VDisk01 -FileSystem CSVFS_REFS -Size 120GB 
       ```
       若要檢視在 SOFS 叢集上的叢集共用磁碟區的相關資訊，請執行下列 cmdlet:

       ```powershell
       Get-ClusterSharedVolume
       ```
   
    6. 建立向外延展檔案伺服器 (SOFS):

       ```powershell
       Add-ClusterScaleOutFileServerRole -Name my-sofs1 -Cluster MY-CL1
       ```

    7. 建立新的 SMB 檔案共用 SOFS 叢集上。

       ```powershell
       New-Item -Path C:\ClusterStorage\Volume1\Data -ItemType Directory
       New-SmbShare -Name UpdStorage -Path C:\ClusterStorage\Volume1\Data
       ```

您現在可以在共用`\\my-sofs1\UpdStorage`，您可以使用針對 UDP 儲存體時您[啟用 UPD](https://social.technet.microsoft.com/wiki/contents/articles/15304.installing-and-configuring-user-profile-disks-upd-in-windows-server-2012.aspx)為您的使用者。 
