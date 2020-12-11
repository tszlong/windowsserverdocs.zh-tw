---
description: 深入瞭解：健全狀況服務錯誤
title: 健全狀況服務錯誤
manager: eldenc
ms.author: cosdar
ms.topic: article
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: 8ac4740481d29f877e70c37993848bb397874691
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97039516"
---
# <a name="health-service-faults"></a>健全狀況服務錯誤

> 適用於：Windows Server 2019、Windows Server 2016

## <a name="what-are-faults"></a>什麼是錯誤

健全狀況服務會持續監視您的儲存空間直接存取叢集，以偵測問題並產生「錯誤」。 一個新的 Cmdlet 會顯示任何目前的錯誤，讓您可以輕鬆地驗證部署的健康情況，而不需要再查看每個實體或功能。 「錯誤」是以精確、容易理解，及可採取動作為設計目標。

每個錯誤都包含五個重要欄位：

-   嚴重性
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
 > 實體位置是衍伸自您的容錯網域組態。 如需容錯網域的詳細資訊，請參閱 [Windows Server 2016 中的容錯網域](fault-domains.md)。 如果您未提供這項資訊，則位置欄位的實用性會較低 - 例如，它可能只會顯示插槽編號。

## <a name="root-cause-analysis"></a>根本原因分析

健全狀況服務可以評估錯誤實體之間的潛在因果，以識別併合並造成相同根本問題的錯誤。 藉由辨識相關聯的影響，可以讓報告較為簡潔。 例如，如果伺服器已關閉，則預期伺服器內的任何磁片磁碟機也都不會有連線能力。 因此，根本原因只會引發一個錯誤，在此案例中為伺服器。

## <a name="usage-in-powershell"></a>PowerShell 中的使用方式

若要在 PowerShell 中查看任何目前的錯誤，請執行下列 Cmdlet：

```PowerShell
Get-StorageSubSystem Cluster* | Debug-StorageSubSystem
```

這會傳回任何會影響整體儲存空間直接存取叢集的錯誤。 這些錯誤通常與硬體或設定有關。 如果沒有任何錯誤，此 Cmdlet 將不會傳回任何內容。

>[!NOTE]
> 在非生產環境中，您也可以自行承擔風險，藉由自行觸發錯誤（例如，藉由移除一個實體磁片或關閉一個節點）來試驗這項功能。 錯誤出現之後，請重新插入實體磁片或重新開機節點，錯誤就會再次消失。

您也可以使用下列 Cmdlet 來查看只影響特定磁片區或檔案共用的錯誤：

```PowerShell
Get-Volume -FileSystemLabel <Label> | Debug-Volume

Get-FileShare -Name <Name> | Debug-FileShare
```

這會傳回只影響特定磁片區或檔案共用的任何錯誤。 這些錯誤最常見的情況與容量規劃、資料復原或功能（例如儲存體服務品質或儲存體複本）有關。

## <a name="usage-in-net-and-c"></a>使用 .NET 和 C#

### <a name="connect"></a>連線

若要查詢健全狀況服務，您必須建立與叢集的 **CimSession** 。 若要這樣做，您將需要一些只能在完整 .NET 中使用的專案，這表示您無法直接從 web 或行動應用程式進行這項作業。 這些程式碼範例會使用 C \# ，這是最直接的資料存取層選項。

```
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

提供的使用者名稱應為目的電腦的本機系統管理員。

建議您以即時方式直接從使用者輸入建立密碼 **SecureString** ，使其密碼永遠不會以純文字儲存在記憶體中。 這有助於減輕各種安全性問題。 但在實務上，以上述方式進行建立通常是為了原型設計之用。

### <a name="discover-objects"></a>探索物件

建立 **CimSession** 之後，您就可以查詢叢集上的 WINDOWS MANAGEMENT INSTRUMENTATION (WMI) 。

在您可以取得錯誤或計量之前，您必須取得數個相關物件的實例。 首先，代表叢集上儲存空間直接存取的 **MSFT \_ StorageSubSystem** 。 使用該功能，您可以取得叢集中的每個 **msft \_ StorageNode** ，以及每個 **msft \_ 磁片** 區的資料磁片區。 最後，您也需要健全狀況服務的 **MSFT \_ StorageHealth**。

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

這些是您在 PowerShell 中使用 **StorageSubSystem**、 **StorageNode** 和 **Volume** 等 Cmdlet 取得的相同物件。

您可以存取所有相同的屬性，記載于 [儲存體管理 API 類別](/previous-versions/windows/desktop/stormgmt/storage-management-api-classes)中。

```
using System.Diagnostics;

