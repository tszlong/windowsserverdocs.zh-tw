---
title: 主機計算網路 (HCN) 案例
ms.author: jmesser
author: jmesser81
ms.date: 11/05/2018
ms.openlocfilehash: c6b09ec65bd76fb63c2bb5c4eb5da1187f62ca75
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955675"
---
# <a name="common-scenarios"></a>常見案例

>適用于： Windows Server (半年通道) 、Windows Server 2019

## <a name="scenario-hcn"></a>案例： HCN


### <a name="create-an-hcn"></a>建立 HCN

此範例示範如何使用主機計算網路服務 API，在主機上建立主機計算網路，以用來將虛擬 NIC 連線至虛擬機器或容器。

```C++
using unique_hcn_network = wil::unique_any<
    HCN_NETWORK,
    decltype(&HcnCloseNetwork),
    HcnCloseNetwork>;


/// Creates a simple HCN Network, waiting synchronously to finish the task
void CreateHcnNetwork()
{

    unique_hcn_network hcnnetwork;
    wil::unique_cotaskmem_string result;
    std::wstring settings = LR"(
    {
        "SchemaVersion": {
            "Major": 2,
            "Minor": 0
        },
        "Owner" : "WDAGNetwork",
        "Flags" : 0,
        "Type"  : 0,
        "Ipams" : [
            {
                "Type" : 0,
                "Subnets" : [
                    {
                        "IpAddressPrefix" : "192.168.1.0/24",
                        "Policies" : [
                            {
                                "Type" : "VLAN",
                                "IsolationId" : 100,
                            }
                        ],
                        "Routes" : [
                            {
                                "NextHop" : "192.168.1.1",
                                "DestinationPrefix" : "0.0.0.0/0",
                            }
                        ]

                    }
                ],
            },
        ],
        "MacPool":  {
            "Ranges" : [
                {
                    "EndMacAddress":  "00-15-5D-52-CF-FF",
                    "StartMacAddress":  "00-15-5D-52-C0-00"
                }
            ],
        },
        "Dns" : {
            "Suffix" : "net.home",
            "ServerList" : ["10.0.0.10"],
        }
    }
    })";

    GUID networkGuid;
    HRESULT result = CoCreateGuid(&networkGuid);

    result = HcnCreateNetwork(
        networkGuid,              // Unique ID
        settings.c_str(),      // Compute system settings document
        &hcnnetwork,
        &result
        );
    if (FAILED(result))
    {
                    // UnMarshal  the result Json
     // ErrorSchema
        //   {
        //    "ErrorCode" : <uint32>,
        //    "Error" : <string>,
        //    "Success" : <bool>,
       //   }

        // Failed to create network
        THROW_HR(result);
    }

    // Close the Handle
    result = HcnCloseNetwork(hcnnetwork.get());

    if (FAILED(result))
    {
        // UnMarshal  the result Json
        THROW_HR(result);
    }

}
```

### <a name="delete-an-hcn"></a>刪除 HCN

此範例說明如何使用主機計算網路服務 API 來開啟 & 刪除主機計算網路

```C++
    wil::unique_cotaskmem_string errorRecord;
    GUID networkGuid; // Initialize it to appropriate network guid value
    HRESULT hr = HcnDeleteNetwork(networkGuid, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal the result Json
        THROW_HR(hr);
    }
```


### <a name="enumerate-all-networks"></a>列舉所有網路

此範例說明如何使用主機計算網路服務 API 來列舉所有主機計算網路。

```C++
     wil::unique_cotaskmem_string resultNetworks;
     wil::unique_cotaskmem_string errorRecord;

     // Filter to select Networks based on properties
     std::wstring filter [] = LR"(
     {
         "Name"  : "WDAG",
     })";
     HRESULT result = HcnEnumerateNetworks(filter.c_str(), &resultNetworks, &errorRecord);
     if (FAILED(result))
     {
         // UnMarshal  the result Json

         THROW_HR(result);
     }
```


### <a name="query-network-properties"></a>查詢網路屬性

此範例示範如何使用主機計算網路服務 API 來查詢網路屬性。

