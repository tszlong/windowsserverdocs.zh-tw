---
title: 控制您的工具在解決方案中的可見度
description: 在 Windows 系統管理中心 SDK (Project 檀香山) 中控制您工具的可見度
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: df939bb1a87c9ded77431661dcabd7faf607bb6e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87945001"
---
# <a name="control-your-tools-visibility-in-a-solution"></a>控制您的工具在解決方案中的可見度 #

>適用於：Windows Admin Center、Windows Admin Center 預覽版

有時候，您可能會想要從 [可用的工具] 清單中排除 (或隱藏) 您的擴充功能或工具。 例如，如果您的工具只以 Windows Server 2016 為目標 (不是舊版) ，您可能不想要連線到 Windows Server 2012 R2 伺服器的使用者完全看不到您的工具。  (想像使用者體驗-他們按一下它，等待工具載入，只是為了取得其功能無法連線的訊息。 ) 您可以定義何時要在工具的 manifest.js檔案中顯示 (或隱藏) 功能的時機。

## <a name="options-for-deciding-when-to-show-a-tool"></a>決定何時要顯示工具的選項 ##

有三種不同的選項可供您用來判斷是否應該顯示您的工具，並可供特定伺服器或叢集連接使用。

* localhost
* 清查 (屬性陣列) 
* 指令碼

### <a name="localhost"></a>發出 ###

條件物件的 localHost 屬性包含一個布林值，如果連接的節點是 localHost (在) 上安裝 Windows 系統管理中心的同一部電腦上，則可以評估為推斷。 藉由將值傳遞給屬性，您可以指定何時 (條件) 以顯示工具。 例如，如果您只想要在使用者實際上是連接到本機主機時顯示工具，請設定如下：

``` json
"conditions": [
{
    "localhost": true
}]
```

或者，如果您只想要在連接的節點*不是*localhost 時顯示工具：

``` json
"conditions": [
{
    "localhost": false
}]
```

以下是當連接節點不是 localhost 時，設定的外觀，只會顯示工具：

``` json
"entryPoints": [
{
    "entryPointType": "tool",
    "name": "main",
    "urlName": "processes",
    "displayName": "resources:strings:displayName",
    "description": "resources:strings:description",
    "icon": "sme-icon:icon-win-serverProcesses",
    "path": "",
    "requirements": [
    {
        "solutionIds": [
        "msft.sme.server-manager!windowsClients"
        ],
        "connectionTypes": [
        "msft.sme.connection-type.windows-client"
        ],
        "conditions": [
        {
            "localhost": true
        }
        ]
    }
    ]
}
```

### <a name="inventory-properties"></a>清查屬性 ###

SDK 包含預先策劃的清查屬性集，可供您用來建立條件，以判斷您的工具何時可供使用。 ' 清查 ' 陣列中有九個不同的屬性：

| 屬性名稱 | 預期的數值型別 |
| ------------- | ------------------- |
| computerManufacturer | string |
| operatingSystemSKU | 數字 |
| operatingSystemVersion | version_string (，例如： "10.1. *" )  |
| productType | 數字 |
| clusterFqdn | string |
| isHyperVRoleInstalled | boolean |
| isHyperVPowershellInstalled | boolean |
| isManagementToolsAvailable | boolean |
| isWmfInstalled | boolean |

清查陣列中的每個物件都必須符合下列 json 結構：

``` json
"<property name>": {
    "type": "<expected type>",
    "operator": "<defined operator to use>",
    "value": "<expected value to evaluate using the operator>"
}
```

#### <a name="operator-values"></a>運算子值 ####

| 運算子 | 描述 |
| -------- | ----------- |
| gt | 大於 |
| ge | 大於或等於 |
| lt | 小於 |
| le | 小於或等於 |
| eq | 等於 |
| ne | 不等於 |
| is | 檢查值是否為 true |
| not | 檢查值是否為 false |
| contains | 專案存在於字串中 |
| notContains | 專案不存在於字串中 |

#### <a name="data-types"></a>資料類型 ####

' Type ' 屬性的可用選項：

| 類型 | 描述 |
| ---- | ----------- |
| version |  (的版本號碼，例如： 10.1. * )  |
| 數字 | 數值 |
| string | 字串值 |
| boolean | true 或 false |

#### <a name="value-types"></a>值類型 ####

' Value ' 屬性會接受下列類型：

* 字串
* 數字
* boolean

正確格式的清查條件集看起來像這樣：

``` json
"entryPoints": [
{
    "entryPointType": "tool",
    "name": "main",
    "urlName": "processes",
    "displayName": "resources:strings:displayName",
    "description": "resources:strings:description",
    "icon": "sme-icon:icon-win-serverProcesses",
    "path": "",
    "requirements": [
    {
        "solutionIds": [
        "msft.sme.server-manager!servers"
        ],
        "connectionTypes": [
        "msft.sme.connection-type.server"
        ],
        "conditions": [
        {
            "inventory": {
            "operatingSystemVersion": {
                "type": "version",
                "operator": "gt",
                "value": "6.3"
            },
            "operatingSystemSKU": {
                "type": "number",
                "operator": "eq",
                "value": "8"
            }
            }
        }
        ]
    }
    ]
}
```

