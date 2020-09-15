---
title: Vm 和容器的主機計算網路 (HCN) 服務 API
description: 主機計算網路 (HCN) 服務 API 是公開的 WIN32 API，可提供平台層級的存取權來管理虛擬網路、虛擬網路端點和相關聯的原則。 這兩者可為虛擬機器 (Vm) 和在 Windows 主機上執行的容器提供連線能力和安全性。
ms.author: daschott
author: daschott
ms.date: 11/05/2018
ms.openlocfilehash: 5f30dfe4c7a373459b8b8ea3915eaaafa9bf517e
ms.sourcegitcommit: 0b3d6661c44aa1a697087e644437279142726d84
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/15/2020
ms.locfileid: "90083629"
---
# <a name="host-compute-network-hcn-service-api-for-vms-and-containers"></a>Vm 和容器的主機計算網路 (HCN) 服務 API

> 適用于： Windows Server (半年通道) 、Windows Server 2019

主機計算網路 (HCN) 服務 API 是公開的 WIN32 API，可提供平台層級的存取權來管理虛擬網路、虛擬網路端點和相關聯的原則。 這兩者可為虛擬機器 (Vm) 和在 Windows 主機上執行的容器提供連線能力和安全性。

開發人員會使用 HCN 服務 API，在其應用程式工作流程中管理 Vm 和容器的網路功能。 HCN API 的設計目的是要為開發人員提供最佳體驗。 終端使用者不會直接與這些 Api 互動。

## <a name="features-of-the-hcn-service-api"></a>HCN 服務 API 的功能
-    實作為主機網路服務所裝載的 C API， (OnCore/VM 上的 HNS) 。

-    提供建立、修改、刪除和列舉 HCN 物件的能力，例如網路、端點、命名空間和原則。 作業會在物件的控制碼上執行 (例如，) 的網路控制碼，而這些控制碼會在內部使用 RPC 內容控制碼執行。

-    以架構為基礎。 API 的大部分功能都會將輸入和輸出參數定義為字串，其中包含做為 JSON 檔的函式呼叫引數。 JSON 檔是以強型別和版本設定的架構為基礎，而這些架構是公用檔的一部分。

-    提供訂用帳戶/回呼 API，可讓用戶端註冊服務範圍事件的通知，例如網路建立和刪除。

-    HCN API 適用于傳統型橋接器 (也稱為 Centennial) 在系統服務中執行的應用程式。 API 會從呼叫端取出使用者權杖來檢查 ACL。

>[!TIP]
>背景工作和非前景視窗都支援 HCN 服務 API。
## <a name="terminology-host-vs-compute"></a>術語：主機與計算
主機計算服務可讓呼叫者在單一實體電腦上建立及管理虛擬機器和容器。 其命名為遵循業界術語。

- **主機** 廣泛用於虛擬化產業，以參考提供虛擬化資源的作業系統。

- **Compute** 可用來參考比虛擬機器更廣泛的虛擬化方法。 Host Compute Network Service 可讓呼叫者在單一實體電腦上建立及管理虛擬機器和容器的網路功能。

## <a name="schema-based-configuration-documents"></a>以架構為基礎的設定檔
以妥善定義的架構為基礎的設定檔是虛擬化空間中已建立的產業標準。 大部分的虛擬化解決方案（例如 Docker 和 Kubernetes）會根據設定檔提供 Api。 有 Microsoft 參與的數個產業創新，推動了定義和驗證這些架構的生態系統，例如 [OpenAPI](https://www.openapis.org/)。  這些計畫也會推動用於容器之架構的特定架構定義的標準化，例如 [ (OCI) 的 Open Container 方案 ](https://www.opencontainers.org/)。

用來撰寫設定檔的語言是 [JSON](https://tools.ietf.org/html/rfc8259)，您可以搭配下列各項使用：
-    定義檔物件模型的架構定義
-    JSON 檔是否符合架構的驗證
-    使用 Api 呼叫端所使用的程式設計語言，從這些架構的原生標記法自動轉換 JSON 檔

常用的架構定義為 [OpenAPI](https://www.openapis.org/) 和 [JSON 架構](http://json-schema.org/)，可讓您指定檔中屬性的詳細定義，例如：
-    屬性值的有效集合，例如代表百分比的屬性的0-100。
-    列舉的定義，以屬性的一組有效字串表示。
-    字串預期格式的正則運算式。

在記載 HCN Api 的過程中，我們打算將 JSON 檔的架構發佈為 OpenAPI 規格。 根據這項規格，架構的語言特定標記法可以允許以型別安全的方式使用用戶端所使用的程式設計語言中的架構物件。

### <a name="example"></a>範例

以下是代表 VM 設定檔中的 SCSI 控制器之物件的此工作流程範例。

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
>[如果是 ( "2.0" ) 批註是架構定義的版本控制支援的一部分。
從這個內部定義中，我們會產生架構的 OpenAPI 規格：

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

您可以使用 [Swagger](https://swagger.io/)等工具來產生用戶端所使用之架構程式設計語言的特定語言標記法。 Swagger 支援多種語言，例如 c #、Go、JAVAscript 和 Python) 。

- 針對最上層 IPAM & 子網物件[產生的 c # 程式碼範例](example-c-sharp.md)。

- 針對最上層 IPAM & 子網物件[產生的 Go 程式碼範例](example-go.md)。 Go 是由 Docker 和 Kubernetes 使用，這是主機計算網路服務 Api 的兩個取用者。 Go 有內建的支援，可將 Go 類型封送處理至 JSON 檔。

除了程式碼產生和驗證之外，您還可以使用工具來簡化 JSON 檔的工作，也就是 [Visual Studio Code](https://code.visualstudio.com/Docs/languages/json)。

### <a name="top-level-objects-defined-in-the-hcnschemasmars-file"></a>HCN 中定義的最上層物件。架構 mars 檔案
如先前所述，您可以在下列一組 mars 檔案中找到 HCN Api 所用檔的檔架構： onecore/vm/dv/net/hns/schema/mars/Schema

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

- 深入瞭解常見的 [HCN 案例](hcn-scenarios.md)。

- 深入瞭解 HCN 的 [RPC 內容控制碼](hcn-declaration-handles.md)。

- 深入瞭解 [HCN JSON 檔架構](hcn-json-document-schemas.md)。