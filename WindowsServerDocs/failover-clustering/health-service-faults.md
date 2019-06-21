---
title: 健全狀況服務錯誤
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: ''
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: 72b1593503db75aa275b9eb45c8342cee6724001
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280392"
---
# <a name="health-service-faults"></a>健全狀況服務錯誤
> 適用於：Windows Server 2019，Windows Server 2016

## <a name="what-are-faults"></a>錯誤有哪些？

健全狀況服務會持續監視您的儲存空間直接存取叢集，以偵測問題並產生 「 錯誤 」。 一個新的 cmdlet 會顯示任何目前的錯誤，可讓您輕鬆地確認部署的健康情況不會在每個實體，或依序功能。 「錯誤」是以精確、容易理解，及可採取動作為設計目標。  

每個錯誤會包含五個重要欄位：  

-   Severity
-   問題描述
-   解決此問題的下一個建議步驟
-   錯誤實體的識別資訊
-   它的實體位置 (若可用)

例如，以下是一個典型的錯誤︰  

```
Severity: MINOR                                         
Reason: Connectivity has been lost to the physical disk.                           
Recommendation: Check that the physical disk is working and properly connected.    
Part: Manufacturer Contoso, Model XYZ9000, Serial 123456789                        
Location: Seattle DC, Rack B07, Node 4, Slot 11
```

 >[!NOTE]
 > 實體位置是衍伸自您的容錯網域組態。 如需有關容錯網域的詳細資訊，請參閱[Windows Server 2016 中的容錯網域](fault-domains.md)。 如果您未提供這項資訊，則位置欄位的實用性會較低 - 例如，它可能只會顯示插槽編號。  

## <a name="root-cause-analysis"></a>根本原因分析

健全狀況服務可以評估錯誤以識別並結合而產生相同的基礎問題的 「 錯誤的實體之間可能的因果關係。 藉由辨識相關聯的影響，可以讓報告較為簡潔。 比方說，如果伺服器已關閉，它必須比伺服器內的任何磁碟機也都會失去連線。 因此，根本原因-在此情況下，伺服器會產生只有一個以上的錯誤。  

## <a name="usage-in-powershell"></a>在 PowerShell 中的使用方式

若要查看任何目前的錯誤，在 PowerShell 中，執行這個指令程式：

```PowerShell
Get-StorageSubSystem Cluster* | Debug-StorageSubSystem  
```

這會傳回任何錯誤，這會影響整體儲存空間直接存取叢集。 大多數情況下，這些錯誤與硬體或組態。 如果不有任何錯誤，此 cmdlet 會傳回任何項目。  

>[!NOTE]
> 在非生產環境中，並自行承擔風險，您可以試驗這項功能自行觸發錯誤-例如，藉由移除實體磁碟或關閉一個節點。 一旦出現錯誤，重新插入的實體磁碟或重新啟動節點，「 錯誤就會再次消失。

您也可以檢視影響只有特定的磁碟區或檔案共用，使用下列 cmdlet 的錯誤：  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Debug-Volume  