foreach (CimInstance Node in Nodes)
{
    // For illustration, write each node's Name to the console. You could also write State (up/down), or anything else!
    Debug.WriteLine("Discovered Node " + Node.CimInstanceProperties["Name"].Value.ToString());
}
```

### <a name="query-faults"></a>查詢錯誤

叫用 **診斷** ，以取得任何範圍為目標 **CimInstance** 的目前錯誤，也就是叢集或任何磁片區。

Windows Server 2016 中每個範圍的可用錯誤完整清單如下所述。

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

### <a name="optional-myfault-class"></a>選用： MyFault 類別

您可以建立並保存自己的錯誤標記法。 例如，這個 **MyFault** 類別會儲存數個錯誤的重要屬性，包括 **FaultId**，稍後可以用來建立更新或移除通知的關聯，或在偵測到相同錯誤的情況下，基於任何原因進行刪除。

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

每個錯誤 (**DiagnoseResult**) 中的屬性完整清單記載于下面。

### <a name="fault-events"></a>錯誤事件

當建立、移除或更新錯誤時，健全狀況服務會產生 WMI 事件。 這些都是讓您的應用程式狀態保持同步而不需要頻繁輪詢的必要專案，而且有助於決定何時傳送電子郵件警示。 為了訂閱這些事件，此範例程式碼會再次使用觀察者設計模式。

首先，訂閱 **MSFT \_ StorageFaultEvent** 事件。

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

接下來，請在每次產生新事件時，執行 **OnNext ( # B1** 方法的觀察者。

每個事件都包含 **ChangeType** ，指出是否正在建立、移除或更新錯誤，以及相關的 **FaultId**。

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

錯誤不應標示為「已顯示」或由使用者解決。 當健全狀況服務觀察到問題時，就會建立它們，而且只有在健全狀況服務無法再觀察到問題時，才會將它們移除。 一般來說，這會反映出問題已獲得修正。

不過，在某些情況下，健全狀況服務的錯誤可能是由 (例如容錯移轉之後，或是因為間歇性的連線能力等 ) 而重新探索。 基於這個理由，保存您自己的錯誤標記法可能會很合理，因此您可以輕鬆地刪除。 如果您傳送電子郵件警示或對等專案，這點特別重要。

### <a name="properties-of-faults"></a>錯誤的屬性

下表提供錯誤物件的數個重要屬性。 如需完整的架構，請檢查 *storagewmi* 中的 **MSFT \_ StorageDiagnoseResult** 類別。

| **屬性**              | **範例**                                                     |
|---------------------------|-----------------------------------------------------------------|
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | FaultType. Volume. 容量                      |
| 原因                    | 「磁片區的可用空間不足。」                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N。 123456789                                  |
| FaultingObjectLocation    | 機架 A06、RU 25、插槽11                                        |
| RecommendedActions        | {"Expand volume."，"將工作負載遷移至其他磁片區"}   |

**FaultId** 在一個叢集的範圍內是唯一的。

**PerceivedSeverity** PerceivedSeverity = {4，5，6} = {"資訊"、"Warning" 和 "Error"}，或對等的色彩，例如藍色、黃色和紅色。

**FaultingObjectDescription** 硬體的部分資訊，通常是軟體物件的空白。

**FaultingObjectLocation** 硬體的位置資訊，通常是軟體物件的空白。

**RecommendedActions** 建議的動作清單，它們是獨立的，而且沒有特定的順序。 目前，這份清單的長度通常是1。

## <a name="properties-of-fault-events"></a>錯誤事件的屬性

下表提供錯誤事件的數個重要屬性。 如需完整的架構，請檢查 *storagewmi* 中的 **MSFT \_ StorageFaultEvent** 類別。

請注意 **ChangeType**，指出是否正在建立、移除或更新錯誤，以及 **FaultId**。 事件也包含受影響錯誤的所有屬性。

| **屬性**              | **範例**                                                     |
|---------------------------|-----------------------------------------------------------------|
| ChangeType                | 0                                                               |
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | FaultType. Volume. 容量                      |
| 原因                    | 「磁片區的可用空間不足。」                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N。 123456789                                  |
| FaultingObjectLocation    | 機架 A06、RU 25、插槽11                                        |
| RecommendedActions        | {"Expand volume."，"將工作負載遷移至其他磁片區"}   |

**ChangeType** ChangeType = {0，1，2} = {"Create"，"Remove"，"Update"}。

## <a name="coverage"></a>涵蓋範圍

在 Windows Server 2016 中，健全狀況服務提供下列錯誤涵蓋範圍：

### <a name="physicaldisk-8"></a>**PhysicalDisk (8)**

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedmedia"></a>FaultType： FaultType. PhysicalDisk. FailedMedia
* 嚴重性：警告
* 原因：「*實體磁片失敗。* 」
* RecommendedAction：「 *更換實體磁片」。*

#### <a name="faulttype-microsofthealthfaulttypephysicaldisklostcommunication"></a>FaultType： FaultType. PhysicalDisk. LostCommunication
* 嚴重性：警告
* 原因：「 *實體磁片的連線已中斷」。*
* RecommendedAction：「 *檢查實體磁片是否正常運作，以及是否已正確連接」。*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunresponsive"></a>FaultType： FaultType. PhysicalDisk. 沒有回應
* 嚴重性：警告
* 原因：「 *實體磁片出現週期性無回應」。*
* RecommendedAction：「 *更換實體磁片」。*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskpredictivefailure"></a>FaultType： FaultType. PhysicalDisk. PredictiveFailure
* 嚴重性：警告
* 原因：「*實體磁片的故障預計很快就會發生。* 」
* RecommendedAction：「 *更換實體磁片」。*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedhardware"></a>FaultType： FaultType. PhysicalDisk. UnsupportedHardware
* 嚴重性：警告
* 原因：「*實體磁片被隔離，因為您的解決方案廠商不支援它。* 」
* RecommendedAction：「 *將實體磁片更換為支援的硬體」。*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedfirmware"></a>FaultType： FaultType. PhysicalDisk. UnsupportedFirmware
* 嚴重性：警告
* 原因：「 *實體磁片在隔離中，因為您的解決方案廠商不支援其固件版本」。*
* RecommendedAction：「將 *實體磁片上的固件更新為目標版本」。*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunrecognizedmetadata"></a>FaultType： FaultType. PhysicalDisk. UnrecognizedMetadata
* 嚴重性：警告
* 原因：「 *實體磁片具有無法辨識中繼資料」。*
* RecommendedAction：「*此磁片可能包含來自不明存放集區的資料。首先，請確定此磁片上沒有任何有用的資料，然後重設磁片。* 」

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedfirmwareupdate"></a>FaultType： FaultType. PhysicalDisk. FailedFirmwareUpdate
* 嚴重性：警告
* 原因：「 *嘗試更新實體磁片上的固件失敗」。*
* RecommendedAction：「*嘗試使用不同的固件二進位* 檔」。

### <a name="virtual-disk-2"></a>**虛擬磁片 (2)**

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksneedsrepair"></a>FaultType： FaultType. VirtualDisks. NeedsRepair
* 嚴重性：告知性
* 原因：「*這個磁片區上的某些資料並未完全復原。它仍然可供存取。* 」
* RecommendedAction：「 *還原資料的復原」。*

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksdetached"></a>FaultType： FaultType VirtualDisks。卸離
* 嚴重性：重大
* 原因：「*無法存取磁片區。某些資料可能會遺失。* 」
* RecommendedAction： *"檢查所有存放裝置的實體和/或網路連線能力。您可能需要從備份還原。* 」

### <a name="pool-capacity-1"></a>**集區容量 (1)**

#### <a name="faulttype-microsofthealthfaulttypestoragepoolinsufficientreservecapacityfault"></a>FaultType： FaultType. StoragePool. InsufficientReserveCapacityFault
* 嚴重性：警告
* 原因：「 *儲存集區沒有最低建議的保留容量。這可能會限制您在發生磁片磁碟機故障 () 時還原資料復原的能力。*
* RecommendedAction：「 *將額外容量新增至存放集區，或釋放容量。最低建議保留會因部署而異，但大約是2個磁片磁碟機的容量。*

### <a name="volume-capacity-2sup1sup"></a>**磁片區容量 (2)** <sup>1</sup>

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType： FaultType。容量
* 嚴重性：警告
* 原因：「*磁片區的可用空間不足。* 」
* RecommendedAction：「*擴充磁片區，或將工作負載遷移至其他磁片* 區」。

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType： FaultType。容量
* 嚴重性：重大
* 原因：「*磁片區的可用空間不足。* 」
* RecommendedAction：「*擴充磁片區，或將工作負載遷移至其他磁片* 區」。

### <a name="server-3"></a>**伺服器 (3)**

#### <a name="faulttype-microsofthealthfaulttypeserverdown"></a>FaultType： FaultType 伺服器。
* 嚴重性：重大
* 原因：「 *無法連線到伺服器」。*
* RecommendedAction： *「啟動或取代伺服器」。*

#### <a name="faulttype-microsofthealthfaulttypeserverisolated"></a>FaultType： FaultType。獨立模式
* 嚴重性：重大
* 原因：「*伺服器因為連接問題而與叢集隔離。* 」
* RecommendedAction： *"如果持續存在，請檢查網路 (s) 或將工作負載遷移至其他節點。*

#### <a name="faulttype-microsofthealthfaulttypeserverquarantined"></a>FaultType： FaultType。已隔離
* 嚴重性：重大
* 原因：「*伺服器因為週期性失敗而被叢集隔離。* 」
* RecommendedAction：「 *取代伺服器或修正網路」。*

### <a name="cluster-1"></a>**Cluster (1)**

#### <a name="faulttype-microsofthealthfaulttypeclusterquorumwitnesserror"></a>FaultType： FaultType. ClusterQuorumWitness. Error
* 嚴重性：重大
* 原因：「叢集 *是一部伺服器失敗而無法關機」。*
* RecommendedAction：「 *檢查見證資源，然後視需要重新開機。啟動或取代失敗的伺服器。」*

### <a name="network-adapterinterface-4"></a>**網路介面卡/介面 (4)**

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisconnected"></a>FaultType： FaultType. NetworkAdapter. Disconnected
* 嚴重性：警告
* 原因：「*網路介面已中斷* 連線」。
* RecommendedAction：「 *重新連接網路纜線」。*

#### <a name="faulttype-microsofthealthfaulttypenetworkinterfacemissing"></a>FaultType： FaultType NetworkInterface。遺漏
* 嚴重性：警告
* 原因：「*伺服器 {server} 的網路介面卡 (s) 連線到叢集網路 {cluster network}。* 」
* RecommendedAction：「將 *伺服器連接到遺失的叢集網路」。*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterhardware"></a>FaultType： FaultType NetworkAdapter. 硬體
* 嚴重性：警告
* 原因：「*網路介面發生硬體失敗。* 」
* RecommendedAction：「 *更換網路介面卡」。*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisabled"></a>FaultType： FaultType NetworkAdapter.. Disabled
* 嚴重性：警告
* 原因：「*網路介面 {network interface} 未啟用且未使用。* 」
* RecommendedAction：「 *啟用網路介面」。*

### <a name="enclosure-6"></a>**主機殼 (6)**

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurelostcommunication"></a>FaultType： FaultType. StorageEnclosure. LostCommunication
* 嚴重性：警告
* 原因：「 *存放裝置主機殼的通訊已遺失」。*
* RecommendedAction： *「啟動或取代存放裝置主機殼」。*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurefanerror"></a>FaultType： FaultType. StorageEnclosure. FanError
* 嚴重性：警告
* 原因：「 *存放裝置主機殼的位置 {position} 上的風扇失敗」。*
* RecommendedAction：「 *更換存放裝置主機殼中的風扇」。*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurecurrentsensorerror"></a>FaultType： FaultType. StorageEnclosure. CurrentSensorError
* 嚴重性：警告
* 原因：「*儲存主機殼的目前位置 {position} 上的感應器失敗。* 」
* RecommendedAction：「 *更換存放裝置主機殼中的目前感應器」。*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurevoltagesensorerror"></a>FaultType： FaultType. StorageEnclosure. VoltageSensorError
* 嚴重性：警告
* 原因：「*儲存主機殼的位置 {position} 的電壓感應器失敗。* 」
* RecommendedAction：「 *更換存放裝置主機殼中的電壓感應器」。*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosureiocontrollererror"></a>FaultType： FaultType. StorageEnclosure. IoControllerError
* 嚴重性：警告
* 原因：「*儲存主機殼的位置 {position} 上的 IO 控制器失敗。* 」
* RecommendedAction：「 *更換儲存主機殼中的 IO 控制器」。*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosuretemperaturesensorerror"></a>FaultType： FaultType. StorageEnclosure. TemperatureSensorError
* 嚴重性：警告
* 原因：「*儲存主機殼的溫度感應器的位置 {position} 失敗。* 」
* RecommendedAction：「 *更換儲存主機殼中的溫度感應器」。*

### <a name="firmware-rollout-3"></a>**(3) 的固件推出**

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfailedmaintenancemode"></a>FaultType： FaultType. FaultDomain. FailedMaintenanceMode
* 嚴重性：警告
* 原因：「 *目前無法在執行固件時進行進度」。*
* RecommendedAction：「 *確認所有儲存空間的狀況良好，而且沒有任何容錯網域目前處於維護模式」。*

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfirmwareverifyversionfaile"></a>FaultType： FaultType. FaultDomain. FirmwareVerifyVersionFaile
* 嚴重性：警告
* 原因：「 *在套用固件更新之後，因為無法讀取或未預期的固件版本資訊，而取消了固件推出」。*
* RecommendedAction：「 *當固件問題解決之後，重新開機固件推出」。*

#### <a name="faulttype-microsofthealthfaulttypefaultdomaintoomanyfailedupdates"></a>FaultType： FaultType. FaultDomain. TooManyFailedUpdates
* 嚴重性：警告
* 原因：「 *因為有太多實體磁片失敗，導致固件更新失敗，所以已取消固件推出」。*
* RecommendedAction：「 *當固件問題解決之後，重新開機固件推出」。*

### <a name="storage-qos-3sup2sup"></a>**儲存體 QoS (3)** <sup>2</sup>

#### <a name="faulttype-microsofthealthfaulttypestorqosinsufficientthroughput"></a>FaultType： FaultType. StorQos. InsufficientThroughput
* 嚴重性：警告
* 原因：「 *儲存體輸送量不足以滿足保留」。*
* RecommendedAction：「 *重新設定存放裝置 QoS 原則」。*

#### <a name="faulttype-microsofthealthfaulttypestorqoslostcommunication"></a>FaultType： FaultType. StorQos. LostCommunication
* 嚴重性：警告
* 原因：「 *存放裝置 QoS 原則管理員已遺失與磁片區的通訊」。*
* RecommendedAction： *"請重新開機節點 {節點}"*

#### <a name="faulttype-microsofthealthfaulttypestorqosmisconfiguredflow"></a>FaultType： FaultType. StorQos. MisconfiguredFlow
* 嚴重性：警告
* 原因：「 *一或多個儲存體取用者 (通常是虛擬機器) 正在使用識別碼為 {id} 的不存在原則」。*
* RecommendedAction：「 *重建任何遺失的儲存體 QoS 原則」。*

<sup>1</sup>  表示磁片區已達 80% full (次要嚴重性) 或 90% full (主要嚴重性) 。
<sup>2</sup> 表示磁片區上的某些 .vhd (s) 未達到其最小 IOPS （超過10%） (次要) 、30% (主要) ，或 50% (輪流24小時時間範圍的重大) 。

>[!NOTE]
> 存放裝置機箱組件 (如風扇、電源供應器和感應器) 的健康情況是衍生自 SCSI 機箱服務 (SES)。 如果您的廠商沒有提供這項資訊，「健全狀況服務」就無法顯示它。

## <a name="additional-references"></a>其他參考資料

- [Windows Server 2016 中的健全狀況服務](health-service-overview.md)
