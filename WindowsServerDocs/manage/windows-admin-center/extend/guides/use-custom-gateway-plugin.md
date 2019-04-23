---
title: 使用工具擴充功能中的自訂閘道外掛程式
description: 開發 Windows Admin Center SDK （專案檀香山） [工具] 延伸模組-使用自訂閘道外掛程式在您的工具延伸模組
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: c9b2e9201d58472286b42a9c89a36423f40d143d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834509"
---
# <a name="use-a-custom-gateway-plugin-in-your-tool-extension"></a>使用工具擴充功能中的自訂閘道外掛程式

>適用於：Windows Admin Center，Windows Admin Center 預覽

在本文中，我們將使用新的空白工具擴充功能，我們建立了使用 Windows Admin Center CLI 中自訂閘道外掛程式。

## <a name="prepare-your-environment"></a>準備您的環境 ##

如果您還沒有這麼做，請依照下列中的指示[開發工具 延伸模組](..\develop-tool.md)若要準備您的環境，並建立新的清空 [工具] 延伸模組。

## <a name="add-a-module-to-your-project"></a>將模組新增至您的專案 ##

如果您還沒有這麼做，新增[空的模組](add-module.md)至您的專案，我們將在下一個步驟中使用。  

## <a name="add-integration-to-custom-gateway-plugin"></a>加入自訂閘道外掛程式的整合 ##

現在，我們將我們剛才建立之新的空白模組中使用自訂閘道外掛程式。

### <a name="create-pluginservicets"></a>建立 plugin.service.ts

將上面建立的新工具模組的目錄 (```\src\app\{!Module-Name}```)，並建立新的檔案```plugin.service.ts```。

下列程式碼加入剛才建立的檔案：
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

變更參考```Sample Uno```和```Sample%20Uno```您視需要的功能名稱。

### <a name="modify-modulets"></a>修改 module.ts

開啟```module.ts```稍早建立的新模組的檔案 (也就是```{!Module-Name}.module.ts```):

新增下列 import 陳述式：

``` ts
import { HttpService } from '@microsoft/windows-admin-center-sdk/angular';
import { Http } from '@microsoft/windows-admin-center-sdk/core';
import { PluginService } from './plugin.service';
```

（之後宣告） 中新增下列提供者：

``` ts
  ,
  providers: [
    HttpService,
    PluginService,
    Http
  ]
```

### <a name="modify-componentts"></a>修改 component.ts

開啟```component.ts```稍早建立的新模組的檔案 (也就是```{!Module-Name}.component.ts```):

新增下列 import 陳述式：

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

修改建構函式，並修改/新增下列函式：

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

### <a name="modify-componenthtml"></a>修改 component.html ###

開啟```component.html```稍早建立的新模組的檔案 (也就是```{!Module-Name}.component.html```):

Html 檔案中加入下列內容：
``` html
<button (click)="onClick()" >go</button>
{{ responseResult }}
```

## <a name="build-and-side-load-your-extension"></a>組建和側邊載入您的延伸模組

現在您已準備好[建置，並端負載](..\develop-tool.md#build-and-side-load-your-extension)您 Windows Admin Center 的延伸模組。
