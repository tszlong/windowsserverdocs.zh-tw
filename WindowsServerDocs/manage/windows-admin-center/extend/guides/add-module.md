---
title: 新增模組至工具擴充功能
description: 開發工具擴充功能的 Windows Admin Center SDK （專案檀香山）-將模組新增至 [工具] 延伸模組
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: e6978ce20a7c6da8addb217de8d30f733b40d261
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834399"
---
# <a name="add-a-module-to-a-tool-extension"></a>新增模組至工具擴充功能

>適用於：Windows Admin Center，Windows Admin Center 預覽

在本文中，我們會將空的模組，加入我們建立了使用 Windows Admin Center CLI 工具擴充功能。

## <a name="prepare-your-environment"></a>準備您的環境

如果您還沒有這麼做，請依照下列指示中開發[工具](..\develop-tool.md)(或[解決方案](..\develop-solution.md)) 來準備您的環境及建立新的空白工具延伸模組的延伸模組。

## <a name="use-the-angular-cli-to-create-a-module-and-component"></a>使用 Angular CLI 建立的模組 （和元件）

如果對 Angular 不熟悉，強烈建議您閱讀 Angular.Io 網站上的文件以了解 Angular 和 NgModule。 如需 NgModule 的詳細資訊，請移至這裡： https://angular.io/guide/ngmodule

* 在 Angular CLI 中產生新模組的詳細資訊： https://github.com/angular/angular-cli/wiki/generate-module
* 在 Angular CLI 中產生新元件的詳細資訊： https://github.com/angular/angular-cli/wiki/generate-component


開啟命令提示字元，將目錄變更為 \src\app 在您的專案，然後執行下列命令，將```{!ModuleName}```以您的模組名稱 （移除空格）：

```
cd \src\app
ng generate module {!ModuleName}
ng generate component {!ModuleName}
```

| 值 | 說明 | 範例 |
| ----- | ----------- | ------- |
| ```{!ModuleName}``` | 您的模組名稱 (移除空格) | ```ManageFooWorksPortal``` |

範例用法：
```
cd \src\app
ng generate module ManageFooWorksPortal
ng generate component ManageFooWorksPortal
```


## <a name="add-routing-information"></a>新增路由資訊

如果不熟悉 Angular，強烈建議您了解一下 Angular 路由與巡覽。 以下章節定義必要的路由元素，這些元素可讓 Windows Admin Center 巡覽至您的擴充功能，並且在擴充功能的檢視之間巡覽，以回應使用者活動。 若要深入了解，請移至這裡： https://angular.io/guide/router

使用您在上述步驟中使用的相同模組名稱。

### <a name="add-content-to-new-routing-file"></a>將內容新增至新的路由檔案

* 瀏覽至 ``` ng generate ``` 於上一個步驟建立的模組資料夾。

* 遵循下列命名慣例建立新的檔案 ```{!module-name}.routing.ts```：

    | 值 | 說明 | 範例檔案名稱 |
    | ----- | ----------- | ------- |
    | ```{!module-name}``` | 您的模組名稱 (小寫，空格以破折號取代) | ```manage-foo-works-portal.routing.ts``` |

* 將此內容新增至剛才建立的檔案：

    ``` ts
    import { NgModule } from '@angular/core';
    import { RouterModule, Routes } from '@angular/router';
    import { {!ModuleName}Component } from './{!module-name}.component';

    const routes: Routes = [
        {
            path: '',
            component: {!ModuleName}Component,
            // if the component has child components that need to be routed to, include them in the children array.
            children: [
                {
                    path: '', 
                    redirectTo: 'base',
                    pathMatch: 'full'
                }
            ]
    }];

    @NgModule({
        imports: [
            RouterModule.forChild(routes)
        ],
        exports: [
            RouterModule
        ]
    })
    export class Routing { }
    ```

* 將剛建立的檔案中的值取代為您想要的值：

    | 值 | 說明 | 範例 |
    | ----- | ----------- | ------- |
    | ```{!ModuleName}``` | 您的模組名稱 (移除空格) | ```ManageFooWorksPortal``` |
    | ```{!module-name}``` | 您的模組名稱 (小寫，空格以破折號取代) | ```manage-foo-works-portal``` |

### <a name="add-content-to-new-module-file"></a>將內容新增至新的模組檔案

開啟使用下列命名慣例找到的檔案 ```{!module-name}.module.ts```：

| 值 | 說明 | 範例檔案名稱 |
| ----- | ----------- | ------- |
| ```{!module-name}``` | 您的模組名稱 (小寫，空格以破折號取代) | ```manage-foo-works-portal.module.ts``` |

* 將內容新增至檔案：

    ``` ts
    import { Routing } from './{!module-name}.routing';
    ```

* 將剛新增的內容中的值取代為您想要的值：

    | 值 | 說明 | 範例 |
    | ----- | ----------- | ------- |
    | ```{!module-name}``` | 您的模組名稱 (小寫，空格以破折號取代) | ```manage-foo-works-portal``` |

* 修改匯入陳述式以匯入路由：

    | 原始值 | 新的值 |
    | -------------- | --------- |
    | ```imports: [ CommonModule ]``` | ```imports: [ CommonModule, Routing ]``` |

* 確定 ```import``` 陳述式是按照來源依字母順序排列。

### <a name="add-content-to-new-component-typescript-file"></a>將內容新增至新的元件 typescript 檔案

開啟使用下列命名慣例找到的檔案 ```{!module-name}.component.ts```：

| 值 | 說明 | 範例檔案名稱 |
| ----- | ----------- | ------- |
| ```{!module-name}``` | 您的模組名稱 (小寫，空格以破折號取代) | ```manage-foo-works-portal.component.ts``` |
    
將檔案中的內容修改如下：

``` ts
constructor() {
    // TODO
}

public ngOnInit() {
    // TODO
}
```
### <a name="update-app-routingmodulets"></a>更新應用程式 routing.module.ts

開啟檔案```app-routing.module.ts```，修改預設路徑，以便它會載入您剛才建立的新模組。  找到的項目```path: ''```，並更新```loadChildren```載入您的模組，而不是預設的模組：

| 值 | 說明 | 範例 |
| ----- | ----------- | ------- |
| ```{!ModuleName}``` | 您的模組名稱 (移除空格) | ```ManageFooWorksPortal``` |
| ```{!module-name}``` | 您的模組名稱 (小寫，空格以破折號取代) | ```manage-foo-works-portal``` |

``` ts
    {
        path: '', 
        loadChildren: 'app/{!module-name}/{!module-name}.module#{!ModuleName}Module'
    },
```
以下是更新的預設路徑的範例：
``` ts
    {
        path: '', 
        loadChildren: 'app/manage-foo-works-portal/manage-foo-works-portal.module#ManageFooWorksPortalModule'
    },
```


## <a name="build-and-side-load-your-extension"></a>組建和側邊載入您的延伸模組

現在您已將模組加入您的延伸模組。  接下來，您可以[建置，並端負載](..\develop-tool.md#build-and-side-load-your-extension)您 Windows Admin Center，來查看結果的延伸模組。