```C++
    unique_hcn_network hcnnetwork;
    wil::unique_cotaskmem_string errorRecord;
    wil::unique_cotaskmem_string properties;
    std:wstring query = LR"(
    {
        // Future
    })";
    GUID networkGuid; // Initialize it to appropriate network guid value
    HRESULT hr = HcnOpenNetwork(networkGuid, &hcnnetwork, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }


    hr = HcnQueryNetworkProperties(hcnnetwork.get(), query.c_str(), &properties, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
```


## <a name="scenario-hcn-endpoint"></a>案例： HCN 端點

### <a name="create-an-hcn-endpoint"></a>建立 HCN 端點

此範例示範如何使用主機計算網路服務 API 來建立主機計算網路端點，然後將其熱新增至虛擬機器或容器。

```C++
using unique_hcn_endpoint = wil::unique_any<
    HCN_ENDPOINT,
    decltype(&HcnCloseEndpoint),
    HcnCloseEndpoint>;

void CreateAndHotAddEndpoint()
{
    unique_hcn_endpoint hcnendpoint;
    unique_hcn_network hcnnetwork;

    wil::unique_cotaskmem_string errorRecord;


    std::wstring settings[] = LR"(
    {
        "SchemaVersion": {
            "Major": 2,
            "Minor": 0
        },
        "Owner" : "Sample",
                   "Flags" : 0,
        "HostComputeNetwork" : "87fdcf16-d210-426e-959d-2a1d4f41d6d3",
        "DNS" : {
            "Suffix" : "net.home",
            "ServerList" : "10.0.0.10",
        }
    })";
    GUID endpointGuid;
    HRESULT result = CoCreateGuid(&endpointGuid);

    result = HcnOpenNetwork(
        networkGuid,              // Unique ID
        &hcnnetwork,
        &errorRecord
        );
    if (FAILED(result))
    {
        // Failed to find network
        THROW_HR(result);
    }

    result = HcnCreateEndpoint(
        hcnnetwork.get(),
        endpointGuid,              // Unique ID
        settings.c_str(),      // Compute system settings document
        &hcnendpoint,
        &errorRecord
        );

    if (FAILED(result))
    {
        // Failed to create endpoint
        THROW_HR(result);
    }

    // Can use the sample from HCS API Spec on how to attach this endpoint
    // to the VM using AddNetworkAdapterToVm

    result = HcnCloseEndpoint(hcnendpoint.get());

    if (FAILED(result))
    {
        // UnMarshal  the result Json
        THROW_HR(result);
    }

}
```


### <a name="delete-an-endpoint"></a>刪除端點

此範例示範如何使用主機計算網路服務 API 來刪除主機計算網路端點。

```C++
    wil::unique_cotaskmem_string errorRecord;
    GUID endpointGuid; // Initialize it to appropriate endpoint guid value
    HRESULT hr = HcnDeleteEndpoint(endpointGuid, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
```


### <a name="modify-and-endpoint"></a>修改和端點

此範例說明如何使用主機計算網路服務 API 來修改主機計算網路端點。

```C++
    unique_hcn_endpoint hcnendpoint;
    GUID endpointGuid; // Initialize it to appropriate endpoint guid value

    HRESULT hr = HcnOpenEndpoint(endpointGuid, &hcnendpoint, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }


    std::wstring  ModifySettingAddPortJson = LR"(
    {
        "ResourceType" : 0,
        "RequestType" : 0,
        "Settings" : {
            "PortName" : "acbd341a-ec08-44c0-9d5e-61af0ee86902"
            "VirtualNicName" : "641313e1-7ae8-4ddb-94e5-3215f3a0b218--87fdcf16-d210-426e-959d-2a1d4f41d6d1"
            "VirtualMachineId" : "641313e1-7ae8-4ddb-94e5-3215f3a0b218"
        }
    }
    )";


    hr = HcnModifyEndpoint(hcnendpoint.get(), ModifySettingAddPortJson.c_str(), &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
```


### <a name="enumerate-all-enpoints"></a>列舉所有 enpoints

此範例說明如何使用主機計算網路服務 API 來列舉所有主機計算網路端點。

