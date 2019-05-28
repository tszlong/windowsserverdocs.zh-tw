---
title: 在獨立伺服器上部署儲存空間
description: 描述如何在獨立的 Windows Server 2012 的伺服器上部署儲存空間。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-spaces
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 7090657a0936aed0f4b2e79007f69d7b082b0b8f
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2019
ms.locfileid: "63750642"
---
# <a name="deploy-storage-spaces-on-a-stand-alone-server"></a>在獨立伺服器上部署儲存空間

>適用於：Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

本主題描述如何在獨立伺服器上部署儲存空間。 如需有關如何建立叢集的儲存空間的資訊，請參閱[部署 Windows Server 2012 R2 上的儲存空間叢集](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>)。

若要建立儲存空間，您必須先建立一或多個儲存集區。 儲存集區是實體磁碟的集合。 儲存集區可啟用儲存彙總、彈性容量擴充，以及委派的系統管理。

您可以從儲存集區建立一或多個虛擬磁碟。 這些虛擬磁碟也稱為「儲存空間」  。 儲存空間會對 Windows 作業系統顯示為一般磁碟，您可以從這個磁碟建立格式化磁碟區。 透過「檔案和存放服務」使用者介面建立虛擬磁碟時，您可以設定復原類型 (簡單、鏡像或同位)、佈建類型 (精簡或固定) 及大小。 透過 Windows PowerShell，您可以設定其他參數，例如欄位數目、間隔值，以及要使用集區中的哪一個實體磁碟。 如需有關這些額外參數的資訊，請參閱 [New-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/new-virtualdisk?view=win10-ps)，以及儲存空間常見問題集 (FAQ) 中的[什麼是資料行及儲存空間如何決定要使用多少資料行？](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx%23what_are_columns_and_how_does_storage_spaces_decide_how_many_to_use)。

>[!NOTE]
>您無法使用儲存空間來裝載 Windows 作業系統。

您可以從虛擬磁碟建立一或多個磁碟區。 當您建立磁碟區時，您可以設定大小、 磁碟機代號或資料夾、 檔案系統 （NTFS 檔案系統或復原檔案系統 (ReFS)），配置單位大小，以及選用的磁碟區標籤。

下圖說明「儲存空間」工作流程。

![儲存空間工作流程](media/deploy-standalone-storage-spaces/storage-spaces-workflow.png)

**圖 1:儲存空間工作流程**

>[!NOTE]
>本主題包含可讓您用來將部分所述的程序自動化的 Windows PowerShell Cmdlet 範例。 如需詳細資訊，請參閱 < [PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-6)。

## <a name="prerequisites"></a>必要條件

若要使用獨立的 Windows Server 2012−based 伺服器上的儲存空間，請確定您想要使用的實體磁碟符合下列必要條件。

> [!IMPORTANT]
> 如果您想要了解如何在容錯移轉叢集上部署儲存空間，請參閱[部署 Windows Server 2012 R2 上的儲存空間叢集](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>)。 容錯移轉叢集部署都有不同的必要條件，例如支援的磁碟匯流排類型、 支援的復原類型，以及所需的最小數目的磁碟。

|區域|需求|附註|
|---|---|---|
|磁碟匯流排類型|序列連結 SCSI (SAS)<br>-Serial Advanced Technology Attachment (SATA)<br>-iSCSI 與光纖通道 」 控制器。 |您也可以使用 USB 磁碟機。 不過，最好是不在伺服器環境中使用 USB 磁碟機。<br>儲存空間支援 iSCSI 與光纖通道 (FC) 控制站，只要在其上建立的虛擬磁碟是不具有恢復功能 （簡單與任意數目的資料行）。<br>|
|磁碟設定|-實體磁碟必須是至少 4 GB<br>磁碟必須為空白且未格式化。 請勿建立磁碟區。||
|HBA 考量|-簡單的主機匯流排介面卡 (Hba) 不支援 RAID 功能的建議<br>-如果 RAID 功能，Hba 必須處於非 RAID 模式停用所有 RAID 功能的<br>-介面卡不得抽象實體磁碟，也就是快取資料，或遮蔽任何連接的裝置。 這包括由連結的集束磁碟 (just-a-bunch-of-disks，JBOD) 裝置所提供的內含服務。 |「儲存空間」是只與您可以完全停用所有 RAID 功能的 HBA 相容。|
|JBOD 機箱|-是選擇性的 JBOD 機箱<br>建議使用儲存空間認證 Windows Server Catalog 上所列的機箱<br>-如果您使用 JBOD 機箱，向存放裝置廠商確認機箱支援儲存空間，以確保完整的功能<br>-若要判斷 JBOD 機箱是否支援機箱和插槽識別，請執行下列 Windows PowerShell cmdlet:<br><br>`Get-PhysicalDisk \| ? {$_.BusType –eq "SAS"} \| fc`<br><br>如果**EnclosureNumber**並**SlotNumber**欄位包含值，然後在機箱支援這些功能。||