Get-FileShare -Name <Name> | Debug-FileShare  
```

這會傳回任何會影響只在特定磁碟區或檔案共用的錯誤。 大多數情況下，這些錯誤與容量規劃、 資料恢復功能或存放裝置服務品質或儲存體複本等功能。 

## <a name="usage-in-net-and-c"></a>在.NET 中的使用方式和C#

### <a name="connect"></a>連線

若要查詢健全狀況服務，您必須先建立**CimSession**與叢集。 若要這樣做，您將需要幾個步驟，才可在完整的.NET 中，這表示您無法輕易地執行這項操作直接從 web 或行動裝置應用程式。 這些程式碼範例會使用 C\#、 最直接了當選擇此資料存取層。

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

提供的使用者名稱應該是目標電腦的本機系統管理員。

建議您建構密碼**SecureString**直接從使用者輸入，在即時的方式，因此他們的密碼永遠不會儲存在記憶體中以純文字。 這有助於減輕各種不同的安全性考量。 但是在實務上，若要建構上述是很常見的原型設計目的。

### <a name="discover-objects"></a>探索物件

具有**CimSession**建立，您可以查詢 Windows Management Instrumentation (WMI) 在叢集上。

您可以取得錯誤或計量之前，您必須取得的數個相關物件執行個體。 首先， **MSFT\_StorageSubSystem**表示儲存空間直接存取叢集上。 使用的您可以取得每個**MSFT\_StorageNode**在叢集中，而每個**MSFT\_大量**，資料磁碟區。 最後，您將需要**MSFT\_StorageHealth**，健全狀況服務本身、 太。

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

這些是使用類似的 cmdlet，在 PowerShell 中取得的相同物件**Get-storagesubsystem**， **Get-storagenode**，並**Get-volume**。

您可以存取所有相同屬性，記載於[存放管理 API 類別](https://msdn.microsoft.com/library/windows/desktop/hh830612(v=vs.85).aspx)。

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

叫用**診斷**若要取得目前範圍設定為目標的任何 「 錯誤**CimInstance**，它是叢集或任何磁碟區。

可用的錯誤，在 Windows Server 2016 中的每個範圍的完整清單會如下所述。

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

### <a name="optional-myfault-class"></a>選擇性：MyFault 類別

它可能會使您建構，並將保存您自己的表示法的錯誤。 比方說，這**MyFault**類別會儲存數個索引鍵的屬性的錯誤，包括**錯誤識別碼**，其可在之後以產生關聯更新或移除通知，或重複資料刪除的情況相同的錯誤偵測到多次，因為任何原因。

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

完整的清單中每個錯誤的屬性 (**DiagnoseResult**) 詳述如下。

### <a name="fault-events"></a>錯誤事件

當建立、 移除或更新錯誤時，健全狀況服務會產生 WMI 事件。 這些是必要項目將您的應用程式狀態保持不頻繁的輪詢，同步，並可協助防堵判斷何時傳送電子郵件警示，例如。 若要訂閱這些事件，此範例程式碼會再次使用觀察者設計模式。

首先，訂閱**MSFT\_StorageFaultEvent**事件。

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

接下來，實作觀察者其**OnNext()** 時就會產生新的事件，會叫用方法。

每個事件包含**ChangeType**指出是否將錯誤建立時，已移除，或已更新，以及相關**錯誤識別碼**。

此外，它們包含本身的錯誤的所有屬性。

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

### <a name="understand-fault-lifecycle"></a>了解錯誤生命週期

要標示為 「 看到 」 或已由使用者解決不適合錯誤。 當健全狀況服務會觀察到的問題，並自動移除，且只有在健全狀況服務無法再可以觀察到問題時，才會建立它們。 一般情況下，這會反映已修正此問題。

不過，在某些情況下，錯誤可能是重新探索健全狀況服務 （例如容錯移轉之後，或是因為間歇性連線等。）。 基於這個理由，它可能可以合理地保存您自己的錯誤，表示法，以便您可以輕鬆地重複資料刪除。 這是特別重要，如果您傳送電子郵件警示或同等權限。

### <a name="properties-of-faults"></a>錯誤的屬性

此表顯示錯誤物件的數個索引鍵的屬性。 針對完整的結構描述中，檢查**MSFT\_StorageDiagnoseResult**類別*storagewmi.mof*。

| **屬性**              | **範例**                                                     |
|---------------------------|-----------------------------------------------------------------|
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft.Health.FaultType.Volume.Capacity                      |
| `Reason`                    | 「 磁碟區用盡可用的空間。 」                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | Rack A06, RU 25, Slot 11                                        |
| RecommendedActions        | {"展開磁碟區。"，"將工作負載移轉至其他磁碟區 」。}   |

**錯誤識別碼**唯一一個叢集中的範圍內。

**PerceivedSeverity** PerceivedSeverity = {4，5，6} = {「 資訊 」、 「 警告 」 和 「 錯誤 」}，或對等的色彩，例如藍色、 黃色和紅色。

**FaultingObjectDescription**部分硬體，通常是空白的軟體物件的資訊。

**FaultingObjectLocation**軟體物件的位置資訊的硬體，通常空白。

**RecommendedActions**無關的建議動作並不依特定順序的清單。 現在，這份清單通常是長度為 1。

## <a name="properties-of-fault-events"></a>錯誤事件的屬性

此表顯示錯誤事件的數個索引鍵的屬性。 針對完整的結構描述中，檢查**MSFT\_StorageFaultEvent**類別*storagewmi.mof*。

附註**ChangeType**，表示是否將錯誤建立時，已移除，或已更新，而**錯誤識別碼**。 事件也包含受影響的錯誤的所有屬性。

| **屬性**              | **範例**                                                     |
|---------------------------|-----------------------------------------------------------------|
| ChangeType                | 0                                                               |
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft.Health.FaultType.Volume.Capacity                      |
| `Reason`                    | 「 磁碟區用盡可用的空間。 」                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | Rack A06, RU 25, Slot 11                                        |
| RecommendedActions        | {"展開磁碟區。"，"將工作負載移轉至其他磁碟區 」。}   |

**ChangeType** ChangeType = {0，1，2} = {「 建立 」、 「 移除 」，「 更新 」}。

## <a name="coverage"></a>涵蓋範圍

在 Windows Server 2016 中，健全狀況服務會提供下列 「 錯誤 」 涵蓋範圍：  

### <a name="physicaldisk-8"></a>**PhysicalDisk (8)**

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedmedia"></a>遺失 FaultType:Microsoft.Health.FaultType.PhysicalDisk.FailedMedia
* 嚴重性:警告
* 原因：*失敗的實體磁碟 」。*
* RecommendedAction: *「 取代實體磁碟 」。*

#### <a name="faulttype-microsofthealthfaulttypephysicaldisklostcommunication"></a>遺失 FaultType:Microsoft.Health.FaultType.PhysicalDisk.LostCommunication
* 嚴重性:警告
* 原因： *「 連線已中斷至實體磁碟。 」*
* RecommendedAction: *「 檢查實體磁碟正在運作中且已正確連線 」。*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunresponsive"></a>遺失 FaultType:Microsoft.Health.FaultType.PhysicalDisk.Unresponsive
* 嚴重性:警告
* 原因： *「 實體磁碟發生週期性無回應。 」*
* RecommendedAction: *「 取代實體磁碟 」。*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskpredictivefailure"></a>遺失 FaultType:Microsoft.Health.FaultType.PhysicalDisk.PredictiveFailure
* 嚴重性:警告
* 原因： *「 實體磁碟故障預測很快就會發生"。*
* RecommendedAction: *「 取代實體磁碟 」。*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedhardware"></a>遺失 FaultType:Microsoft.Health.FaultType.PhysicalDisk.UnsupportedHardware
* 嚴重性:警告
* 原因： *「 因為它不支援您的解決方案廠商被隔離的實體磁碟 」。*
* RecommendedAction: *「 取代實體磁碟與支援的硬體。 」*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedfirmware"></a>遺失 FaultType:Microsoft.Health.FaultType.PhysicalDisk.UnsupportedFirmware
* 嚴重性:警告
* 原因： *「 實體磁碟已隔離因為解決方案廠商不支援其韌體版本。 」*
* RecommendedAction: *「 目標版本更新的實體磁碟上的軔體。 」*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunrecognizedmetadata"></a>遺失 FaultType:Microsoft.Health.FaultType.PhysicalDisk.UnrecognizedMetadata
* 嚴重性:警告
* 原因： *「 實體磁碟有無法辨識的中繼資料 」。*
* RecommendedAction: *「 這個磁碟可能包含未知的存放集區中的資料。先確定沒有任何有用的資料，在此磁碟上，然後重設磁片。 」*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedfirmwareupdate"></a>遺失 FaultType:Microsoft.Health.FaultType.PhysicalDisk.FailedFirmwareUpdate
* 嚴重性:警告
* 原因： *「 嘗試失敗更新的實體磁碟上的軔體。 」*
* RecommendedAction: *「 請嘗試使用不同的韌體二進位 」。*

### <a name="virtual-disk-2"></a>**虛擬磁碟 (2)**

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksneedsrepair"></a>遺失 FaultType:Microsoft.Health.FaultType.VirtualDisks.NeedsRepair
* 嚴重性:資訊
* 原因： *「 這個磁碟區上的資料不是完全復原。它仍可存取。 」*
* RecommendedAction: *「 還原復原的資料。 」*

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksdetached"></a>遺失 FaultType:Microsoft.Health.FaultType.VirtualDisks.Detached
* 嚴重性:嚴重
* 原因： *「 磁碟區是無法存取。某些資料可能會遺失。 」*
* RecommendedAction: *「 檢查實體和/或網路連線的所有存放裝置。您可能需要從備份還原。 」*

### <a name="pool-capacity-1"></a>**集區容量 (1)**

#### <a name="faulttype-microsofthealthfaulttypestoragepoolinsufficientreservecapacityfault"></a>遺失 FaultType:Microsoft.Health.FaultType.StoragePool.InsufficientReserveCapacityFault
* 嚴重性:警告
* 原因： *「 存放集區沒有最小建議的保留容量。這可能會限制您能夠還原資料磁碟機失敗時的恢復功能。 」*
* RecommendedAction: *「 存放集區中，新增額外的容量或釋出容量。最小的建議保留部署，而異，但是是容量的大約 2 個磁碟機的價值。 」*

### <a name="volume-capacity-2sup1sup"></a>**(2) 的磁碟區容量**<sup>1</sup>

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>遺失 FaultType:Microsoft.Health.FaultType.Volume.Capacity
* 嚴重性:警告
* 原因： *「 磁碟區用盡可用的空間。 」*
* RecommendedAction: *「 擴充的磁碟區，或將工作負載移轉至其他磁碟區 」。*

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>遺失 FaultType:Microsoft.Health.FaultType.Volume.Capacity
* 嚴重性:嚴重
* 原因： *「 磁碟區用盡可用的空間。 」*
* RecommendedAction: *「 擴充的磁碟區，或將工作負載移轉至其他磁碟區 」。*

### <a name="server-3"></a>**Server (3)**

#### <a name="faulttype-microsofthealthfaulttypeserverdown"></a>遺失 FaultType:Microsoft.Health.FaultType.Server.Down
* 嚴重性:嚴重
* 原因： *「 無法連線到伺服器 」。*
* RecommendedAction: *「 啟動，或取代伺服器 」。*

#### <a name="faulttype-microsofthealthfaulttypeserverisolated"></a>遺失 FaultType:Microsoft.Health.FaultType.Server.Isolated
* 嚴重性:嚴重
* 原因： *「 伺服器 」 從叢集中，由於連線問題隔離。*
* RecommendedAction: *「 如果隔離持續發生，請檢查網路或工作負載移轉至其他節點 」。*

#### <a name="faulttype-microsofthealthfaulttypeserverquarantined"></a>遺失 FaultType:Microsoft.Health.FaultType.Server.Quarantined
* 嚴重性:嚴重
* 原因： *「 伺服器隔離，由於週期性失敗的叢集 」。*
* RecommendedAction: *「 取代伺服器或修正網路 」。*

### <a name="cluster-1"></a>**叢集 (1)**

#### <a name="faulttype-microsofthealthfaulttypeclusterquorumwitnesserror"></a>遺失 FaultType:Microsoft.Health.FaultType.ClusterQuorumWitness.Error
* 嚴重性:嚴重
* 原因： *「 叢集是一個遠離下降的伺服器失敗。 」*
* RecommendedAction: *「 檢查見證資源，並視需要重新啟動。啟動或取代失敗的伺服器 」。*

### <a name="network-adapterinterface-4"></a>**網路介面卡/介面 (4)**

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisconnected"></a>遺失 FaultType:Microsoft.Health.FaultType.NetworkAdapter.Disconnected
* 嚴重性:警告
* 原因： *「 網路介面連線已中斷。 」*
* RecommendedAction: *「 重新連接網路纜線 」。*

#### <a name="faulttype-microsofthealthfaulttypenetworkinterfacemissing"></a>遺失 FaultType:Microsoft.Health.FaultType.NetworkInterface.Missing
* 嚴重性:警告
* 原因： *「 伺服器 {server} 有遺漏的網路介面卡連線到叢集網路 {叢集網路} 」。*
* RecommendedAction: *「 請到遺漏的叢集網路來連接伺服器。 」*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterhardware"></a>遺失 FaultType:Microsoft.Health.FaultType.NetworkAdapter.Hardware
* 嚴重性:警告
* 原因： *「 網路介面已在硬體失敗。 」*
* RecommendedAction: *「 取代網路介面卡 」。*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisabled"></a>遺失 FaultType:Microsoft.Health.FaultType.NetworkAdapter.Disabled
* 嚴重性:警告
* 原因： *「 網路介面 {網路介面} 未啟用的並不使用 」。*
* RecommendedAction: *「 啟用網路介面 」。*

### <a name="enclosure-6"></a>**機箱 (6)**

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurelostcommunication"></a>遺失 FaultType:Microsoft.Health.FaultType.StorageEnclosure.LostCommunication
* 嚴重性:警告
* 原因： *「 通訊已中斷與儲存體機箱。 」*
* RecommendedAction: *「 啟動，或將存放裝置機箱 」。*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurefanerror"></a>遺失 FaultType:Microsoft.Health.FaultType.StorageEnclosure.FanError
* 嚴重性:警告
* 原因： *「 位置 {位置} 存放裝置機箱的風扇已失敗 」。*
* RecommendedAction: *「 取代存放裝置機箱的風扇。 」*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurecurrentsensorerror"></a>遺失 FaultType:Microsoft.Health.FaultType.StorageEnclosure.CurrentSensorError
* 嚴重性:警告
* 原因： *「 已失敗 {位置} 的位置存放裝置機箱的電流感應器 」。*
* RecommendedAction: *「 取代在 存放裝置機箱的電流感應器 」。*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurevoltagesensorerror"></a>遺失 FaultType:Microsoft.Health.FaultType.StorageEnclosure.VoltageSensorError
* 嚴重性:警告
* 原因： *「 電壓感應器位置 {位置} 的存放裝置機箱已失敗 」。*
* RecommendedAction: *「 取代電壓感應器的存放裝置機箱中 」。*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosureiocontrollererror"></a>遺失 FaultType:Microsoft.Health.FaultType.StorageEnclosure.IoControllerError
* 嚴重性:警告
* 原因： *「 已失敗 {位置} 的位置存放裝置機箱的 IO 控制站 」。*
* RecommendedAction: *「 取代存放裝置機箱中的 IO 控制站 」。*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosuretemperaturesensorerror"></a>遺失 FaultType:Microsoft.Health.FaultType.StorageEnclosure.TemperatureSensorError
* 嚴重性:警告
* 原因： *「 已失敗 {位置} 的位置存放裝置機箱溫度感應器 」。*
* RecommendedAction: *「 取代在 存放裝置機箱溫度感應器 」。*

### <a name="firmware-rollout-3"></a>**韌體首度發行 (3)**

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfailedmaintenancemode"></a>遺失 FaultType:Microsoft.Health.FaultType.FaultDomain.FailedMaintenanceMode
* 嚴重性:警告
* 原因： *「 目前無法執行韌體推出時進行。 」*
* RecommendedAction: *「 確認所有的儲存空間狀況良好，而沒有容錯網域目前處於維護模式 」。*

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfirmwareverifyversionfaile"></a>遺失 FaultType:Microsoft.Health.FaultType.FaultDomain.FirmwareVerifyVersionFaile
* 嚴重性:警告
* 原因： *「 因為無法讀取或非預期的韌體版本資訊，在套用韌體更新後已取消韌體推出 」。*
* RecommendedAction: *「 重新啟動韌體推出後韌體問題已經解決。 」*

#### <a name="faulttype-microsofthealthfaulttypefaultdomaintoomanyfailedupdates"></a>遺失 FaultType:Microsoft.Health.FaultType.FaultDomain.TooManyFailedUpdates
* 嚴重性:警告
* 原因： *「 因為太多失敗嘗試韌體更新的實體磁碟已取消韌體推出 」。*
* RecommendedAction: *「 重新啟動韌體推出後韌體問題已經解決。 」*

### <a name="storage-qos-3sup2sup"></a>**(3) 的存放裝置 QoS**<sup>2</sup>

#### <a name="faulttype-microsofthealthfaulttypestorqosinsufficientthroughput"></a>遺失 FaultType:Microsoft.Health.FaultType.StorQos.InsufficientThroughput
* 嚴重性:警告
* 原因： *「 儲存體輸送量是不足以滿足保留 」。*
* RecommendedAction: *「 重新設定存放裝置 QoS 原則。 」*

#### <a name="faulttype-microsofthealthfaulttypestorqoslostcommunication"></a>遺失 FaultType:Microsoft.Health.FaultType.StorQos.LostCommunication
* 嚴重性:警告
* 原因： *「 存放裝置 QoS 原則管理員已遺失與磁碟區的通訊。 」*
* RecommendedAction: *「 請重新啟動節點 {節點}"*

#### <a name="faulttype-microsofthealthfaulttypestorqosmisconfiguredflow"></a>遺失 FaultType:Microsoft.Health.FaultType.StorQos.MisconfiguredFlow
* 嚴重性:警告
* 原因： *「 一或多個儲存體取用者 （通常是虛擬機器） 會使用不存在的原則識別碼為 {id}。 」*
* RecommendedAction: *「 重新建立任何遺失的存放裝置 QoS 原則。 」*

<sup>1</sup>表示磁碟區已達到 80%（次要嚴重性） 或 90%（重大嚴重性）。  
<sup>2</sup>指出某些.vhd 在磁碟區上的有不符合其最小值 IOPS 高於 10%（次要）、 30%（重大）、 或 50%（重大） 提昇 24 小時範圍。  

>[!NOTE]
> 存放裝置機箱組件 (如風扇、電源供應器和感應器) 的健康情況是衍生自 SCSI 機箱服務 (SES)。 如果您的廠商沒有提供這項資訊，「健全狀況服務」就無法顯示它。  

## <a name="see-also"></a>另請參閱

- [Windows Server 2016 中的健全狀況服務](health-service-overview.md)