```C++
    wil::unique_cotaskmem_string errorRecord;

    wil::unique_cotaskmem_string resultEndpoints;
    wil::unique_cotaskmem_string errorRecord;

    // Filter to select Endpoint based on properties
    std::wstring filter [] = LR"(
    {
        "Name"  : "sampleNetwork",
    })";
    result = HcnEnumerateEndpoints(filter.c_str(), &resultEndpoints, &errorRecord);
    if (FAILED(result))
    {
        THROW_HR(result);
    }
```


### <a name="query-endpoint-properties"></a>查詢端點屬性

此範例示範如何使用主機計算網路服務 API 來查詢主機計算網路端點的所有屬性。

```C++
    unique_hcn_endpoint hcnendpoint;
    wil::unique_cotaskmem_string errorRecord;
    GUID endpointGuid; // Initialize it to appropriate endpoint guid value

    HRESULT hr = HcnOpenEndpoint(endpointGuid, &hcnendpoint, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }



    wil::unique_cotaskmem_string properties;
    std:wstring query = LR"(
    {
        // Future
    })";

    hr = HcnQueryEndpointProperties(hcnendpoint.get(), query.c_str(), &properties, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal  the errorRecord Json
        THROW_HR(hr);
    }
```


## <a name="scenario-hcn-namespace"></a>案例： HCN 命名空間

### <a name="create-an-hcn-namespace"></a>建立 HCN 命名空間

此範例示範如何使用主機計算網路服務 API，在主機上建立可用於連接端點和容器的主機計算網路命名空間。

```C++
using unique_hcn_namespace = wil::unique_any<
    HCN_NAMESPACE,
    decltype(&HcnCloseNamespace),
    HcnCloseNamespace>;

/// Creates a simple HCN Network, waiting synchronously to finish the task
void CreateHcnNamespace()
{

    unique_hcn_namespace handle;
    wil::unique_cotaskmem_string errorRecord;
    std::wstring settings = LR"(
    {
        "SchemaVersion": {
            "Major": 2,
            "Minor": 0
        },
        "Owner" : "Sample",
        "Flags" : 0,
        "Type" : 0,
    })";

    GUID namespaceGuid;
    HRESULT result = CoCreateGuid(&namespaceGuid);

    result = HcnCreateNamespace(
        namespaceGuid,              // Unique ID
        settings.c_str(),      // Compute system settings document
        &handle,
        &errorRecord
        );
    if (FAILED(result))
    {
                    // UnMarshal  the result Json
     // ErrorSchema
        //   {
        //    "ErrorCode" : <uint32>,
        //    "Error" : <string>,
        //    "Success" : <bool>,
       //   }

        // Failed to create network
        THROW_HR(result);
    }

    result = HcnCloseNamespace(handle.get());

    if (FAILED(result))
    {
        // UnMarshal  the result Json
        THROW_HR(result);
    }

}
```


### <a name="delete-an-hcn-namespace"></a>刪除 HCN 命名空間

此範例示範如何使用主機計算網路服務 API 來刪除主機計算網路命名空間。

```C++
    wil::unique_cotaskmem_string errorRecord;
    GUID namespaceGuid; // Initialize it to appropriate namespace guid value
    HRESULT hr = HcnDeleteNamespace(namespaceGuid, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal the result Json
        THROW_HR(hr);
    }

```


### <a name="modify-an-hcn-namespace"></a>修改 HCN 命名空間

此範例說明如何使用主機計算網路服務 API 來修改主機計算網路命名空間。

```C++
    unique_hcn_namespace handle;
    GUID namespaceGuid; // Initialize it to appropriate namespace guid value
    HRESULT hr = HcnOpenNamespace(namespaceGuid, &handle, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }

    wil::unique_cotaskmem_string errorRecord;
    static std::wstring  ModifySettingAddEndpointJson = LR"(
    {
        "ResourceType" : 1,
        "ResourceType" : 0,
        "Settings" : {
            "EndpointId" : "87fdcf16-d210-426e-959d-2a1d4f41d6d1"
        }
    }
    )";


    hr = HcnModifyNamespace(handle.get(), ModifySettingAddEndpointJson.c_str(), &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal the result Json
        THROW_HR(hr);
    }
    hr = HcnCloseNamespace(handle.get());

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }

```


