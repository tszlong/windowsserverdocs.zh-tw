---
title: 開發工具擴充功能
description: 開發工具擴充功能 Windows Admin Center SDK (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 2269a2ac2cabda6f8fdd829994f36e89d581bd23
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296928"
---
# 在受管理的節點上安裝延伸模組承載

>適用於：Windows Admin Center、Windows Admin Center 預覽版

## 設定
> [!NOTE]
> 若要遵循本指南，您將需要建置 1.2.1904.02001 或更高版本。 若要檢查您的組建編號開啟 Windows Admin Center，然後按一下右上方問號。

如果您還沒有這樣做，會建立 Windows Admin center[工具擴充功能](../develop-tool.md)。 當您完成這個讓注意建立擴充功能時使用的值：
| 值 | 說明 | 範例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 您的公司名稱 （以空格） | ```Contoso``` |
| ```{!Tool Name}``` | 您的工具名稱 （以空格） | ```InstallOnNode``` |

在您的工具擴充功能資料夾內，建立```Node```資料夾 (```{!Tool Name}\Node```)。 放在此資料夾中的任何項目將會複製到受管理的節點，使用此 API 時。 新增您的使用案例所需的任何檔案。 

也建立```{!Tool Name}\Node\installNode.ps1```指令碼。 一旦所有檔案都複製從，將會在受管理的節點上執行這個指令碼```{!Tool Name}\Node```資料夾，以受管理的節點。 新增您的使用案例的任何額外的邏輯。 範例```{!Tool Name}\Node\installNode.ps1```檔案：

``` ps1
# Add logic for installing payload on managed node
echo 'Success'
```

> [!NOTE]
> ```{!Tool Name}\Node\installNode.ps1``` 有 API 會尋找具有特定名稱。 變更這個檔案的名稱會導致錯誤。


## 與 UI 的整合

更新```\src\app\default.component.ts```給下列項目：

``` ts
import { Component } from '@angular/core';
import { AppContextService } from '@microsoft/windows-admin-center-sdk/angular';
import { Observable } from 'rxjs';

@Component({
  selector: 'default-component',
  templateUrl: './default.component.html',
  styleUrls: ['./default.component.css']
})

export class DefaultComponent {
  constructor(private appContextService: AppContextService) { }

  public response: any;
  public loading = false;

  public installOnNode() {
    this.loading = true;
    this.post('{!Company Name}.{!Tool Name}', '1.0.0',
      this.appContextService.activeConnection.nodeName).subscribe(
        (response: any) => {
          console.log(response);
          this.response = response;
          this.loading = false;
        },
        (error) => {
          console.log(error);
          this.response = error;
          this.loading = false;
        }
      );
  }

  public post(id: string, version: string, targetNode: string): Observable<any> {
    return this.appContextService.node.post(targetNode,
      `features/extensions/${id}/versions/${version}/install`);
  }

}
```
更新預留位置來建立延伸模組時，所使用的值：
``` ts
this.post('contoso.install-on-node', '1.0.0',
      this.appContextService.activeConnection.nodeName).subscribe(
        (response: any) => {
          console.log(response);
          this.response = response;
          this.loading = false;
        },
        (error) => {
          console.log(error);
          this.response = error;
          this.loading = false;
        }
      );
```

也更新```\src\app\default.component.html```來：
``` html
<button (click)="installOnNode()">Click to install</button>
<sme-loading-wheel *ngIf="loading" size="large"></sme-loading-wheel>
<p *ngIf="response">{{response}}</p>
```
和最後```\src\app\default.module.ts```:
``` ts
import { CommonModule } from '@angular/common';
import { NgModule } from '@angular/core';

import { LoadingWheelModule } from '@microsoft/windows-admin-center-sdk/angular';
import { DefaultComponent } from './default.component';
import { Routing } from './default.routing';

@NgModule({
  imports: [
    CommonModule,
    LoadingWheelModule,
    Routing
  ],
  declarations: [DefaultComponent]
})
export class DefaultModule { }

```

## 建立及安裝 NuGet 套件

最後一個步驟是建置我們已新增之檔案的 NuGet 套件，然後 Windows Admin Center 中安裝該套件。

如果您沒有建立延伸模組套件之前，請依照本指南[發佈擴充功能](../publish-extensions.md)。 
> [!IMPORTANT]
> 在您.nuspec 檔案以進行此延伸模組，是很重要的```<id>```值符合您的專案中的名稱```manifest.json```和```<version>```相符項目已新增至```\src\app\default.component.ts```。 也將在下的一個項目新增```<files>```: 

> ```<file src="Node\**\*.*" target="Node" />```.

``` xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2011/08/nuspec.xsd">
  <metadata>
    <id>contoso.install-on-node</id>
    <version>1.0.0</version>
    <authors>Contoso</authors>
    <owners>Contoso</owners>
    <requireLicenseAcceptance>false</requireLicenseAcceptance>
    <projectUrl>https://msft-sme.myget.org/feed/windows-admin-center-feed/package/nuget/contoso.sme.install-on-node-extension</projectUrl>
    <licenseUrl>http://YourLicenseLink</licenseUrl>
    <iconUrl>http://YourLogoLink</iconUrl>
    <description>Install on node extension by Contoso</description>
    <copyright>(c) Contoso. All rights reserved.</copyright> 
  </metadata>
    <files>
    <file src="bundle\**\*.*" target="ux" />
    <file src="package\**\*.*" target="gateway" />
    <file src="Node\**\*.*" target="Node" />
  </files>
</package>
```

一旦建立此套件時，請將路徑新增至該摘要。 Windows Admin Center 中移至設定 > 延伸模組 > 同步發佈摘要，並將路徑新增至該套件存在於的位置。 當您的擴充功能完成安裝，您應該可以按一下```install```按鈕和 API，就會呼叫。  