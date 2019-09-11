---
title: 建立解決方案延伸模組的連接提供者
description: 開發解決方案延伸模組 Windows 管理中心 SDK （Project 檀香山）-建立連接提供者
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: c1f3a7f7004b573fece71cdaf2f43661c13ad496
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869627"
---
# <a name="create-a-connection-provider-for-a-solution-extension"></a>建立解決方案延伸模組的連接提供者

>適用於：Windows Admin Center、Windows Admin Center 預覽版

連接提供者在 Windows 系統管理中心如何定義及與可連線物件或目標通訊的方式中扮演重要角色。 主要是連接提供者在進行連接時執行動作，例如確保目標在線上且可供使用，以及確保連接的使用者具有存取目標的許可權。

根據預設，Windows 管理中心會隨附下列連接提供者：

* 伺服器
* Windows 用戶端
* 容錯移轉叢集
* HCI 叢集

若要建立您自己的自訂連接提供者，請遵循下列步驟：

* 將連接提供者詳細資料新增至```manifest.json```
* 定義連接狀態提供者
* 在應用層中執行連接提供者

## <a name="add-connection-provider-details-to-manifestjson"></a>將連接提供者詳細資料新增至資訊清單 json

現在，我們將逐步解說您在專案的```manifest.json```檔案中定義連接提供者所需知道的事項。

### <a name="create-entry-in-manifestjson"></a>在資訊清單中建立專案。 json

此```manifest.json```檔案位於 \src 資料夾中，其中包含專案的進入點定義。 進入點的類型包括工具、解決方案和連接提供者。 我們將定義連接提供者。

在資訊清單中，連接提供者專案的範例如下：

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

類型為 "connnectionProvider" 的進入點向 Windows 管理中心 shell 表示要設定的專案是解決方案用來驗證連接狀態的提供者。 連接提供者進入點包含許多重要的屬性，定義如下：

| 屬性 | 描述 |
| -------- | ----------- |
| entryPointType | 這是必要的屬性。 有三個有效的值： "tool"、"solution" 和 "connectionProvider"。 | 
| name | 識別解決方案範圍內的連接提供者。 這個值在完整的 Windows 管理中心實例（而不只是解決方案）內必須是唯一的。 |
| path | 表示「新增連接」 UI 的 URL 路徑（如果它將由方案設定）。 此值必須對應至在應用程式路由中設定的路由。模組. ts 檔案。 當方案進入點設定為使用 [連線] rootNavigationBehavior 時，此路由會載入命令介面所使用的模組，以顯示 [新增連線] UI。 RootNavigationBehavior 一節中提供的詳細資訊。 |
| displayName | 在這裡輸入的值會顯示在命令介面的右手邊，在使用者載入解決方案的 [連線] 頁面時，會在黑色 Windows 系統管理中心列底下。 |
| 圖示 | 表示解決方案下拉式功能表中用來表示方案的圖示。 |
| description | 輸入進入點的簡短描述。 |
| connectionType | 表示提供者將載入的連線類型。 這裡輸入的值也會用於解決方案進入點，以指定解決方案可以載入這些連接。 這裡輸入的值也會用在工具進入點中，表示該工具與這個型別相容。 在此輸入的這個值也會用於在「新增視窗」上提交至 RPC 呼叫的連線物件（在應用層的執行步驟中）。 |
| connectionTypeName | 在 connections 資料表中用來代表使用您連接提供者的連接。 這應該是類型的複數名稱。 |
| connectionTypeUrlName | 在 Windows 系統管理中心已連接到實例之後，用來建立 URL 以代表載入的方案。 此專案會在連接之後和目標之前使用。 在此範例中，"connectionexample" 是此值出現在 URL 中的位置：`http://localhost:6516/solutionexample/connections/connectionexample/con-fake1.corp.contoso.com` |
| connectionTypeDefaultSolution | 表示連接提供者應該載入的預設元件。 這個值是的組合： <br>[a] 在資訊清單頂端定義的擴充套件名稱; <br>[b] 驚嘆號（！）; <br>[c] 方案進入點名稱。    <br>對於名稱為 "msft. mySample-extension" 的專案，以及名稱為 "example" 的方案進入點，此值會是 "msft. solutionExample-extension！ example"。 |
| connectionTypeDefaultTool | 表示應該在成功連接時載入的預設工具。 這個屬性值是由兩個部分所組成，類似于 connectionTypeDefaultSolution。 這個值是的組合： <br>[a] 在資訊清單頂端定義的擴充套件名稱; <br>[b] 驚嘆號（！）; <br>[c] 應該一開始載入之工具的工具進入點名稱。 <br>對於名稱為 "msft. solutionExample-extension" 的專案，以及名稱為 "example" 的方案進入點，此值會是 "msft. solutionExample-extension！ example"。 |
| connectionStatusProvider | 請參閱「定義連接狀態提供者」一節。 |

