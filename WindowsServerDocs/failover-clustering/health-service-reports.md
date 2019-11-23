---
title: 健全狀況服務報表
ms.prod: windows-server
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: ''
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: e65db8834bd0b059dc7bbebbcaf9288fb46da225
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369685"
---
# <a name="health-service-reports"></a>健全狀況服務報表
> 適用于： Windows Server 2019、Windows Server 2016

## <a name="what-are-reports"></a>什麼是報表  

健全狀況服務可減少從儲存空間直接存取叢集取得即時效能和容量資訊所需的工作。 其中一個新的 Cmdlet 會提供基本計量的策劃清單，這些度量會在節點之間以動態方式收集和匯總，並具有內建邏輯來偵測叢集成員資格。 所有的值都是即時且為該時間點的值。  

## <a name="usage-in-powershell"></a>在 PowerShell 中的使用方式

使用此 Cmdlet 取得整個儲存空間直接存取叢集的計量：

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport
```

選擇性的**Count**參數表示要傳回多少組值（以一秒為間隔）。  

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport -Count <Count>  
```

您也可以取得一個特定磁片區或伺服器的計量：  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Get-StorageHealthReport -Count <Count>  

Get-StorageNode -Name <Name> | Get-StorageHealthReport -Count <Count>
```

## <a name="usage-in-net-and-c"></a>.NET 和中的使用方式C#

### <a name="connect"></a>連線

為了查詢健全狀況服務，您必須建立叢集的**CimSession** 。 若要這麼做，您將需要一些只在完整 .NET 中提供的專案，這表示您無法直接從 web 或行動裝置應用程式進行這項操作。 這些程式碼範例將使用 C\#，這是最直接的此資料存取層選擇。

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

在您取得錯誤或計量之前，您必須取得數個相關物件的實例。 首先， **MSFT\_StorageSubSystem** ，代表叢集上的儲存空間直接存取。 使用這種方式，您就可以取得叢集中的每個**msft\_StorageNode** ，以及每個**msft\_磁片**區的資料磁片區。 最後，您還需要**MSFT\_StorageHealth**，也就是健全狀況服務本身。

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

叫用**GetReport**以開始串流處理基本計量的策劃清單，這些範例會在節點之間以動態方式收集，並以內建邏輯來偵測叢集成員資格。 之後每秒就會到達範例。 所有的值都是即時且為該時間點的值。

計量可以針對三個範圍進行串流處理：叢集、任何節點或任何磁片區。

Windows Server 2016 中每個領域的可用計量完整清單記載于下面。

### <a name="iobserveronnext"></a>IObserver. Iobserver.onnext （）

這個範例程式碼會使用[觀察者設計模式](https://msdn.microsoft.com/library/ee850490(v=vs.110).aspx)來執行觀察器，其**iobserver.onnext （）** 方法會在每個新的度量樣本抵達時叫用。 如果串流結束，將會呼叫其**OnCompleted （）** 方法。 例如，您可以使用它來重新起始串流，因此它會無限期地繼續。

```
class MetricsObserver<T> : IObserver<T>
{
    public void OnNext(T Result)
    {
        // Cast
        CimMethodStreamedResult StreamedResult = Result as CimMethodStreamedResult;

        if (StreamedResult != null)
        {
            // For illustration, you could store the metrics in this dictionary
            Dictionary<string, string> Metrics = new Dictionary<string, string>();

            // Unpack
            CimInstance Report = (CimInstance)StreamedResult.ItemValue;
            IEnumerable<CimInstance> Records = (IEnumerable<CimInstance>)Report.CimInstanceProperties["Records"].Value;
            foreach (CimInstance Record in Records)
            {
                /// Each Record has "Name", "Value", and "Units"
                Metrics.Add(
                    Record.CimInstanceProperties["Name"].Value.ToString(),
                    Record.CimInstanceProperties["Value"].Value.ToString()
                    );
            }

            // TODO: Whatever you want!
        }
    }
    public void OnError(Exception e)
    {
        // Handle Exceptions
    }
    public void OnCompleted()
    {
        // Reinvoke BeginStreamingMetrics(), defined in the next section
    }
}
```

### <a name="begin-streaming"></a>開始串流

定義觀察者之後，您就可以開始串流處理。

指定您想要設定計量範圍的目標**CimInstance** 。 它可以是叢集、任何節點或任何磁片區。

Count 參數是串流結束前的樣本數。

```
CimInstance Target = Cluster; // From among the objects discovered in DiscoverObjects()

