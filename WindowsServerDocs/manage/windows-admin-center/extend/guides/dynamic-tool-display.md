---
title: 控制您的工具在解決方案中的可見性
description: 控制 Windows Admin Center SDK （專案檀香山） 方案中的工具的可見性
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f3f34b4c86854bfc55cf4b1b57a0fd3c2baf2ffc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839249"
---
# <a name="control-your-tools-visibility-in-a-solution"></a>控制您的工具在解決方案中的可見性 #

>適用於：Windows Admin Center，Windows Admin Center 預覽

可能有您想来排除 （或隱藏） 擴充功能或工具，從可用的工具清單。 例如，如果您的工具的目標只有 Windows Server 2016 （不是較舊版本），您可能不想連接到 Windows Server 2012 R2 伺服器若要查看您的工具在所有的使用者。 (假設使用者體驗中-他們按一下它，等到工具載入，才能收到訊息，其功能不適用於其連線。)您可以定義何時顯示 （或隱藏） 您的工具 manifest.json 檔案中的功能。

## <a name="options-for-deciding-when-to-show-a-tool"></a>用於決定何時要顯示的工具 ##

有三個不同的選項，您可以使用它來判斷您的工具是否顯示以及是否適用於特定伺服器或叢集連線。

* localhost
* 清查 （的屬性陣列）
* 指令碼

### <a name="localhost"></a>LocalHost ###

LocalHost 條件物件屬性包含布林值，如果連接的節點為 localHost （相同電腦上所安裝的 Windows Admin Center） 推斷可評估與否。 將值傳遞給屬性，您指定何時 （條件） 若要顯示的工具。 例如若您只想要顯示如果使用者實際上連線到本機主機的工具，請將它設定如下：

``` json
"conditions": [
{
    "localhost": true
}]
```

或者，如果您只想您時要顯示的工具連接的節點*不是*localhost:

``` json
"conditions": [
{
    "localhost": false
}]
```

以下是組態設定如下所示只顯示一個工具時連接的節點不是 localhost:

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

### <a name="inventory-properties"></a>清查內容 ###

SDK 包含一組預先策劃的清查屬性，可用來建置條件來確定當您的工具應該是可用與否。 在 [清查] 陣列中有九個不同的屬性：

| 屬性名稱 | 必須是實值型別 |
| ------------- | ------------------- |
| computerManufacturer | 字串 |
| operatingSystemSKU | 數字 |
| operatingSystemVersion | version_string (例如："10.1.*") |
| productType | 數字 |
| clusterFqdn | 字串 |
| isHyperVRoleInstalled | 布林值 |
| isHyperVPowershellInstalled | 布林值 |
| isManagementToolsAvailable | 布林值 |
| isWmfInstalled | 布林值 |

清查陣列中的每個物件必須符合下列的 json 結構：

``` json
"<property name>": {
    "type": "<expected type>",
    "operator": "<defined operator to use>",
    "value": "<expected value to evaluate using the operator>"
}
```

#### <a name="operator-values"></a>運算子的值 ####

| 運算子 | 描述 |
| -------- | ----------- |
| gt | 大於 |
| ge | 大於或等於 |
| lt | 小於 |
| le | 小於或等於 |
| eq | 等於 |
| ne | 不等於 |
| 為 | 檢查某個值是否為 true |
| 否 | 檢查某個值是否為 false |
| 包含 | 將字串中存在的項目 |
| notContains | 項目不存在於字串 |

#### <a name="data-types"></a>資料類型 ####

'Type' 屬性的可用選項：

| 類型 | 描述 |
| ---- | ----------- |
| version | 版本號碼 (例如：10.1.*) |
| 數字 | 數字的值 |
| 字串 | 字串值 |
| 布林值 | true 或 false |

#### <a name="value-types"></a>實值型別 ####

'Value' 屬性會接受這些類型：

* 字串
* 數字
* 布林值

格式正確的清查條件集看起來像這樣：

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

最後，您可以執行自訂的 PowerShell 指令碼，來識別節點的狀態與可用性。 所有的指令碼必須傳回的物件具有下列結構：

``` ps
@{
    State = 'Available' | 'NotSupported' | 'NotConfigured';
    Message = '<Message to explain the reason of state such as not supported and not configured.>';
    Properties =
        @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' },
        @{Name='Prop2'; Value = 12345678; Type='number'; };
}
```
State 屬性是重要的值，會控制要顯示或隱藏您在 [工具] 清單中的延伸模組的決策。  允許的值包括：
| 值 | 描述 |
| ---- | ----------- |
| 可用 | 擴充功能應該會顯示在 [工具] 清單中。 |
| NotSupported | 擴充功能應該不會顯示在 [工具] 清單中。 |
| NotConfigured | 這是可供此工具之前，會提示使用者進行其他設定的未來工作的預留位置值。  目前此值將會導致顯示的工具，功能相當於 '可用'。 |

例如，如果我們想載入只有當遠端伺服器已安裝的 BitLocker 工具時，指令碼看起來像這樣：

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

使用指令碼選項的進入點設定看起來像這樣：

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

您可以使用多個一組需求來判斷何時要藉由定義多個 「 需求 」 區塊中顯示您的工具。

例如，若要顯示您的工具，如果 < 狀況 A > 或者 < 狀況 B > 為 true 時，定義兩個需求區塊;如果任何一種，則為 true （也就是符合所有條件需求區塊內），此工具會顯示。

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

您也可以藉由定義多個具有相同的屬性，但具有不同運算子的 [條件] 區塊來定義一組條件。

當相同的屬性定義具有不同的運算子時，此工具會顯示值之間的兩個條件，則為。

比方說，這項工具會顯示，只要作業系統是 6.3.0 和 10.0.0 之間的版本：

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
