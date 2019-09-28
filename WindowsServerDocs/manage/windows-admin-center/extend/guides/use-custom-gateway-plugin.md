---
title: 使用工具擴充功能中的自訂閘道外掛程式
description: 開發工具擴充功能 Windows 系統管理中心 SDK （Project 檀香山）-使用您工具延伸模組中的自訂閘道外掛程式
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 829cbf6df8cc2738bf4066b36210b860595774ed
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385235"
---
# <a name="use-a-custom-gateway-plugin-in-your-tool-extension"></a>使用工具擴充功能中的自訂閘道外掛程式

>適用於：Windows Admin Center、Windows Admin Center 預覽版

在本文中，我們會將自訂閘道外掛程式用於我們使用 Windows 管理中心 CLI 所建立的全新空白工具延伸模組。

## <a name="prepare-your-environment"></a>準備您的環境 ##

如果您還沒有這麼做，請遵循[開發工具擴充](../develop-tool.md)功能中的指示來準備您的環境，並建立新的空白工具擴充功能。

## <a name="add-a-module-to-your-project"></a>將模組新增至您的專案 ##

如果您還沒有這麼做，請將新的[空白模組](add-module.md)新增至您的專案，我們將在下一個步驟中使用它。  

## <a name="add-integration-to-custom-gateway-plugin"></a>將整合新增至自訂閘道外掛程式 ##

現在，我們將在剛剛建立的新空白模組中使用自訂閘道外掛程式。

### <a name="create-pluginservicets"></a>建立外掛程式。 ts

切換至上面所建立之新工具模組的目錄（```\src\app\{!Module-Name}```），並建立新的檔案 ```plugin.service.ts```。

將下列程式碼新增至剛建立的檔案：
``` ts
import { Injectable } from '@angular/core';
import { AppContextService, HttpService } from '@microsoft/windows-admin-center-sdk/angular';
import { Cim, Http, PowerShell, PowerShellSession } from '@microsoft/windows-admin-center-sdk/core';
import { AjaxResponse, Observable } from 'rxjs';

@Injectable()
export class PluginService {
    constructor(private appContextService: AppContextService, private http: Http) {
    }
    
    public getGatewayRestResponse(): Observable<any> {
        let callUrl = this.appContextService.activeConnection.nodeName;

        return this.appContextService.node.get(callUrl, 'features/Sample%20Uno').map(
            (response: any) => {
                return response;
            }
        )
    }
}
```

視需要將參考變更為 ```Sample Uno```，並 ```Sample%20Uno``` 至您的功能名稱。

[!WARNING]
> 建議使用內建的 ```this.appContextService.node``` 來呼叫自訂閘道外掛程式中定義的任何 API。 這可確保您的閘道外掛程式內必須有認證，才會正確處理它們。

### <a name="modify-modulets"></a>修改模組。 ts

開啟稍早建立之新模組的 ```module.ts``` 檔案（亦即 ```{!Module-Name}.module.ts```）：

新增下列 import 語句：

``` ts
import { HttpService } from '@microsoft/windows-admin-center-sdk/angular';
import { Http } from '@microsoft/windows-admin-center-sdk/core';
import { PluginService } from './plugin.service';
```

新增下列提供者（在宣告之後）：

``` ts
  ,
  providers: [
    HttpService,
    PluginService,
    Http
  ]
```

### <a name="modify-componentts"></a>修改元件。 ts

開啟稍早建立之新模組的 ```component.ts``` 檔案（亦即 ```{!Module-Name}.component.ts```）：

新增下列 import 語句：

``` ts
import { ActivatedRouteSnapshot } from '@angular/router';
import { AppContextService } from '@microsoft/windows-admin-center-sdk/angular';
import { Subscription } from 'rxjs';
import { Strings } from '../../generated/strings';
import { PluginService } from './plugin.service';
```

新增下列變數：

``` ts
  private serviceSubscription: Subscription;
  private responseResult: string;
```

修改此函式，並修改/新增下列函數：

``` ts
  constructor(private appContextService: AppContextService, private plugin: PluginService) {
    //
  }

  public ngOnInit() {
    this.responseResult = 'click go to do something';
  }

  public onClick() {
    this.serviceSubscription = this.plugin.getGatewayRestResponse().subscribe(
      (response: any) => {
        this.responseResult = 'response: ' + response.message;
      },
      (error) => {
        console.log(error);
      }
    );
  }
```

### <a name="modify-componenthtml"></a>修改元件 .html ###

開啟稍早建立之新模組的 ```component.html``` 檔案（亦即 ```{!Module-Name}.component.html```）：

將下列內容新增至 html 檔案：
``` html
<button (click)="onClick()" >go</button>
{{ responseResult }}
```

## <a name="build-and-side-load-your-extension"></a>組建和側邊載入您的擴充功能

現在您已準備好在 Windows 系統管理中心內[建立和側邊載入](../develop-tool.md#build-and-side-load-your-extension)擴充功能。
