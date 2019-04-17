---
title: "健康服務錯誤"
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: c9e1fb4568ee93739c49ccc1a13106b09161c5f3
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="health-service-faults"></a>健康服務錯誤
> 適用於 Windows Server 2016

## <a name="what-are-faults"></a>有哪些錯誤？

健康服務持續監視儲存空間直接存取叢集偵測到問題並產生」錯誤」。 一個新 cmdlet 會顯示目前錯誤，可讓您輕鬆地不想在每個實體驗證您的部署的健康狀態，或依序功能。 錯誤專為精確、 輕鬆地了解，且可執行動作。  

每個錯誤包含五個重要欄位：  

-   嚴重性
-   問題描述
-   建議的下一個步驟地問題
-   錯誤實體辨識資訊
-   （如果有的話），其所在的位置

例如，以下是一般錯誤：  

```
Severity: MINOR                                         
Reason: Connectivity has been lost to the physical disk.                           
Recommendation: Check that the physical disk is working and properly connected.    
Part: Manufacturer Contoso, Model XYZ9000, Serial 123456789                        
Location: Seattle DC, Rack B07, Node 4, Slot 11
```

 >[!NOTE]
 > 從您的錯誤網域設定衍生所在的位置。 如需網域錯誤，請查看[在 Windows Server 2016 錯誤網域](fault-domains.md)。 如果您不提供這項資訊，會較很有幫助的 [位置] 欄位-，例如它可能只顯示卡插槽的數字。  

## <a name="root-cause-analysis"></a>根本原因分析

健康服務可以存取之間發生錯誤實體以找出並結合錯誤相同的基本問題的結果，這可能的原因。 來辨識鏈結的影響，如此較頻繁報告。 例如，伺服器當機，如果預期比任何伺服器的磁碟機而不需要連接也會。 因此，只有一個錯誤都會引發的根本原因-在本案例中伺服器。  

## <a name="usage-in-powershell"></a>使用中的 PowerShell

若要查看目前在 PowerShell 錯誤，請執行下列 cmdlet:

```PowerShell
Get-StorageSubSystem Cluster* | Debug-StorageSubSystem  
```

這會影響儲存空間直接存取叢集整體的任何錯誤。 最常，這些錯誤與硬體或設定。 如果不有任何錯誤，此 cmdlet 會傳回執行任何動作。  

>[!NOTE]
> 在非 production 環境中，並自行承擔，您可以嘗試使用此功能觸發錯誤自己-，例如移除一個所在的磁碟或關機一個節點。 出現錯誤之後, 重新插入所在的磁碟或重新開機] 節點和錯誤會消失再試一次。

您也可以檢視的影響，只有特定磁碟區或使用下列 cmdlet 檔案共用的錯誤：  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Debug-Volume  

Get-FileShare -Name <Name> | Debug-FileShare  
```

這會影響只有在特定音量或檔案共用的錯誤。 最常，這些錯誤與容量規劃、資料恢復功能或功能，例如儲存服務品質或儲存複本。 

## <a name="usage-in-net-and-c"></a>使用.NET 和 C#

### <a name="connect"></a>連接

為了查詢健康服務，您將需要建立**CimSession**與叢集。 若要這樣做，您將需要幾個步驟，將只提供完整.NET 中，這表示您無法輕易地執行此動作直接從 web 或行動裝置應用程式。 這些程式碼範例將會使用 C\ #，此資料的存取層最簡單的選擇。

``` 
...
using System.Security;
using Microsoft.Management.Infrastructure;

