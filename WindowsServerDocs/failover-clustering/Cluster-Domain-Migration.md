---
title: 跨網域在 Windows Server 2016/2019年的叢集移轉
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/18/2019
description: 本文說明將 Windows Server 2019 叢集從一個網域移到另一個
ms.localizationpriority: medium
ms.openlocfilehash: 1054de942e807f00586903683faeaf695ec2f033
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452936"
---
# <a name="failover-cluster-domain-migration"></a>容錯移轉叢集網域移轉

> 適用於：Windows Server 2019，Windows Server 2016

本主題提供到另一個叢集從一個網域的移動 Windows Server 容錯移轉的概觀。

## <a name="why-migrate-between-domains"></a>為什麼要移轉的網域之間

有數個案例是需要從一個 doamin 的叢集移轉到另一個。

- CompanyA CompanyB 與合併，並且必須將所有叢集都移至 CompanyA 網域
- 叢集會在主要資料中心內建和出貨至遠端位置
- 叢集已建置為 workgroup 叢集，現在需要是網域的一部分
- 叢集已建置為網域的叢集，而且現在必須屬於工作群組
- 叢集正在移動到另一個公司的其中一個區域，而且非常不同的子網域

Microsoft 不提供支援，系統管理員嘗試移動資源從某個網域，到另一個基礎的應用程式作業是否不受支援。 比方說，Microsoft 不提供支援的系統管理員嘗試將 Microsoft Exchange server 移到另一個網域。

   > [!WARNING]
   > 我們建議您在叢集中執行的所有共用的儲存體的完整備份之前移動該叢集。

## <a name="windows-server-2016-and-earlier"></a>Windows Server 2016 及更早版本

在 Windows Server 2016 和舊版中，叢集服務沒有從一個網域移動到另一個的能力。  這是因為 Active Directory 網域服務和所建立的虛擬名稱的增加相依性。   

## <a name="options"></a>選項。

為了執行這類移動，有兩個選項。

第一個選項涉及摧毀叢集，並重建新的網域中。

![終結並重新建置](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-1.gif)

動畫所示，此選項是破壞性的步驟要：

1. 摧毀叢集。
2. 將節點的網域成員資格變更為新的網域中。
3. 重新建立已更新網域中的新叢集。  這會需要不必重新建立所有資源。

第二個選項是較不具破壞性，但需要額外的硬體，如此新的叢集時就需要建立新的網域中。  新的網域中的叢集之後，執行叢集移轉精靈來移轉資源。 請注意，這不會將資料移轉-您必須使用另一個工具來移轉資料，例如[儲存體移轉服務](../storage/storage-migration-service/overview.md)（一旦新增叢集支援）。

![建置和移轉](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-2.gif)

動畫所示，此選項不是破壞性，但確實需要不同的硬體或從現有的叢集中的節點比已移除。

1. 建立新的定義域新 clusterin，同時仍然有舊的叢集中。
2. 使用[叢集移轉精靈](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754481(v=ws.10))移轉到新叢集的所有資源。 提醒您，這不會複製資料，因此必須個別進行。
3. 解除委任或摧毀舊叢集。

