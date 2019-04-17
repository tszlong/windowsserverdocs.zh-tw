---
ms.assetid: 56fc7f80-9558-467e-a6e9-a04c9abbee33
title: "錯誤網域感知"
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-failover-clustering
ms.topic: article
author: cosmosdarwin
ms.date: 09/16/2016
ms.openlocfilehash: f5c64bb8f8b7d4b8d13c76c4e94cfcf52ee32c30
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="fault-domain-awareness-in-windows-server-2016"></a>在 Windows Server 2016 錯誤網域感知

> 適用於： Windows Server 2016

容錯讓多部伺服器一同合作，以提供可用性 – 或放另一種方法，以提供節點容錯。 但今天的企業需要從他們的基礎結構以往更多可用性。 若要達到雲朵執行的時間，甚至不太項目，例如底座失敗，架或自然嚴重損壞必須針對保護。 這就是為何在 Windows Server 2016 容錯也引入底座、架和網站容錯。

錯誤網域和容錯是密切相關的概念。 錯誤網域是一組分享單點失敗的硬體元件。 若要將容錯某種程度，您需要在這個層級多個錯誤網域。 例如，將架容錯、您的伺服器和您的資料必須散發跨多個架。

這個簡短的影片將會顯示錯誤網域中的 Windows Server 2016 的概觀：  
[![按一下此映像來監看錯誤網域中的 Windows Server 2016 的概觀](media/Fault-Domains-in-Windows-Server-2016/Part-1-Fault-Domains-Overview.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-1-Overview)

## <a name="benefits"></a>優點
- **儲存空間，包括儲存空間直接存取、使用錯誤網域來最大化資料的安全。**  
    靈活度中儲存空間是概念分散式、軟體定義 RAID 等。 多個複本所有的資料會保持同步，而且如果硬體故障及一份會遺失，其他人的之後還原恢復。 若要達到最佳可能恢復，複本則會保留不同錯誤網域中。

- **[健康服務](health-service-overview.md)使用錯誤網域提供更有幫助警示。**  
    可以位置中繼資料，自動會連同任何後續警示相關聯的每個錯誤網域。 這些項可以協助作業或維護人員，並減少明示硬體錯誤。  

- **儲存空間相關性伸展叢集使用錯誤的網域。** 叢集上可遠方加入常見叢集伺服器。 為獲得最佳效能，應用程式或虛擬機器應該執行這些提供其儲存到附近的伺服器上。 錯誤網域感知能讓這個儲存相關性。   

## <a name="levels-of-fault-domains"></a>層級錯誤網域  
有四個錯誤網域-網站、架、底座和節點正式層級。 自動; 探索節點每個其他的層級是選擇性的。 例如，如果您的部署不會使用讓伺服器，底座層級不合理的您。  

![錯誤網域的不同層級的簡圖](media/Fault-Domains-in-Windows-Server-2016/levels-of-fault-domains.png)

## <a name="usage"></a>使用  
您可以使用標記 PowerShell 或 XML 指定錯誤的網域。 這兩種方法是相同，並提供完整的功能。

>[!IMPORTANT]
> 如果可能的話讓儲存空間直接存取、之前先指定錯誤網域。 這可讓底座或架容錯準備集區，層級，並設定靈活度和行計數，例如自動設定。 一旦建立集區與磁碟區，資料將不回溯的回應做的變更來移動錯誤網域拓撲。 若要切換節點底座或架讓儲存空間直接存取之後，您應該先收回] 節點，其使用集區的磁碟機`Remove-ClusterNode -CleanUpDisks`。

### <a name="defining-fault-domains-with-powershell"></a>透過 PowerShell 定義錯誤網域
Windows Server 2016 介紹錯誤網域使用下列 cmdlet:
* `Get-ClusterFaultDomain`
* `Set-ClusterFaultDomain`
* `New-ClusterFaultDomain` 
* `Remove-ClusterFaultDomain`

