---
title: 跨網域叢集移轉 Windows Server 2016 2019
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/18/2019
description: 本文說明 Windows Server 2019 叢集移動到另一個網域
ms.localizationpriority: medium
ms.openlocfilehash: bcfd458c94d33820f434cde3313dc069fc42ffd9
ms.sourcegitcommit: 21677706eb85cb1396c1f40bf443146c09ef1b0d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/31/2019
ms.locfileid: "9042032"
---
# 容錯移轉叢集網域移轉

> 適用於： Windows Server 2019、 Windows Server 2016

本主題提供一個網域概略說明適用於移動的 Windows Server 容錯移轉叢集，到另一個。

## 為何要將網域之間移轉

有數個其中從一個 doamin 的叢集移轉到另一個是必要的案例。

- CompanyA 與 CompanyB 會合併並必須將所有叢集都移入 CompanyA 網域
- 叢集是內建的主要的資料中心和貨到遠端位置
- 叢集專為工作群組叢集與現在必須是網域的一部分
- 叢集專為網域叢集，而現在必須是工作群組的一部分
- 叢集移動到的公司到另一個區域，而是不同的子網域

Microsoft 不會嘗試資源從一個網域移動到另一個如果基礎的應用程式作業是不受支援的系統管理員提供支援。 例如，Microsoft 不會嘗試將 Microsoft Exchange server 移到另一個網域系統管理員提供支援。

   > [!WARNING]
   > 我們建議您叢集中執行所有的共用存放裝置的完整備份，再移動叢集。

## Windows Server 2016 和較舊版本

Windows Server 2016 和較舊版本，叢集服務也沒有之功能的移動到另一個網域。  這是因為增加依賴 Active Directory Domain Services 和所建立的虛擬名稱。   

## 選項

若要這樣做這類移動，有兩個選項。

摧毀叢集，以及在新的網域中重建它，牽涉到第一個選項。

![終結並重新建置](media\Cross-Domain-Cluster-Migration\Cross-Cluster-Domain-Migration-1.gif)

如所示的動畫，此選項會破壞性的步驟正在：

1. 摧毀叢集。
2. 將節點的網域成員資格變更成新的網域中。
3. 重新建立為新的更新的網域叢集。  這會伴隨需重新建立所有的資源。

第二個選項是較不具破壞性，但需要額外的硬體，因為要建置新的網域中需要新的叢集。  一旦在新的網域叢集，執行叢集移轉精靈来移轉的資源。 請注意，這不會移轉資料-您將需要使用另一種工具來移轉資料，例如[存放裝置移轉服務](../storage/storage-migration-service/overview.md)（一旦加入叢集支援）。

![建置和移轉](media\Cross-Domain-Cluster-Migration\Cross-Cluster-Domain-Migration-2.gif)

動畫顯示，因為此選項不具破壞性，但也需要不同的硬體或從現有的叢集節點比已移除。

1. 建立新的網域新的 clusterin，同時仍能使用舊的叢集。
2. 若要將所有資源都移轉到新叢集使用[叢集移轉精靈](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754481(v=ws.10))。 提醒，這不會複製資料，因此必須分別來完成。
3. 解除委任或摧毀舊的叢集。

