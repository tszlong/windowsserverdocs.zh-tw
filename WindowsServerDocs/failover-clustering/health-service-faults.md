---
title: 健全狀況服務錯誤
ms.prod: windows-server
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: ''
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: 11af69d1c6f32205b87ad4605edebacb59b0b710
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369714"
---
# <a name="health-service-faults"></a>健全狀況服務錯誤
> 適用於：Windows Server 2019、Windows Server 2016

## <a name="what-are-faults"></a>什麼是錯誤

健全狀況服務會持續監視您的儲存空間直接存取叢集，以偵測問題並產生「錯誤」。 其中一個新的 Cmdlet 會顯示任何目前的錯誤，讓您可以輕鬆地驗證部署的健康情況，而不必再查看每個實體或功能。 「錯誤」是以精確、容易理解，及可採取動作為設計目標。  

每個錯誤都包含五個重要欄位：  

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
 > 實體位置是衍伸自您的容錯網域組態。 如需容錯網域的詳細資訊，請參閱[Windows Server 2016 中的容錯網域](fault-domains.md)。 如果您未提供這項資訊，則位置欄位的實用性會較低 - 例如，它可能只會顯示插槽編號。  

## <a name="root-cause-analysis"></a>根本原因分析

健全狀況服務可以評估錯誤實體之間的潛在因果，以識別併合並因相同基礎問題而產生的錯誤。 藉由辨識相關聯的影響，可以讓報告較為簡潔。 例如，如果伺服器已關閉，則預期伺服器中的任何磁片磁碟機也不會有連線能力。 因此，根本原因只會引發一個錯誤，在此案例中為伺服器。  

## <a name="usage-in-powershell"></a>在 PowerShell 中的使用方式

若要查看 PowerShell 中的任何目前錯誤，請執行此 Cmdlet：

```PowerShell
Get-StorageSubSystem Cluster* | Debug-StorageSubSystem  
```

這會傳回影響整體儲存空間直接存取叢集的任何錯誤。 這些錯誤通常與硬體或配置有關。 如果沒有任何錯誤，此 Cmdlet 就不會傳回任何內容。  

>[!NOTE]
> 在非生產環境中，以及您自己的風險下，您可以自行觸發錯誤來試驗這項功能-例如，藉由移除一個實體磁片或關閉一個節點。 錯誤出現之後，請重新插入實體磁片或重新開機節點，錯誤就會再次消失。

您也可以使用下列 Cmdlet 來查看只影響特定磁片區或檔案共用的錯誤：  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Debug-Volume  

