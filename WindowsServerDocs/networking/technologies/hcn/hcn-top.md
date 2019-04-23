---
title: 適用於 Vm 和容器來裝載計算網路 (HCN) 服務 API
description: 主機計算網路 (HCN) 服務 API 是公開的 Win32 API，提供平台層級存取，來管理虛擬網路、 虛擬網路端點，以及相關聯的原則。 同時這會為虛擬機器 (Vm) 和 Windows 主機上執行的容器提供連線和安全性。
ms.author: jmesser
author: jmesser81
ms.date: 11/05/2018
ms.openlocfilehash: 50af0dab69633aa6e07ded68e9246aa0315377f0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844979"
---
# <a name="host-compute-network-hcn-service-api-for-vms-and-containers"></a>適用於 Vm 和容器來裝載計算網路 (HCN) 服務 API

>適用於：Windows Server （半年通道），Windows Server 2016

主機計算網路 (HCN) 服務 API 是公開的 Win32 API，提供平台層級存取，來管理虛擬網路、 虛擬網路端點，以及相關聯的原則。 同時這會為虛擬機器 (Vm) 和 Windows 主機上執行的容器提供連線和安全性。 

開發人員會使用 HCN 服務 API 來管理 Vm 的網路功能和其應用程式工作流程中的容器。 HCN API 專為開發人員提供最佳體驗。 使用者不直接互動與這些 Api。  

## <a name="features-of-the-hcn-service-api"></a>HCN 服務 API 的功能
-   實作為裝載的主機網路服務 (HNS) OnCore/VM 上的 C API。

-   讓您能夠建立、 修改、 刪除及列舉 HCN 物件，例如網路、 端點、 命名空間和原則。 物件 （例如，網路控制代碼），控制代碼上執行的作業，以及這些控制代碼在內部實作使用 RPC 內容控制代碼。

-   結構描述為基礎。 大部分的函式的 api 並定義輸入和輸出參數為包含函式呼叫的引數，以 JSON 文件的字串。 JSON 文件以強型別和建立版本的結構描述，這些結構描述包括公開文件的一部分。 

-   提供訂用帳戶/回呼 API，讓用戶端註冊的整個服務的事件，例如網路建立和刪除作業的通知。

-   HCN API （也稱為適用於傳統型橋接器 在 系統服務執行的 centennial) 應用程式。 API 會檢查 ACL，藉由呼叫端從擷取的使用者 token。

>[!TIP]
>HCN 服務 API 支援背景工作和非前景視窗中。 

## <a name="terminology-host-vs-compute"></a>詞彙︰主機與計算
主機計算服務可讓呼叫者建立和管理虛擬機器和單一的實體電腦上的容器。 它會命名為遵循業界術語。 

- **主機**廣泛用來參考提供虛擬化的資源的作業系統虛擬化產業中。

- **計算**用來參考會遠遠不只是虛擬機器的虛擬化方法。 主機計算網路服務可讓呼叫者建立和管理虛擬機器和容器的單一實體電腦上的網路功能。

