---
title: 建立方案延伸模組的連線提供者
description: 開發解決方案的擴充功能 Windows Admin Center SDK （專案檀香山）-建立連線提供者
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: b79e832ee45990d18baf4c211ab68b907134ceb7
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811841"
---
# <a name="create-a-connection-provider-for-a-solution-extension"></a>建立方案延伸模組的連線提供者

>適用於：Windows Admin Center，Windows Admin Center 預覽

連線提供者會扮演著重要的角色，在 Windows Admin Center 如何定義及與可連接物件或多個目標通訊。 主要連接提供者執行動作，而要建立連線，例如確保目標在線上而且可用，並同時確保連線的使用者具有權限來存取目標。

根據預設，Windows Admin Center 隨附下列連線提供者：

* Server
* Windows 用戶端
* 容錯移轉叢集
* HCI 叢集

若要建立您自己自訂的連線提供者，請遵循下列步驟：

* 新增到連線提供者詳細資料 ```manifest.json```
* 定義連線狀態提供者
* 在 應用程式層中實作連接提供者

## <a name="add-connection-provider-details-to-manifestjson"></a>將連線提供者詳細資料新增至 manifest.json

現在我們將詳細說明您需要知道要在您的專案中定義連線提供者```manifest.json```檔案。

### <a name="create-entry-in-manifestjson"></a>Manifest.json 中建立項目

```manifest.json```檔案位於 \src 資料夾中，而且在其他方面，包含定義的進入點到您的專案。 項目點的型別包含工具、 解決方案和連線提供者。 我們會定義連線提供者。

連線提供者中的項目 manifest.json 的範例如下：

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

類型"connnectionProvider"的進入點表示 Windows Admin Center 殼層所設定的項目是方案將會用來驗證連線狀態的提供者。 連線提供者的進入點包含數個重要屬性，定義如下：

| 屬性 | 描述 |
| -------- | ----------- |
| entryPointType | 這是必要的屬性。 有三個有效的值: 「 工具 」、 「 解決方案 」 和 「 消費者 」。 | 
| name | 識別解決方案的範圍內的連線提供者。 此值必須是完整的 Windows Admin Center 執行個體 （而不只是方案） 內唯一的。 |
| path | 如果它將會設定解決方案，表示 [新增連線] UI 中，URL 路徑。 此值必須對應到已在應用程式 routing.module.ts 檔案中的路由。 當方案進入點設定為使用連線 rootNavigationBehavior 時，此路由會載入殼層來顯示 新增連接 UI 的模組。 如需詳細資訊一節中上 rootNavigationBehavior。 |
| displayName | 在此處輸入的值會顯示在殼層中，當使用者在載入解決方案的 [連線] 頁面上的黑色 Windows Admin Center 列下方的右邊。 |
| 圖示 | 代表解決方案下拉式功能表中，用以代表方案的圖示。 |
| description | 輸入的進入點的簡短描述。 |
| connectionType | 表示將會載入提供者的連接類型。 此處輸入的值也將使用在解決方案進入點中，但會指定解決方案可以載入這些連線。 此處輸入的值會也用於工具項目點，表示此工具會與此型別相容。 在此輸入此值也會用於送出至 RPC 的連接物件上的 「 新增視窗 」，在應用程式層的實作步驟中呼叫。 |
| connectionTypeName | 表示使用您的連線提供者的連接，請使用 [連線] 表格中。 這應該是類型的複數名稱。 |
| connectionTypeUrlName | 用於建立 URL，以代表載入的方案之後 Windows Admin Center 已連線到執行個體。 連線之後，以及目標之前，會使用此項目。 在此範例中，「 connectionexample"會是，這個值會出現在 URL 中： `http://localhost:6516/solutionexample/connections/connectionexample/con-fake1.corp.contoso.com` |
| connectionTypeDefaultSolution | 表示應該載入連線提供者的預設元件。 這個值是組成： <br>[a] 頂端的資訊清單; 定義的延伸模組套件的名稱 <br>[b] 驚嘆號 （！）; <br>[c] 方案進入點名稱。    <br>專案副檔名"msft.sme.mySample-"，並具有名稱"example"的方案項目點，這個值會是"msft.sme.solutionExample 延伸模組 ！ 範例 」。 |
| connectionTypeDefaultTool | 表示預設應該在成功連接之後載入的工具。 這個屬性值組成兩個部分，類似於 connectionTypeDefaultSolution。 這個值是組成： <br>[a] 頂端的資訊清單; 定義的延伸模組套件的名稱 <br>[b] 驚嘆號 （！）; <br>[一開始載入工具 c] 工具進入點名稱。 <br>專案副檔名"msft.sme.solutionExample-"，並具有名稱"example"的方案項目點，這個值會是"msft.sme.solutionExample 延伸模組 ！ 範例 」。 |
| connectionStatusProvider | 請一節 「 定義連線狀態提供者 」，參閱 |

