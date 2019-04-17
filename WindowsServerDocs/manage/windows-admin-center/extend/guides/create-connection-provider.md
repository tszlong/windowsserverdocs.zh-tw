---
title: 建立連線的提供者的解決方案擴充功能
description: 開發 Windows Admin Center SDK (Project Honolulu) 的解決方案擴充功能-建立連線的提供者
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 883fba96fcb71cb1c6e8162c1564d66924c4e24d
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081135"
---
# 建立連線的提供者的解決方案擴充功能

>適用於：Windows Admin Center、Windows Admin Center 預覽版

連線提供者中扮演重要角色在 Windows Admin Center 如何定義，並會與可連接的物件或多個目標進行通訊。 主要連線提供者會執行動作時正在進行連線，例如確保目標在線上且可以使用，以及確保能連線的使用者有存取目標權限。

根據預設，Windows Admin Center 隨附於下列連線提供者：

* 伺服器
* Windows 用戶端
* 容錯移轉叢集
* HCI 叢集

若要建立您自己的自訂連線提供者，請依照下列步驟：

* 新增至連接提供者詳細資料 ```manifest.json```
* 定義連線狀態提供者
* 在應用程式層級中實作連接提供者

## 新增 manifest.json 連接提供者的詳細資料

現在，我們會逐步引導您需要知道要在您的專案中定義連線提供者```manifest.json```檔案。

### 在 manifest.json 建立項目

```manifest.json```檔案位於 \src 資料夾，除了其他眾多包含定義的進入點到您的專案。 進入點的類型包括工具、 解決方案，以及連線提供者。 我們會定義連線提供者。

以下是連接提供者中的項目 manifest.json 的範例：

``` json
    {
      "entryPointType": "connectionProvider",
      "name": "addServer",
      "path": "/add",
      "displayName": "resources:strings:addServer_displayName",
      "icon": "sme-icon:icon-win-server",
      "description": "resources:strings:description",
      "connectionType": "msft.sme.connection-type.server",
      "connectionTypeName": "resources:strings:addServer_connectionTypeName",
      "connectionTypeUrlName": "server",
      "connectionTypeDefaultSolution": "msft.sme.server-manager!servers",
      "connectionTypeDefaultTool": "msft.sme.server-manager!overview",
      "connectionStatusProvider": {
        "powerShell": {
          "script": "## Get-My-Status ##\nfunction Get-Status()\n{\n# A function like this would be where logic would exist to identify if a node is connectable.\n$status = @{label = $null; type = 0; details = $null; }\n$caption = \"MyConstCaption\"\n$productType = \"MyProductType\"\n# A result object needs to conform to the following object structure to be interpreted properly by the Windows Admin Center shell.\n$result = @{ status = $status; caption = $caption; productType = $productType; version = $version }\n# DO FANCY LOGIC #\n# Once the logic is complete, the following fields need to be populated:\n$status.label = \"Display Thing\"\n$status.type = 0 # This value needs to conform to the LiveConnectionStatusType enum. >= 3 represents a failure.\n$status.details = \"success stuff\"\nreturn $result}\nGet-Status"
        },
        "displayValueMap": {
          "wmfMissing-label": "resources:strings:addServer_status_wmfMissing_label",
          "wmfMissing-details": "resources:strings:addServer_status_wmfMissing_details",
          "unsupported-label": "resources:strings:addServer_status_unsupported_label",
          "unsupported-details": "resources:strings:addServer_status_unsupported_details"
        }
      }
    },
```

「 ConnnectionProvider 」 類型的進入點表示 Windows Admin Center 殼層的設定項目是將由解決方案所用來驗證連線狀態的提供者。 連線提供者進入點包含數個重要的屬性，定義如下：

