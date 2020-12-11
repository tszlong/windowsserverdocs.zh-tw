---
description: 深入瞭解：容錯網域感知
ms.assetid: 56fc7f80-9558-467e-a6e9-a04c9abbee33
title: 容錯網域感知
ms.author: cosdar
manager: eldenc
ms.topic: article
author: cosmosdarwin
ms.date: 09/16/2016
ms.openlocfilehash: 5f0c3df62ec7385a3402a8b71b8f50d4a7b359a2
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97048656"
---
# <a name="fault-domain-awareness"></a>容錯網域感知

> 適用于： Windows Server 2019 和 Windows Server 2016

「容錯移轉叢集」可讓多部伺服器共同合作以提供高可用性，或透過另一種方式來提供節點容錯功能。 但是現今的企業需要從其基礎結構獲得更高的可用性。 為了達到類似雲端的存留時間，即使是不太可能發生的項目 (例如，底座故障、機架中斷或天然災害) 都必須受到保護。 這就是為什麼 Windows Server 2016 中的容錯移轉叢集也引進了底座、機架和站出容錯功能。

## <a name="fault-domain-awareness"></a>容錯網域感知

容錯網域和容錯功能是密切相關的概念。 容錯網域是一組共用單一失敗點的硬體元件。 若要容錯到特定層級，您在該層級上需要有多個容錯網域。 例如，若要進行機架容錯，您的伺服器和資料必須散佈於多個機架上。