public void BeginStreamingMetrics(CimSession Session, CimInstance HealthService, CimInstance Target)
{
    // Set Parameters
    CimMethodParametersCollection MetricsParams = new CimMethodParametersCollection();
    MetricsParams.Add(CimMethodParameter.Create("TargetObject", Target, CimType.Instance, CimFlags.In));
    MetricsParams.Add(CimMethodParameter.Create("Count", 999, CimType.UInt32, CimFlags.In));
    // Enable WMI Streaming
    CimOperationOptions Options = new CimOperationOptions();
    Options.EnableMethodResultStreaming = true;
    // Invoke API
    CimAsyncMultipleResults<CimMethodResultBase> InvokeHandler;
    InvokeHandler = Session.InvokeMethodAsync(
        HealthService.CimSystemProperties.Namespace, HealthService, "GetReport", MetricsParams, Options
        );
    // Subscribe the Observer
    MetricsObserver<CimMethodResultBase> Observer = new MetricsObserver<CimMethodResultBase>(this);
    IDisposable Disposeable = InvokeHandler.Subscribe(Observer);
}
```

不用說，這些計量可以視覺化、儲存在資料庫中，或以您認為適合的任何方式使用。

### <a name="properties-of-reports"></a>報表的屬性

計量的每個範例都是一個「報表」，其中包含多個對應于個別計量的「記錄」。

如需完整的架構，請檢查*storagewmi*中的**msft\_StorageHealthReport**和**msft\_HealthRecord**類別。

根據此資料表，每個計量都只有三個屬性。

| **Property** | **範例**       |
| -------------|-------------------|
| 名稱         | IOLatencyAverage  |
| 值        | 0.00021           |
| 單位        | 3                 |

單位 = {0，1，2，3，4}，其中 0 = "Bytes"，1 = "BytesPerSecond"，2 = "CountPerSecond"，3 = "Seconds"，或 4 = "百分比"。

## <a name="coverage"></a>涵蓋範圍

以下是 Windows Server 2016 中每個領域的可用計量。

### <a name="msft_storagesubsystem"></a>MSFT_StorageSubSystem

| **名稱**                        | **數量** |
|---------------------------------|-----------|
| CPUUsage                        | 4         |
| CapacityPhysicalPooledAvailable | 0         |
| CapacityPhysicalPooledTotal     | 0         |
| CapacityPhysicalTotal           | 0         |
| CapacityPhysicalUnpooled        | 0         |
| CapacityVolumesAvailable        | 0         |
| CapacityVolumesTotal            | 0         |
| IOLatencyAverage                | 3         |
| IOLatencyRead                   | 3         |
| IOLatencyWrite                  | 3         |
| IOPSRead                        | 2         |
| IOPSTotal                       | 2         |
| IOPSWrite                       | 2         |
| IOThroughputRead                | 1         |
| IOThroughputTotal               | 1         |
| IOThroughputWrite               | 1         |
| MemoryAvailable                 | 0         |
| MemoryTotal                     | 0         |


### <a name="msft_storagenode"></a>MSFT_StorageNode

| **名稱**            | **數量** |
|---------------------|-----------|
| CPUUsage            | 4         |
| IOLatencyAverage    | 3         |
| IOLatencyRead       | 3         |
| IOLatencyWrite      | 3         |
| IOPSRead            | 2         |
| IOPSTotal           | 2         |
| IOPSWrite           | 2         |
| IOThroughputRead    | 1         |
| IOThroughputTotal   | 1         |
| IOThroughputWrite   | 1         |
| MemoryAvailable     | 0         |
| MemoryTotal         | 0         |

### <a name="msft_volume"></a>MSFT_Volume

| **名稱**            | **數量** |
|---------------------|-----------|
| CapacityAvailable   | 0         |
| CapacityTotal       | 0         |
| IOLatencyAverage    | 3         |
| IOLatencyRead       | 3         |
| IOLatencyWrite      | 3         |
| IOPSRead            | 2         |
| IOPSTotal           | 2         |
| IOPSWrite           | 2         |
| IOThroughputRead    | 1         |
| IOThroughputTotal   | 1         |
| IOThroughputWrite   | 1         |

## <a name="see-also"></a>請參閱

- [Windows Server 2016 中的健全狀況服務](health-service-overview.md)
