---
title: 開發工具擴充功能
description: 開發工具擴充功能 Windows 系統管理中心 SDK （Project 檀香山）
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: c5c87be882a32958946198eb6ff1b9d7000577e7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385286"
---
# <a name="install-extension-payload-on-a-managed-node"></a>在受管理的節點上安裝延伸模組承載

>適用於：Windows Admin Center、Windows Admin Center 預覽版

## <a name="setup"></a>安裝程式
> [!NOTE]
> 若要遵循本指南，您將需要組建1.2.1904.02001 或更高版本。 若要檢查組建編號，請開啟 [Windows 管理中心]，然後按一下右上方的問號。

如果您還沒有這麼做，請建立適用于 Windows 系統管理中心的[工具擴充](../develop-tool.md)功能。 完成之後，請記下建立擴充功能時所使用的值：

| 值 | 說明 | 範例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 您的公司名稱（含空格） | ```Contoso``` |
| ```{!Tool Name}``` | 您的工具名稱（含空格） | ```InstallOnNode``` |

在您的工具擴充功能資料夾內，建立 ```Node``` 資料夾（```{!Tool Name}\Node```）。 使用此 API 時，會將放在此資料夾中的任何內容複寫到受管理的節點。 新增您的使用案例所需的任何檔案。 

另請建立 ```{!Tool Name}\Node\installNode.ps1``` 腳本。 將所有檔案從 ```{!Tool Name}\Node``` 資料夾複製到受管理的節點之後，就會在受管理的節點上執行此腳本。 為您的使用案例新增任何其他邏輯。 範例 ```{!Tool Name}\Node\installNode.ps1``` 檔案：

``` ps1
# Add logic for installing payload on managed node
echo 'Success'
```

> [!NOTE]
> ```{!Tool Name}\Node\installNode.ps1``` 具有 API 將尋找的特定名稱。 變更此檔案的名稱將會導致錯誤。


## <a name="integration-with-ui"></a>與 UI 整合

更新 ```\src\app\default.component.ts```，如下所示：

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
將預留位置更新為建立擴充功能時所使用的值：
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

同時 ```\src\app\default.component.html``` 更新為：
``` html
<button (click)="installOnNode()">Click to install</button>
<sme-loading-wheel *ngIf="loading" size="large"></sme-loading-wheel>
<p *ngIf="response">{{response}}</p>
```
最後 ```\src\app\default.module.ts```：
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

## <a name="creating-and-installing-a-nuget-package"></a>建立和安裝 NuGet 套件

最後一個步驟是使用我們已新增的檔案來建立 NuGet 套件，然後在 Windows 系統管理中心安裝該套件。

如果您之前尚未建立延伸模組套件，請遵循[發行延伸](../publish-extensions.md)模組指南。 
> [!IMPORTANT]
> 在此延伸模組的 nuspec 檔案中，請務必讓 ```<id>``` 值符合專案的 ```manifest.json``` 中的名稱，而 ```<version>``` 則符合新增至 ```\src\app\default.component.ts``` 的內容。 此外，在 [```<files>```] 下新增專案： 
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

建立此套件之後，請新增該摘要的路徑。 在 [Windows 管理中心] 中，移至 [設定] > [延伸模組 > 摘要]，並將路徑新增至該套件所在的位置。 當您的擴充功能安裝完成時，您應該可以按一下 [```install```] 按鈕，就會呼叫 API。  