---
title: 健全狀況服務報告
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: ''
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: e018c0270a0bf410dada9c05d2c25e51fdfac1d8
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280158"
---
# <a name="health-service-reports"></a>健全狀況服務報告
> 適用於：Windows Server 2019，Windows Server 2016

## <a name="what-are-reports"></a>報告有哪些？  

健全狀況服務可減少從您的儲存空間直接存取叢集取得即時的效能和容量資訊所需的工作。 一個新的 cmdlet 提供重要度量，可有效率地收集並動態彙總到節點，內建的邏輯，可偵測叢集成員資格的策劃的的清單。 所有的值都是即時且為該時間點的值。  

## <a name="usage-in-powershell"></a>在 PowerShell 中的使用方式

您可以使用這個指令程式取得整個儲存空間直接存取叢集中的度量：

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport
```

選擇性**計數**參數會指出多少組的值傳回，在 1 秒的間隔。  

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport -Count <Count>  
```

您也可以針對一個特定的磁碟區或伺服器取得度量資訊：  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Get-StorageHealthReport -Count <Count>  

Get-StorageNode -Name <Name> | Get-StorageHealthReport -Count <Count>
```

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

叫用**GetReport**若要開始串流專家策劃的重要度量，可有效率地收集並動態彙總到節點，內建的邏輯，可偵測叢集成員資格清單的範例。 範例會送達之後每隔一秒。 所有的值都是即時且為該時間點的值。

計量可以串流的三個範圍： 叢集、 任何節點或任何磁碟區。

在 Windows Server 2016 中的每個範圍的可用度量的完整清單會如下所述。

### <a name="iobserveronnext"></a>IObserver.OnNext()

此範例程式碼會使用[觀察者設計模式](https://msdn.microsoft.com/library/ee850490(v=vs.110).aspx)實作觀察者其**OnNext()** 度量的每個新的範例時，會叫用方法。 其**OnCompleted()** 方法會呼叫如果/當串流結束時。 比方說，您可能會並用來重新初始化資料流中，因此它會繼續無限期。

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

定義觀察者，您可以開始串流處理。

指定目標**CimInstance**至想要計量的範圍。 它可以是叢集、 任何節點或任何磁碟區。

計數參數串流結束之前的範例數。

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

不用說，這些計量可以視覺化，儲存在資料庫中，或使用您喜歡的方式。

### <a name="properties-of-reports"></a>報表的屬性

度量的每個範例是一個 「 報告 」 包含許多 「 記錄 」 對應至個別的計量。

針對完整的結構描述中，檢查**MSFT\_StorageHealthReport**並**MSFT\_HealthRecord**中的類別*storagewmi.mof*。

每個計量都有三個屬性，每個此資料表。

| **屬性** | **範例**       |
| -------------|-------------------|
| 名稱         | IOLatencyAverage  |
| 值        | 0.00021           |
| 單位        | 3                 |

單位 = {0、 1、 2、 3、 4}，0 ="Bytes"，1 ="BytesPerSecond"，2 ="CountPerSecond"，3 ="Seconds"或 4 = 「 百分比 」。

## <a name="coverage"></a>涵蓋範圍

以下是計量適用於 Windows Server 2016 中的每個範圍。

### <a name="msftstoragesubsystem"></a>MSFT_StorageSubSystem

| **名稱**                        | **單位** |
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


### <a name="msftstoragenode"></a>MSFT_StorageNode

| **名稱**            | **單位** |
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

### <a name="msftvolume"></a>MSFT_Volume

| **名稱**            | **單位** |
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

## <a name="see-also"></a>另請參閱

- [Windows Server 2016 中的健全狀況服務](health-service-overview.md)
