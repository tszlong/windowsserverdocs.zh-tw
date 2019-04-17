---
title: "Windows Server 2016 中 health 服務"
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 5bc71e71-920e-454f-8195-afebd2a23725
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: 834fcfb749e89e4768dce3f229564feea550a432
ms.sourcegitcommit: 30fcae929ce7b611f5d3a5f8fee64b0299272110
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/15/2017
---
# <a name="health-service-in-windows-server-2016"></a>Windows Server 2016 中 health 服務
> 適用於 Windows Server 2016

健康服務是 Windows Server 2016，可改善您的日常監視和操作叢集執行儲存空間直接存取體驗中的新功能。

## <a name="prerequisites"></a>必要條件  

健康服務是預設儲存空間直接存取的功能。 任何其他動作，不才能設定或 [開始] 它。 若要深入了解儲存空間直接存取，請查看[儲存空間直接存取 Windows Server 2016 在](../storage/storage-spaces/storage-spaces-direct-overview.md)。  

## <a name="reports"></a>報告

查看[健康服務報告](health-service-reports.md)。

## <a name="faults"></a>錯誤

查看[健康服務錯誤](health-service-faults.md)。

## <a name="actions"></a>控制項目

查看[健康服務執行](health-service-actions.md)。

## <a name="automation"></a>自動化  

本節工作流程自動化在磁碟階段 Health 服務的。  

### <a name="disk-lifecycle"></a>磁碟週期   

健康服務會自動執行大部分的實體磁碟週期一樣。 讓我們顯示的初始狀態部署，是完美健康-也就是顯示所有實體磁碟正常運作。  

#### <a name="retirement"></a>淘汰  

他們可以不使用，對應的錯誤引發時，自動的淘汰實體磁碟。 有幾個案例：  

-   媒體錯誤： 實體磁碟肯定失敗或損壞，而且取代。  

-   遺失通訊： 實體磁碟已遺失連接連續的超過 15 分鐘。  

-   無法回應時應： 實體磁碟已出現超過 5.0 秒三個或更多時間在小時的時間的延遲。  

>[!NOTE]
> 如果連接一次，會以多個實體磁碟遺失或 Health 服務會在整個節點或儲存圍繞，*未*淘汰這些磁碟因為有太可能是問題。  

如果已停用的磁碟已為多個實體的磁碟的快取提供服務，這些將會自動將指派到另一個快取磁碟如果有的話。 特殊使用者不不需要任何動作。  

#### <a name="restoring-resiliency"></a>還原恢復  

一旦已停用所在的磁碟，Health 服務會立即開始複製到剩餘實體磁碟還原完整恢復及其資料。 這已完成後，資料完全安全，並重新錯誤容錯。  

>[!NOTE]
> 此立即還原需要剩餘實體磁碟之間不足可用的容量。  

#### <a name="blinking-the-indicator-light"></a>閃爍指示燈  

如果可能，請 Health 服務會開始閃爍指示燈已淘汰的磁碟或其位置。 這會繼續不斷，直到已淘汰的磁碟會取代。  

>[!NOTE]
> 有時候，磁碟可能會失敗，無法運作-使甚至其指示燈的方式，例如全部遺失電源。  

#### <a name="physical-replacement"></a>實體更換  

您應該會取代時可能已停用實體磁碟。 最常，這組成熱-交換-也就是 關閉節點或儲存圍繞插電就不需要的。 出現錯誤的很有幫助位置和隨附的資訊。  

#### <a name="verification"></a>驗證

插入更換磁碟之後，它會確認針對支援的元件文件 （查看下一節）。

#### <a name="pooling"></a>共用  

如果您允許，在更換磁碟自動會取代輸入使用其前置的集區。 此時，系統會返回完美的健康狀態的初始狀態，然後錯誤消失。  

## <a name="supported-components-document"></a>支援的元件文件  

健康服務提供執法機制限制的提供系統管理員或方案廠商支援元件文件上使用的儲存空間直接存取的元件。 這可以避免使用蓄意不支援的硬體您或其他的瑕疵擔保或支援的合約 compliance 可以幫助。 此功能目前僅適用於實體磁碟裝置，包括 Ssd，HDDs，且 NVMe 磁碟機。 支援元件文件可以限制模型、 製造商 （選擇性），以及韌體版本 （選擇性）。

### <a name="usage"></a>使用  

支援元件文件使用 XML 式就迫不及待語法。 我們建議使用最愛的文字編輯器中，例如 Visual Studio 程式碼 (使用免費[在此](http://code.visualstudio.com/)) 或 「 記事本 」，若要建立的 XML 文件，您可以儲存及重複使用。

#### <a name="sections"></a>區段

文件具有兩個獨立的章節：**磁碟**和**快取**。

如果**磁碟**區段會提供、 列出的磁碟機才能加入集區。 任何未列出的磁碟機將無法從加入集區的有效使其使用正式作業。 如果空白的此一節，會允許任何磁碟機加入集區。

如果**快取**區段會提供、 列出的磁碟機用於快取。 如果空白的此一節，儲存空間直接存取會嘗試猜測根據媒體類型及匯流排類型。 例如，如果您的部署使用固態磁碟機 (SSD) 和硬碟機 (HDD)，前者會自動選擇快取。不過，如果您的部署使用所有-flash，您必須指定您要使用的快取以下的高耐力裝置。

>[!IMPORTANT]
> 已經集區的磁碟機，以及使用中支援元件文件不回溯適用。  

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
        <BinaryPath>\\path\to\image.bin</BinaryPath>
      </TargetFirmware>
    </Disk>
  </Disks>

  <Cache>
    <Disk>
      <Manufacturer>Fabrikam</Manufacturer>
      <Model>QRSTUV</Model>
    </Disk>
  </Cache>

</Components>

```

若要列出多個磁碟機，只要新增其他**&lt;磁碟&gt;**中任一個區段的標籤。

若要將此 XML 部署儲存空間直接存取時，使用**XML**旗標：

```PowerShell
Enable-ClusterS2D -XML <MyXML>
```

若要設定或變更支援元件文件儲存空間直接存取部署後 （亦即之後健康服務已執行），請使用下列 PowerShell cmdlet:

```PowerShell
$MyXML = Get-Content <\\path\to\file.xml> | Out-String  
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.SupportedComponents.Document" -Value $MyXML  
```

>[!NOTE]
>型號、 製造商和韌體版本屬性應該完全符合您取得使用的值**取得-平均**cmdlet。 這可能會與您的 「 通用據用量感知器 」 期望，根據您的供應商實作有所不同。 例如 「 Contoso 」，除了製造商可能會 「 CONTOSO LTD，「 或它可能會是空白的模型 」 Contoso XZY9000 」 時。  

您可以使用下列 PowerShell cmdlet，確認：  

```PowerShell
Get-PhysicalDisk | Select Model, Manufacturer, FirmwareVersion  
```

## <a name="settings"></a>設定

查看[健康服務設定](health-service-settings.md)。

## <a name="see-also"></a>也了

- [健康服務報告](health-service-reports.md)
- [健康服務錯誤](health-service-faults.md)
- [健康服務動作](health-service-actions.md)
- [健康服務設定](health-service-settings.md)
- [Windows Server 2016 中的儲存空間直接存取](../storage/storage-spaces/storage-spaces-direct-overview.md)