### <a name="enumerate-all-namespaces"></a>列舉所有命名空間

此範例說明如何使用主機計算網路服務 API 來列舉所有主機計算網路命名空間。

```C++
    wil::unique_cotaskmem_string resultNamespaces;
    wil::unique_cotaskmem_string errorRecord;

    std::wstring filter [] = LR"(
    {
            // Future
    })";
    HRESULT hr = HcnEnumerateNamespace(filter.c_str(), &resultNamespaces, &errorRecord);
    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }

```


### <a name="query-namespace-properties"></a>查詢命名空間屬性

此範例說明如何使用主機計算網路服務 API 來查詢主機計算網路命名空間屬性

```C++
    unique_hcn_namespace handle;
    GUID namespaceGuid; // Initialize it to appropriate namespace guid value
    HRESULT hr = HcnOpenNamespace(namespaceGuid, &handle, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }


    wil::unique_cotaskmem_string errorRecord;
    wil::unique_cotaskmem_string properties;
    std:wstring query = LR"(
    {
        // Future
    })";

    HRESULT hr = HcnQueryNamespaceProperties(handle.get(), query.c_str(), &properties, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }

```


## <a name="scenario-hcn-load-balancer"></a>案例： HCN 負載平衡器

### <a name="create-an-hcn-load-balancer"></a>建立 HCN 負載平衡器

這個範例會示範如何使用主機計算網路服務 API，在主機上建立主機計算網路 Load Balancer，以用來在計算之間平衡端點的負載。

```C++
using unique_hcn_loadbalancer = wil::unique_any<
    HCN_LOADBALANCER,
    decltype(&HcnCloseLoadBalancer),
    HcnCloseLoadBalancer>;

/// Creates a simple HCN LoadBalancer, waiting synchronously to finish the task
void CreateHcnLoadBalancer()
{

    unique_hcn_loadbalancer handle;
    wil::unique_cotaskmem_string errorRecord;
    std::wstring settings = LR"(
     {
        "SchemaVersion": {
            "Major": 2,
            "Minor": 0
        },
        "Owner" : "Sample",
        "HostComputeEndpoints" : [
            "87fdcf16-d210-426e-959d-2a1d4f41d6d1"
        ],
        "VirtualIPs" : [ "10.0.0.10" ],
        "PortMappings" : [
            {
                "Protocol" : 0,
                "InternalPort" : 8080,
                "ExternalPort" : 80,
            }
        ],
        "EnableDirectServerReturn" : true,
        "InternalLoadBalancer" : false,
    }
     )";

    GUID lbGuid;
    HRESULT result = CoCreateGuid(&lbGuid);


    HRESULT hr = HcnCreateLoadBalancer(
        lbGuid,              // Unique ID
        settings.c_str(),      // LoadBalancer settings document
        &handle,
        &errorRecord
        );
    if (FAILED(hr))
    {
                    // UnMarshal  the result Json
     // ErrorSchema
        //   {
        //    "ErrorCode" : <uint32>,
        //    "Error" : <string>,
        //    "Success" : <bool>,
       //   }

        // Failed to create network
        THROW_HR(hr);
    }

    hr = HcnCloseLoadBalancer(handle.get());

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }

}
```


### <a name="delete-an-hcn-load-balancer"></a>刪除 HCN 負載平衡器

此範例示範如何使用主機計算網路服務 API 來刪除主機計算網路平衡器。

```C++
    wil::unique_cotaskmem_string errorRecord;
    GUID lbGuid; // Initialize it to appropriate loadbalancer guid value
    HRESULT hr = HcnDeleteLoadBalancer(lbGuid , &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal the result Json
        THROW_HR(hr);
    }
```


### <a name="modify-an-hcn-load-balancer"></a>修改 HCN 負載平衡器

此範例說明如何使用主機計算網路服務 API 來修改主機計算網路命名空間。