## <a name="schema-based-configuration-documents"></a>結構描述為基礎的設定文件
根據定義完善的結構描述的設定文件是既定的業界標準的虛擬化空間中。 大部分的虛擬化解決方案，例如 Docker 和 Kubernetes，提供設定文件為基礎的 Api。 數個產業計劃，與 Microsoft 的參與計劃磁碟機的定義和驗證這些結構描述，例如生態系統[OpenAPI](https://www.openapis.org/)。  這些計劃也會驅動的容器，例如使用的結構描述的特定結構描述定義標準化[開放容器計劃 (OCI)](https://www.opencontainers.org/)。

用來撰寫設定文件的語言[JSON](https://tools.ietf.org/html/rfc8259)，讓您用於搭配：
-   定義文件物件模型的結構描述定義
-   JSON 文件是否符合結構描述驗證
-   自動化的 JSON 文件來回轉換的兩個結構描述中的 Api 呼叫端所使用的程式設計語言的原生表示法 

常用的結構描述定義的[OpenAPI](https://www.openapis.org/)並[JSON 結構描述](http://json-schema.org/)，可讓您指定之屬性的詳細的定義在文件，例如：
-   有效的屬性，例如 0-100，則為表示百分比的屬性值組。
-   列舉型別，以一組屬性的有效字串表示的定義。
-   預期的格式字串的規則運算式。 

加入註解 HCN Api 的一部分，我們打算發佈 OpenAPI 規格將 JSON 文件的結構描述。 此規格為基礎，可允許在用戶端所使用的程式設計語言中的結構描述物件的型別安全使用的結構描述的特定語言表示法。 

### <a name="example"></a>範例 

以下是此物件代表 VM 的設定文件中的 SCSI 控制器的工作流程的範例。 

Windows 在原始程式碼中，我們會定義使用.mars 檔案的結構描述： onecore/vm/dv/net/hns/schema/mars/Schema/HCN.Schema.Network.mars

```
enum IpamType
{
    [NewIn("2.0")] Static,
    [NewIn("2.0")] Dhcp,
};

class Ipam
{
    // Type : dhcp
    [NewIn("2.0"),OmitEmpty] IpamType   Type;
    [NewIn("2.0"),OmitEmpty] Subnet     Subnets[];
};

class Subnet : HCN.Schema.Common.Base
{
    [NewIn("2.0"),OmitEmpty] string         IpAddressPrefix;
    [NewIn("2.0"),OmitEmpty] SubnetPolicy   Policies[];
    [NewIn("2.0"),OmitEmpty] Route          Routes[];
};


enum SubnetPolicyType
{
    [NewIn("2.0")] VLAN
};

class SubnetPolicy
{
    [NewIn("2.0"),OmitEmpty] SubnetPolicyType                 Type;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Common.PolicySettings Data;
};

class PolicySettings
{
    [NewIn("2.0"),OmitEmpty]  string      Name;
};

class VlanPolicy : HCN.Schema.Common.PolicySettings 
{
    [NewIn("2.0")] uint32 IsolationId;
};

class Route 
{
    [NewIn("2.0"),OmitEmpty] string NextHop;
    [NewIn("2.0"),OmitEmpty] string DestinationPrefix;
    [NewIn("2.0"),OmitEmpty] uint16 Metric;
};

```

>[!TIP]
>[NewIn("2.0") 註解會是一部分的結構描述定義的版本控制支援。

從這個內部的定義，我們會產生結構描述的 OpenAPI 規格：

```
{ 
    "swagger" : "2.0", 
    "info" : { 
       "version" : "2.1", 
       "title" : "HCN API" 
    },
    "definitions": {        
        "Ipam": {
            "type": "object",
            "properties": {
                "Type": {
                    "type": "string",
                    "enum": [
                        "Static",
                        "Dhcp"
                    ],
                    "description": " Type : dhcp"
                },
                "Subnets": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/Subnet"
                    }
                }
            }
        },
        "Subnet": {
            "type": "object",
            "properties": {
                "ID": {
                    "type": "string",
                    "pattern": "^[0-9A-Fa-f]{8}-([0-9A-Fa-f]{4}-){3}[0-9A-Fa-f]{12}$"
                },                
                "IpAddressPrefix": {
                    "type": "string"
                },
                "Policies": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/SubnetPolicy"
                    }
                },
                "Routes": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/Route"
                    }
                }
            }
        },
        "SubnetPolicy": {
            "type": "object",
            "properties": {
                "Type": {
                    "type": "string",
                    "enum": [
                        "VLAN",
                        "VSID"
                    ]
                },
                "Data": {
                    "$ref": "#/definitions/PolicySettings"
                }
            }
        },
        "PolicySettings": {
            "type": "object",
            "properties": {
                "Name": {
                    "type": "string"
                }
            }
        },                      
        "VlanPolicy": {
            "type": "object",
            "properties": {
                "Name": {
                    "type": "string"
                },
                "IsolationId": {
                    "type": "integer",
                    "format": "uint32"
                }
            }
        },
        "Route": {
            "type": "object",
            "properties": {
                "NextHop": {
                    "type": "string"
                },
                "DestinationPrefix": {
                    "type": "string"
                },
                "Metric": {
                    "type": "integer",
                    "format": "uint16"
                }
            }
        }        
    }
} 
```

您可以使用工具，例如[Swagger](https://swagger.io/)、 產生的程式語言的用戶端使用的結構描述的特定語言表示法。 Swagger 支援各種不同的語言，例如C#、 Go、 Javascript 和 Python)。

- [產生的範例C#程式碼](example-c-sharp.md)上方層級的 IPAM 與子網路的物件。

- [產生的 Go 程式碼範例](example-go.md)上方層級的 IPAM 與子網路的物件。 Go 會使用 Docker 和 Kubernetes，也就是兩個主機計算網路服務 api 取用者。 移至具有與 JSON 文件的 Go 類型封送處理的內建支援。

除了程式碼產生和驗證，您可以使用工具來簡化處理 JSON 文件的工作 — 亦即[Visual Studio Code](https://code.visualstudio.com/Docs/languages/json)。

### <a name="top-level-objects-defined-in-the-hcnschemasmars-file"></a>HCN 中定義的最上層物件。Schemas.mars 檔案
如先前所述，您可以找到文件結構描述 HCN Api.mars 下的檔案集合中所使用的文件： onecore/vm/dv/net/hns/結構/mars/結構描述

最上層物件如下：
- [HostComputeNetwork](hcn-scenarios.md#scenario-hcn)
- [HostComputeEndpoint](hcn-scenarios.md#scenario-hcn-endpoint)
- [HostComputeNamespace](hcn-scenarios.md#scenario-hcn-namespace)
- [HostComputeLoadBalancer](hcn-scenarios.md#scenario-hcn-load-balancer)

```
class HostComputeNetwork : HCN.Schema.Common.Base
{
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.NetworkMode          Type;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.NetworkPolicy        Policies[];
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.MacPool              MacPool;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.DNS                  Dns;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.Ipam                 Ipams[];
};

class HostComputeEndpoint : HCN.Schema.Common.Base
{
    [NewIn("2.0"),OmitEmpty] string                                     HostComputeNetwork;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.Endpoint.EndpointPolicy Policies[];
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.Endpoint.IpConfig       IpConfigurations[];
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.DNS                     Dns;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.Route                   Routes[];
    [NewIn("2.0"),OmitEmpty] string                                     MacAddress;
};

class HostComputeNamespace : HCN.Schema.Common.Base
{
    [NewIn("2.0"),OmitEmpty] uint32                                    NamespaceId;
    [NewIn("2.0"),OmitEmpty] Guid                                      NamespaceGuid;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Namespace.NamespaceType        Type;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Namespace.NamespaceResource    Resources[];
};

class HostComputeLoadBalancer : HCN.Schema.Common.Base
{
    [NewIn("2.0"), OmitEmpty] string                                               HostComputeEndpoints[]; 
    [NewIn("2.0"), OmitEmpty] string                                               VirtualIPs[]; 
    [NewIn("2.0"), OmitEmpty] HCN.Schema.Network.Endpoint.Policy.PortMappingPolicy PortMappings[]; 
    [NewIn("2.0"), OmitEmpty] HCN.Schema.LoadBalancer.LoadBalancerPolicy           Policies[];
};
```

## <a name="next-steps"></a>後續步驟

- 深入了解[HCN 的常見案例](hcn-scenarios.md)。

- 深入了解[RPC 內容處理為 HCN](hcn-declaration-handles.md)。

- 深入了解[HCN JSON 文件結構描述](hcn-json-document-schemas.md)。