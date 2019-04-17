---
title: 控制您的工具在方案中的可見性
description: 控制您的工具在 Windows Admin Center SDK (Project Honolulu) 解決方案中的可見性
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f3f34b4c86854bfc55cf4b1b57a0fd3c2baf2ffc
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/20/2018
ms.locfileid: "4080965"
---
# 控制您的工具在方案中的可見性 #

>適用於：Windows Admin Center、Windows Admin Center 預覽版

有時可能會當您想要排除 （或隱藏） 您的擴充功能或從可用的工具清單的工具。 例如，如果您的工具的目標是只有 Windows Server 2016 （不是較舊版本），您可能不想連線到 Windows Server 2012 R2 的伺服器，以查看您的工具完全的使用者。 (假設使用者體驗-他們在其上按一下，等待工具載入，只以取得其功能不適用於其連線的訊息。)您可以定義何時要顯示 （或隱藏） 您的工具 manifest.json 檔案中的功能。

## 決定何時顯示工具的選項 ##

有三種不同的選項，您可以使用它來判斷是否應該是您的工具，適用於特定的伺服器或叢集連線和顯示。

* localhost
* （在陣列的內容） 的詳細目錄
* 指令碼

### LocalHost ###

條件物件的 localHost 屬性包含可以評估推斷如果連線節點是本機主機 （相同的電腦安裝 Windows Admin Center） 的布林值，或不。 藉由傳遞給屬性的值，您指示何時 （條件） 來顯示工具。 如果您只想要顯示如果本機主機事實上連線的使用者的工具範例中，將它設定如下：

``` json
"conditions": [
{
    "localhost": true
}]
```

或者，如果您只想要顯示當您工具連線的節點*不是*localhost:

``` json
"conditions": [
{
    "localhost": false
}]
```

以下的組態設定起來會像這樣以便只顯示一個工具時的連線的節點不是本機主機：

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

### 詳細目錄屬性 ###

SDK 包含一組預先於經過挑選的詳細目錄屬性，您可以用來建置條件，以判斷當您的工具應該使用或不。 '詳細目錄' 陣列中有九個不同的屬性：

| 屬性名稱 | 預期的值類型 |
| ------------- | ------------------- |
| computerManufacturer | 字串 |
| operatingSystemSKU | 數目 |
| operatingSystemVersion | version_string (例如: 「 10.1。 * 」) |
| productType | 數目 |
| clusterFqdn | 字串 |
| isHyperVRoleInstalled | 布林值 |
| isHyperVPowershellInstalled | 布林值 |
| isManagementToolsAvailable | 布林值 |
| isWmfInstalled | 布林值 |

詳細目錄陣列中的每個物件都必須符合下列 json 結構：

``` json
"<property name>": {
    "type": "<expected type>",
    "operator": "<defined operator to use>",
    "value": "<expected value to evaluate using the operator>"
}
```

#### 電信業者的值 ####

| 運算子 | 描述 |
| -------- | ----------- |
| t | 大於 |
| ge | 大於或等於 |
| l | 小於 |
| le | 小於或等於 |
| eq | 等於 |
| ne | 不等於 |
|  為  | 檢查是否值為 true |
| 不 | 檢查是否值 false |
| 包含 | 將字串中的項目存在 |
| notContains | 項目不存在的字串 |

#### 資料類型 ####

'類型' 屬性可用的選項：

| 類型 | 說明 |
| ---- | ----------- |
| 版本 | 版本號碼 (例如： 10.1。 *) |
| 數目 | 數字值 |
| 字串 | 字串值 |
| 布林值 | true 或 false |

#### 值類型 ####

'Value' 屬性，可接受這些類型：

* 字串
* 數目
* 布林值

格式錯誤的詳細目錄條件組看起來像這樣：

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

### 指令碼 ###

最後，您可以執行自訂 PowerShell 指令碼來識別的可用性和節點的狀態。 所有指令碼必須傳回具有下列結構的物件：

``` ps
@{
    State = 'Available' | 'NotSupported' | 'NotConfigured';
    Message = '<Message to explain the reason of state such as not supported and not configured.>';
    Properties =
        @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' },
        @{Name='Prop2'; Value = 12345678; Type='number'; };
}
```
狀態屬性時將會控制決定来顯示或隱藏您的擴充功能，在 [工具] 清單中的重要值。  允許的值為：
| 值 | 描述 |
| ---- | ----------- |
| 可用 | 擴充功能應該會顯示在 [工具] 清單中。 |
| NotSupported | 擴充功能應該不會顯示在 [工具] 清單中。 |
| NotConfigured | 這是未來會提示使用者提供額外的設定，此工具進行可用之前，進行的工作的預留位置值。  目前這個值會造成在工具中顯示，且是以 '可用' 功能對等項目。 |

例如，如果我們想要載入只有當遠端伺服器有 BitLocker 安裝的工具，指令碼看起來像這樣：

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

使用 \ 指令碼選項的進入點設定看起來像這樣：

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

## 支援多個需求集 ##

您可以使用多個組需求來判斷何時顯示您的工具，方法是定義多個 < 需求 > 區塊。

例如，若要顯示您的工具，如果 「 案例 A 」 或者 「 案例 B 」 為 true，請定義兩個需求區塊。如果任何一種 （也就是所有需求區塊內符合條件），此工具會顯示。

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

## 支援的條件範圍 ##

您也可以定義一系列的條件，方法是定義多個 「 條件 」 區塊具有相同的屬性，但不含不同的運算子。

使用不同的運算子定義的相同屬性時，此工具會顯示，只要的值是之間的兩個條件。

例如，此工具會顯示，只要作業系統是 6.3.0 和 10.0.0 之間的版本：

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