這個簡短的影片中示範這些 cmdlet] 的使用量。
[![按一下 [上叢集錯誤網域 cmdlet] 的使用量觀看這映像](media/Fault-Domains-in-Windows-Server-2016/Part-2-Using-PowerShell.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-2-Using-PowerShell)

使用`Get-ClusterFaultDomain`以查看目前網域拓撲錯誤。 這將會列出所有節點叢集，加上任何底座、架，或您所建立的網站。 您也可以篩選使用參數，例如**-類型**或**-名稱**，但這些不一定。

```PowerShell
Get-ClusterFaultDomain
Get-ClusterFaultDomain -Type Rack
Get-ClusterFaultDomain -Name "server01.contoso.com"
```

使用`New-ClusterFaultDomain`以建立新的底座、架或網站。 `-Type`與`-Name`是必要的參數。 預設值的`-Type`的`Chassis`，`Rack`，與`Site`。 `-Name`可以是任何字串。 (適用於`Node`輸入錯誤網域名稱必須實際節點名稱之下，將設定為自動)。

```PowerShell
New-ClusterFaultDomain -Type Chassis -Name "Chassis 007"
New-ClusterFaultDomain -Type Rack -Name "Rack A"
New-ClusterFaultDomain -Type Site -Name "Shanghai"
```

> [!IMPORTANT]  
> Windows Server 無法並不會驗證的任何您所建立的錯誤網域對應至真的世界中的任何項目。 （聽明顯，但請務必以了解）。兩個實體的世界中，您的節點都在一個架，如果，然後建立`-Type Rack`錯誤網域中的軟體不神奇地提供架容錯。 您必須負責確保您使用下列 cmdlet 所建立的拓撲符合實際排列您的硬體。

使用`Set-ClusterFaultDomain`到另一個移動一個錯誤網域。 條款「家長「和」的子女」常用描述此巢關係。 `-Name`與`-Parent`是必要的參數。 在`-Name`，提供移動; 錯誤網域名稱在`-Parent`，提供目的地的名稱。 若要將多個錯誤網域一次，會列出他們的名稱。

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent "Rack A"
Set-ClusterFaultDomain -Name "Rack A", "Rack B", "Rack C", "Rack D" -Parent "Shanghai"
```

> [!IMPORTANT]  
> 錯誤網域移動，當子女移動它們。 在上面範例中，如果架 A server01.contoso.com 父系，後者不另行購買需要移至上海網站 – 已經有問題，就像是實體世界父一定。

您可以看到父子關係輸出中的`Get-ClusterFaultDomain`，請在`ParentName`和`ChildrenNames`欄。

您也可以使用`Set-ClusterFaultDomain`修改其他的某些屬性，錯誤網域。 例如，您可以提供選擇性`-Location`或`-Description`的任何錯誤網域中繼資料。 如果提供，這項資訊會納入硬體警示從健康服務。 您也可以重新命名錯誤網域使用`-NewName`的參數。 不要重新命名`Node`輸入錯誤的網域。

```PowerShell
Set-ClusterFaultDomain -Name "Rack A" -Location "Building 34, Room 4010"
Set-ClusterFaultDomain -Type Node -Description "Contoso XYZ Server"
Set-ClusterFaultDomain -Name "Shanghai" -NewName "China Region"
```

使用`Remove-ClusterFaultDomain`移除底座、架或您所建立的網站。 `-Name`是必要的參數。 您無法移除錯誤網域包含子女 – 第一次，移除子女，或將它們移使用外部`Set-ClusterFaultDomain`。 若要將以外的所有其他錯誤網域錯誤網域，將其`-Parent`字串空白 (」」)。 您無法移除`Node`輸入錯誤的網域。 若要一次移除多個錯誤網域，清單他們的名稱。

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent ""
Remove-ClusterFaultDomain -Name "Rack A"
```

### <a name="defining-fault-domains-with-xml-markup"></a>定義錯誤網域 XML 標記
可以使用 XML 式就迫不及待語法指定錯誤網域。 我們建議使用最愛的文字編輯器中，例如 Visual Studio 程式碼 (使用免費*[在此](https://code.visualstudio.com/)*) 或「記事本」，若要建立的 XML 文件，您可以儲存及重複使用。  

這個簡短的影片示範指定錯誤網域 XML 標記] 的使用量。

[![C按一下此映像來監看簡短的影片，如何使用 XML 指定錯誤網域](media/Fault-Domains-in-Windows-Server-2016/Part-3-Using-XML-Markup.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-3-Using-XML)

在 [PowerShell，執行下列 cmdlet: `Get-ClusterFaultDomainXML`。 這會傳回為 XML 叢集，目前錯誤網域規格。 這會反映每個發現`<Node>`、包裝開頭和結尾在`<Topology>`標籤。  

執行此輸出儲存到檔案。  

```PowerShell
Get-ClusterFaultDomainXML | Out-File <Path>  
```

打開檔案，並新增`<Site>`，`<Rack>`，以及`<Chassis>`標籤來指定這些節點如何分散的網站、架，以及底座到。 每個標籤必須由唯一**名稱**。 用於節點，您必須在填入預設為保持節點的名稱。  

> [!IMPORTANT]  
> 所有其他標籤選擇性時，它們也必須遵守轉移網站&gt;架&gt;底座&gt;] 節點階層、，必須能正常關閉。  
除了名稱手`Location="..."`與`Description="..."`描述新增到任何標記。  

#### <a name="example-two-sites-one-rack-each"></a>範例：兩個網站一個架  

```XML
<Topology>  
  <Site Name="SEA" Location="Contoso HQ, 123 Example St, Room 4010, Seattle">  
    <Rack Name="A01" Location="Aisle A, Rack 01">  
      <Node Name="Server01" Location="Rack Unit 33" />  
      <Node Name="Server02" Location="Rack Unit 35" />  
      <Node Name="Server03" Location="Rack Unit 37" />  
    </Rack>  
  </Site>  
  <Site Name="NYC" Location="Regional Datacenter, 456 Example Ave, New York City">  
    <Rack Name="B07" Location="Aisle B, Rack 07">  
      <Node Name="Server04" Location="Rack Unit 20" />  
      <Node Name="Server05" Location="Rack Unit 22" />  
      <Node Name="Server06" Location="Rack Unit 24" />  
    </Rack>  
  </Site>  
</Topology> 
``` 

#### <a name="example-two-chassis-blade-servers"></a>範例：兩個底座，讓伺服器  
```XML
<Topology>  
  <Rack Name="A01" Location="Contoso HQ, Room 4010, Aisle A, Rack 01">  
    <Chassis Name="Chassis01" Location="Rack Unit 2 (Upper)" >  
      <Node Name="Server01" Location="Left" />  
      <Node Name="Server02" Location="Right" />  
    </Chassis>  
    <Chassis Name="Chassis02" Location="Rack Unit 6 (Lower)" >  
      <Node Name="Server03" Location="Left" />  
      <Node Name="Server04" Location="Right" />  
    </Chassis>  
  </Rack>  
</Topology>  
```

若要設定新的網域錯誤規格，儲存您的 XML 並執行 PowerShell 中。  

```PowerShell
$xml = Get-Content <Path> | Out-String  
Set-ClusterFaultDomainXML -XML $xml
```

本指南將只兩個範例，但`<Site>`，`<Rack>`，`<Chassis>`，與`<Node>`混合標記，許多其他方式，以反映部署的實體拓撲符合有可能會有任何。 我們希望這些範例所示這些標記的目的彈性與手位置描述明確他們的值。  

### <a name="optional-location-and-description-metadata"></a>選項：位置和描述中繼資料

您可以提供選擇性**位置**或**描述**的任何錯誤網域中繼資料。 如果提供，這項資訊會納入硬體警示從健康服務。 這個簡短的影片示範新增這類描述的值。

[![C上看到簡短的影片示範位置描述加入網域錯誤的值](media/Fault-Domains-in-Windows-Server-2016/part-4-location-description.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-4-Location-Description)

## <a name="see-also"></a>也了  
-   [Windows Server 2016](../get-started/windows-server-2016.md)  
-   [Windows Server 2016 中的儲存空間直接存取](../storage/storage-spaces/storage-spaces-direct-overview.md) 
