---
title: 在獨立伺服器上部署儲存空間
description: 說明如何在獨立的 Windows Server 2012 為主伺服器上部署儲存空間。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-spaces
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 40265e767ac9aca05386c0893def259aca3a5633
ms.sourcegitcommit: e73fbe1046a8bd2bf4f24ccffc11465ad8dfab1d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2019
ms.locfileid: "8992531"
---
# 在獨立伺服器上部署儲存空間

>適用於： Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

本主題說明如何在獨立伺服器上部署儲存空間。 如需如何建立叢集的儲存空間的資訊，請參閱 <<c0>部署 Windows Server 2012 R2 上的儲存空間叢集。

若要建立儲存空間，您必須先建立一或多個儲存集區。 儲存集區是實體磁碟的集合。 儲存體彙總、 撤離容量擴充和委派的系統，可讓儲存集區。

從儲存集區中，您可以建立一或多個虛擬磁碟。 這些虛擬磁碟也稱為 「*儲存空間*」。 儲存空間會出現 Windows 作業系統，您可以建立為一般磁碟格式化磁碟區。 當您建立虛擬磁碟透過檔案和存放服務的使用者介面時，您可以設定復原類型 (簡單、 鏡像或同位)，佈建類型 （薄或固定） 和大小。 透過 Windows PowerShell 中，您可以設定額外的參數，例如資料行、 間隔值，以及哪些實體磁碟使用集區中的數目。 這些額外的參數的相關資訊，請參閱[新 VirtualDisk](https://docs.microsoft.com/powershell/module/storage/new-virtualdisk?view=win10-ps)及[什麼是資料行，並儲存空間決定如何使用多少？](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx%23what_are_columns_and_how_does_storage_spaces_decide_how_many_to_use)在儲存空間常見問題集 (FAQ)。

>[!NOTE]
>您無法使用儲存空間來裝載 Windows 作業系統。

從虛擬磁碟，您可以建立一或多個磁碟區。 當您建立磁碟區時，您可以設定大小、 磁碟機代號或資料夾、 檔案系統 （NTFS 檔案系統或復原檔案系統 (ReFS)）、 配置單位大小，以及選用的磁碟區標籤。

下圖顯示 「 儲存空間 」 工作流程。

![儲存空間工作流程](media/deploy-standalone-storage-spaces/storage-spaces-workflow.png)

**圖 1： 儲存空間工作流程**

>[!NOTE]
>本主題包含您可用來自動化某些所述的程序的範例 Windows PowerShell cmdlet。 如需詳細資訊，請參閱[PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-6)。

## 必要條件

若要使用獨立的 Windows Server 2012−based 伺服器上的儲存空間，請確定您想要使用的實體磁碟符合下列先決條件。

> [!IMPORTANT]
> 如果您想要了解如何在容錯移轉叢集上部署儲存空間，請參閱 <<c0>部署 Windows Server 2012 R2 上的儲存空間叢集。 部署容錯移轉叢集會有不同的必要條件，例如支援的磁碟匯流排類型、 支援的復原類型，以及必要的最小的磁碟數目。

|區域|需求|備註|
|---|---|---|
|磁碟匯流排類型|-序列連結 SCSI (SAS)<br>-序列進階技術附件 (SATA)<br>-iSCSI 和光纖通道控制器。 |您也可以使用 USB 磁碟機。 不過，並非最佳伺服器環境中使用 USB 磁碟機。<br>儲存空間支援 iSCSI 和光纖通道 (FC) 控制器，只要是非具彈性 （簡單使用任何數目的欄） 的虛擬磁碟建立上方。<br>|
|磁碟設定|-實體磁碟都必須是至少 4 GB<br>-磁碟必須空白且未格式化。 無法建立磁碟區。||
|HBA 考量|-建議使用簡單主機匯流排介面卡 (Hba) 不支援 RAID 功能<br>-如果 RAID nics，Hba 必須在非 RAID 模式已停用所有 RAID 功能<br>-介面卡不必須抽象的實體磁碟，快取資料，或遮蓋到任何附加的裝置。 這包括機箱服務所提供的附加只是-a-堆--磁碟 (JBOD) 裝置。 |儲存空間是只與 Hba 相容，您可以完全停用所有 RAID 功能。|
|JBOD 機箱|-JBOD 機箱都是選擇性<br>建議使用儲存空間認證機箱列在 Windows Server 類別目錄<br>-如果您使用 JBOD 機箱，確認與您的廠商存放裝置機箱支援儲存空間，以確保完整的功能<br>-若要判斷 JBOD 機箱是否支援機箱和插槽識別，執行下列 Windows PowerShell cmdlet:<br><br>`Get-PhysicalDisk \| ? {$_.BusType –eq "SAS"} \| fc`<br><br>如果**EnclosureNumber**和**SlotNumber**欄位包含值，機箱支援這些功能。||

若要規劃實體磁碟和獨立伺服器部署所需的復原類型的數目，使用下列指導方針。

|復原類型|磁碟需求|使用時機|
|---|---|---|
|**簡單**<br><br>-條紋實體磁碟上的資料<br>-將磁碟容量最大化，提高輸送量<br>-（不會從磁碟失敗保護） 無復原功能<br><br><br><br><br><br><br>|要求至少需要一個實體磁碟。|請勿使用主機無可取代的資料。 簡單的空間不保護對磁碟故障。<br><br>使用主機或輕鬆地重新建立暫時資料，以折扣價格。<br><br>適用於高效能的工作負載復原功能並非必要或所提供的應用程式的位置。|
|**鏡像**<br><br>-會儲存資料的兩個或三個複本的所有實體磁碟之間<br>-會增加可靠性，但可降低容量。 每次寫入會發生重複資料刪除。 鏡像空間也條狀方式資料配置跨多個實體磁碟機。<br>-更大的資料輸送量和延遲更低，存取同位<br>-使用髒地區追蹤 (DRT) 來追蹤集區中的磁碟的修改。 當系統繼續執行從意外的關機，空間上線時，DRT，使磁碟中集區與彼此一致。|需要至少兩個實體磁碟，來保護，防止單一磁碟失敗的情況。<br><br>需要至少具備 5 部實體磁碟，來保護，防止兩個同時發生的磁碟故障。|使用於大部分的部署。 例如，鏡像空間適合用在一般用途的檔案共用或虛擬硬碟 (VHD) 程式庫。|
|**同位**<br><br>-條紋實體磁碟上的資料和同位資訊<br>可提高可靠性時，它相較於簡單的空間，但有些降低容量<br>-增加透過日誌復原。 這有助於防止資料損毀，如果發生意外的關機。|需要至少三個實體磁碟，來保護，防止單一磁碟失敗的情況。|使用於是高度循序，例如封存或備份的工作負載。|

## 步驟 1： 建立儲存集區

您必須使用第一個實體磁碟群組至一或多個儲存集區。

1. 在伺服器管理員瀏覽窗格中，選取 [**檔案和存放服務**。

2. 在瀏覽窗格中，選取 [**儲存集區**\] 頁面。
    
    根據預設，可用的磁碟包含名為*primordial*集區的集區中。 如果沒有 primordial 集區列在**儲存集區**，這表示存放裝置不符合適用於儲存空間需求。 請確定磁碟符合必要條件一節所述的需求。
    
    >[!TIP]
    >如果您選取**Primordial**儲存集區，在 [**實體磁碟**列出可用的實體磁碟。

3. 在**儲存集區**，選取**工作**清單，然後選取**新的儲存集區**。 新的儲存集區精靈將會開啟。

4. **開始之前，先**在頁面上，選取 [**下一步**。

5. 在**指定的儲存集區的名稱和子系統**頁面上，輸入名稱和選用描述儲存集區，選取您想要使用的可用實體磁碟的群組，然後選取**下一步**。

6. **選取儲存集區的實體磁碟**在頁面上，執行下列]，然後選取**下一步**：
    
    1. 選取您想要包含在儲存集區中每個實體磁碟旁邊的核取方塊。
    
    2. 如果您想要指定一個或更多磁碟做為經常存取的備用，**配置**，底下選取下拉式箭號，然後選取**熱備用**。

7. 在**確認選取項目**頁面上，確認設定正確無誤，然後選取 [**建立**。

8. 在**檢視結果]** 頁面上，確認所有工作完成，然後選取 [**關閉**]。
    
    >[!NOTE]
    >（選擇性） 若要直接繼續下一個步驟，您可以選取 [**建立虛擬磁碟此精靈關閉時**核取方塊。

9. 在**儲存集區**，請確認會列出新的儲存集區。

### Windows PowerShell 建立儲存集區的對等命令

下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 單行，即使它們可能會顯示以下幾行跨 word 包裝因為格式限制式。

下列範例顯示 primordial 集區中是可用的實體磁碟。

```PowerShell
Get-StoragePool -IsPrimordial $true | Get-PhysicalDisk | Where-Object CanPool -eq $True
```

下列範例會建立新的儲存集區名為*StoragePool1*使用所有可用的磁碟。

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk –CanPool $True)
```

下列範例會建立新的儲存集區， *StoragePool1*，使用四個可用的磁碟。

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk PhysicalDisk1, PhysicalDisk2, PhysicalDisk3, PhysicalDisk4)
```

下列範例順序的 cmdlet 會示範如何將可用的實體磁碟*PhysicalDisk5*為經常存取的備用新增至儲存集區*StoragePool1*。

```PowerShell
$PDToAdd = Get-PhysicalDisk –FriendlyName PhysicalDisk5
Add-PhysicalDisk –StoragePoolFriendlyName StoragePool1 –PhysicalDisks $PDToAdd –Usage HotSpare
```

## 步驟 2： 建立虛擬磁碟

接下來，您必須建立一或多個虛擬磁碟從儲存集區。 當您建立虛擬磁碟時，您可以選取如何跨實體磁碟配置資料。 這會影響效能和可靠性。 您也可以選擇是否要建立精簡或固定-佈建的磁碟。

1. 如果尚未在伺服器管理員中，**儲存集區**頁面上，開啟新的虛擬硬碟精靈在**儲存集區**，請確定已選取所需的儲存集區。

2. 在下**虛擬磁碟**，選取**工作**清單，然後選取 [**新增虛擬硬碟**。 新的虛擬硬碟精靈將會開啟。

3. **開始之前，先**在頁面上，選取 [**下一步**。

4. 在**選取儲存集區**的頁面上，選取您想要的儲存集區，，然後選取**下一步**。

5. 在**指定的虛擬磁碟名稱]** 頁面上，輸入的名稱和選用的描述，然後選取 [**下一步**]。

6. 在**選取的儲存體版面配置**的頁面上，選取所需的配置，然後選取 [**下一步**。
    
    >[!NOTE]
    >如果您選取的版面配置，其中您沒有足夠的實體磁碟，您會收到錯誤訊息，當您選取 [**下一步**。 如需使用及磁碟需求哪種配置資訊，請參閱[先決條件](#prerequisites)）。

7. 如果您選取**鏡像**儲存的版面配置，而且您有五個或多個磁碟集區中，會顯示 [**設定復原設定**\] 頁面。 選取其中一個下列選項：
    
      - **雙向鏡像**
      - **三向鏡像**

8. 在**指定的佈建的類型**頁面上，選取其中一個下列選項，然後選取 [**下一步**。
    
      - **精簡型**
        
        使用精簡佈建，做為所需的基礎上會配置空間。 這可最佳化可用的存放裝置的使用方式。 不過，因為這可讓您過度配置存放區，您必須謹慎監視有多少磁碟空間。
    
      - **固定**
        
        使用固定佈建的儲存容量被配置，立即在建立虛擬磁碟的時間。 因此，從儲存集區的虛擬磁碟大小固定佈建的使用空間。
    
    >[!TIP]
    >您可以使用儲存空間，來建立相同的儲存集區這兩個精簡和固定-佈建的虛擬磁碟。 例如，您可以使用精簡佈建的虛擬磁碟來裝載資料庫和固定的佈建虛擬磁碟來裝載相關聯的記錄檔。

9. 在**指定的虛擬磁碟大小**頁面上，執行下列動作：
    
    如果您選取精簡佈建在上一個步驟中，**虛擬磁碟大小**] 方塊中輸入虛擬磁碟大小、 選取單位 （**MB**、 **GB**或**TB**），然後選取 [**下一步**。
    
    如果您選取固定佈建在上一個步驟中，選取下列其中一項：
    
      - **指定大小**
        
        若要指定大小，在**虛擬磁碟大小**] 方塊中輸入的值，然後選取 [（**MB**、 **GB**或**TB**） 的單位。
        
        如果您使用簡單的以外的儲存體版面配置，虛擬磁碟使用更多的可用空間，比您指定的大小。 若要避免潛在錯誤，其中的磁碟區大小超過儲存集區的可用空間，您可以選取**最大的虛擬磁碟可能高達指定的大小建立**核取方塊。
    
      - **大小上限**
        
        選取此選項來建立虛擬磁碟使用儲存集區的最大容量。

10. 在**確認選取項目**頁面上，確認設定正確無誤，然後選取 [**建立**。

11. 在**檢視結果]** 頁面上，確認所有工作完成，然後選取 [**關閉**]。
    
    >[!TIP]
    >根據預設，會選取**的磁碟區，此精靈關閉時建立**核取方塊。 這會讓您直接前往下一步。

### Windows PowerShell 建立虛擬磁碟的對等命令

下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 單行，即使它們可能會顯示以下幾行跨 word 包裝因為格式限制式。

下列範例會建立名為*VirtualDisk1*上儲存集區名為*StoragePool1*50 GB 虛擬磁碟。

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB)
```

下列範例會建立名為*VirtualDisk1*上儲存集區名為*StoragePool1*鏡像虛擬磁碟。 磁碟使用儲存集區的最大儲存空間容量。

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –ResiliencySettingName Mirror –UseMaximumSize
```

下列範例會建立名為*VirtualDisk1*上儲存集區名為*StoragePool1*50 GB 虛擬磁碟。 磁碟使用精簡佈建的類型。

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB) –ProvisioningType Thin
```

下列範例會建立名為*VirtualDisk1*名為*StoragePool1*儲存集區上的虛擬磁碟。 虛擬磁碟使用三向鏡像和是 20 GB 的固定的大小。

>[!NOTE]
>您必須擁有至少具備 5 部實體磁碟儲存集區，此 cmdlet，才能運作。 （這不包含任何磁碟配置為經常存取的備用。）

```PowerShell
New-VirtualDisk -StoragePoolFriendlyName StoragePool1 -FriendlyName VirtualDisk1 -ResiliencySettingName Mirror -NumberOfDataCopies 3 -Size 20GB -ProvisioningType Fixed
```

## 步驟 3： 建立磁碟區

接下來，您必須建立磁碟區從虛擬磁碟。 您可以指派選用的磁碟機代號或資料夾，然後格式化檔案系統的磁碟區。

1. 如果新的磁碟區精靈不是已開啟，**儲存集區**在伺服器管理員中，在**虛擬磁碟**] 頁面上所需的虛擬磁碟，以滑鼠右鍵按一下，然後選取 [**新的磁碟區**。
    
    新的磁碟區精靈便會開啟。

2. **開始之前，先**在頁面上，選取 [**下一步**。

3. 在**選取的伺服器和磁碟**] 頁面上，執行下列]，然後選取**下一步**。
    
    1. 在**伺服器**區域中，選取您要佈建磁碟區的伺服器。
    
    2. 在**磁碟**區中，選取您要建立磁碟區的虛擬磁碟。

4. 在**指定的磁碟區的大小**頁面上，輸入磁碟區的大小，指定單位 （**MB**、 **GB**或**TB**），，然後選取 [**下一步**。

5. 在**指派給磁碟機代號或資料夾**的頁面上，設定想要的選項，然後選取 [**下一步**。

6. 在**選取的檔案系統設定**頁面上，執行下列]，然後選取**下一步**。
    
    1. 在**檔案系統**清單中，選取 [ **NTFS**或**ReFS**。
    
    2. 在**配置單位大小**清單中，\ [保留在**預設**的設定，或設定的配置單位大小。
        
        >[!NOTE]
        >如需有關配置單位大小的詳細資訊，請參閱[預設 NTFS、 FAT 和 exFAT 叢集大小](https://support.microsoft.com/help/140365/default-cluster-size-for-ntfs-fat-and-exfat)。

    
    3. 或者，在 [**磁碟區標籤**中，輸入磁碟區標籤名稱，例如**HR 資料**。

7. 在**確認選取項目**頁面上，確認設定正確無誤，然後選取 [**建立**。

8. 在**檢視結果]** 頁面上，確認所有工作完成，然後選取 [**關閉**]。

9. 若要確認磁碟區已建立，在伺服器管理員中，選取 [**磁碟區**\] 頁面。 磁碟區列在其建立所在的伺服器。 您也可以確認磁碟區是在 Windows 檔案總管] 中。

### Windows PowerShell 建立磁碟區的對等命令

下列 Windows PowerShell cmdlet 會執行相同的函式做為先前的程序。 在單一列上，輸入下列命令。

下列範例會初始化針對虛擬磁碟*VirtualDisk1*磁碟、 建立磁碟分割使用受指派磁碟機代號，並再格式預設 NTFS 檔案系統的磁碟區。

```PowerShell
Get-VirtualDisk –FriendlyName VirtualDisk1 | Get-Disk | Initialize-Disk –Passthru | New-Partition –AssignDriveLetter –UseMaximumSize | Format-Volume
```

## 其他資訊

- [儲存空間](overview.md)
- [Windows PowerShell 中的儲存體 Cmdlet](https://docs.microsoft.com/powershell/module/storage/index?view=win10-ps)
- [部署叢集的儲存空間](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11))
- [儲存空間常見問題集 (FAQ)](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx)
