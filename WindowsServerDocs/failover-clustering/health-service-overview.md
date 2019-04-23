---
title: Windows Server 中的健全狀況服務
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 5bc71e71-920e-454f-8195-afebd2a23725
author: cosmosdarwin
ms.date: 02/09/2018
ms.openlocfilehash: 5afb64dcf0c59697ed55d7cf51ef1bc36e7e0e36
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863809"
---
# <a name="health-service-in-windows-server"></a>Windows Server 中的健全狀況服務

> 適用於 Windows Server 2016

健全狀況服務是中改善的日常監視的 Windows Server 2016 和操作經驗執行儲存空間直接存取叢集的新功能。

## <a name="prerequisites"></a>必要條件  

根據預設，「健全狀況服務」會隨「儲存空間直接存取」一起啟用。 不需要其他動作來設定或啟動它。 若要深入了解儲存空間直接存取，請參閱[儲存空間直接存取 Windows Server 2016 中](../storage/storage-spaces/storage-spaces-direct-overview.md)。  

## <a name="reports"></a>報告

請參閱[健全狀況服務報告指出](health-service-reports.md)。

## <a name="faults"></a>錯誤

請參閱[健全狀況服務錯誤](health-service-faults.md)。

## <a name="actions"></a>動作

請參閱[健全狀況服務動作](health-service-actions.md)。

## <a name="automation"></a>自動化  

本節說明「健全狀況服務」在磁碟生命週期內自動執行的工作流程。  

### <a name="disk-lifecycle"></a>磁碟生命週期   

「健全狀況服務」會自動執行實體磁碟生命週期的大部分階段。 假設您部署的初始階段健康情況都良好，也就表示說所有實體磁碟都正常運作。  

#### <a name="retirement"></a>淘汰  

當實體磁碟已無法再使用時，系統便會將它們淘汰，並且會引發相對應的「錯誤」。 有幾種情況：  

-   媒體故障：實體磁碟確實已故障或損壞，因此必須更換。  

-   遺失通訊：實體磁碟已經超過 15 分鐘失去連線。  

-   沒有回應：實體磁碟在一小時內發生三次以上超過 5.0 秒的延遲。  

>[!NOTE]
> 如果同時失去對多個實體磁碟 (或整個節點或存放裝置機箱) 的連線，健全狀況服務「不會」淘汰這些磁碟，因為它們是根本問題的可能性較低。  

如果被淘汰的磁碟曾作為其他多個實體磁碟的快取，且有其他可用的快取磁碟，系統將會自動重新指派一個給它們。 使用者不需要採取特別的動作。  

#### <a name="restoring-resiliency"></a>還原復原能力  

實體磁碟一旦被淘汰，「健全狀況服務」會立即開始將其資料複製到其餘的實體磁碟，以還原完整復原能力。 一旦完成，資料便完全安全，容錯也重新開始。  

>[!NOTE]
> 此立即還原需要其餘的實體磁碟有足夠的可用空間。  

#### <a name="blinking-the-indicator-light"></a>讓指示燈閃爍  

可能的話，「健全狀況服務」會讓已淘汰的實體磁碟或其插槽上的指示燈開始閃爍。 指示燈會無限期持續閃爍，直到更換淘汰的磁碟。  

>[!NOTE]
> 某些情況下，磁碟故障的方式可能使其指示燈也無法運作 - 例如，完全失去電源。  

#### <a name="physical-replacement"></a>實體磁碟更換  

若情況允許，您應該更換淘汰的實體磁碟。 大多數情況下，這包含熱交換-也就關閉節點或存放裝置機箱則不需要。 請參閱「錯誤」以取得很有用的位置和組件資訊。  

#### <a name="verification"></a>驗證

插入取代用磁碟時，它將會驗證對支援元件文件 （請參閱下一節）。

#### <a name="pooling"></a>加入集區  

若情況允許，取代用磁碟會自動取代到其前身的集區中以開始使用。 此時，系統會回到其良好健康情況的初始狀態，然後「錯誤」會消失。  

## <a name="supported-components-document"></a>支援的元件的文件  

