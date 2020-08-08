---
title: 修改根瀏覽行為
description: 開發解決方案延伸模組 Windows 系統管理中心 SDK (Project 檀香山) -修改根導覽行為
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 08/07/2018
ms.localizationpriority: medium
ms.openlocfilehash: bc7ef3c25c41abd6c7b91e37cecca017664ee223
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87944939"
---
# <a name="modify-root-navigation-behavior-for-a-solution-extension"></a>修改解決方案延伸模組的根導覽行為

>適用於：Windows Admin Center、Windows Admin Center 預覽版

在本指南中，我們將學習如何修改解決方案的根導覽行為，使其具有不同的連接清單行為，以及如何隱藏或顯示工具清單。

## <a name="modifying-root-navigation-behavior"></a>修改根導覽行為

開啟 {extension root} \src 中檔案的 manifest.js，並尋找 "rootNavigationBehavior" 屬性。 此屬性有兩個有效的值： "connections" 或 "path"。 本檔稍後會詳述「連接」行為。

### <a name="setting-path-as-a-rootnavigationbehavior"></a>將路徑設定為 rootNavigationBehavior

將的值設定 ```rootNavigationBehavior``` 為 ```path``` ，然後刪除 ```requirements``` 屬性，並將 ```path``` 屬性保留為空字串。 您已完成最基本的必要設定，以建立解決方案延伸模組。 儲存檔案，gulp 組建 > gulp 會如同您的工具般處理，然後將擴充功能載入本機的 Windows 管理中心延伸模組。

有效的資訊清單 e 陣列看起來像這樣：
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

以這種結構建立的工具不需要連接載入，但也不會有節點連接功能。

### <a name="setting-connections-as-a-rootnavigationbehavior"></a>將連接設定為 rootNavigationBehavior

當您將 ```rootNavigationBehavior``` 屬性設定為時 ```connections``` ，您會告訴 Windows Admin Center Shell，將會有一個連接的節點 (一定要連接到某種) 類型的伺服器，並確認連接狀態。 使用此功能時，驗證連接有2個步驟。 1) Windows 管理中心會嘗試使用您的認證登入節點， (建立遠端 PowerShell 會話) ，以及 2) 它會執行您提供的 PowerShell 腳本，以識別節點是否為可線上狀態。

具有連接的有效解決方案定義如下所示：

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

當 rootNavigationBehavior 設定為「連接」時，您必須在資訊清單中建立連接定義。 這包括「標頭」屬性 (當使用者從功能表中選取它時，就會用來顯示在您的方案標頭中) connectionTypes 陣列 (這會指定要在解決方案中使用的 connectionTypes。 ConnectionProvider 檔中的詳細資訊。 ) 。

## <a name="enabling-and-disabling-the-tools-menu"></a>啟用和停用 [工具] 功能表 ##

解決方案定義中提供的另一個屬性是「工具」屬性。 這會指示是否顯示 [工具] 功能表，以及要載入的工具。 啟用時，Windows 管理中心會轉譯左側 [工具] 功能表。 使用 defaultTool 時，您必須將工具進入點新增至資訊清單，才能載入適當的資源。 在資訊清單中定義時，"defaultTool" 的值必須是工具的「名稱」屬性。