在這兩個選項，新的叢集必須已安裝的所有[叢集感知的應用程式](https://technet.microsoft.com/aa369082(v=vs.90))、 驅動程式的所有最新狀態，而可能以確保所有測試將會正常執行。  如果資料也需要移動，這會是耗時的程序。

## Windows Server 2019

在 Windows Server 2019 中，我們導入了跨叢集網域移轉功能。  現在，可以輕鬆地完成上面所列的案例，需要重建不再需要。  

從一個網域移叢集是直截了當的處理程序。 若要這樣做，有兩個新的 PowerShell commandlet。

**新增 ClusterNameAccount** – 在 Active Directory**移除 ClusterNameAccount**建立叢集名稱帳戶 – 移除從 Active Directory 的叢集名稱帳戶

若要完成這個程序是變更從一個網域的叢集工作群組，並回到新的網域。  需要摧毀叢集，重新建置叢集，安裝應用程式等等，就不需要。 例如，它看起來像這樣：

![移轉](media\Cross-Domain-Cluster-Migration\Cross-Cluster-Domain-Migration-3.gif)

## 將叢集移轉到新的網域

在下列步驟，在叢集中正在移動 Contoso.com 網域從新 Fabrikam.com 網域。  叢集名稱是*CLUSCLUS*和使用稱為*FS CLUSCLUS*的檔案伺服器角色。

1. 具有相同名稱和密碼叢集中所有伺服器上的建立本機系統管理員帳戶。  這可能需要登入伺服器網域之間移動時。
2. 登入網域使用者或系統管理員帳戶具有 Active Directory 權限以叢集名稱物件 (CNO)，虛擬電腦物件 (VCO) 的第一個伺服器有存取權的叢集和開啟 PowerShell。
3. 確保所有的叢集網路名稱資源處於離線狀態並執行下列命令。  此命令將會移除叢集中可能會有的 Active Directory 物件。

   ```PowerShell
   Remove-ClusterNameAccount -Cluster CLUSCLUS -DeleteComputerObjects
   ```
4. 您可以使用 Active Directory 使用者和電腦，以確保 CNO 和 VCO 電腦已移除所有的叢集名稱相關聯的物件。

   > [!NOTE]
   > 它是不錯的想法叢集中所有伺服器上停止的叢集服務和設定服務啟動類型為手動，以便伺服器會重新啟動時變更網域時，不會啟動叢集服務。

   ```PowerShell
   Stop-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Manual
   ```

5. 變更工作群組伺服器的網域成員資格、 重新啟動伺服器，伺服器加入新的網域，並重新啟動一次。
6. 當伺服器中新的網域時，請登入網域使用者或系統管理員帳戶的 Active Directory 權限來建立物件的叢集，存取的伺服器，並開啟 PowerShell。 啟動叢集服務，並將它設定為 [自動。

   ```PowerShell
   Start-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Automatic
   ```
7. 將叢集名稱和所有其他的叢集網路命名資源到線上狀態。

   ```PowerShell
   Start-ClusterResource -Name "Cluster Name"

   Start-ClusterResource -Name FS-CLUSCLUS
   ```

8. 變更到應該屬於相關聯的 active directory 物件與新的網域叢集。 若要這樣做，命令低於和網路命名資源必須是在線上的狀態。  此命令將會執行的動作是重新建立在 Active Directory 中的名稱物件。

   ```PowerShell
   New-ClusterNameAccount -Name CLUSTERNAME -Domain NEWDOMAINNAME.com -UpgradeVCOs
   ```

    注意： 如果您不需要任何額外的群組，網路名稱 （也就是只有虛擬機器與 HYPER-V 叢集），-UpgradeVCOs 參數交換器就不需要。

9. 使用 Active Directory 使用者和電腦檢查新的網域，並確保已建立關聯的電腦物件。 如果有，然後帶入其餘資源群組上線。

   ```PowerShell
   Start-ClusterGroup -Name "Cluster Group"

   Start-ClusterGroup -Name FS-CLUSCLUS
   ```

## 已知問題

如果您使用新的 USB 見證功能，您無法在新的網域加入叢集。  原因是檔案共用見證類型，必須利用 Kerberos 驗證。  變更為 [無] 之前加入網域叢集見證。  一旦完成，請重新建立 USB 見證。  您會看到的錯誤是：

```
New-ClusternameAccount : Cluster name account cannot be created.  This cluster contains a file share witness with invalid permissions for a cluster of type AdministrativeAccesssPoint ActiveDirectoryAndDns. To proceed, delete the file share witness.  After this you can create the cluster name account and recreate the file share witness.  The new file share witness will be automatically created with valid permissions.
```

