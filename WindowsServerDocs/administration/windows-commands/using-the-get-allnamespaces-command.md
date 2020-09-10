---
title: AllNamespaces
description: AllNamespaces 的參考文章，會顯示伺服器上所有命名空間的相關資訊。
ms.topic: reference
ms.assetid: e8fe896d-a69a-4180-923b-9f18185f5941
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b87b1fb3b2a7a1a7bb21f9c5a6a389494532dde5
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626375"
---
# <a name="get-allnamespaces"></a>AllNamespaces

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示伺服器上所有命名空間的相關資訊。

## <a name="syntax"></a>語法
Windows Server 2008：
```
wdsutil /Get-AllNamespaces [/Server:<Server name>] [/ContentProvider:<name>] [/Show:Clients] [/ExcludedeletePending]
```
Windows Server 2008 R2：
```
wdsutil /Get-AllNamespaces [/Server:<Server name>] [/ContentProvider:<name>] [/details:Clients] [/ExcludedeletePending]
```
### <a name="parameters"></a>參數

|         參數         |                                                                               Windows Server 2008                                                                               | Windows Server 2008 R2 |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
|  [/Server： <Server name> ]  | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。 |                        |
| [/ContentProvider： <name> ] |                                                        只顯示指定之內容提供者的命名空間。                                                         |                        |
|      [/Show：用戶端]      |                            僅支援 Windows Server 2008。 顯示連接到命名空間之用戶端電腦的相關資訊。                             |                        |
|    [/details：用戶端]     |                           僅支援 Windows Server 2008 R2。 顯示連接到命名空間之用戶端電腦的相關資訊。                           |                        |
|  [/ExcludedeletePending]  |                                                              從清單中排除任何停用的傳輸。                                                              |                        |

## <a name="examples"></a>範例
若要查看所有命名空間，請輸入：
```
wdsutil /Get-AllNamespaces
```
若要查看除了停用的命名空間以外的所有命名空間，請輸入：
- Windows Server 2008
  ```
  wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:MyContentProv /Show:Clients /ExcludedeletePending
  ```
- Windows Server 2008 R2
  ```
  wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:MyContentProv /details:Clients /ExcludedeletePending
  ```
  ## <a name="additional-references"></a>其他參考資料
  - [命令列語法索引鍵](command-line-syntax-key.md) 
  [使用新的-Namespace 命令](using-the-new-namespace-command.md) 
  [使用 Remove 命名空間命令](using-the-remove-namespace-command.md) 
  [子命令： start-Namespace](subcommand-start-namespace.md)