健全狀況服務會提供一種強化機制來限制所儲存空間直接存取使用的系統管理員或解決方案廠商所提供的支援元件文件的元件。 這可以防止您或其他人誤用不支援的硬體，這有助於符合保固或支援合約的規定。 這項功能僅限於目前實體磁碟裝置，包括 Ssd、 Hdd，，和 NVMe 磁碟機。 支援元件文件可以限制型號、 製造商 （選擇性） 和韌體版本 （選擇性）。

### <a name="usage"></a>使用量  

支援元件文件會使用 XML 的語法。 我們建議使用慣用的文字編輯器，例如安裝免費[Visual Studio Code](http://code.visualstudio.com/)或 Notepad，來建立您可以儲存並重複使用的 XML 文件。

#### <a name="sections"></a>區段

文件有兩個獨立的區段：`Disks`和`Cache`。

如果`Disks`一節會提供列出的磁碟機 (為`Disk`) 才能加入集區。 任何未列出的磁碟機將無法加入集區，其有效避免它們被用於生產環境。 如果此區段為空白，您就會允許任何磁碟機加入集區。

如果`Cache`一節會提供列出的磁碟機 (為`CacheDisk`) 用於快取。 如果此區段為空白，儲存空間直接存取嘗試[猜數字為基礎的媒體類型和匯流排類型](../storage/storage-spaces/understand-the-cache.md#cache-drives-are-selected-automatically)。 此處所列的磁碟機應該也會列在`Disks`。

>[!IMPORTANT]
> 支援元件文件不會溯及已經加入集區的磁碟機和使用中。  

#### <a name="example"></a>範例

```XML
<Components>

  <Disks>
    <Disk>
      <Manufacturer>Contoso</Manufacturer>
      <Model>XYZ9000</Model>
      <AllowedFirmware>
        <Version>2.0</Version>
        <Version>2.1</Version>
        <Version>2.2</Version>
      </AllowedFirmware>
      <TargetFirmware>
        <Version>2.1</Version>
        <BinaryPath>C:\ClusterStorage\path\to\image.bin</BinaryPath>
      </TargetFirmware>
    </Disk>
    <Disk>
      <Manufacturer>Fabrikam</Manufacturer>
      <Model>QRSTUV</Model>
    </Disk>
  </Disks>

  <Cache>
    <CacheDisk>
      <Manufacturer>Fabrikam</Manufacturer>
      <Model>QRSTUV</Model>
    </CacheDisk>
  </Cache>

</Components>

```

若要列出多個磁碟機，只要另外加`<Disk>`或`<CacheDisk>`標記。

若要部署儲存空間直接存取時，請將這段 XML，使用`-XML`參數：

```PowerShell
$MyXML = Get-Content <Filepath> | Out-String  
Enable-ClusterS2D -XML $MyXML
```

若要設定或修改部署儲存空間直接存取之後的支援元件文件：

```PowerShell
$MyXML = Get-Content <Filepath> | Out-String  
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.SupportedComponents.Document" -Value $MyXML  
```

>[!NOTE]
>型號、製造商，和韌體版本屬性應與您使用 **Get-PhysicalDisk** Cmdlet 所取得的值完全相符。 視您廠商的實作而定，這可能會與您一般預期的有所不同。 例如，製造商可能是 "CONTOSO-LTD"，而，不是 "Contoso"，或者當型號是 "Contoso-XZY9000" 時，製造商可能會是空白。  

您可以使用以下 PowerShell Cmdlet 來驗證：  

```PowerShell
Get-PhysicalDisk | Select Model, Manufacturer, FirmwareVersion  
```

## <a name="settings"></a>設定

請參閱[健全狀況服務設定](health-service-settings.md)。

## <a name="see-also"></a>另請參閱

- [健全狀況服務報告](health-service-reports.md)
- [健全狀況服務錯誤](health-service-faults.md)
- [健全狀況服務動作](health-service-actions.md)
- [健全狀況服務設定](health-service-settings.md)
- [Windows Server 2016 中的儲存空間直接存取](../storage/storage-spaces/storage-spaces-direct-overview.md)