Get-FileShare -Name <Name> | Debug-FileShare  
```

這會傳回只影響特定磁片區或檔案共用的任何錯誤。 這些錯誤通常與容量規劃、資料復原，或儲存體服務品質或儲存體複本等功能有關。 

## <a name="usage-in-net-and-c"></a>.NET 和中的使用方式C#

### <a name="connect"></a>連線

為了查詢健全狀況服務，您必須建立叢集的**CimSession** 。 若要這麼做，您將需要一些只在完整 .NET 中提供的專案，這表示您無法直接從 web 或行動裝置應用程式進行這項操作。 這些程式碼範例會使用 C @ no__t-0，這是最直接的此資料存取層選擇。

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

提供的使用者名稱應該是目的電腦的本機系統管理員。

建議您以即時的方式直接從使用者輸入來**SecureString**密碼，因此其密碼絕對不會以純文字的方式儲存在記憶體中。 這有助於減輕各種安全性考慮。 但是在實務上，如上面所述，通常是針對原型設計的用途。

### <a name="discover-objects"></a>探索物件

建立**CimSession**之後，您就可以查詢叢集上的 WINDOWS MANAGEMENT INSTRUMENTATION （WMI）。

在您取得錯誤或計量之前，您必須取得數個相關物件的實例。 首先， **MSFT @ no__t-1StorageSubSystem**代表叢集儲存空間直接存取。 使用這種方式，您可以取得叢集中的每個**msft @ no__t-1StorageNode** ，以及每個**msft @ no__t-3Volume**，資料磁片區。 最後，您還需要**MSFT @ no__t-1StorageHealth**，也就是健全狀況服務本身。

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

這些是您在 PowerShell 中使用**StorageSubSystem**、 **StorageNode**和**取得磁片**區等 Cmdlet 取得的相同物件。

您可以存取[儲存管理 API 類別](https://msdn.microsoft.com/library/windows/desktop/hh830612(v=vs.85).aspx)中記載的所有相同屬性。

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

叫用**診斷**以取得任何範圍設定為目標**CimInstance**的目前錯誤，也就是叢集或任何磁片區。

Windows Server 2016 中每個領域的可用錯誤完整清單記載于下文。

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

您可以建立自己的錯誤表示並加以保存，這是合理的做法。 例如，這個**MyFault**類別會儲存幾個錯誤的索引鍵屬性，包括**FaultId**，稍後可以用來關聯更新或移除通知，或在偵測到相同錯誤多次時刪除重複。基於任何原因。

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

每個錯誤（**DiagnoseResult**）中的屬性完整清單記載于下面。

### <a name="fault-events"></a>錯誤事件

當您建立、移除或更新錯誤時，健全狀況服務會產生 WMI 事件。 這些是讓應用程式狀態保持同步，而不需要經常輪詢的必要專案，而且有助於判斷傳送電子郵件警示的時機，例如。 為了訂閱這些事件，此範例程式碼會再次使用觀察者設計模式。

首先，訂閱**MSFT @ no__t-1StorageFaultEvent**事件。

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

接下來，執行會在每次產生新事件時叫用**iobserver.onnext （）** 方法的觀察者。

每個事件都包含**ChangeType** ，指出是否正在建立、移除或更新錯誤，以及相關的**FaultId**。

此外，它們還包含錯誤本身的所有屬性。

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

### <a name="understand-fault-lifecycle"></a>瞭解錯誤生命週期

錯誤不應標示為「已顯示」或由使用者解決。 當健全狀況服務觀察到問題時，就會建立它們，而且只有在健全狀況服務無法再觀察問題時，才會自動移除。 一般來說，這反映出問題已獲得修正。

不過，在某些情況下，健全狀況服務可能會重新探索錯誤（例如容錯移轉之後，或因為間歇連線等）。 基於這個理由，保存您自己的錯誤表示可能是合理的，因此您可以輕鬆地刪除重複。 如果您傳送電子郵件警示或對等項，這就特別重要。

### <a name="properties-of-faults"></a>錯誤的屬性

下表提供錯誤物件的數個重要屬性。 如需完整的架構，請檢查*storagewmi*中的**MSFT @ no__t-1StorageDiagnoseResult**類別。

| **屬性**              | **範例**                                                     |
|---------------------------|-----------------------------------------------------------------|
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | FaultType. Volume. 容量                      |
| `Reason`                    | 「磁片區的可用空間不足。」                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N。 123456789                                  |
| FaultingObjectLocation    | 機架 A06，RU 25，插槽11                                        |
| RecommendedActions        | {「展開磁片區」，「將工作負載遷移至其他磁片區」。   |

**FaultId**在一個叢集的範圍內是唯一的。

**PerceivedSeverity**PerceivedSeverity = {4，5，6} = {"資訊"，"Warning" 和 "Error"}，或對等的色彩，例如藍色、黃色和紅色。

**FaultingObjectDescription**硬體的元件資訊，通常是軟體物件的空白。

**FaultingObjectLocation**硬體的位置資訊，通常是軟體物件的空白。

**RecommendedActions**建議的動作清單，它們是獨立的，而且沒有特定的順序。 今天，這份清單的長度通常是1。

## <a name="properties-of-fault-events"></a>錯誤事件的屬性

下表提供錯誤事件的數個重要屬性。 如需完整的架構，請檢查*storagewmi*中的**MSFT @ no__t-1StorageFaultEvent**類別。

請注意**ChangeType**，它會指出是否正在建立、移除或更新錯誤，以及**FaultId**。 事件也包含受影響之錯誤的所有屬性。

| **屬性**              | **範例**                                                     |
|---------------------------|-----------------------------------------------------------------|
| ChangeType                | 0                                                               |
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | FaultType. Volume. 容量                      |
| `Reason`                    | 「磁片區的可用空間不足。」                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N。 123456789                                  |
| FaultingObjectLocation    | 機架 A06，RU 25，插槽11                                        |
| RecommendedActions        | {「展開磁片區」，「將工作負載遷移至其他磁片區」。   |

**ChangeType**ChangeType = {0，1，2} = {"Create"，"Remove"，"Update"}。

## <a name="coverage"></a>涵蓋範圍

在 Windows Server 2016 中，健全狀況服務提供下列錯誤涵蓋範圍：  

### <a name="physicaldisk-8"></a>**PhysicalDisk （8）**

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedmedia"></a>FaultTypeFaultType. PhysicalDisk. FailedMedia
* 嚴重性：警告
* 原因： *「實體磁片已失敗」。*
* RecommendedAction: *「更換實體磁片」。*

#### <a name="faulttype-microsofthealthfaulttypephysicaldisklostcommunication"></a>FaultTypeFaultType. PhysicalDisk. LostCommunication
* 嚴重性：警告
* 原因： *「實體磁片的連線已中斷」。*
* RecommendedAction: *「檢查實體磁片是否正常運作並已正確連線」。*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunresponsive"></a>FaultTypeFaultType. PhysicalDisk. 沒有回應
* 嚴重性：警告
* 原因： *「實體磁片出現週期性的無回應」。*
* RecommendedAction: *「更換實體磁片」。*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskpredictivefailure"></a>FaultTypeFaultType. PhysicalDisk. PredictiveFailure
* 嚴重性：警告
* 原因： *「實體磁片故障預測即將發生。」*
* RecommendedAction: *「更換實體磁片」。*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedhardware"></a>FaultTypeFaultType. PhysicalDisk. UnsupportedHardware
* 嚴重性：警告
* 原因： *「實體磁片已隔離，因為解決方案廠商不支援。」*
* RecommendedAction: *「以支援的硬體取代實體磁片」。*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedfirmware"></a>FaultTypeFaultType. PhysicalDisk. UnsupportedFirmware
* 嚴重性：警告
* 原因： *「實體磁片已在隔離中，因為您的解決方案廠商不支援其固件版本」。*
* RecommendedAction: *「將實體磁片上的固件更新為目標版本」。*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunrecognizedmetadata"></a>FaultTypeFaultType. PhysicalDisk. UnrecognizedMetadata
* 嚴重性：警告
* 原因： *「實體磁片具有無法辨識中繼資料」。*
* RecommendedAction: *"此磁片可能包含來自未知存放集區的資料。請先確定此磁片上沒有任何有用的資料，然後重設磁片。」*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedfirmwareupdate"></a>FaultTypeFaultType. PhysicalDisk. FailedFirmwareUpdate
* 嚴重性：警告
* 原因： *「嘗試更新實體磁片上的固件失敗。」*
* RecommendedAction: *「嘗試使用不同的固件二進位檔」。*

### <a name="virtual-disk-2"></a>**虛擬磁片（2）**

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksneedsrepair"></a>FaultTypeFaultType. VirtualDisks. NeedsRepair
* 嚴重性：資訊
* 原因： *"此磁片區上的某些資料未完全復原。它仍然可供存取。」*
* RecommendedAction: *「還原資料的復原」。*

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksdetached"></a>FaultTypeFaultType. VirtualDisks. 已卸離
* 嚴重性：重大
* 原因： *」無法存取磁片區。有些資料可能會遺失。」*
* RecommendedAction: *"檢查所有存放裝置的實體和/或網路連線能力。您可能需要從備份還原。」*

### <a name="pool-capacity-1"></a>**集區容量（1）**

#### <a name="faulttype-microsofthealthfaulttypestoragepoolinsufficientreservecapacityfault"></a>FaultTypeFaultType. StoragePool. InsufficientReserveCapacityFault
* 嚴重性：警告
* 原因： *"存放集區沒有最小的建議保留容量。這可能會限制您在發生磁片磁碟機失敗時還原資料復原的能力。」*
* RecommendedAction: *"將額外的容量新增至存放集區，或釋放容量。最小的建議保留會因部署而有所不同，但大約是2個磁片磁碟機的容量。」*

### <a name="volume-capacity-2sup1sup"></a>**磁片區容量（2）** <sup>1</sup>

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultTypeFaultType. Volume. 容量
* 嚴重性：警告
* 原因： *「磁片區的可用空間不足。」*
* RecommendedAction: *「擴充磁片區或將工作負載遷移至其他磁片區」。*

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultTypeFaultType. Volume. 容量
* 嚴重性：重大
* 原因： *「磁片區的可用空間不足。」*
* RecommendedAction: *「擴充磁片區或將工作負載遷移至其他磁片區」。*

### <a name="server-3"></a>**伺服器（3）**

#### <a name="faulttype-microsofthealthfaulttypeserverdown"></a>FaultTypeFaultType. 伺服器向下
* 嚴重性：重大
* 原因： *「無法連線到伺服器。」*
* RecommendedAction: *「啟動或取代伺服器」。*

#### <a name="faulttype-microsofthealthfaulttypeserverisolated"></a>FaultTypeFaultType。獨立模式
* 嚴重性：重大
* 原因： *「伺服器因連線問題而與叢集隔離。」*
* RecommendedAction: *「如果隔離持續，請檢查網路，或將工作負載遷移至其他節點。」*

#### <a name="faulttype-microsofthealthfaulttypeserverquarantined"></a>FaultTypeFaultType. 伺服器隔離
* 嚴重性：重大
* 原因： *「伺服器因重複失敗而由叢集隔離。」*
* RecommendedAction: *「更換伺服器或修正網路」。*

### <a name="cluster-1"></a>**叢集（1）**

#### <a name="faulttype-microsofthealthfaulttypeclusterquorumwitnesserror"></a>FaultTypeFaultType. ClusterQuorumWitness。錯誤
* 嚴重性：重大
* 原因： *「叢集是離開的一部伺服器失敗。」*
* RecommendedAction: *"檢查見證資源，並視需要重新開機。啟動或取代失敗的伺服器。」*

### <a name="network-adapterinterface-4"></a>**網路介面卡/介面（4）**

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisconnected"></a>FaultTypeFaultType. NetworkAdapter. 已中斷連線
* 嚴重性：警告
* 原因： *「網路介面已中斷連線」。*
* RecommendedAction: *「重新連接網路纜線」。*

#### <a name="faulttype-microsofthealthfaulttypenetworkinterfacemissing"></a>FaultTypeFaultType. NetworkInterface. 遺失
* 嚴重性：警告
* 原因： *「伺服器 {伺服器} 缺少連線到叢集網路 {cluster network} 的網路介面卡。」*
* RecommendedAction: *「將伺服器連接到遺失的叢集網路」。*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterhardware"></a>FaultTypeFaultType. NetworkAdapter. 硬體
* 嚴重性：警告
* 原因： *「網路介面發生硬體故障。」*
* RecommendedAction: *「更換網路介面介面卡」。*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisabled"></a>FaultTypeFaultType. NetworkAdapter. Disabled
* 嚴重性：警告
* 原因： *「網路介面 {網路介面} 未啟用，而且未在使用中。」*
* RecommendedAction: *「啟用網路介面」。*

### <a name="enclosure-6"></a>**主機殼（6）**

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurelostcommunication"></a>FaultTypeFaultType. StorageEnclosure. LostCommunication
* 嚴重性：警告
* 原因： *「存放裝置主機殼的通訊已遺失。」*
* RecommendedAction: *「啟動或更換存放裝置主機殼」。*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurefanerror"></a>FaultTypeFaultType. StorageEnclosure. FanError
* 嚴重性：警告
* 原因： *「儲存主機殼位置 {位置} 的風扇已失敗。」*
* RecommendedAction: *「更換儲存主機殼中的風扇」。*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurecurrentsensorerror"></a>FaultTypeFaultType. StorageEnclosure. CurrentSensorError
* 嚴重性：警告
* 原因： *「儲存主機殼的目前感應器位於位置 {位置} 已失敗。」*
* RecommendedAction: *「更換儲存主機殼中的目前感應器。」*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurevoltagesensorerror"></a>FaultTypeFaultType. StorageEnclosure. VoltageSensorError
* 嚴重性：警告
* 原因： *「儲存主機殼的位置 {位置} 的電壓感應器失敗。」*
* RecommendedAction: *「更換儲存主機殼中的電壓感應器」。*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosureiocontrollererror"></a>FaultTypeFaultType. StorageEnclosure. IoControllerError
* 嚴重性：警告
* 原因： *「儲存主機殼的位置 {位置} 的 IO 控制器已失敗。」*
* RecommendedAction: *「更換儲存主機殼中的 IO 控制器」。*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosuretemperaturesensorerror"></a>FaultTypeFaultType. StorageEnclosure. TemperatureSensorError
* 嚴重性：警告
* 原因： *「儲存主機殼位置 {位置} 的溫度感應器失敗。」*
* RecommendedAction: *「更換儲存主機殼中的溫度感應器」。*

### <a name="firmware-rollout-3"></a>**固件推出（3）**

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfailedmaintenancemode"></a>FaultTypeFaultType. FaultDomain. FailedMaintenanceMode
* 嚴重性：警告
* 原因： *「在執行固件推出時，目前無法進行進度」。*
* RecommendedAction: *「確認所有儲存空間都狀況良好，而且沒有任何容錯網域目前處於維護模式。」*

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfirmwareverifyversionfaile"></a>FaultTypeFaultType. FaultDomain. FirmwareVerifyVersionFaile
* 嚴重性：警告
* 原因： *「在套用固件更新之後，因為無法讀取或非預期的固件版本資訊，所以已取消固件推出」。*
* RecommendedAction: *「一旦固件問題解決，即重新開機固件」。*

#### <a name="faulttype-microsofthealthfaulttypefaultdomaintoomanyfailedupdates"></a>FaultTypeFaultType. FaultDomain. TooManyFailedUpdates
* 嚴重性：警告
* 原因： *「因為有太多實體磁片導致固件更新嘗試失敗，所以已取消固件推出。」*
* RecommendedAction: *「一旦固件問題解決，即重新開機固件」。*

### <a name="storage-qos-3sup2sup"></a>**存放裝置 QoS （3）** <sup>2</sup>

#### <a name="faulttype-microsofthealthfaulttypestorqosinsufficientthroughput"></a>FaultTypeFaultType. StorQos. InsufficientThroughput
* 嚴重性：警告
* 原因： *「儲存體輸送量不足以滿足保留。」*
* RecommendedAction: *「重新設定存放裝置 QoS 原則」。*

#### <a name="faulttype-microsofthealthfaulttypestorqoslostcommunication"></a>FaultTypeFaultType. StorQos. LostCommunication
* 嚴重性：警告
* 原因： *「存放裝置 QoS 原則管理員已失去與磁片區的通訊。」*
* RecommendedAction: *「請重新開機節點 {節點}」*

#### <a name="faulttype-microsofthealthfaulttypestorqosmisconfiguredflow"></a>FaultTypeFaultType. StorQos. MisconfiguredFlow
* 嚴重性：警告
* 原因： *「一或多個儲存體取用者（通常虛擬機器）使用識別碼為 {id} 的不存在原則」。*
* RecommendedAction: *「重新建立任何遺失的儲存體 QoS 原則」。*

<sup>1</sup>表示磁片區已達到 80% 的完整（次要嚴重性）或 90% 已滿（主要嚴重性）。  
<sup>2</sup>表示磁片區上的某些 .vhd 並未達到其最小 IOPS （超過 10% （次要））、30% （主要）或 50% （重大）的輪流24小時時間範圍。  

>[!NOTE]
> 存放裝置機箱組件 (如風扇、電源供應器和感應器) 的健康情況是衍生自 SCSI 機箱服務 (SES)。 如果您的廠商沒有提供這項資訊，「健全狀況服務」就無法顯示它。  

## <a name="see-also"></a>另請參閱

- [Windows Server 2016 中的健全狀況服務](health-service-overview.md)