```C++
    unique_hcn_loadbalancer handle;
    GUID lbGuid; // Initialize it to appropriate loadbalancer guid value

    HRESULT hr = HcnOpenLoadBalancer(lbGuid, &handle, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }

    wil::unique_cotaskmem_string errorRecord;
    static std::wstring  ModifySettingAddEndpointJson = LR"(
    {
        "ResourceType" : 1,
        "ResourceType" : 0,
        "Settings" : {
            "EndpointId" : "87fdcf16-d210-426e-959d-2a1d4f41d6d1"
        }
    }
    )";


    hr = HcnModifyLoadBalancer(handle.get(), ModifySettingAddEndpointJson.c_str(), &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal the result Json
        THROW_HR(hr);
    }
    hr = HcnCloseLoadBalancer(handle.get());

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
```


### <a name="enumerate-all-load-balancers"></a>列舉所有負載平衡器

此範例示範如何使用主機計算網路服務 API 來列舉所有主機計算網路 Load Balancer。

```C++
    wil::unique_cotaskmem_string resultLoadBalancers;
    wil::unique_cotaskmem_string errorRecord;

    std::wstring filter [] = LR"(
    {
         // Future

    })";
    HRESULT result = HcnEnumerateLoadBalancers(filter.c_str(), & resultLoadbalancers, &errorRecord);
    if (FAILED(result))
    {
            // UnMarshal  the result Json

            THROW_HR(result);
    }
```


### <a name="query-load-balancer-properties"></a>查詢負載平衡器屬性

此範例示範如何使用主機計算網路服務 API 來查詢主機計算網路平衡器屬性。

```C++
    unique_hcn_loadbalancer handle;
    GUID lbGuid; // Initialize it to appropriate loadbalancer guid value

    HRESULT hr = HcnOpenLoadBalancer(lbGuid, &handle, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }


    wil::unique_cotaskmem_string errorRecord;
    wil::unique_cotaskmem_string properties;
    std:wstring query = LR"(
    {
        "ID"  : "",
        "Type" : 0,
    })";

    hr = HcnQueryNProperties(handle.get(), query.c_str(), &properties, &errorRecord);

    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
```


## <a name="scenario-hcn-notifications"></a>案例： HCN 通知

### <a name="register-and-unregister-service-wide-notifications"></a>註冊及取消註冊整個服務的通知

這個範例示範如何使用主機計算網路服務 API 來註冊和取消註冊整個服務的通知。 這可讓呼叫者透過在) 註冊期間所指定的回呼函式（如新的網路建立事件發生）來接收通知 (。

```C++
using unique_hcn_callback = wil::unique_any<
    HCN_CALLBACK,
    decltype(&HcnUnregisterServiceCallback),
    HcnUnregisterServiceCallback>;

// Callback handle returned by registration api. Kept at
// global or module scope as it will automatically be
// unregistered when it goes out of scope.
unique_hcn_callback g_Callback;

// Event notification callback function.
void
CALLBACK
ServiceCallback(
    DWORD   NotificationType,
    void*   Context,
    HRESULT NotificationStatus,
    PCWSTR  NotificationData)
{
    // Optional client context
    UNREFERENCED_PARAMETER(context);
    // Reserved for future use
    UNREFERENCED_PARAMETER(NotificationStatus);

    switch (NotificationType)
    {
        case HcnNotificationNetworkCreate:
            // TODO: UnMarshal the NotificationData
            //
            // // Notification
            // {
            //     "ID" : Guid,
            //     "Flags" : <uint32>,
            // };
            break;

        case HcnNotificationNetworkDelete:
            // TODO: UnMarshal the NotificationData
            break;

        Default:
            // TODO: handle other events.
            break;
    }
}

/// Register for service-wide notifications
void RegisterForServiceNotifications()
{
    THROW_IF_FAILED(HcnRegisterServiceCallback(
        ServiceCallback,
        nullptr,
        &g_Callback));
}

/// Unregister from service-wide notifications
void UnregisterForServiceNotifications()
{
    // As this is a unique_hcn_callback, this will cause HcnUnregisterServiceCallback to be invoked
    g_Callback.reset();

}
```

## <a name="next-steps"></a>後續步驟

- 深入瞭解 HCN 的[RPC 內容控制碼](hcn-declaration-handles.md)。

- 深入瞭解[HCN JSON 檔架構](hcn-json-document-schemas.md)。