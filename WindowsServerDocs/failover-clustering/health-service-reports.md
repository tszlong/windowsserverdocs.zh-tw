---
title: "健康服務報告"
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: bc21b9fdec5700fec23dc6af7ca15873ded34bea
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="health-service-reports"></a>健康服務報告
> 適用於 Windows Server 2016

## <a name="what-are-reports"></a>報告為何？  

健康服務降低需要取得動態效能與容量的資訊，從您的儲存空間直接存取叢集的工作。 一個新 cmdlet 提供基本計量、 有效率地收集，是動態累積節點，以建邏輯偵測叢集成員資格 curated 的的清單。 所有值只都是即時與的時間點。  

## <a name="usage-in-powershell"></a>使用中的 PowerShell

使用下列 cmdlet 取得儲存空間直接存取整個叢集計量︰

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport
```

選擇性**計數**參數指出多少設值的退貨，在一個第二個時間間隔。  

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport -Count <Count>  
```

您也可以一個特定的音量或伺服器取得計量︰  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Get-StorageHealthReport -Count <Count>  

Get-StorageNode -Name <Name> | Get-StorageHealthReport -Count <Count>
```

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

叫用**GetReport**以開始串流處理我們專家-整理清單的基本計量、有效率地收集，是動態累積節點，以建邏輯偵測叢集成員資格的範例。 樣本將會抵達之後每秒。 所有值只都是即時與的時間點。

有三種範圍的可以串流處理計量︰ 叢集、任何] 節點或任何磁碟區。

下列被記載計量可在 Windows Server 2016 中的每個範圍的完整清單。

### <a name="iobserveronnext"></a>IObserver.OnNext()

範例程式碼使用[觀察者設計模式](https://msdn.microsoft.com/en-us/library/ee850490(v=vs.110).aspx)實作觀察者的**OnNext()**計量的每個新範例到達時，將會叫用方法。 其**OnCompleted()**方法將會被如何稱呼如果/串流結束時。 例如，您可能會使用該重新起始串流處理，讓它永遠會繼續。

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

### <a name="begin-streaming"></a>開始串流處理

使用定義觀察者，您就可以開始串流處理。

指定目標**CimInstance**以您想計量範圍。 它可以是叢集、任何] 節點或任何磁碟區。

計數參數串流結束之前是範例數目。

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

可想而知，這些計量可以視覺化、儲存在資料庫中，或使用您它們的方式。

### <a name="properties-of-reports"></a>屬性的報告

計量的每一個範例是一個」報告」其中包含許多」記錄」個人計量相對應。

完整架構、檢查**MSFT\_StorageHealthReport**和**MSFT\_HealthRecord**中類別*storagewmi.mof*。

每個計量具有每本表只需三個屬性。

| **屬性** | **範例**       |
| -------------|-------------------|
| 名稱         | IOLatencyAverage  |
| 值。        | 0.00021           |
| 單位        | 3                 |

單位 = {0 1、2、3、4}、0 =」位元組」，1 =」BytesPerSecond」，2 =」CountPerSecond」，3 =」秒」或 4 =」百分比」。

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

## <a name="see-also"></a>也了

- [Windows Server 2016 中 health 服務](health-service-overview.md)
