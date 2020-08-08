---
title: 適用于 Vm 和容器的主機計算網路 (HCN) 服務 API
description: 主機計算網路 (HCN) 服務 API 是公開的 WIN32 API，可提供平台層級的存取權來管理虛擬網路、虛擬網路端點和相關聯的原則。 這兩者可提供虛擬機器的連線能力和安全性， (Vm) 和在 Windows 主機上執行的容器。
ms.author: jmesser
author: jmesser81
ms.date: 11/05/2018
ms.openlocfilehash: 8e83af4ea54d2fcc75ff8ff054f4ad253a5422ea
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955656"
---
# <a name="host-compute-network-hcn-service-api-for-vms-and-containers"></a>適用于 Vm 和容器的主機計算網路 (HCN) 服務 API

>適用于： Windows Server (半年通道) 、Windows Server 2019

主機計算網路 (HCN) 服務 API 是公開的 WIN32 API，可提供平台層級的存取權來管理虛擬網路、虛擬網路端點和相關聯的原則。 這兩者可提供虛擬機器的連線能力和安全性， (Vm) 和在 Windows 主機上執行的容器。

開發人員會使用 HCN 服務 API 來管理其應用程式工作流程中 Vm 和容器的網路功能。 HCN API 已經過設計，可為開發人員提供最佳的體驗。 終端使用者不會直接與這些 Api 互動。

## <a name="features-of-the-hcn-service-api"></a>HCN 服務 API 的功能
-    實作為主機網路服務所主控的 C API (OnCore/VM 上的 HNS) 。

-    提供建立、修改、刪除和列舉 HCN 物件（例如網路、端點、命名空間和原則）的功能。 作業會在物件的控制碼上執行 (例如，) 的網路控制碼，並在內部使用 RPC 內容控制碼來實作為這些控制碼。

-    以架構為基礎。 API 的大部分函式會將輸入和輸出參數定義為字串，其中包含當做 JSON 檔之函式呼叫的引數。 JSON 檔是以強型別和已建立版本的架構為基礎，這些架構是公用檔的一部分。

-    提供訂用帳戶/回呼 API，讓用戶端註冊服務範圍事件（例如網路建立和刪除）的通知。

-    HCN API 適用于 Desktop Bridge (也稱為 Centennial 在系統服務中執行) 應用程式。 API 會藉由從呼叫者抓取使用者 token 來檢查 ACL。

>[!TIP]
>背景工作和非前景視窗支援 HCN 服務 API。

## <a name="terminology-host-vs-compute"></a>術語：主機與計算的比較
主機計算服務可讓呼叫者在單一實體電腦上建立和管理虛擬機器和容器。 其名為遵循產業術語。

- 虛擬化產業廣泛使用**主機**來參考提供虛擬化資源的作業系統。

- **Compute**是用來參照比虛擬機器更廣的虛擬化方法。 主機計算網路服務可讓呼叫者在單一實體電腦上，建立和管理虛擬機器和容器的網路功能。

## <a name="schema-based-configuration-documents"></a>以架構為基礎的設定檔
以妥善定義的架構為基礎的設定檔是在虛擬化空間中建立的業界標準。 大部分的虛擬化解決方案（例如 Docker 和 Kubernetes）都會提供以設定檔為基礎的 Api。 參與 Microsoft 的幾個產業創新，會驅動一個生態系統來定義和驗證這些架構，例如[OpenAPI](https://www.openapis.org/)。  這些計畫也會針對用於容器的架構（例如[ (OCI) 的開放容器計畫](https://www.opencontainers.org/)），推動特定架構定義的標準化。

用於撰寫設定檔的語言是[JSON](https://tools.ietf.org/html/rfc8259)，與搭配使用：
-    定義檔之物件模型的架構定義
-    驗證 JSON 檔是否符合架構
-    以 Api 的呼叫端所使用的程式設計語言，在這些架構的原生表示中自動轉換 JSON 檔

常用的架構定義為[OpenAPI](https://www.openapis.org/)和[JSON 架構](http://json-schema.org/)，可讓您在檔中指定屬性的詳細定義，例如：
-    屬性的一組有效值，例如0-100 代表百分比的屬性。
-    列舉的定義，以屬性的一組有效字串表示。
-    字串之預期格式的正則運算式。

在記載 HCN Api 的過程中，我們打算將 JSON 檔的架構發佈為 OpenAPI 規格。 根據此規格，架構的語言特定標記法可讓您以用戶端所使用的程式設計語言，以型別安全的方式使用架構物件。

### <a name="example"></a>範例

以下是此工作流程的範例，該物件代表 VM 設定檔中的 SCSI 控制器。

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
>[NewIn ( "2.0" ) 注釋是架構定義的版本控制支援的一部分。

在此內部定義中，我們會產生架構的 OpenAPI 規格：

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

您可以使用[Swagger](https://swagger.io/)之類的工具來產生用戶端所使用之架構程式設計語言的特定語言標記法。 Swagger 支援多種語言，例如 c #、Go、JAVAscript 和 Python) 。

- 最上層 IPAM & 子網物件的已[產生 c # 程式碼範例](example-c-sharp.md)。

- 最上層 IPAM & 子網物件[產生的 Go 程式碼範例](example-go.md)。 Docker 和 Kubernetes 會使用 Go，這是主機計算網路服務 Api 的兩個取用者。 Go 具有內建支援，可將 Go 類型與 JSON 檔進行封送處理。

除了程式碼產生和驗證之外，您還可以使用工具來簡化 JSON 檔的工作，也就是[Visual Studio Code](https://code.visualstudio.com/Docs/languages/json)。

### <a name="top-level-objects-defined-in-the-hcnschemasmars-file"></a>定義于 HCN 中的最上層物件。架構 mars 檔案
如上所述，您可以在下列一組 mars 檔案中，找到 HCN Api 所使用檔的檔架構： onecore/vm/dv/net/hns/架構/mars/架構

最上層物件為：
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

- 深入瞭解常見的[HCN 案例](hcn-scenarios.md)。

- 深入瞭解 HCN 的[RPC 內容控制碼](hcn-declaration-handles.md)。

- 深入瞭解[HCN JSON 檔架構](hcn-json-document-schemas.md)。
