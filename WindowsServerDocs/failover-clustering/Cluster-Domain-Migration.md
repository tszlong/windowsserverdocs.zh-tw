---
title: Windows Server 2016/2019 中的跨網域叢集遷移
description: 本文說明如何將 Windows Server 2019 叢集從一個網域移到另一個網域
ms.prod: windows-server
manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.author: johnmar
ms.date: 01/18/2019
ms.localizationpriority: medium
ms.openlocfilehash: 6062dd987a136bc2be67c09efbe399bb8fae24f6
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/27/2020
ms.locfileid: "87178524"
---
# <a name="failover-cluster-domain-migration"></a>容錯移轉叢集網域遷移

> 適用於：Windows Server 2019、Windows Server 2016

本主題提供將 Windows Server 容錯移轉叢集從一個網域移至另一個網域的總覽。

## <a name="why-migrate-between-domains"></a>為何要在網域之間進行遷移

在許多情況下，需要將叢集從一個 doamin 遷移至另一個。

- CompanyA 與 CompanyB 合併，且必須將所有叢集移至 CompanyA 網域
- 叢集建置於主要資料中心，並出貨到遠端位置
- 叢集是以工作組叢集的形式建立，現在必須是網域的一部分
- 叢集是以網域叢集的形式建立，現在必須是工作組的一部分
- 正在將叢集移至公司的一個區域，而且是不同的子域

如果不支援基礎應用程式作業，Microsoft 不會針對嘗試將資源從某個網域移至另一個網域的系統管理員提供支援。 例如，Microsoft 不會針對嘗試將 Microsoft Exchange server 從一個網域移至另一個網域的系統管理員提供支援。

   > [!WARNING]
   > 我們建議您在移動叢集之前，先執行叢集中所有共用存放裝置的完整備份。

## <a name="windows-server-2016-and-earlier"></a>Windows Server 2016 (含) 以前版本

在 Windows Server 2016 和更早版本中，叢集服務無法從某個網域移至另一個網域。  這是因為 Active Directory Domain Services 和所建立之虛擬名稱的相依性增加。

## <a name="options"></a>選項。

為了進行這類移動，有兩個選項。

第一個選項包括終結叢集，並在新的網域中重建叢集。

![損毀和重建](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-1.gif)

如動畫所示，此選項會破壞下列步驟：

1. 損毀叢集。
2. 將節點的網域成員資格變更為新的網域。
3. 在更新的網域中，將叢集重新建立為新的叢集。  這需要重新建立所有資源。

第二個選項的破壞性較低，但需要額外的硬體，因為新的叢集需要建立新的網域。  一旦叢集在新網域中，請執行 [叢集遷移嚮導] 以遷移資源。 請注意，這不會遷移資料，您必須使用另一個工具來遷移資料，例如[儲存體遷移服務](../storage/storage-migration-service/overview.md)（新增叢集支援之後）。

![組建和遷移](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-2.gif)

如動畫所示，此選項不具破壞性，但需要來自現有叢集的不同硬體或節點，而不是已移除。

1. 在新網域中建立新的 clusterin，同時仍然具有舊的叢集可用。
2. 使用 [叢集[遷移嚮導]](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754481(v=ws.10))將所有資源遷移至新叢集。 提醒您，這不會複製資料，因此必須另外完成。
3. 解除委任或摧毀舊叢集。