## <a name="define-connection-status-provider"></a>定義連接狀態提供者

「線上狀態提供者」是一種機制，可讓目標驗證為線上且可用，同時確保連接的使用者具有存取目標的許可權。 目前有兩種類型的連接狀態提供者：PowerShell 和 RelativeGatewayUrl。

*   <strong>Powershell 線上狀態提供者</strong>-判斷目標是否在線上，以及是否可以使用 PowerShell 腳本來存取。 結果必須在具有單一屬性 "status" 的物件中傳回，如下所定義。
*   <strong>RelativeGatewayUrl 連接狀態提供者</strong>-判斷目標是否在線上，以及是否可透過 rest 呼叫來存取。 結果必須在具有單一屬性 "status" 的物件中傳回，如下所定義。

### <a name="define-status"></a>定義狀態

連接狀態提供者必須傳回具有符合下列格式之單一屬性```status```的物件：

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

* <strong>標籤</strong>-描述狀態傳回類型的標籤。 請注意，您可以在執行時間中對應標籤的值。 請參閱下方的專案，以在執行時間中對應值。

* <strong>類型</strong>-狀態傳回類型。 類型具有下列列舉值。 若為任何值2或以上，平臺將不會流覽至已連接的物件，而且 UI 中會顯示錯誤。

   各種

  | 值 | 描述 |
  | ----- | ----------- |
  | 0 | Online |
  | 1 | 警告 |
  | 2 | 未經授權 |
  | 3 | Error |
  | 4 | 時發生 |
  | 5 | 不明 |

* <strong>詳細資料</strong>-描述狀態傳回類型的其他詳細資料。

### <a name="powershell-connection-status-provider-script"></a>PowerShell 線上狀態提供者腳本

線上狀態提供者 PowerShell 腳本會判斷目標是否在線上，以及是否可以使用 PowerShell 腳本來存取。 必須在具有單一屬性「狀態」的物件中傳回結果。 範例腳本如下所示。

範例 PowerShell 腳本：

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

線上狀態提供者```RelativeGatewayUrl```方法會呼叫 rest API，以判斷目標是否在線上且可供存取。 必須在具有單一屬性「狀態」的物件中傳回結果。 RelativeGatewayUrl 的範例連接提供者專案如下所示。

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

使用 RelativeGatewayUrl 的相關注意事項：

* "relativeGatewayUrl" 指定從閘道 URL 取得連接狀態的位置。 此 URI 是相對於/api。 如果在 URL 中找到 $connectionName，則會以連接的名稱取代。
* 所有的 relativeGatewayUrl 屬性都必須針對主機閘道執行，這可以藉由建立閘道延伸模組來完成

### <a name="map-values-in-runtime"></a>在執行時間中對應值

您可以在提供者的 "defaultValueMap" 屬性中包含索引鍵和值，以微調時間來格式化狀態傳回物件中的標籤和詳細資料值。

例如，如果您新增下列值，則每當「defaultConnection_test」顯示為標籤或詳細資料的值時，Windows 管理中心會自動將金鑰取代為設定的資源字串值。

``` json
    "defaultConnection_test": "resources:strings:addServer_status_defaultConnection_label"
```

## <a name="implement-connection-provider-in-application-layer"></a>在應用層中執行連接提供者

現在，我們將建立可執行 OnInit 的 TypeScript 類別，以在應用層中實作為連接提供者。 類別具有下列函數：

| 函數 | 描述 |
| -------- | ----------- |
| 函數（私用 appCoNtextService：AppCoNtextService，私用路由：ActivatedRoute) |  |
| 公用 ngOnInit （） |  |
| 公用 onSubmit （） | 包含在嘗試進行新增連接時，用來更新 shell 的邏輯 |
| 公用 onCancel （） | 包含在取消新增連接嘗試時，用來更新 shell 的邏輯 |

### <a name="define-onsubmit"></a>定義 onSubmit

```onSubmit```向應用程式內容發出 RPC 呼叫，以通知 shell 「新增連接」。 基本呼叫會使用 "updateData"，如下所示：

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

結果是連接屬性，這是符合下列結構的物件陣列：

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

```onCancel```藉由傳遞空的連接陣列來取消「新增連接」嘗試：

``` ts
this.appContextService.rpc.updateData(EnvironmentModule.nameOfShell, '##', <RpcUpdateData>{ results: { connections: [] } });
```

## <a name="connection-provider-example"></a>連接提供者範例

執行連接提供者的完整 TypeScript 類別如下。 請注意，"connectionType" 字串符合在資訊清單中的連線提供者中所定義的 connectionType。

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