| 屬性 | 描述 |
| -------- | ----------- |
| entryPointType | 這是必要的屬性。 有三個有效的值: 「 工具 」、 「 解決方案 」，以及 「 消費者 」。 | 
| name | 識別連接提供者解決方案的範圍內。 這個值必須是唯一的完整的 Windows Admin Center 執行個體 （而不只是方案） 內。 |
| path | 如果您將會設定解決方案所來表示 「 新增連接 「 UI，URL 路徑。 這個值必須對應到設定應用程式 routing.module.ts 檔案中的路線。 當方案進入點設定為使用連線 rootNavigationBehavior 時，這個路由將會載入的殼層來顯示新增連線 UI 的模組。 詳細資訊可在 [\] 區段中 rootNavigationBehavior。 |
| displayName | 在此處輸入的值會顯示在右邊的殼層，黑色 Windows Admin Center 列使用者載入的解決方案連線頁面時的下方。 |
| icon | 代表的解決方案下拉式功能表中，用以代表方案的圖示。 |
| description | 輸入的進入點的簡短描述。 |
| 連線類型 | 代表將會載入，提供者的連線類型。 在此處輸入的值也會在方案進入點指定方案可以載入這些連線使用。 在此處輸入的值也會使用工具進入點中以表示該工具與這種類型相容。 在此處輸入這個值也會用於提交到 RPC 連線物件呼叫 「 新增視窗上 「，在應用程式層級的實作步驟。 |
| connectionTypeName | 連線表格中用來代表使用您的連線提供者的連線。 此屬性預期會得到的型別複數的名稱。 |
| connectionTypeUrlName | 用於建立 URL 表示載入的方案之後 Windows Admin Center 已連線到執行個體。 連線之後，以及目標之前，會使用此項目。 在此範例中，「 connectionexample 」 會是這個值 URL 中出現的位置：http://localhost:6516/solutionexample/connections/connectionexample/con-fake1.corp.contoso.com |
| connectionTypeDefaultSolution | 表示應該會載入連接提供者的預設元件。 這個值是的組合: [a] 頂端的資訊清單; 定義的延伸模組套件的名稱[b] 驚嘆號 （！）;[c] 方案進入點名稱。    針對專案，以副檔名 「 msft.sme.mySample-」，並使用名稱 「 範例 」 的解決方案進入點，此值會是 「 msft.sme.solutionExample 擴充功能 ！ 範例 」。 |
| connectionTypeDefaultTool | 代表預設工具，應該會載入成功的連線。 這個屬性值組成的兩個部分，類似於 connectionTypeDefaultSolution。 這個值是的組合: [a] 頂端的資訊清單; 定義的延伸模組套件的名稱[b] 驚嘆號 （！）;[一開始就應該會載入工具 c] 工具進入點名稱。 針對專案，以副檔名 「 msft.sme.solutionExample-」，並使用名稱 「 範例 」 的解決方案進入點，此值會是 「 msft.sme.solutionExample 擴充功能 ！ 範例 」。 |
| connectionStatusProvider | 請參閱 「 定義連線狀態提供者 」 區段 |

## 定義連線狀態提供者

連線狀態提供者為的機制，目標驗證在線上且可以使用，以確保能連線的使用者有存取目標權限。 目前有兩種類型的連線狀態提供者： PowerShell 和 RelativeGatewayUrl。

*   PowerShell 連線狀態提供者
    *   判斷目標是連線且可存取的 PowerShell 指令碼。 在下面定義的單一屬性 「 status 」，將物件必須傳回結果。
*   RelativeGatewayUrl 連線狀態提供者
    *   判斷目標是連線且可存取 rest 呼叫。 在下面定義的單一屬性 「 status 」，將物件必須傳回結果。

### 定義狀態

連線狀態提供者所需的單一屬性的物件會傳回```status```，符合下列格式：

``` json
{
    status: {
        label: string;
        type: int;
        details: string;
    }
}
```

狀態屬性：

* Label
    * 描述的狀態標籤傳回類型。 請注意，可以在執行階段中對應的標籤值。 請參閱下方的項目，適用於執行階段中的對應值。

* 類型
    * 狀態的傳回類型。 類型都有下列列舉值。 針對任何值為 2 或上方，平台會不瀏覽到已連接的物件，並將會在 UI 中顯示錯誤。

類型：