在這兩個選項中，新的叢集都必須安裝所有叢集[感知應用程式](https://technet.microsoft.com/aa369082(v=vs.90))、驅動程式都是最新狀態，而且可能會進行測試，以確保所有元件都能正常執行。  如果也需要移動資料，這就是耗時的進程。

## <a name="windows-server-2019"></a>Windows Server 2019

在 Windows Server 2019 中，我們引進了跨叢集網域遷移功能。  因此，您可以輕鬆地完成上述案例，而且不再需要重建的需求。

從一個網域移動叢集是一個簡單的程式。 為了達成此目的，有兩個新的 PowerShell commandlet。

**ClusterNameAccount** –在 Active Directory**移除-ClusterNameAccount**中建立叢集名稱帳戶–移除叢集名稱帳戶，Active Directory

完成這項作業的程式是將叢集從一個網域變更為一個工作組，並回到新的網域。  不需要摧毀叢集、重建叢集、安裝應用程式等。 例如，它看起來會像這樣：

![遷移](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-3.gif)

## <a name="migrating-a-cluster-to-a-new-domain"></a>將叢集遷移至新的網域

在下列步驟中，會將叢集從 Contoso.com 網域移動到新的 Fabrikam.com 網域。  叢集名稱是*CLUSCLUS* ，而且具有名為*FS-CLUSCLUS*的檔案伺服器角色。

1. 在叢集中的所有伺服器上，建立具有相同名稱和密碼的本機系統管理員帳戶。  當伺服器在網域之間移動時，可能需要進行這項登入。
2. 使用 Active Directory 具有叢集名稱物件（CNO）、虛擬電腦物件（VCO）許可權的網域使用者或系統管理員帳戶登入第一部伺服器，以存取叢集並開啟 PowerShell。
3. 請確定所有叢集網路名稱資源都處於離線狀態，然後執行下列命令。  此命令將會移除叢集可能擁有的 Active Directory 物件。

   ```PowerShell
   Remove-ClusterNameAccount -Cluster CLUSCLUS -DeleteComputerObjects
   ```
4. 請使用 Active Directory 使用者和電腦，以確保已移除與所有叢集名稱相關聯的 CNO 和 VCO 電腦物件。

   > [!NOTE]
   > 建議您在叢集中的所有伺服器上停止叢集服務，並將服務啟動類型設定為 [手動]，如此一來，當伺服器在變更網域時重新開機時，叢集服務就不會啟動。

   ```PowerShell
   Stop-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Manual
   ```

5. 將伺服器的網域成員資格變更為工作組、重新開機伺服器、將伺服器加入新網域，然後重新開機。
6. 一旦伺服器位於新網域中，請使用具有 Active Directory 許可權的網域使用者或系統管理員帳戶登入伺服器，以建立物件、擁有叢集的存取權，以及開啟 PowerShell。 啟動叢集服務，並將它設定回 [自動]。

   ```PowerShell
   Start-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Automatic
   ```
7. 將叢集名稱和所有其他叢集網路名稱資源帶入線上狀態。

   ```PowerShell
   Start-ClusterResource -Name "Cluster Name"

   Start-ClusterResource -Name FS-CLUSCLUS
   ```

8. 使用相關聯的 active directory 物件，將叢集變更為新網域的一部分。 若要這樣做，此命令會在下方，且網路名稱資源必須處於線上狀態。  此命令將會在 Active Directory 中重新建立名稱物件。

   ```PowerShell
   New-ClusterNameAccount -Name CLUSTERNAME -Domain NEWDOMAINNAME.com -UpgradeVCOs
   ```

    注意：如果您沒有任何其他具有網路名稱的群組（也就是僅含虛擬機器的 Hyper-v 叢集），則不需要-UpgradeVCOs 參數切換。

9. 使用 Active Directory 的使用者和電腦來檢查新的網域，並確定已建立相關聯的電腦物件。 如果有，則將群組中的其餘資源上線。

   ```PowerShell
   Start-ClusterGroup -Name "Cluster Group"

   Start-ClusterGroup -Name FS-CLUSCLUS
   ```

## <a name="known-issues"></a>已知問題

如果您使用的是新的 USB 見證功能，將無法將叢集新增到新的網域。  其原因是檔案共用見證類型必須使用 Kerberos 進行驗證。  在將叢集新增至網域之前，請將見證變更為 [無]。  完成後，重新建立 USB 見證。  您將看到的錯誤如下：

```
New-ClusternameAccount : Cluster name account cannot be created.  This cluster contains a file share witness with invalid permissions for a cluster of type AdministrativeAccesssPoint ActiveDirectoryAndDns. To proceed, delete the file share witness.  After this you can create the cluster name account and recreate the file share witness.  The new file share witness will be automatically created with valid permissions.
```