這段短片概要說明 Windows Server 2016 中的容錯網域： [ ![ 按一下此映射以查看 windows server 2016 中的容錯網域總覽](media/Fault-Domains-in-Windows-Server-2016/Part-1-Fault-Domains-Overview.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-1-Overview)

### <a name="fault-domain-awareness-in-windows-server-2019"></a>Windows Server 2019 中的容錯網域感知

容錯網域感知功能可在 Windows Server 2019 中使用，但預設為停用，且必須透過 Windows 登錄啟用。

若要在 Windows Server 2019 中啟用容錯網域感知，請移至 Windows 登錄並設定 (的) 。AutoAssignNodeSite 登錄機碼設為1。

```Registry
    (Get-Cluster).AutoAssignNodeSite=1
```

若要在 Windows 2019 中停用容錯網域感知，請移至 Windows 登錄並設定 (的) 。AutoAssignNodeSite 登錄機碼設為0。

```Registry
    (Get-Cluster).AutoAssignNodeSite=0
```

## <a name="benefits"></a>優點
- **「儲存空間」(包括「儲存空間直接存取」) 會使用容錯網域充分保護資料安全。**
    「儲存空間」中的復原能力在概念上類似分散式的軟體定義 RAID。 所有資料的多個複本會保持同步，而且如果硬體故障且遺失了某一個複本，則會重新複製其他複本以還原復原能力。 若要達到最佳可行的復原能力，應該在不同的容錯網域中保留複本。

- **[健全狀況服務](health-service-overview.md)使用容錯網域來提供更有用的警示。**
    每個容錯網域可以關聯至位置中繼資料，它將會自動包含於任何後續的警示中。 這些描述項可以協助操作或維護人員，藉由釐清硬體來減少錯誤。

- **Stretch 叢集使用容錯網域進行儲存體親和性。** 延展式叢集可讓遠方伺服器加入常用的叢集。 為獲得最佳效能，應用程式或虛擬機器應該在鄰近可提供其儲存空間之伺服器的伺服器上執行。 容錯網域感知可啟用此儲存體親和性。

## <a name="levels-of-fault-domains"></a>容錯網域層級
容錯網域有四個標準層級 - 站台、機架、底座和節點。 系統會自動探索節點，每個額外層級都是選擇性的。 例如，如果您的部署未使用刀鋒伺服器，則底座層級對您而言可能毫無意義。

![不同層級的容錯網域圖表](media/Fault-Domains-in-Windows-Server-2016/levels-of-fault-domains.png)

## <a name="usage"></a>使用方式
您可以使用 PowerShell 或 XML 標記來指定容錯網域。 這兩種方法是一樣的，而且都可以提供完整功能。

>[!IMPORTANT]
> 盡可能在啟用「儲存空間直接存取」之前指定容錯網域。 這讓自動設定能夠備妥集區、階層及設定 (例如復原能力和欄數)，來進行底座或機架容錯。 一旦建立集區和磁碟區之後，資料將不會回溯移動，以回應容錯網域拓撲的變更。 若要在啟用「儲存空間直接存取」之後，於底座或機架之間移動節點，您應該先使用 `Remove-ClusterNode -CleanUpDisks` 將節點及其磁碟機從集區撤出。

### <a name="defining-fault-domains-with-powershell"></a>使用 PowerShell 定義容錯網域
Windows Server 2016 引進了下列 Cmdlet 來處理容錯網域：
* `Get-ClusterFaultDomain`
* `Set-ClusterFaultDomain`
* `New-ClusterFaultDomain`
* `Remove-ClusterFaultDomain`

這段短片示範這些 Cmdlet 的用法。
[![按一下此影像以觀看有關使用叢集容錯網域 Cmdlet 的短片](media/Fault-Domains-in-Windows-Server-2016/Part-2-Using-PowerShell.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-2-Using-PowerShell)

使用 `Get-ClusterFaultDomain` 以查看目前的容錯網域拓撲。 這將列出叢集中的所有節點，外加您所建立的任何底座、機架或站台。 您可以使用像是 **-Type** 或 **-Name** 等參數來篩選，不過這些並非必要參數。

```PowerShell
Get-ClusterFaultDomain
Get-ClusterFaultDomain -Type Rack
Get-ClusterFaultDomain -Name "server01.contoso.com"
```

用 `New-ClusterFaultDomain` 來建立新的底座、機架或網站。 `-Type` 和 `-Name` 是必要參數。 的可能值為 `-Type` `Chassis` 、 `Rack` 和 `Site` 。 `-Name`可以是任何字串。 針對 `Node` 類型容錯網域 (，名稱必須是實際的節點名稱，如自動設定) 。

```PowerShell
New-ClusterFaultDomain -Type Chassis -Name "Chassis 007"
New-ClusterFaultDomain -Type Rack -Name "Rack A"
New-ClusterFaultDomain -Type Site -Name "Shanghai"
```

> [!IMPORTANT]
> Windows Server 無法執行，也不會驗證您所建立的任何容錯網域是否對應至真實的實體世界中的任何一個。  (這可能會很明顯，但請務必瞭解。 ) 如果在實體世界中，您的節點全都在一個機架中，那麼在軟體中建立兩個 `-Type Rack` 容錯網域並不會神奇地提供機架容錯能力。 您必須負責確保使用這些 Cmdlet 所建立的拓撲符合硬體的實際排列方式。

使用將 `Set-ClusterFaultDomain` 一個容錯網域移至另一個。 詞彙「父項」和「子項」常用來描述這個巢狀關聯性。 `-Name` 和 `-Parent` 是必要參數。 在中 `-Name` ，提供正在移動之容錯網域的名稱; 在中 `-Parent` 提供目的地的名稱。 若要一次移動多個容錯網域，請列出其名稱。

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent "Rack A"
Set-ClusterFaultDomain -Name "Rack A", "Rack B", "Rack C", "Rack D" -Parent "Shanghai"
```

> [!IMPORTANT]
> 當容錯網域移動時，其子項也會跟著移動。 在上述範例中，如果機架 A 是 server01.contoso.com 的父項，則不需要將後者個別移至 Shanghai 站台，因為它的父項已位於該處，所以它也已經位於該處，就像在真實世界一樣。

在 `Get-ClusterFaultDomain` 和資料行中，您可以在的輸出中看到父子式關聯性 `ParentName` `ChildrenNames` 。

您也可以使用 `Set-ClusterFaultDomain` 來修改容錯網域的某些其他屬性。 例如，您可以提供 `-Location` `-Description` 任何容錯網域的選擇性或中繼資料。 如已提供，此資訊將會納入來自「健康情況服務」的硬體警示中。 您也可以使用參數重新命名容錯網域 `-NewName` 。 請勿重新命名 `Node` 類型容錯網域。

```PowerShell
Set-ClusterFaultDomain -Name "Rack A" -Location "Building 34, Room 4010"
Set-ClusterFaultDomain -Type Node -Description "Contoso XYZ Server"
Set-ClusterFaultDomain -Name "Shanghai" -NewName "China Region"
```

用 `Remove-ClusterFaultDomain` 來移除您已建立的底座、機架或網站。 `-Name` 是必要參數。 您無法移除包含子系的容錯網域–首先，移除子系，或使用以外的外部移動 `Set-ClusterFaultDomain` 。 若要在其他所有容錯網域以外移動容錯網域，請將其設 `-Parent` 為空字串 ( "" ) 。 您無法移除 `Node` 類型容錯網域。 若要一次移除多個容錯網域，請列出其名稱。

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent ""
Remove-ClusterFaultDomain -Name "Rack A"
```

### <a name="defining-fault-domains-with-xml-markup"></a>以 XML 標記定義容錯網域
容錯網域可以使用 XML 語法來指定。 我們建議您使用慣用的文字編輯器，例如 [Visual Studio Code] (可在 *[這裡](https://code.visualstudio.com/)* 免費取得) 或 [記事本]，來建立可儲存並重複使用的 XML 文件。

這段短片示範使用 XML 標記來指定容錯網域的用法。

[![按一下此影像以觀看短片，瞭解如何使用 XML 來指定容錯網域](media/Fault-Domains-in-Windows-Server-2016/Part-3-Using-XML-Markup.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-3-Using-XML)

在 PowerShell 中，執行下列 Cmdlet： `Get-ClusterFaultDomainXML` 。 這會以 XML 形式傳回叢集目前的容錯網域規格。 這會反映每個探索到的 `<Node>` 開頭和結尾 `<Topology>` 標記。

執行下列命令，將此輸出儲存至檔案。

```PowerShell
Get-ClusterFaultDomainXML | Out-File <Path>
```

開啟檔案，並新增 `<Site>` 、 `<Rack>` 和標記， `<Chassis>` 以指定如何在網站、機架和底座間散發這些節點。 每個標記都必須以唯一的 **Name** 來識別。 對於節點，您必須保留預設填入的節點名稱。

> [!IMPORTANT]
> 儘管所有其他標記都是選擇性的，但它們還是必須遵守可轉移的站台 &gt; 機架 &gt; 底座 &gt; 節點階層，而且必須正確地關閉。
除了名稱之外，您還 `Location="..."` 可以將自由格式和 `Description="..."` 描述項加入至任何標記。

#### <a name="example-two-sites-one-rack-each"></a>範例︰兩個站台，每個都有一個機架

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

#### <a name="example-two-chassis-blade-servers"></a>範例︰兩個底座，刀鋒伺服器
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

若要設定新的容錯網域規格，請儲存您的 XML，並在 PowerShell 中執行下列程式。

```PowerShell
$xml = Get-Content <Path> | Out-String
Set-ClusterFaultDomainXML -XML $xml
```

本指南只會提供兩個範例， `<Site>` 但 `<Rack>` `<Chassis>` 可以透過 `<Node>` 許多其他方式來混合、、和標記，以反映部署的實體拓撲（可能是什麼）。 我們希望這些範例說明這些標記的彈性及自由格式位置描述項的值，以釐清它們。

### <a name="optional-location-and-description-metadata"></a>選用：位置和描述中繼資料

您可以為任何容錯網域提供選擇性的 **位置** 或 **描述** 中繼資料。 如已提供，此資訊將會納入來自「健康情況服務」的硬體警示中。 這段簡短的影片示範新增這類描述項的價值。

[![按一下以查看簡短的影片，示範將位置描述項新增至容錯網域的價值](media/Fault-Domains-in-Windows-Server-2016/part-4-location-description.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-4-Location-Description)

## <a name="see-also"></a>另請參閱
- [開始使用 Windows Server 2019](../get-started-19/get-started-19.md)
- [Windows Server 2016 入門](../get-started/server-basics.md)
-   [儲存空間直接存取概觀](../storage/storage-spaces/storage-spaces-direct-overview.md)