| 值 | 說明 |
| ----- | ----------- |
| 0 | Online |
| 1 | 警告 |
| 2 | Unauthorized |
| 3 | 錯誤 |
| 4 | 嚴重 |
| 5 | Unknown |

* 詳細資料
    * 描述狀態的其他詳細資料傳回類型。

### PowerShell 連線狀態提供者指令碼

連線狀態提供者 PowerShell 指令碼判斷目標是連線且可存取的 PowerShell 指令碼。 必須包含單一屬性為 「 status 」 物件傳回結果。 如下所示的範例指令碼。

範例 PowerShell 指令碼：

``` ts
## Get-My-Status ##

function Get-Status()
{
    # A function like this would be where logic would exist to identify if a node is connectable.
    $status = @{label = $null; type = 0; details = $null; }
    $caption = "MyConstCaption"
    $productType = "MyProductType"

    # A result object needs to conform to the following object structure to be interperated properly by the Windows Admin Center shell.
    $result = @{ status = $status; caption = $caption; productType = $productType; version = $version }

    # DO FANCY LOGIC #

    # Once the logic is complete, the following fields need to be populated:
    $status.label = "Display Thing"
    $status.type = 0 # This value needs to conform to the LiveConnectionStatusType enum. >= 3 represents a failure.
    $status.details = "success stuff"

    return $result
}

Get-Status
```

### 定義 RelativeGatewayUrl 連線狀態提供者方法

連線狀態提供者```RelativeGatewayUrl```方法會呼叫 rest API 來判斷目標是否連線且可存取。 必須包含單一屬性為 「 status 」 物件傳回結果。 RelativeGatewayUrl 的連線提供者 manifest.json 中的項目範例如下所示。

``` json
    {
      "entryPointType": "connectionProvider",
      "name": "addServer",
      "path": "/add/server",
      "displayName": "resources:strings:addServer_displayName",
      "icon": "sme-icon:icon-win-server",
      "description": "resources:strings:description",
      "connectionType": "msft.sme.connection-type.server",
      "connectionTypeName": "resources:strings:addServer_connectionTypeName",
      "connectionTypeUrlName": "server",
      "connectionTypeDefaultSolution": "msft.sme.server-manager!servers",
      "connectionTypeDefaultTool": "msft.sme.server-manager!overview",
      "connectionStatusProvider": {
        "relativeGatewayUrl": "<URL here post /api>",
        "displayValueMap": {
          "wmfMissing-label": "resources:strings:addServer_status_wmfMissing_label",
          "wmfMissing-details": "resources:strings:addServer_status_wmfMissing_details",
          "unsupported-label": "resources:strings:addServer_status_unsupported_label",
          "unsupported-details": "resources:strings:addServer_status_unsupported_details"
        }
      }
    },
```

使用 RelativeGatewayUrl 附註：

* 「 relativeGatewayUrl 」 指定位置，以取得從閘道 URL 的連線狀態。 此 URI 是從 /api 相對的。 如果 URL 中找到 $connectionName，它將會取代連線的名稱。
* 所有 relativeGatewayUrl 屬性必須都執行主機閘道，可以達到這個目的藉由建立閘道擴充功能

### 在執行階段中的對應值

傳回的物件可以用來格式化的狀態標籤與詳細資料值調整時間的提供者的 「 defaultValueMap 」 屬性中包含機碼和值。

為例，如果您新增下列值，任何時候該 「 defaultConnection_test 」 顯示做為值標籤或詳細資料，Windows Admin Center 將會自動將金鑰取代已設定的資源字串值。

``` json
    "defaultConnection_test": "resources:strings:addServer_status_defaultConnection_label"
```

## 在應用程式層級中實作連接提供者

現在我們要在應用程式層中，實作連接提供者，藉由建立實作 OnInit Html5、css、angular、typescript 類別。 這個類別具有下列功能：

| 函式 | 描述 |
| -------- | ----------- |
| 建構函式 (私人 appContextService: AppContextService，私人路由： ActivatedRoute) |  |
| 公用 ngOnInit() |  |
| 公用 onSubmit() | 包含邏輯以新增嘗試建立連線時，更新殼層 |
| 公用 onCancel() | 其中包含取消加入嘗試連線時，更新殼層的邏輯 |