### <a name="script"></a>指令碼 ###

最後，您可以執行自訂的 PowerShell 腳本，以識別節點的可用性和狀態。 所有腳本都必須傳回具有下列結構的物件：

``` ps
@{
    State = 'Available' | 'NotSupported' | 'NotConfigured';
    Message = '<Message to explain the reason of state such as not supported and not configured.>';
    Properties =
        @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' },
        @{Name='Prop2'; Value = 12345678; Type='number'; };
}
```
State 屬性是重要的值，將控制在 [工具] 清單中顯示或隱藏您的延伸模組的決策。  允許的值包括：

| 值 | 描述 |
| ---- | ----------- |
| 可用 | 延伸模組應該會顯示在 [工具] 清單中。 |
| NotSupported | 延伸模組不應該顯示在 [工具] 清單中。 |
| NotConfigured | 這是未來工作的預留位置值，會在工具可供使用之前，提示使用者進行其他設定。  目前，此值會導致工具顯示，而且功能相當於「可用」。 |

例如，如果我們想要在遠端伺服器已安裝 BitLocker 的情況下載入工具，腳本看起來會像這樣：

``` ps
$response = @{
    State = 'NotSupported';
    Message = 'Not executed';
    Properties = @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' },
        @{Name='Prop2'; Value = 12345678; Type='number'; };
}

if (Get-Module -ListAvailable -Name servermanager) {
    Import-module servermanager;
    $isInstalled = (Get-WindowsFeature -name bitlocker).Installed;
    $isGood = $isInstalled;
}

if($isGood) {
    $response.State = 'Available';
    $response.Message = 'Everything should work.';
}

$response
```

使用 [腳本] 選項的進入點設定看起來像這樣：

``` json
"entryPoints": [
{
    "entryPointType": "tool",
    "name": "main",
    "urlName": "processes",
    "displayName": "resources:strings:displayName",
    "description": "resources:strings:description",
    "icon": "sme-icon:icon-win-serverProcesses",
    "path": "",
    "requirements": [
    {
        "solutionIds": [
        "msft.sme.server-manager!windowsClients"
        ],
        "connectionTypes": [
        "msft.sme.connection-type.windows-client"
        ],
        "conditions": [
        {
            "localhost": true,
            "inventory": {
            "operatingSystemVersion": {
                "type": "version",
                "operator": "eq",
                "value": "10.0.*"
            },
            "operatingSystemSKU": {
                "type": "number",
                "operator": "eq",
                "value": "4"
            }
            },
            "script": "$response = @{ State = 'NotSupported'; Message = 'Not executed'; Properties = @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' }, @{Name='Prop2'; Value = 12345678; Type='number'; }; }; if (Get-Module -ListAvailable -Name servermanager) { Import-module servermanager; $isInstalled = (Get-WindowsFeature -name bitlocker).Installed; $isGood = $isInstalled; }; if($isGood) { $response.State = 'Available'; $response.Message = 'Everything should work.'; }; $response"
        }
        ]
    }
    ]
}
```

## <a name="supporting-multiple-requirement-sets"></a>支援多個需求集 ##

您可以使用一組以上的需求，藉由定義多個「需求」區塊來判斷何時要顯示您的工具。

例如，若要顯示您的工具（如果「案例 A」或「案例 B」為 true），請定義兩個需求區塊;如果其中一個為 true (也就是，符合需求區塊內的所有條件) ，就會顯示工具。

``` json
"entryPoints": [
{
    "requirements": [
    {
        "solutionIds": [
            …"scenario A"…
        ],
        "connectionTypes": [
            …"scenario A"…
        ],
        "conditions": [
            …"scenario A"…
        ]
    },
    {
        "solutionIds": [
            …"scenario B"…
        ],
        "connectionTypes": [
            …"scenario B"…
        ],
        "conditions": [
            …"scenario B"…
        ]
    }
    ]
}

```

## <a name="supporting-condition-ranges"></a>支援的條件範圍 ##

您也可以定義條件範圍，方法是使用相同的屬性來定義多個「條件」區塊，但使用不同的運算子。

當相同的屬性使用不同的運算子來定義時，只要此值介於兩個條件之間，就會顯示此工具。

例如，只要作業系統是6.3.0 與10.0.0 之間的版本，就會顯示此工具：

``` json
"entryPoints": [
{
    "entryPointType": "tool",
    "name": "main",
    "urlName": "processes",
    "displayName": "resources:strings:displayName",
    "description": "resources:strings:description",
    "icon": "sme-icon:icon-win-serverProcesses",
    "path": "",
    "requirements": [
    {
        "solutionIds": [
             "msft.sme.server-manager!servers"
        ],
        "connectionTypes": [
             "msft.sme.connection-type.server"
        ],
        "conditions": [
        {
            "inventory": {
                "operatingSystemVersion": {
                    "type": "version",
                    "operator": "gt",
                    "value": "6.3.0"
                },
            }
        },
        {
            "inventory": {
                "operatingSystemVersion": {
                    "type": "version",
                    "operator": "lt",
                    "value": "10.0.0"
                }
            }
        }
        ]
    }
    ]
}

```
