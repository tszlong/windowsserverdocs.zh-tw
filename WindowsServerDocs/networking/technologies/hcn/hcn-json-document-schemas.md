---
title: 主機計算網路 (HCN) JSON 檔架構
description: HCN JSON 檔架構的相關資訊。
ms.author: daschott
author: daschott
ms.date: 11/05/2018
ms.topic: article
ms.openlocfilehash: fbd0f7ac10b735f872911e605d56ed219d8f6ea0
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948744"
---
# <a name="hcn-json-document-schemas"></a>HCN JSON 文件結構描述

> 適用于： Windows Server (半年通道) 、Windows Server 2019

## <a name="hcn-schema"></a>HCN 架構

```json
// Network
{
    "Id" : <string>,
    "Owner" : <string>,
    "SchemaVersion" : {
        "Major" : <uint32>,
        "Minor" : <uint32>
    },
    "Flags" : <enum bit mask>,
         // AsString; Values:
         // "None" (0),
         // "EnableDnsProxy" (1),
         // "EnableDhcpServer" (2),
         // "IsolateVSwitch" (8)
    "Type"  : <enum>,
         // AsString; Values:
         // "NAT" (0),
         // "ICS" (1),
         // "Transparent" (2)
    "Ipams" : [ {
         "Type" : <enum>,
             // AsString; Values:
             // "Static" (0),
             // "Dhcp" (1)
         "Subnets" : [ {
                "IpAddressPrefix" : <ip prefix in CIDR>,
                "Policies" : [ {
                        "Type" : <enum>,
                            // AsString; Values:
                            // "VLAN" (0)
                        "Data" : <any>
                 } ],
                "Routes" : [ {
                       "NextHop" : <ip address of the next hop gateway>,
                       "DestinationPrefix" : <ip prefix in cidr>,
                       "Metric" : <route metric in uint8>,
                 } ],
          } ],
     } ],
    "Policies" : [{
         "Type" : <enum>,
              // AsString; Values:
              // "NetAdapterName" (1),
              // "InterfaceConstraint" (2)
         "Data" : <any>
    }],
    "Dns" : {
        "Suffix" : <local connection specific suffix>,
        "Search" : [<list of additional suffixes>],
        "ServerList" : [<string>],
        "Options" : [<string>],
    },
    "MacPool" : {
        "Ranges" : [ {
              "StartMacAddress" : <string>,
              "EndMacAddress" : <string>
         } ],
    },
}
```

## <a name="hcn-endpoint-schema"></a>HCN 端點架構

```json
// Endpoint
{
    "Id" : <string>,
    "Owner" : <string>,
    "SchemaVersion" : {
        "Major" : <uint32>,
        "Minor" : <uint32>
    },
    "Flags" : <enum bit mask>,
         // AsString; Values:
         // "None" (0),
         // "DisableInterComputeCommunication" (2)
    "HostComputeNetwork" : <string>,
    "MacAddress" : <string>,
    "Policies" : [ {
         "Type" : <enum>,
              // AsString; Values:
              // "PortMapping" (0),
              // "ACL" (1)
         "Data" : <any>
    } ],
    "Dns" : {
        "Suffix" : <local connection specific suffix>,
        "Search" : [<list of additional suffixes>],
        "ServerList" : [<string>],
        "Options" : [<string>],
    },
    "IPConfigurations" : [ {
        "IPAddress" : <ip address>,
        "PrefixLength" : <prefix length uint16>,
    } ],
    "Routes" : [ {
        "NextHop" : <ip address of the next hop gateway>,
        "DestinationPrefix" : <ip prefix in cidr>,
        "Metric" : <route metric in uint8>,
    } ],
}
```

## <a name="hcn-policy-schema"></a>HCN 原則架構

```json
// VlanPolicy
{
    "Type" : "VLAN",
    "IsolationId" : <uint32>,
}
// PortMappingPolicy
{
    "Type" : "PortMapping",
    "Protocol" : <enum>,
         // AsString; Values:
         // "Unknown" (0),
         // "ICMPv4" (1),
         // "IGMP" (2),
         // "TCP" (6),
         // "UDP" (17),
         // "ICMPv6" (58)
    "InternalPort" : <uint16>,
    "ExternalPort" : <uint16>,
}
```

## <a name="hcn-load-balancer-schema"></a>HCN 負載平衡器架構

```json
// Host Compute LoadBalancer
{
    "Id" : <string>,
    "Owner" : <string>,
    "SchemaVersion" : {
        "Major" : <uint32>,
        "Minor" : <uint32>
    },
    "Flags" : <enum bit mask>,
         // AsString; Values:
         // "None" (0),
         // "EnableDirectServerReturn" (1)
         // "EnableInternalLoadBalancer" (2)
    "HostComputeEndpoints" : [<Host compute Endpoint id>],
    "VirtualIPs" : [<Virtual IpAddress>],
    "PortMappings" : [ {
        "Type" : "PortMapping",
        "Protocol" : <enum>,
             // AsString; Values:
             // "Unknown" (0),
             // "ICMPv4" (1),
             // "IGMP" (2),
             // "TCP" (6),
             // "UDP" (17),
             // "ICMPv6" (58)
        "InternalPort" : <uint16>,
        "ExternalPort" : <uint16>,
    } ],
    "Policies" : [ {
         "Type" : <enum>,
              // AsString; Values:
              // "SourceVirtualIp" (0),
         "Data" : <any>
    } ],
}
```

## <a name="hcn-namespace-schema"></a>HCN 命名空間架構

```json
// Namespace
{
    "Id" : <string>,
    "Owner" : <string>,
    "SchemaVersion" : {
        "Major" : <uint32>,
        "Minor" : <uint32>
    },
    "NamespaceId" : <uint32>,
    "NamespaceGuid" : <guid>,
    "Type"  : <enum>,
              // AsString; Values:
              // "Host" (0),
              // "HostDefault" (1),
              // "Guest" (2),
              // "GuestDefault" (3)
    "Resources" : [ {
          "Type"  : <enum>,
              // AsString; Values:
              // "Container" (0),
              // "Endpoint" (1)
          "Data"  : <any>
    } ],
}
```

## <a name="hcn-notification-schema"></a>HCN 通知架構

```json
// Notification
{
    "ID" : Guid,
    "Flags" : <uint32>,
};
```

## <a name="result-error-schema"></a>結果錯誤架構

```json
// ErrorSchema
{
    "ErrorCode" : <uint32>,
    "Error" : <string>,
    "Success" : <bool>,
}
```