## <a name="define-connection-status-provider"></a>定義連線狀態提供者

連接狀態提供者是用來在線上而且可用，驗證目標的機制，同時確保連線的使用者具有權限來存取目標。 目前有兩種類型的連線狀態提供者：PowerShell 和 RelativeGatewayUrl。

*   <strong>PowerShell 連接狀態提供者</strong>-判斷目標是否為線上且可存取的 PowerShell 指令碼。 傳回的結果必須具有單一屬性 「 狀態 」 下, 面定義的物件。
*   <strong>RelativeGatewayUrl 連接狀態提供者</strong>-判斷目標是否為線上且可存取的 rest 呼叫。 傳回的結果必須具有單一屬性 「 狀態 」 下, 面定義的物件。

### <a name="define-status"></a>定義狀態

連線狀態提供者所傳回的物件具有單一屬性```status```符合下列格式：

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

* <strong>標籤</strong>-標籤描述狀態的傳回型別。 請注意，可以在執行階段對應標籤的值。 請參閱下方的項目，執行階段中的對應值。

* <strong>型別</strong>-狀態傳回型別。 型別具有下列的列舉值。 任何值為 2 以上，平台會瀏覽至已連接的物件，並將在 UI 中顯示一個錯誤。

   類型：

  | 值 | 描述 |
  | ----- | ----------- |
  | 0 | Online |
  | 1 | 警告 |
  | 2 | 未經授權 |
  | 3 | 錯誤 |
  | 4 | 嚴重 |
  | 5 | 不明 |

* <strong>詳細資料</strong>-詳細資料，請描述狀態的傳回型別。

### <a name="powershell-connection-status-provider-script"></a>PowerShell 連接狀態提供者的指令碼

連線狀態提供者的 PowerShell 指令碼會判斷目標是否為線上且可存取的 PowerShell 指令碼。 傳回的結果必須具有 「 狀態 」 的單一屬性的物件。 如下所示的範例指令碼。

範例 PowerShell 指令碼：

```PowerShell
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

### <a name="define-relativegatewayurl-connection-status-provider-method"></a>定義 RelativeGatewayUrl 連接狀態提供者方法

連接狀態提供者```RelativeGatewayUrl```方法會呼叫 rest API，以判斷目標是否線上且可存取。 傳回的結果必須具有 「 狀態 」 的單一屬性的物件。 如下所示的 RelativeGatewayUrl manifest.json 中的連線提供者項目範例。

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

有關使用 RelativeGatewayUrl 注意事項：

* 「 relativeGatewayUrl"會指定從閘道器 URL 取得的連接狀態的位置。 此 URI 是相對於 /api。 如果在 URL 中找到 $connectionName，則它會取代連接的名稱。
* 對主應用程式閘道，這可以藉由建立閘道的擴充功能來達成，必須執行所有 relativeGatewayUrl 屬性

### <a name="map-values-in-runtime"></a>在執行階段中的對應值

傳回的物件可以用來格式化的狀態中的標籤和詳細資料的值來微調時間包括提供者的 「 defaultValueMap"屬性中的索引鍵和值。

比方說，如果您加入下列的值時，隨時該 「 defaultConnection_test"顯示為標籤或詳細資料，值 Windows Admin Center 會自動取代金鑰設定的資源字串值。

``` json
    "defaultConnection_test": "resources:strings:addServer_status_defaultConnection_label"
```

## <a name="implement-connection-provider-in-application-layer"></a>在 應用程式層中實作連接提供者

現在我們要在應用程式層中實作連接提供者，藉由建立可實作 OnInit TypeScript 類別。 此類別具有下列功能：

| 函數 | 描述 |
| -------- | ----------- |
| constructor(private appContextService:AppContextService，私用的路由：ActivatedRoute) |  |
| public ngOnInit() |  |
| public onSubmit() | 包含邏輯，以新增連線嘗試時更新 shell |
| public onCancel() | 包含邏輯，以新增連線嘗試取消時，更新命令介面 |

### <a name="define-onsubmit"></a>定義 onSubmit

```onSubmit``` 問題 RPC 回呼來通知的 [新增連線] 殼層的應用程式內容。 基本的呼叫會使用 「 updateData"，就像這樣：

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

結果是連接屬性，也就是符合下列結構的物件的陣列：

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

### <a name="define-oncancel"></a>定義 onCancel

```onCancel``` 取消 [新增連線] 嘗試藉由傳遞陣列空白的連線：

``` ts
this.appContextService.rpc.updateData(EnvironmentModule.nameOfShell, '##', <RpcUpdateData>{ results: { connections: [] } });
```

## <a name="connection-provider-example"></a>連線提供者範例

以下是完整的 TypeScript 類別，實作連線提供者。 請注意，"connectionType 」 字串符合 「 connectionType manifest.json 中的連線提供者中所定義。

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
