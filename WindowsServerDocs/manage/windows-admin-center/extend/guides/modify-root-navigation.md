---
title: 修改根瀏覽行為
description: 開發 Windows Admin Center SDK (Project Honolulu) 的解決方案擴充功能-修改根瀏覽行為
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 08/07/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 4a5cba228aa3a0afed99c0d853c3720a5b46f650
ms.sourcegitcommit: 546229d6b5fa7e16f725c6c35f4dcc272711b811
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/18/2018
ms.locfileid: "4905038"
---
# 修改根瀏覽行為的解決方案擴充功能

>適用於：Windows Admin Center、Windows Admin Center 預覽版

本指南中我們將會了解如何修改有不同的連線清單行為，您的方案的根瀏覽行為，以及如何隱藏或顯示 [工具] 清單。

## 修改根瀏覽行為

{延伸模組根} \src 中開啟 manifest.json 檔案並尋找屬性 」 rootNavigationBehavior 」。 這個屬性有兩個有效的值: 「 連線 」 或 「 路徑 」。 「 連線 」 行為被詳細文件中的更新版本。

### 設定路徑做為 rootNavigationBehavior

設定的值```rootNavigationBehavior```到```path```，然後刪除```requirements```屬性，並離開```path```屬性為空字串。 您已完成所需的最小設定，來建置解決方案擴充功能。 儲存檔案，並 gulp 使用組建-> gulp 使用做為您的服務會工具，然後側邊載入延伸模組至您本機的 Windows Admin Center 擴充功能。

有效資訊清單的進入點陣列看起來像這樣：
```
    "entryPoints": [
        {
          "entryPointType": "solution",
          "name": "main",
          "urlName": "testsln",
          "displayName": "resources:strings:displayName",
          "description": "resources:strings:description",
          "icon": "sme-icon:icon-win-powerShell",
          "path": "",
          "rootNavigationBehavior": "path"
        }
    ],
```

這種類型的結構與內建工具將會不必要的連線，若要載入，但可能不會有節點連線功能。

### 將連線設定為 rootNavigationBehavior

當您設定```rootNavigationBehavior```屬性```connections```，您告訴 Windows Admin Center 殼層會有連接的節點 （一律某些類型的伺服器），它應該與它連線，並確認連線狀態。 考慮到這，有 2 個步驟中驗證連線。 1) Windows Admin Center 會嘗試進行嘗試登入的節點使用您的認證 （適用於建立遠端 PowerShell 工作階段），及 2） 它將會執行 PowerShell 指令碼，您提供給識別如果節點是在可連接的狀態。

連線的有效的方案定義看起來像這樣：

``` json
        {
          "entryPointType": "solution",
          "name": "example",
          "urlName": "solutionexample",
          "displayName": "resources:strings:displayName",
          "description": "resources:strings:description",
          "icon": "sme-icon:icon-win-powerShell",
          "rootNavigationBehavior": "connections",
          "connections": {
            "header": "resources:strings:connectionsListHeader",
            "connectionTypes": [
                "msft.sme.connection-type.example"
                ]
            },
            "tools": {
                "enabled": false,
                "defaultTool": "solution"
            }
        },
```

RootNavigationBehavior 設為 「 連線 」 時，才能建置出連線定義在資訊清單中。 這包括 （將會用來顯示您的方案標頭中，當使用者選取它從功能表） 的 「 標頭 」 屬性、 connectionTypes 陣列 （這會指定哪些 connectionTypes 使用方案中。 更多有關該消費者文件中。）。

## 啟用和停用 [工具 \] 功能表 ##

在方案定義中使用的另一個屬性是 「 工具 」 屬性。 這將會聽寫是否顯示 [工具] 功能表，以及將會載入工具。 啟用時，Windows Admin Center 會轉譯左側工具 \] 功能表。 使用 defaultTool，就需要在資訊清單來新增工具進入點，以便載入適當的資源。 「 DefaultTool 」 的值必須是"name"屬性，此工具，它是在資訊清單中定義。