若要規劃獨立伺服器部署所需的實體磁碟數目和復原類型，請使用下列指導方針。

|復原類型|磁碟需求|使用時機|
|---|---|---|
|**Simple**<br><br>層將資料等量分散在實體磁碟<br>為達到最大磁碟容量，並且會增加輸送量<br>-（不會保護磁碟故障） 沒有復原<br><br><br><br><br><br><br>|至少需要一個實體磁碟。|請勿用來裝載無法取代的資料。 簡單空間集無法防止磁碟故障。<br><br>用來裝載暫時或容易重新建立的資料以降低成本。<br><br>適用於高效能工作負載，則不需要復原能力，或所提供的應用程式。|
|**鏡像**<br><br>-兩個或三份資料儲存在集合的實體磁碟<br>增加可靠性，但會降低容量。 每次寫入時都進行複製。 鏡像空間也會將資料等量分散在多個實體磁碟上。<br>-更大的資料輸送量和較低的存取延遲，比同位檢查<br>-使用已變更追蹤 （DRT） 來追蹤對集區中磁碟的區域。 當系統從非計劃中的關機繼續而空間重新上線時，DRT 會讓集區中的磁碟彼此一致。|至少需要兩個實體磁碟，才能在單一磁碟故障時提供防護。<br><br>至少需要五個實體磁碟，才能在兩個磁碟同時故障時提供防護。|用於大多數部署。 例如，鏡像空間適用於一般用途的檔案共用或虛擬硬碟 (VHD) 程式庫。|
|**同位檢查**<br><br>-在實體磁碟等量分散資料及同位資訊<br>-增加可靠性，當它相較於簡單空間，但稍微降低容量<br>-透過日誌記錄增加恢復功能。 這有助於在發生非計劃中的關機時防止資料損毀。|至少需要三個實體磁碟，才能在單一磁碟故障時提供防護。|用於高度循序的工作負載 (例如封存或備份)。|

## <a name="step-1-create-a-storage-pool"></a>步驟 1：建立儲存集區

您必須先將可用的實體磁碟組成一或多個儲存集區。

1. 在 [伺服器管理員瀏覽] 窗格中，選取**檔案和存放服務**。

2. 在 [導覽] 窗格中，選取**儲存體集區**頁面。
    
    根據預設，可用的磁碟會包含在名為「原始」  集區的集區中。 如果 [儲存集區]  底下沒有列出任何原始集區，即表示存放裝置不符合「儲存空間」的需求。 請確定磁碟符合＜先決條件＞一節所述的需求。
    
    >[!TIP]
    >如果您選取 [原始]  儲存集區，則可用的實體磁碟會列在 [實體磁碟]  底下。

3. 底下**儲存體集區**，選取**工作**清單，然後再選取**新增儲存集區**。 新的儲存體集區精靈隨即開啟。

4. 在 [**在您開始之前**頁面上，選取**下一步]** 。

5. 在 [**指定儲存集區名稱和子系統**頁面上，輸入名稱和儲存集區的選擇性描述，選取您想要使用此項目，然後選取的可用實體磁碟群**下一步]** 。

6. 在上**選取 [儲存集區的實體磁碟**頁面上，執行下列作業，並選取**下一步]** :
    
    1. 選取要包含在儲存集區中之每個實體磁碟旁的核取方塊。
    
    2. 如果您想要將一或多個磁碟指定為熱備援，底下**配置**，選取下拉式箭號，然後選取**熱備援**。

7. 在 **確認選取項目**頁面上，確認設定都正確無誤，然後選取**建立**。

8. 在 **檢視結果**頁面上，確認所有工作完成，然後選取**關閉**。
    
    >[!NOTE]
    >(選擇性) 若要直接繼續下一個步驟，您可以選取 [當此精靈關閉時建立虛擬磁碟]  核取方塊。

9. 在 [儲存集區]  底下，確認已列出新的儲存集區。

### <a name="windows-powershell-equivalent-commands-for-creating-storage-pools"></a>Windows PowerShell 對應的命令，來建立儲存集區

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

下列範例示範原始集區中有哪些實體磁碟可用。

```PowerShell
Get-StoragePool -IsPrimordial $true | Get-PhysicalDisk | Where-Object CanPool -eq $True
```

