---
title: 部署雙節點儲存空間直接存取 SOFS 以在 Azure 中儲存 UPD
description: 了解如何將儲存空間直接存取用於 RDS。
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
ms.openlocfilehash: 30f2d97c93c3df72eaf21896d596a4a10666013c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870748"
---
# <a name="deploy-a-two-node-storage-spaces-direct-scale-out-file-server-for-upd-storage-in-azure"></a>部署雙節點儲存空間直接存取向外延展檔案伺服器以在 Azure 中儲存 UPD

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

遠端桌面服務 (RDS) 需要已加入網域的檔案伺服器，使用者設定檔磁碟 (UPD) 才能運作。 若要在 Azure 中部署已加入網域的高可用性向外延展檔案伺服器 (SOFS)，請搭配使用儲存空間直接存取與 Windows Server 2016。 如果您不熟悉 UPD 或遠端桌面服務，請參閱[歡迎使用遠端桌面服務](welcome-to-rds.md)。

> [!NOTE] 
> Microsoft 近期才發佈[部署儲存空間直接存取向外延展檔案伺服器的 Azure 範本](https://azure.microsoft.com/documentation/templates/301-storage-spaces-direct/)！ 您可以使用此範本來建立部署，或使用本文中的步驟。 

建議您使用 DS 系列 VM 和進階儲存體資料磁碟來部署您的 SOFS，因為在此配置下，每個 VM 都會有相同數目和大小的資料磁碟。 您必須至少有兩個儲存體帳戶。 

針對小型部署，建議使用具有雲端見證的雙節點叢集；其中的磁碟區會有 2 個鏡像複本。 小型部署可藉由新增資料磁碟來擴充。 較大的部署可藉由新增節點 (VM) 來擴充。 

這些指示適用於雙節點部署。 下表列出您要為企業中的使用者數目儲存 UPD 時所需的 VM 和磁碟大小。 

| 使用者 | 總計 (GB) | VM | 磁碟數 | 磁碟類型 | 磁碟大小 (GB) | 設定   |
|-------|------------|----|---------|-----------|----------------|-----------------|
| 10    | 50         | DS1 | 2       | P10       | 128            | 2x(DS1 + 2 P10)  |
| 25    | 125        | DS1 | 2       | P10       | 128            | 2x(DS1 + 2 P10)  |
| 50    | 250        | DS1 | 2       | P10       | 128            | 2x(DS1 + 2 P10)  |
| 100   | 500        | DS1 | 2       | P20       | 512            | 2x(DS1 + 2 P20)  |
| 250   | 1250       | DS1 | 2       | P30       | 1024           | 2x(DS1 + 2 P30)  |
| 500   | 2500       | DS2 | 3       | P30       | 1024           | 2x(DS2 + 3 P30)  |
| 1000  | 5000       | DS3 | 5       | P30       | 1024           | 2x(DS3 + 5 P30)  |
| 2500  | 12500      | DS4 | 13      | P30       | 1024           | 2x(DS4 + 13 P30) |
| 5000  | 25000      | DS5 | 25      | P30       | 1024           | 2x(DS5 + 25 P30) | 

使用下列步驟可建立網域控制站 (我們在此將其命名為 "my-dc") 和兩個節點 VM ("my-fsn1" 和 "my-fsn2")，並設定要作為雙節點儲存空間直接存取 SOFS 的 VM。

1. 建立 [Microsoft Azure 訂用帳戶](https://azure.microsoft.com)。
2. 登入 [Azure 入口網站](https://ms.portal.azure.com)。
3. 在 Azure Resource Manager 中建立 [Azure 儲存體帳戶](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account)。 請將其建立在新的資源群組中，並使用下列設定：
   - 部署模型：資源管理員
   - 儲存體帳戶的類型：一般用途
   - 效能層：進階
   - 複寫選項：LRS
4. 藉由使用快速入門範本或手動部署樹系，來設定 Active Directory 樹系。 
   - 使用 Azure 快速入門範本進行部署：
      - [建立含有新 AD 樹系的 Azure VM](https://azure.microsoft.com/documentation/templates/active-directory-new-domain/)
      - [建立含有 2 個網域控制站的新 AD 網域](https://azure.microsoft.com/documentation/templates/active-directory-new-domain-ha-2-dc/) (以達到高可用性)
   - 使用下列設定手動[部署樹系](https://azure.microsoft.com/documentation/articles/active-directory-new-forest-virtual-machine/)：
      - 在與儲存體帳戶相同的資源群組中建立虛擬網路。
      - 建議大小：DS2 (如果網域控制站將託管多個網域物件，請增加大小)
      - 使用自動產生的 VNet。
      - 依照下列步驟安裝 AD DS。
5. 設定檔案伺服器叢集節點。 為此，您可以部署 [Windows Server 2016 儲存空間直接存取 SOFS 叢集 Azure 範本](https://azure.microsoft.com/resources/templates/301-storage-spaces-direct/)，或依照步驟 6-11 以手動方式部署。
6. 若要手動設定檔案伺服器叢集節點：
   1. 建立第一個節點： 
      1. 使用 Windows Server 2016 映像建立新的虛擬機器。 (按一下 [新增 > 虛擬機器 > Windows Server 2016]  。 選取 [資源管理員]  ，然後按一下 [建立]  。)
      2. 依照下列方式設定基本設定：
         - 名稱：my-fsn1
         - VM 磁碟類型：SSD
         - 使用您在步驟 3 中建立的現有資源群組。 
      3. 大小:DS1、DS2、DS3、DS4 或 DS5，視您的使用者需求而定 (請參閱這些指示開頭處的表格)。 請確定已選取進階磁碟支援。
      4. 設定： 
         - 儲存體帳戶：選擇您在步驟 3 中建立的儲存體帳戶。
         - 高可用性 - 建立新的可用性設定組。 (按一下 [高可用性 > 新建]  ，然後輸入名稱 (例如 s2d-cluster)。 針對 [更新網域]  和 [容錯網域]  請使用預設值。)
   2. 建立第二個節點。 以下列變更重複上述步驟：
      - 名稱：my-fsn2
      - 高可用性 - 選取您先前建立的可用性設定組。  
7. 根據您的使用者需求 (請見上表)，將[資料磁碟連結](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-attach-disk-portal/)至叢集節點 VM。 在資料磁碟建立並連結至 VM 後，將 [主機快取]  設為 [無]  。
8. 將所有 VM 的 IP 位址設為 [靜態]  。 
   1. 在資源群組中選取 VM，然後按一下 [網路介面]  (位於 [設定]  下方)。 選取列出的網路介面，然後按一下 [IP 設定]  。 選取列出的 IP 設定，並選取 [靜態]  ，然後按一下 [儲存]  。
   2. 記下網域控制站 (在我們的範例中為 my-dc) 的私人 IP 位址 (10.x.x.x)。
9. 將叢集節點 VM NIC 的 主要 DNS 伺服器位址設為 my-dc 伺服器。 選取 VM，然後按一下 [網路介面 > DNS 伺服器 > 自訂 DNS]  。 輸入您先前記下的私人 IP 位址，然後按一下 [儲存]  。
10. 建立[要作為雲端見證的 Azure 儲存體帳戶](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness)。 (如果您使用連結的指示，請先不要執行「使用容錯移轉叢集管理員 GUI 設定雲端見證」- 我們後續會執行該步驟。)
11. 設定儲存空間直接存取檔案伺服器。 連線至節點 VM，然後執行下列 Windows PowerShell Cmdlet。
    1. 在兩個檔案伺服器叢集節點 VM 上安裝容錯移轉叢集功能和檔案伺服器功能：

       ```powershell
       $nodes = ("my-fsn1", "my-fsn2")
       icm $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools} 
       icm $nodes {Install-WindowsFeature FS-FileServer} 
       ```
    2. 驗證叢集節點 VM，並建立雙節點 SOFS 叢集：

       ```powershell
       Test-Cluster -node $nodes
       New-Cluster -Name MY-CL1 -Node $nodes –NoStorage –StaticAddress [new address within your addr space]
       ``` 
    3. 設定雲端見證。 請使用您的雲端見證儲存體帳戶名稱和存取金鑰。

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
       若要檢視 SOFS 叢集上的叢集共用磁碟區的相關資訊，請執行下列 Cmdlet：

       ```powershell
       Get-ClusterSharedVolume
       ```
   
    6. 建立向外延展檔案伺服器 (SOFS)：

       ```powershell
       Add-ClusterScaleOutFileServerRole -Name my-sofs1 -Cluster MY-CL1
       ```

    7. 在 SOFS 叢集上建立新的 SMB 檔案共用。

       ```powershell
       New-Item -Path C:\ClusterStorage\Volume1\Data -ItemType Directory
       New-SmbShare -Name UpdStorage -Path C:\ClusterStorage\Volume1\Data
       ```

現在，您在 `\\my-sofs1\UpdStorage` 上已有共用位置，您可在為使用者[啟用 UPD](https://social.technet.microsoft.com/wiki/contents/articles/15304.installing-and-configuring-user-profile-disks-upd-in-windows-server-2012.aspx)時將其用於 UPD 儲存體。 