public CimSession Connect(string Domain = "...", string Computer = "...", string Username = "...", string Password = "...")
{
    SecureString PasswordSecureString = new SecureString();
    foreach (char c in Password)
    {
        PasswordSecureString.AppendChar(c);
    }

    CimCredential Credentials = new CimCredential(
        PasswordAuthenticationMechanism.Default, Domain, Username, PasswordSecureString);
    WSManSessionOptions SessionOptions = new WSManSessionOptions();
    SessionOptions.AddDestinationCredentials(Credentials);
    Session = CimSession.Create(Computer, SessionOptions);
    return Session;
}
```

提供的使用者名稱應該的目標電腦本機系統管理員。

我們建議您建構密碼**SecureString**直接中的使用者輸入的即時，讓他們的密碼永遠不會儲存在記憶體中明文。 這有助於減少各種不同的安全性問題。 但實際上，它建構上述常見的原型用途。

### <a name="discover-objects"></a>探索物件

使用**CimSession**建立，您可以查詢 Windows 管理檢測 (WMI) 叢集上。

您可以取得錯誤或計量之前，您必須將數個相關的物件的執行個體。 第一次，**MSFT\_StorageSubSystem**，表示叢集上儲存空間直接存取。 使用的您可以取得每個**MSFT\_StorageNode**中叢集，與每個**MSFT\_Volume**，資料磁碟區。 最後，您將必須**MSFT\_StorageHealth**，健康太服務本身。

```
CimInstance Cluster;
List<CimInstance> Nodes;
List<CimInstance> Volumes;
CimInstance HealthService;

public void DiscoverObjects(CimSession Session)
{
    // Get MSFT_StorageSubSystem for Storage Spaces Direct
    Cluster = Session.QueryInstances(@"root\microsoft\windows\storage", "WQL", "SELECT * FROM MSFT_StorageSubSystem")
        .First(Instance => (Instance.CimInstanceProperties["FriendlyName"].Value.ToString()).Contains("Cluster"));

    // Get MSFT_StorageNode for each cluster node
    Nodes = Session.EnumerateAssociatedInstances(Cluster.CimSystemProperties.Namespace,
        Cluster, "MSFT_StorageSubSystemToStorageNode", null, "StorageSubSystem", "StorageNode").ToList();

    // Get MSFT_Volumes for each data volume
    Volumes = Session.EnumerateAssociatedInstances(Cluster.CimSystemProperties.Namespace,
        Cluster, "MSFT_StorageSubSystemToVolume", null, "StorageSubSystem", "Volume").ToList();

    // Get MSFT_StorageHealth itself
    HealthService = Session.EnumerateAssociatedInstances(Cluster.CimSystemProperties.Namespace,
        Cluster, "MSFT_StorageSubSystemToStorageHealth", null, "StorageSubSystem", "StorageHealth").First();
}
```

這些是您收到 PowerShell 中使用像是 cmdlet 相同的物件**取得-StorageSubSystem**，**取得-StorageNode**，並**取得磁碟區**。

您具有相同可以存取屬性，記載在[儲存管理 API 類別](https://msdn.microsoft.com/en-us/library/windows/desktop/hh830612(v=vs.85).aspx)。

```
...
using System.Diagnostics;

foreach (CimInstance Node in Nodes)
{
    // For illustration, write each node's Name to the console. You could also write State (up/down), or anything else!
    Debug.WriteLine("Discovered Node " + Node.CimInstanceProperties["Name"].Value.ToString());
}
```

### <a name="query-faults"></a>查詢錯誤

叫用**診斷**將目前的範圍目標錯誤**CimInstance**，這是叢集或任何磁碟區。

下述是可在 Windows Server 2016 中的每個範圍錯誤的完整清單。

```       
public void GetFaults(CimSession Session, CimInstance Target)
{
    // Set Parameters (None)
    CimMethodParametersCollection FaultsParams = new CimMethodParametersCollection();
    // Invoke API
    CimMethodResult Result = Session.InvokeMethod(Target, "Diagnose", FaultsParams);
    IEnumerable<CimInstance> DiagnoseResults = (IEnumerable<CimInstance>)Result.OutParameters["DiagnoseResults"].Value;
    // Unpack
    if (DiagnoseResults != null)
    {
        foreach (CimInstance DiagnoseResult in DiagnoseResults)
        {
            // TODO: Whatever you want!
        }
    }
}
```

### <a name="optional-myfault-class"></a>選項：MyFault 課

這可能會讓您建立和保存您自己的代表項錯誤。 例如，此**MyFault**課程儲存錯誤，包括重要的數個屬性**FaultId**，可以使用稍後之間的關聯更新或移除通知，或 deduplicate，是相同的錯誤偵測到有任何原因多個時間。

```       
public class MyFault {
    public String FaultId { get; set; }
    public String Reason { get; set; }
    public String Severity { get; set; }
    public String Description { get; set; }
    public String Location { get; set; }