下列範例會建立名為新的儲存體集區*StoragePool1*使用所有可用的磁碟。

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk –CanPool $True)
```

下列範例會建立新的儲存體集區中， *StoragePool1*，使用四個可用的磁碟。

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk PhysicalDisk1, PhysicalDisk2, PhysicalDisk3, PhysicalDisk4)
```

下列範例 Cmdlet 序列示範如何新增可用的實體磁碟 *PhysicalDisk5* 做為儲存集區 *StoragePool1* 的熱備援。

```PowerShell
$PDToAdd = Get-PhysicalDisk –FriendlyName PhysicalDisk5
Add-PhysicalDisk –StoragePoolFriendlyName StoragePool1 –PhysicalDisks $PDToAdd –Usage HotSpare
```

## <a name="step-2-create-a-virtual-disk"></a>步驟 2：建立虛擬磁碟

接著，您必須從儲存集區建立一或多個虛擬磁碟。 建立虛擬磁碟時，您可以選取將資料配置在各個實體磁碟上的方式。 這會同時影響可靠性和效能。 您也可以選取要建立精簡佈建或固定佈建的磁碟。

1. 如果 [新增虛擬磁碟精靈] 尚未開啟，請在 [伺服器管理員] 的 [儲存集區]  頁面中，確定在 [儲存集區]  底下已選取想要的儲存集區。

2. 底下**虛擬磁碟**，選取**工作**清單，然後再選取**新的虛擬磁碟**。 新的虛擬磁碟精靈 會開啟。

3. 在 [**在您開始之前**頁面上，選取**下一步]** 。

4. 在 [**選取的儲存體集區**頁面上，選取所需的儲存體集區，然後選取**下一步]** 。

5. 在 [**指定虛擬磁碟名稱**頁面上，輸入名稱和選擇性描述，然後選取**下一步]** 。

6. 在 [**選取的儲存體配置**頁面上，選取所需的版面配置，然後選取**下一步]** 。
    
    >[!NOTE]
    >如果您選取的配置，其中您沒有足夠的實體磁碟，您就會收到一則錯誤訊息，當您選取**下一步** 。 如需使用和磁碟需求一個版面配置資訊，請參閱[必要條件](#prerequisites))。

7. 如果您選取**鏡像**因為儲存配置，而且您會在集區中，有五個或多個磁碟**設定的復原設定**頁面會隨即出現。 選取下列其中一個選項：
    
      - **雙向鏡像**
      - **三向鏡像**

8. 在 [**指定的佈建的型別**頁面上，選取其中一個下列的選項，然後選取**下一步]** 。
    
      - **精簡**
        
        使用精簡佈建時，會視需要配置空間。 這會將可用存放區的使用情況最佳化。 不過，由於這可讓您超額配置存放區，因此您必須小心監視有多少磁碟空間可用。
    
      - **固定**
        
        使用固定佈建時，會在建立虛擬磁碟時立即配置儲存容量。 因此，固定佈建會從儲存集區使用與虛擬磁碟大小相等的空間。
    
    >[!TIP]
    >使用「儲存空間」，您便可以將精簡佈建和固定佈建的虛擬磁碟建立在同一個儲存集區中。 例如，您可以使用精簡佈建的虛擬磁碟來裝載資料庫，使用固定佈建的虛擬磁碟來裝載關聯的記錄檔。

9. 在 [指定的虛擬磁碟的大小]  頁面上，執行下列動作：
    
    如果您選取了精簡佈建在上一個步驟中，在**虛擬磁碟大小**方塊中，輸入虛擬磁碟大小，選取單位 (**MB**， **GB**，或**TB**)，然後選取**下一步** 。
    
    如果您選取了固定佈建在上一個步驟中，選取下列其中一項：
    
      - **指定大小**
        
        若要指定大小，請輸入中的值**虛擬磁碟大小**，然後選取單位 (**MB**， **GB**，或**TB**)。
        
        如果您使用簡單以外的儲存配置，虛擬磁碟使用的可用空間將會超過您指定的大小。 若要避免發生磁碟區大小超過儲存集區可用空間的可能錯誤，您可以選取 [建立所能建立的最大虛擬磁碟，但不超過指定的大小]  核取方塊。
    
      - **最大容量**
        
        選取此選項以建立使用儲存集區最大容量的虛擬磁碟。

10. 在 **確認選取項目**頁面上，確認設定都正確無誤，然後選取**建立**。

11. 在 **檢視結果**頁面上，確認所有工作完成，然後選取**關閉**。
    
    >[!TIP]
    >[當此精靈關閉時建立磁碟區]  核取方塊預設為已選取狀態。 這會讓您直接進入下一個步驟。

### <a name="windows-powershell-equivalent-commands-for-creating-virtual-disks"></a>Windows PowerShell 對應的命令，建立虛擬磁碟

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

下列範例會建立名為 50 GB 虛擬磁碟*VirtualDisk1*存放集區上名為*StoragePool1*。

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB)
```

下列範例會建立名為之鏡像虛擬磁碟*VirtualDisk1*存放集區上名為*StoragePool1*。 磁碟會使用儲存體集區的最大儲存容量。

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –ResiliencySettingName Mirror –UseMaximumSize
```

