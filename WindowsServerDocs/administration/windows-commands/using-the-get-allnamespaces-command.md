---
title: AllNamespaces
description: AllNamespaces 的參考文章，它會顯示伺服器上所有命名空間的相關資訊。
ms.topic: article
ms.assetid: e8fe896d-a69a-4180-923b-9f18185f5941
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 470aab904f9404b8dbe99409445b0533fa83fedd
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896396"
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
|  [/Server： <Server name> ]  | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或 (FQDN) 的完整功能變數名稱。 如果未指定伺服器名稱，則會使用本機伺服器。 |                        |
| [/ContentProvider： <name> ] |                                                        僅顯示指定之內容提供者的命名空間。                                                         |                        |
|      [/Show：用戶端]      |                            僅支援 Windows Server 2008。 顯示連接到命名空間之用戶端電腦的相關資訊。                             |                        |
|    [/details：用戶端]     |                           僅支援 Windows Server 2008 R2。 顯示連接到命名空間之用戶端電腦的相關資訊。                           |                        |
|  [/ExcludedeletePending]  |                                                              排除清單中任何已停用的傳輸。                                                              |                        |

## <a name="examples"></a>範例
若要查看所有命名空間，請輸入：
```
wdsutil /Get-AllNamespaces
```
若要查看已停用的所有命名空間，請輸入：
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
  [使用 remove-Namespace 命令](using-the-remove-namespace-command.md) 
  [子命令： start-Namespace](subcommand-start-namespace.md)