在這兩個選項中，新的叢集需要將所有[感知叢集應用程式](https://technet.microsoft.com/aa369082(v=vs.90))安裝，驅動程式的所有最新狀態，並可能測試，以確保所有將正常執行。  如果也需要移動資料，這會是耗時的程序。

## <a name="windows-server-2019"></a>Windows Server 2019

在 Windows Server 2019，我們引進了跨叢集網域移轉功能。  現在，可以輕鬆地完成以上所列的案例，並不再需要重建的需要。  

在網域之間移動叢集是簡單的程序。 若要達成此目的，有兩個新的 PowerShell 指令程式。

**新 ClusterNameAccount** – 在 Active Directory 中建立的叢集名稱帳戶**移除 ClusterNameAccount** – 從 Active Directory 中移除叢集名稱帳戶

若要完成此程序是從某個網域變更叢集中，加入工作群組，再回復成新的網域。  若要摧毀叢集、 重建叢集，請安裝應用程式等需要不一定需要。 比方說，看起來應該像這樣：

![移轉](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-3.gif)

## <a name="migrating-a-cluster-to-a-new-domain"></a>移轉至新網域的叢集

在下列步驟中，叢集會在從移 Contoso.com 網域到新的 Fabrikam.com 網域。  叢集名稱是*CLUSCLUS*並呼叫檔案伺服器角色*FS CLUSCLUS*。

1. 建立具有相同的名稱和密碼，在叢集中的所有伺服器上的本機系統管理員帳戶。  這可能需要登入，而伺服器會在網域之間移動。
2. 登入以使用網域使用者或系統管理員帳戶具有 Active Directory 權限到叢集名稱物件 (CNO)，虛擬電腦物件 (VCO) 的第一個伺服器可以存取叢集，並開啟 PowerShell。
3. 確定所有叢集網路名稱資源都處於離線狀態，執行下列命令。  此命令會移除叢集可能有的 Active Directory 物件。

   ```PowerShell
   Remove-ClusterNameAccount -Cluster CLUSCLUS -DeleteComputerObjects
   ```
4. 使用 Active Directory 使用者和電腦以確保在 CNO 與 VCO 電腦已移除所有的叢集名稱相關聯的物件。

   > [!NOTE]
   > 它是個不錯的主意，叢集中的所有伺服器上停止叢集服務和服務啟動類型設定為手動，使叢集服務無法啟動時變更網域時重新啟動伺服器。

   ```PowerShell
   Stop-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Manual
   ```

5. 變更伺服器的網域成員資格工作群組、 重新啟動伺服器，將伺服器加入新的網域，並重新啟動一次。
6. 新的網域中的伺服器之後，登入的網域使用者或系統管理員帳戶具有建立物件的 Active Directory 權限，可存取叢集的伺服器，並開啟 PowerShell。 啟動叢集服務，並將它設定為 自動。

   ```PowerShell
   Start-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Automatic
   ```
7. 將叢集名稱和所有其他叢集網路名稱資源為線上狀態。

   ```PowerShell
   Start-ClusterResource -Name "Cluster Name"

   Start-ClusterResource -Name FS-CLUSCLUS
   ```

8. 變更叢集是與相關聯的 active directory 物件的新網域的一部分。 若要這樣做，則命令如下，而且網路名稱資源必須處於線上狀態。  此命令會執行什麼動作是重新建立命名 Active Directory 中的物件。

   ```PowerShell
   New-ClusterNameAccount -Name CLUSTERNAME -Domain NEWDOMAINNAME.com -UpgradeVCOs
   ```

    注意：如果您沒有任何其他群組與網路名稱 （也就是使用只有虛擬機器的 HYPER-V 叢集），就不會需要-UpgradeVCOs 切換參數。

9. 使用 Active Directory 使用者和電腦來檢查新的網域，並確定相關聯的電腦物件所建立。 如果已建立，然後將剩餘的資源中群組上線。

   ```PowerShell
   Start-ClusterGroup -Name "Cluster Group"

   Start-ClusterGroup -Name FS-CLUSCLUS
   ```

## <a name="known-issues"></a>已知問題

如果您使用新的 USB 見證功能，您將無法將叢集新增到新的網域。  原因是檔案共用見證類型，必須利用 Kerberos 進行驗證。  變更為 無加入網域的叢集之前見證。  完成之後，重新建立 USB 見證。  您會看到此錯誤是：

```
New-ClusternameAccount : Cluster name account cannot be created.  This cluster contains a file share witness with invalid permissions for a cluster of type AdministrativeAccesssPoint ActiveDirectoryAndDns. To proceed, delete the file share witness.  After this you can create the cluster name account and recreate the file share witness.  The new file share witness will be automatically created with valid permissions.
```

