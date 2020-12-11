---
description: 深入瞭解：健全狀況服務報表
title: 健全狀況服務報表
manager: eldenc
ms.author: cosdar
ms.topic: article
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: e453744524381240f8b870326275fae56eca1635
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97039496"
---
# <a name="health-service-reports"></a>健全狀況服務報表

> 適用於：Windows Server 2019、Windows Server 2016

## <a name="what-are-reports"></a>什麼是報表

健全狀況服務可減少從儲存空間直接存取叢集取得即時效能和容量資訊所需的工作。 其中一個新的 Cmdlet 提供基本計量的策劃清單，這些計量可透過內建的邏輯來偵測叢集成員資格，以有效率地跨節點收集並動態地匯總。 所有的值都是即時且為該時間點的值。

## <a name="usage-in-powershell"></a>PowerShell 中的使用方式

使用這個 Cmdlet 取得整個儲存空間直接存取叢集的度量：

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport
```

選擇性的 **Count** 參數表示要傳回的值集合數目（以一秒為間隔）。

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport -Count <Count>
```

您也可以取得一個特定磁片區或伺服器的計量：

```PowerShell
Get-Volume -FileSystemLabel <Label> | Get-StorageHealthReport -Count <Count>

Get-StorageNode -Name <Name> | Get-StorageHealthReport -Count <Count>
```

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

叫用 **GetReport** 來開始串流處理基本計量策劃清單的串流範例，這些範例會使用內建的邏輯來偵測叢集成員資格，以有效率地跨節點收集並動態地匯總。 之後每秒都會抵達範例。 所有的值都是即時且為該時間點的值。

計量可以串流處理三個範圍：叢集、任何節點或任何磁片區。

您可以在 Windows Server 2016 的每個範圍取得可用計量的完整清單，如下所述。

### <a name="iobserveronnext"></a>IObserver. OnNext ( # A1

此範例程式碼會使用 [觀察者設計模式](/dotnet/standard/events/observer-design-pattern) 來執行觀察器，其 **OnNext ( # B1** 方法會在每個新的度量樣本抵達時叫用。 如果串流結束，則會呼叫其 **OnCompleted ( # B1** 方法。 例如，您可以使用它來重新起始串流，因此它會無限期地繼續進行。

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

指定您想要設定計量範圍的目標 **CimInstance** 。 它可以是叢集、任何節點或任何磁片區。

Count 參數是串流結束之前的樣本數。

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

不用說，這些計量可以視覺化、儲存在資料庫中，或以您所能使用的任何方式使用。

### <a name="properties-of-reports"></a>報表的屬性

計量的每個範例都是一個「報表」，其中包含與個別計量對應的許多「記錄」。

如需完整的架構，請檢查 *storagewmi* 中的 **Msft \_ StorageHealthReport** 和 **msft \_ HealthRecord** 類別。

每個度量都只有三個屬性，每個都有此資料表。

| **屬性** | **範例**       |
| -------------|-------------------|
| 名稱         | IOLatencyAverage  |
| 值        | 0.00021           |
| 單位        | 3                 |

單位 = {0，1，2，3，4}，其中 0 = "Bytes"，1 = "BytesPerSecond"，2 = "CountPerSecond"，3 = "Seconds" 或 4 = "百分比"。

## <a name="coverage"></a>涵蓋範圍

以下是 Windows Server 2016 中每個範圍的可用計量。

### <a name="msft_storagesubsystem"></a>MSFT_StorageSubSystem

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


### <a name="msft_storagenode"></a>MSFT_StorageNode

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

### <a name="msft_volume"></a>MSFT_Volume

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

## <a name="additional-references"></a>其他參考資料

- [Windows Server 2016 中的健全狀況服務](health-service-overview.md)