### 定義 onSubmit

```onSubmit``` 問題 RPC 回呼來通知 「 新增連線 」 的殼層應用程式內容。 基本的呼叫會單獨使用 「 updateData 」，就像這樣：

``` ts
this.appContextService.rpc.updateData(
    EnvironmentModule.nameOfShell,
    '##',
    <RpcUpdateData>{
        results: {
            connections: connections,
            credentials: this.useCredentials ? this.creds : null
        }
    }
);
```

結果是一個連線內容，也就是符合下列結構的物件陣列：

``` ts

/**
 * The connection attributes class.
 */
export interface ConnectionAttribute {

    /**
     * The id string of this attribute
     */
    id: string;

    /**
     * The value of the attribute. used for attributes that can have variable values such as Operating System
     */
    value?: string | number;
}

/**
 * The connection class.
 */
export interface Connection {

    /**
     * The id of the connection, this is unique per connection
     */
    id: string;

    /**
     * The type of connection
     */
    type: string;

    /**
     * The name of the connection, this is unique per connection type
     */
    name: string;

    /**
     * The property bag of the connection
     */
    properties?: ConnectionProperties;

    /**
     * The ids of attributes identified for this connection
     */
    attributes?: ConnectionAttribute[];

    /**
     * The tags the user(s) have assigned to this connection
     */
    tags?: string[];
}

/**
 * Defines connection type strings known by core
 * Be careful that these strings match what is defined by the manifest of @msft-sme/server-manager
 */
export const connectionTypeConstants = {
    server: 'msft.sme.connection-type.server',
    cluster: 'msft.sme.connection-type.cluster',
    hyperConvergedCluster: 'msft.sme.connection-type.hyper-converged-cluster',
    windowsClient: 'msft.sme.connection-type.windows-client',
    clusterNodesProperty: 'nodes'
};
```

### 定義 onCancel

```onCancel``` 藉由傳遞空白連線陣列取消 「 新增 」 嘗試連線：

``` ts
this.appContextService.rpc.updateData(EnvironmentModule.nameOfShell, '##', <RpcUpdateData>{ results: { connections: [] } });
```

## 連線提供者範例

以下是完整 Html5、css、angular、typescript 類別實作連接提供者。 請注意，「 連線類型 」 字串，符合 「 連線類型 manifest.json 的連線提供者中所定義。

``` ts
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
import { AppContextService } from '@msft-sme/shell/angular';
import { Connection, ConnectionUtility } from '@msft-sme/shell/core';
import { EnvironmentModule } from '@msft-sme/shell/dist/core/manifest/environment-modules';
import { RpcUpdateData } from '@msft-sme/shell/dist/core/rpc/rpc-base';
import { Strings } from '../../generated/strings';

@Component({
  selector: 'add-example',
  templateUrl: './add-example.component.html',
  styleUrls: ['./add-example.component.css']
})
export class AddExampleComponent implements OnInit {
  public newConnectionName: string;
  public strings = MsftSme.resourcesStrings<Strings>().SolutionExample;
  private connectionType = 'msft.sme.connection-type.example'; // This needs to match the connectionTypes value used in the manifest.json.
  
  constructor(private appContextService: AppContextService, private route: ActivatedRoute) {
    // TODO:
  }

  public ngOnInit() {
    // TODO
  }

  public onSubmit() {
    let connections: Connection[] = [];

    let connection = <Connection> {
      id: ConnectionUtility.createConnectionId(this.connectionType, this.newConnectionName),
      type: this.connectionType,
      name: this.newConnectionName
    };

    connections.push(connection);

    this.appContextService.rpc.updateData(
      EnvironmentModule.nameOfShell,
      '##',
      <RpcUpdateData> {
        results: {
          connections: connections,
          credentials: null
        }
      }
    );
  }

  public onCancel() {
    this.appContextService.rpc.updateData(
      EnvironmentModule.nameOfShell, '##', <RpcUpdateData>{ results: { connections: [] } });
  }
}

```
