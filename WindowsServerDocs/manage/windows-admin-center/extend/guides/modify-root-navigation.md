---
title: 修改根瀏覽行為
description: 開發解決方案的擴充功能 Windows Admin Center SDK （專案檀香山）-修改根導覽行為
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 08/07/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 4a5cba228aa3a0afed99c0d853c3720a5b46f650
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861729"
---
# <a name="modify-root-navigation-behavior-for-a-solution-extension"></a>修改解決方案延伸模組的根導覽行為

>適用於：Windows Admin Center，Windows Admin Center 預覽

本指南中我們將了解如何修改根導覽行為，您的解決方案會有不同的連接清單中的行為，以及如何隱藏或顯示於工具清單。

## <a name="modifying-root-navigation-behavior"></a>修改根導覽行為

開啟 manifest.json 檔案中 {擴充功能根目錄} \src 並尋找 「 rootNavigationBehavior"的屬性。 這個屬性有兩個有效的值: 「 連線 」 或 「 路徑 」。 「 連線 」 行為會文件中稍後詳細說明。

### <a name="setting-path-as-a-rootnavigationbehavior"></a>設定路徑為 rootNavigationBehavior

設定的值```rootNavigationBehavior```要```path```，然後再刪除```requirements```屬性，並保留```path```屬性為空字串。 您已完成所需的最低設定，建置解決方案的擴充功能。 儲存檔案，並 gulp 組建]-> [gulp 做為您想工具，以及然後端擴充功能載入至您本機的 Windows Admin Center 延伸模組。

有效的資訊清單進入點陣列看起來像這樣：
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

以這種結構的工具將不必要的連線，若要載入，但不是具有節點連線 」 功能。

### <a name="setting-connections-as-a-rootnavigationbehavior"></a>將連線設定為 rootNavigationBehavior

當您設定```rootNavigationBehavior```屬性設```connections```，會告訴 Windows Admin Center 殼層會有它應該連接的連線的節點 （一律某些類型的伺服器），並確認連線狀態。 以此方式，有 2 個步驟中驗證連接。 1) Windows Admin Center 會嘗試將會嘗試登入的節點，使用您的認證 （適用於建立遠端 PowerShell 工作階段），以及 2） 它將會執行 PowerShell 指令碼，您提供給識別節點是否處於可連接的狀態。

連接的有效的方案定義看起來像這樣：

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

RootNavigationBehavior 設為 [連線] 時，您必須建置資訊清單中的連線定義。 這包括 「 標頭 」 屬性 （會用來顯示您方案的標頭中，當使用者從功能表選取它）、 connectionTypes 陣列 （這會指定哪一個 connectionTypes 解決方案中使用。 更多消費者文件中。）。

## <a name="enabling-and-disabling-the-tools-menu"></a>啟用和停用 [工具] 功能表 ##

可用的方案定義中的另一個屬性是 [工具] 屬性。 這會指定是否顯示 [工具] 功能表，以及將會載入此工具。 啟用時，Windows Admin Center 會呈現左側的 [工具] 功能表。 使用 defaultTool，就需要資訊清單新增工具進入點，以載入適當的資源。 「 DefaultTool"的值必須是工具的"name"屬性，因為它定義資訊清單中。
