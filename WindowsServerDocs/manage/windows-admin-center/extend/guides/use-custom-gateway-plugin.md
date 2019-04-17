---
title: 使用工具擴充功能中的自訂閘道外掛程式
description: 開發工具擴充功能 Windows Admin Center SDK (Project Honolulu)-使用工具擴充功能中的自訂閘道外掛程式
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 4652616478b7b05bde97db48bf84648984b5a325
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296760"
---
# 使用工具擴充功能中的自訂閘道外掛程式

>適用於：Windows Admin Center、Windows Admin Center 預覽版

在本文中，我們會使用自訂閘道外掛程式，我們已使用 Windows Admin Center CLI 建立新的空白工具擴充功能中。

## 準備您的環境 ##

如果您還沒有這樣做，請依照[開發工具擴充功能](..\develop-tool.md)來準備您的環境，並建立新的空白工具擴充功能中的指示。

## 新增模組至您的專案 ##

如果您還沒有這樣做，將新的[空白模組](add-module.md)新增到您的專案，我們將在下一個步驟中使用。  

## 將整合新增到自訂閘道外掛程式 ##

現在，我們會在我們剛建立的新的空白模組中使用自訂閘道外掛程式。

### 建立 plugin.service.ts

變更至新的工具模組上面所建立的目錄 (```\src\app\{!Module-Name}```)，並建立新的檔案```plugin.service.ts```。

將下列程式碼新增至剛才建立的檔案：
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

將參考來變更為```Sample Uno```和```Sample%20Uno```為您做為適當的功能名稱。

[!WARNING]
> 它是建議的內建中的```this.appContextService.node```用於呼叫您的自訂閘道外掛程式中定義的任何 API。 如果認證是必要項目內，它們將會正確地處理您閘道外掛程式，這可確保做。

### 修改 module.ts

開啟```module.ts```稍早建立的新模組的檔案 (亦即```{!Module-Name}.module.ts```):

新增下列匯入陳述式：

``` ts
import { HttpService } from '@microsoft/windows-admin-center-sdk/angular';
import { Http } from '@microsoft/windows-admin-center-sdk/core';
import { PluginService } from './plugin.service';
```

新增下列的提供者 （之後宣告）：

``` ts
  ,
  providers: [
    HttpService,
    PluginService,
    Http
  ]
```

### 修改 component.ts

開啟```component.ts```稍早建立的新模組的檔案 (亦即```{!Module-Name}.component.ts```):

新增下列匯入陳述式：

``` ts
import { ActivatedRouteSnapshot } from '@angular/router';
import { AppContextService } from '@microsoft/windows-admin-center-sdk/angular';
import { Subscription } from 'rxjs';
import { Strings } from '../../generated/strings';
import { PluginService } from './plugin.service';
```

將下列變數新增：

``` ts
  private serviceSubscription: Subscription;
  private responseResult: string;
```

修改建構函式，並修改/新增下列功能：

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

### 修改 component.html ###

開啟```component.html```稍早建立的新模組的檔案 (亦即```{!Module-Name}.component.html```):

以 html 檔案中新增下列內容：
``` html
<button (click)="onClick()" >go</button>
{{ responseResult }}
```

## 建置並側載您的擴充功能

現在您已經準備好[建置並側載入](..\develop-tool.md#build-and-side-load-your-extension)您 Windows Admin Center 中的擴充功能。