下列範例會建立名為 50 GB 虛擬磁碟*VirtualDisk1*名為存放集區上*StoragePool1*。 這個磁碟使用精簡佈建類型。

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB) –ProvisioningType Thin
```

下列範例會建立名為的虛擬磁碟*VirtualDisk1*存放集區上名為*StoragePool1*。 這個虛擬磁碟使用三向鏡像，並且具有 20 GB 的固定大小。

>[!NOTE]
>儲存集區中必須至少有五個虛擬磁碟，這個 Cmdlet 才能運作。 (這不包括任何已配置為熱備援的磁碟。)

```PowerShell
New-VirtualDisk -StoragePoolFriendlyName StoragePool1 -FriendlyName VirtualDisk1 -ResiliencySettingName Mirror -NumberOfDataCopies 3 -Size 20GB -ProvisioningType Fixed
```

## <a name="step-3-create-a-volume"></a>步驟 3：建立磁碟區

接著，您必須從虛擬磁碟建立磁碟區。 您可以指派選用的磁碟機代號或資料夾，然後使用檔案系統磁碟區格式化。

1. 如果新的磁碟區精靈 尚未開啟，請在**儲存體集區**頁面中 伺服器管理員 中，於**虛擬磁碟**，以滑鼠右鍵按一下想要的虛擬磁碟，然後選取**新的磁碟區**.
    
    此時會開啟 [新增磁碟區精靈]。

2. 在 [**在您開始之前**頁面上，選取**下一步]** 。

3. 在 **選取 伺服器和磁碟**頁面上，執行下列作業，並選取**下一步** 。
    
    1. 在  **Server**區域中，選取您想要佈建磁碟區所在的伺服器。
    
    2. 在 **磁碟**區域中，選取您要建立磁碟區的虛擬磁碟。

4. 在上**指定的磁碟區大小**頁面上，輸入磁碟區大小，指定的單位 (**MB**， **GB**，或**TB**)，然後選取 [ **下一步]** 。

5. 在 [**指派成磁碟機代號或資料夾**頁面上，設定想要的選項，然後選取**下一步]** 。

6. 在 [**選取檔案系統設定**頁面上，執行下列作業，並選取**下一步]** 。
    
    1. 在 **檔案系統**清單中，選取**NTFS**或是**ReFS**。
    
    2. 在 [配置單位大小]  清單中，將設定保留為 [預設]  或設定配置單位大小。
        
        >[!NOTE]
        >如需有關配置單位大小的詳細資訊，請參閱 [NTFS、FAT 及 exFAT 的預設叢集大小](https://support.microsoft.com/help/140365/default-cluster-size-for-ntfs-fat-and-exfat)。

    
    3. (選擇性) 在 [磁碟區標籤]  方塊中，輸入磁碟區標籤名稱，例如「HR 資料」  。

7. 在 **確認選取項目**頁面上，確認設定都正確無誤，然後選取**建立**。

8. 在 **檢視結果**頁面上，確認所有工作完成，然後選取**關閉**。

9. 若要確認磁碟區已建立的在 [伺服器管理員] 中，選取**磁碟區**頁面。 磁碟區會列在其建立位置所屬的伺服器底下。 您也可以在 [Windows 檔案總管] 中確認該磁碟區。

### <a name="windows-powershell-equivalent-commands-for-creating-volumes"></a>Windows PowerShell 對應的命令，來建立磁碟區

下列 Windows PowerShell cmdlet 執行與上一個程序相同的功能。 請以單行輸入命令。

下列範例會將虛擬磁碟 *VirtualDisk1* 的磁碟初始化、以指派的磁碟機代號建立磁碟分割，然後以預設的 NTFS 檔案系統格式將磁碟區格式化。

```PowerShell
Get-VirtualDisk –FriendlyName VirtualDisk1 | Get-Disk | Initialize-Disk –Passthru | New-Partition –AssignDriveLetter –UseMaximumSize | Format-Volume
```

## <a name="additional-information"></a>其他資訊

- [儲存空間](overview.md)
- [Windows PowerShell 中的儲存體 Cmdlet](https://docs.microsoft.com/powershell/module/storage/index?view=win10-ps)
- [部署叢集的儲存空間](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11))
- [儲存空間常見問題集 (Faq)](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx)