    // Constructor
    public MyFault(CimInstance DiagnoseResult)
    {
        CimKeyedCollection<CimProperty> Properties = DiagnoseResult.CimInstanceProperties;
        FaultId     = Properties["FaultId"                  ].Value.ToString();
        Reason      = Properties["Reason"                   ].Value.ToString();
        Severity    = Properties["PerceivedSeverity"        ].Value.ToString();
        Description = Properties["FaultingObjectDescription"].Value.ToString();
        Location    = Properties["FaultingObjectLocation"   ].Value.ToString();
    }
}
```

```
List<MyFault> Faults = new List<MyFault>;

foreach (CimInstance DiagnoseResult in DiagnoseResults)
{
    Faults.Add(new Fault(DiagnoseResult));
}
```

在每個錯誤屬性的完整清單 (**DiagnoseResult**) 下述。

### <a name="fault-events"></a>錯誤事件

當您建立、移除或更新錯誤時，Health 服務會產生 WMI 事件。 這些是基本保持不常用輪詢，同步您的應用程式的狀態，可協助如判斷時傳送電子郵件通知，例如。 若要希望這些事件，此程式碼範例再試一次使用觀察者設計模式。

首先，希望**MSFT\_StorageFaultEvent**事件。

```      
public void ListenForFaultEvents()
{
    IObservable<CimSubscriptionResult> Events = Session.SubscribeAsync(
        @"root\microsoft\windows\storage", "WQL", "SELECT * FROM MSFT_StorageFaultEvent");
    // Subscribe the Observer
    FaultsObserver<CimSubscriptionResult> Observer = new FaultsObserver<CimSubscriptionResult>(this);
    IDisposable Disposeable = Events.Subscribe(Observer);
}   
```

接下來，實作觀察者其**OnNext()**當新的事件也會叫用方法。

每個事件包含**變更型別**表示是否錯誤已建立、移除或更新，並相關**FaultId**。

此外，它們會包含錯誤本身的所有屬性。

```
class FaultsObserver : IObserver
{
    public void OnNext(T Event)
    {
        // Cast
        CimSubscriptionResult SubscriptionResult = Event as CimSubscriptionResult;

        if (SubscriptionResult != null)
        {
            // Unpack            
            CimKeyedCollection<CimProperty> Properties = SubscriptionResult.Instance.CimInstanceProperties;
            String ChangeType = Properties["ChangeType"].Value.ToString();
            String FaultId = Properties["FaultId"].Value.ToString();

            // Create
            if (ChangeType == "0")
            {
                Fault MyNewFault = new MyFault(SubscriptionResult.Instance);
                // TODO: Whatever you want!
            }
            // Remove
            if (ChangeType == "1")
            {
                // TODO: Use FaultId to find and delete whatever representation you have...
            }
            // Update
            if (ChangeType == "2")
            {
                // TODO: Use FaultId to find and modify whatever representation you have...
            }
        }
    }
    public void OnError(Exception e)
    {
        // Handle Exceptions
    }
    public void OnCompleted()
    {
        // Nothing
    }
}
```

### <a name="understand-fault-lifecycle"></a>了解錯誤週期

錯誤不打算會標示為「出現「或解析使用者。 建立和時 Health 服務會遵守問題，會自動移除才不會再健康服務觀察到的問題。 一般而言，這會反映已修正該問題。

不過，有時候，錯誤可能被 rediscovered 健康服務（例如後移轉後，或因暫時性連接等。）。 基於這個原因，可能會使得保存您自己的代表項錯誤，因此您可以輕鬆地 deduplicate 據用量感知器。 這是您傳送電子郵件通知或相當於的非常重要。

### <a name="properties-of-faults"></a>錯誤的屬性

此表格提供幾個重要錯誤物件的屬性。 完整架構、檢查**MSFT\_StorageDiagnoseResult**在*storagewmi.mof*。

| **屬性**              | **範例**                                                     |
|---------------------------|-----------------------------------------------------------------|
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft.Health.FaultType.Volume.Capacity                      |
| 原因                    | 「磁碟區可用空間不足。」                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | 提供架狀 A06 RU 25、插槽 11                                        |
| RecommendedActions        | {」展開磁碟區。」、「將工作負載移轉到其他磁碟區」。}   |

**FaultId**在一個叢集的範圍中唯一。

**PerceivedSeverity** PerceivedSeverity = {4、5、6} {」資訊」、「警告，「和」錯誤「} = 或相當於藍色、黃色和紅色的色彩。

**FaultingObjectDescription**第一的硬體、軟體物件通常空白的資訊。

**FaultingObjectLocation**的位置資訊的硬體，通常是空白的軟體物件。

**RecommendedActions**和任何特定訂單中的建議的動作，無關清單。 今天，這份清單通常是 1 長度。

## <a name="properties-of-fault-events"></a>屬性的錯誤事件

此表格提供幾個重要的屬性的錯誤事件。 完整架構、檢查**MSFT\_StorageFaultEvent**在*storagewmi.mof*。

注意**變更型別**，來表示是否錯誤所建立，移除，或更新和**FaultId**。 事件也包含的所有受影響的錯誤屬性。

| **屬性**              | **範例**                                                     |
|---------------------------|-----------------------------------------------------------------|
| 變更型別                | 0                                                               |
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft.Health.FaultType.Volume.Capacity                      |
| 原因                    | 「磁碟區可用空間不足。」                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | 提供架狀 A06 RU 25、插槽 11                                        |
| RecommendedActions        | {」展開磁碟區。」、「將工作負載移轉到其他磁碟區」。}   |

**變更型別**變更型別 = {0、1、2} = {「建立」，」（移除），」更新」}。

## <a name="coverage"></a>涵蓋範圍

Windows Server 2016 中健康服務提供下列錯誤涵蓋範圍：  

### **<a name="physicaldisk-8"></a>平均 (8)**

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedmedia"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.FailedMedia
* 嚴重性：警告
* 理由：*[實體磁碟已無法」。*
* RecommendedAction: *「取代實體磁碟」。*

#### <a name="faulttype-microsofthealthfaulttypephysicaldisklostcommunication"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.LostCommunication
* 嚴重性：警告
* 理由：*「連接遺失了實體磁碟。」*
* RecommendedAction: *「檢查實體磁碟是否工作及正確連接。」*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunresponsive"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.Unresponsive
* 嚴重性：警告
* 理由：*[實體磁碟正呈現週期性一堆」。*
* RecommendedAction: *「取代實體磁碟」。*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskpredictivefailure"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.PredictiveFailure
* 嚴重性：警告
* 理由：*「失敗實體磁碟很快就會發生預測」。*
* RecommendedAction: *「取代實體磁碟」。*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedhardware"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnsupportedHardware
* 嚴重性：警告
* 理由：*[實體磁碟隔離因為您的方案廠商不支援「。*
* RecommendedAction: *「取代實體磁碟支援的硬體。」*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedfirmware"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnsupportedFirmware
* 嚴重性：警告
* 理由：*[實體磁碟處於隔離因為其韌體版本不支援方案廠商。」*
* RecommendedAction: *「更新實體磁碟上的韌體目標版本」。*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunrecognizedmetadata"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnrecognizedMetadata
* 嚴重性：警告
* 理由：*[實體磁碟已發現無法辨識的中繼資料。」*
* RecommendedAction: *「這個磁碟可能包含來自不明的儲存集區的資料。第一次確定有任何可用的資料硬碟，然後重設磁片。」*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedfirmwareupdate"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.FailedFirmwareUpdate
* 嚴重性：警告
* 理由：*「失敗嘗試更新韌體實體磁碟」。*
* RecommendedAction: *「請嘗試使用不同韌體二進位」。*

### **<a name="virtual-disk-2"></a>Virtual 磁碟 (2)**

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksneedsrepair"></a>FaultType: Microsoft.Health.FaultType.VirtualDisks.NeedsRepair
* 嚴重性：資訊
* 理由：*「這個磁碟區上的某些資料不完整能因應。該 app 會維持無障礙。」*
* RecommendedAction: *「還原恢復的資料」。*

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksdetached"></a>FaultType: Microsoft.Health.FaultType.VirtualDisks.Detached
* 嚴重性：重要
* 理由：*「磁碟區就無法存取。某些資料可能會遺失。」*
* RecommendedAction: *「檢查實體和/或網路的所有存放裝置連接。您可能需要從備份還原。」*

### **<a name="pool-capacity-1"></a>集區容量 (1)**

#### <a name="faulttype-microsofthealthfaulttypestoragepoolinsufficientreservecapacityfault"></a>FaultType: Microsoft.Health.FaultType.StoragePool.InsufficientReserveCapacityFault
* 嚴重性：警告
* 理由：*「儲存集區未最低建議的保留容量。這可能會限制您要還原資料恢復發生磁碟機故障。」*
* RecommendedAction: *「到儲存集區中新增額外的容量或釋出容量。最小值建議保留視部署，但容量大約 2 磁碟機價值。」*

### <a name="volume-capacity-2sup1sup"></a>**磁碟區容量 (2)**<sup>1</sup>

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType: Microsoft.Health.FaultType.Volume.Capacity
* 嚴重性：警告
* 理由：*「磁碟區可用空間不足。」*
* RecommendedAction: *「展開音量，或工作負載移轉給其他磁碟區」。*

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType: Microsoft.Health.FaultType.Volume.Capacity
* 嚴重性：重要
* 理由：*「磁碟區可用空間不足。」*
* RecommendedAction: *「展開音量，或工作負載移轉給其他磁碟區」。*

### **<a name="server-3"></a>伺服器 (3)**

#### <a name="faulttype-microsofthealthfaulttypeserverdown"></a>FaultType: Microsoft.Health.FaultType.Server.Down
* 嚴重性：重要
* 理由：*「無法存取伺服器」。*
* RecommendedAction: *[[開始] 或更換伺服器。」*

#### <a name="faulttype-microsofthealthfaulttypeserverisolated"></a>FaultType: Microsoft.Health.FaultType.Server.Isolated
* 嚴重性：重要
* 理由：*「伺服器是因為連接問題的隔離的「。*
* RecommendedAction: *「如果隔離持續發生，請查看的網路或工作負載移轉給其他節點」。*

#### <a name="faulttype-microsofthealthfaulttypeserverquarantined"></a>FaultType: Microsoft.Health.FaultType.Server.Quarantined
* 嚴重性：重要
* 理由：*「伺服器隔離，因為週期性失敗叢集」。*
* RecommendedAction: *「取代伺服器，或修正網路」。*

### **<a name="cluster-1"></a>(1) 叢集**

#### <a name="faulttype-microsofthealthfaulttypeclusterquorumwitnesserror"></a>FaultType: Microsoft.Health.FaultType.ClusterQuorumWitness.Error
* 嚴重性：重要
* 理由：*「叢集是原位前往一個伺服器失敗」。*
* RecommendedAction: *[見證資源，請檢查並視需要重新開機。[開始] 或更換失敗的伺服器」。*

### **<a name="network-adapterinterface-4"></a>網路介面卡日介面 (4)**

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisconnected"></a>FaultType: Microsoft.Health.FaultType.NetworkAdapter.Disconnected
* 嚴重性：警告
* 理由：*[網路介面已經成為中斷連接。」*
* RecommendedAction: *「重新網路線」。*

#### <a name="faulttype-microsofthealthfaulttypenetworkinterfacemissing"></a>FaultType: Microsoft.Health.FaultType.NetworkInterface.Missing
* 嚴重性：警告
* 理由：*「{伺服器} 已遺失網路介面卡連接叢集網路 {叢集網路}」。*
* RecommendedAction: *「連接伺服器叢集網路遺失」。*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterhardware"></a>FaultType: Microsoft.Health.FaultType.NetworkAdapter.Hardware
* 嚴重性：警告
* 理由：*[網路介面已經有硬體故障」。*
* RecommendedAction: *「取代網路介面卡]。*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisabled"></a>FaultType: Microsoft.Health.FaultType.NetworkAdapter.Disabled
* 嚴重性：警告
* 理由：*[網路介面 {網路介面} 不支援並不使用]*
* RecommendedAction: *「讓的網路介面。」*

### **<a name="enclosure-6"></a>圍繞 (6)**

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurelostcommunication"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.LostCommunication
* 嚴重性：警告
* 理由：*「通訊遺失了儲存圍繞。」*
* RecommendedAction: *[[開始] 畫面或取代儲存圍繞」。*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurefanerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.FanError
* 嚴重性：警告
* 理由：*「失敗位置 {位置} 儲存圍繞的粉絲」。*
* RecommendedAction: *「取代粉絲儲存圍繞在「。*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurecurrentsensorerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.CurrentSensorError
* 嚴重性：警告
* 理由：*「失敗目前位置 {位置} 儲存圍繞的感應器」。*
* RecommendedAction: *「取代目前感應器儲存圍繞在「。*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurevoltagesensorerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.VoltageSensorError
* 嚴重性：警告
* 理由：*「失敗電壓位置 {位置} 儲存圍繞的感應器」。*
* RecommendedAction: *「取代電壓感應器儲存圍繞在「。*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosureiocontrollererror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.IoControllerError
* 嚴重性：警告
* 理由：*「失敗 IO 控制器位置 {位置} 儲存圍繞在「。*
* RecommendedAction: *「取代儲存圍繞在 IO 控制器」。*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosuretemperaturesensorerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.TemperatureSensorError
* 嚴重性：警告
* 理由：*「失敗溫度計位置 {位置} 儲存圍繞在「。*
* RecommendedAction: *「取代測器儲存圍繞在「。*

### **<a name="firmware-rollout-3"></a>韌體推出 (3)**

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfailedmaintenancemode"></a>FaultType: Microsoft.Health.FaultType.FaultDomain.FailedMaintenanceMode
* 嚴重性：警告
* 理由：*「執行韌體大規模時進行目前無法」。*
* RecommendedAction: *「驗證所有的儲存空間的健康狀態，並不錯誤網域目前正在維護模式」。*

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfirmwareverifyversionfaile"></a>FaultType: Microsoft.Health.FaultType.FaultDomain.FirmwareVerifyVersionFaile
* 嚴重性：警告
* 理由：*「韌體大規模已取消因為套用韌體更新後無法讀取或非預期的韌體版本資訊」。*
* RecommendedAction: *[重新開機韌體儲蓄韌體問題解析之後。」*

#### <a name="faulttype-microsofthealthfaulttypefaultdomaintoomanyfailedupdates"></a>FaultType: Microsoft.Health.FaultType.FaultDomain.TooManyFailedUpdates
* 嚴重性：警告
* 理由：*「因為失敗的韌體更新嘗試太多實體磁碟已取消韌體大規模」。*
* RecommendedAction: *[重新開機韌體儲蓄韌體問題解析之後。」*

### <a name="storage-qos-3sup2sup"></a>**儲存空間 QoS (3)**<sup>2</sup>

#### <a name="faulttype-microsofthealthfaulttypestorqosinsufficientthroughput"></a>FaultType: Microsoft.Health.FaultType.StorQos.InsufficientThroughput
* 嚴重性：警告
* 理由：*「輸送量儲存空間不足滿足會保留」。*
* RecommendedAction: *「重新設定存放區 QoS 原則」。*

#### <a name="faulttype-microsofthealthfaulttypestorqoslostcommunication"></a>FaultType: Microsoft.Health.FaultType.StorQos.LostCommunication
* 嚴重性：警告
* 理由：*「儲存空間 QoS 原則管理員已遺失通訊的磁碟區」。*
* RecommendedAction: *「請重新開機節點 {節點}」*

#### <a name="faulttype-microsofthealthfaulttypestorqosmisconfiguredflow"></a>FaultType: Microsoft.Health.FaultType.StorQos.MisconfiguredFlow
* 嚴重性：警告
* 理由：*「一或多個儲存消費者（通常是虛擬機器）使用不存在原則 {橋接器} 與。」*
* RecommendedAction: *「重新建立任何缺少的儲存空間 QoS 原則」。*

<sup>1</sup>表示 80%完整 （次要嚴重性） 或 90%完整 （主要嚴重性） 人數已達磁碟區。  
<sup>2</sup>表示一些.vhd(s) 磁碟區上的有未符合為他們最小值 IOPS 超過 10%（次要）、 30%（主要） 或 50%的循環 24 小時視窗 （重大）。  

>[!NOTE]
> 健康的粉絲，感應器電源供應器，例如儲存圍繞元件衍生從 SCSI 圍繞服務 (SES)。 如果您的供應商不提供這項資訊，健康服務不會顯示。  

## <a name="see-also"></a>也了

- [Windows Server 2016 中 health 服務](health-service-overview.md)
