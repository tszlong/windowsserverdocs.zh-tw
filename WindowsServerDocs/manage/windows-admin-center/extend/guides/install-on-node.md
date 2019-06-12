---
title: 開發工具擴充功能
description: 開發 Windows Admin Center SDK （專案檀香山） [工具] 延伸模組
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 1a068c0d33887e8e9287ff15c1aa14f3dc84915a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445928"
---
# <a name="install-extension-payload-on-a-managed-node"></a>受管理的節點上安裝擴充功能內容

>適用於：Windows Admin Center，Windows Admin Center 預覽

## <a name="setup"></a>安裝程式
> [!NOTE]
> 若要遵循本指南，您將需要建置 1.2.1904.02001 或更高版本。 若要檢查您的組建編號會開啟 Windows Admin Center，然後按一下右上角的問號。

如果您還沒有這麼做，建立[工具延伸模組](../develop-tool.md)的 Windows Admin Center。 在您完成此讓後下建立延伸模組時所使用的值：

| 值 | 說明 | 範例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 您的公司名稱 （含空格） | ```Contoso``` |
| ```{!Tool Name}``` | 您的工具名稱 （含空格） | ```InstallOnNode``` |

在您的工具延伸模組資料夾內建立```Node```資料夾 (```{!Tool Name}\Node```)。 放在此資料夾的任何項目將複製到受管理的節點，使用此 API 時。 加入您的使用案例所需的任何檔案。 

也會建立```{!Tool Name}\Node\installNode.ps1```指令碼。 將受管理的節點上執行此指令碼之後的所有檔案都會從都複製,```{!Tool Name}\Node```資料夾，以受管理的節點。 加入您的使用案例的任何其他邏輯。 範例```{!Tool Name}\Node\installNode.ps1```檔案：

``` ps1
# Add logic for installing payload on managed node
echo 'Success'
```

> [!NOTE]
> ```{!Tool Name}\Node\installNode.ps1``` 具有特定名稱，將會尋找 API。 變更這個檔案的名稱會造成錯誤。


## <a name="integration-with-ui"></a>使用 UI 的整合

更新```\src\app\default.component.ts```如下：

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
建立擴充功能時所使用的值來更新預留位置：
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
最後```\src\app\default.module.ts```:
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

## <a name="creating-and-installing-a-nuget-package"></a>建立及安裝 NuGet 套件

最後一個步驟是使用我們新增了檔案建置 NuGet 套件，然後安裝該套件，在 Windows Admin Center

請遵循[發佈的延伸模組](../publish-extensions.md)引導如果您尚未建立延伸模組套件之前。 
> [!IMPORTANT]
> 此延伸模組的.nuspec 檔案，是很重要，```<id>```值符合您的專案中的名稱```manifest.json```而```<version>```符合項目加入至```\src\app\default.component.ts```。 也加入一個項目```<files>```: 
> 
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

一旦建立此套件，新增該摘要的路徑。 在 Windows Admin Center 中前往 設定 > 擴充功能 > 摘要並將該套件所在的路徑。 當完成您的延伸模組安裝，您應該能夠按一下```install```按鈕和 API 將會呼叫。  