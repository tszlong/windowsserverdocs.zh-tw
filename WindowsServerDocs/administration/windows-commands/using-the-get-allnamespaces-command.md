---
title: 使用 AllNamespaces 命令
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e8fe896d-a69a-4180-923b-9f18185f5941
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0cd90fc650271c863459dd809e47ca6309132de5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363294"
---
# <a name="using-the-get-allnamespaces-command"></a>使用 AllNamespaces 命令

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

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
## <a name="parameters"></a>參數

|         參數         |                                                                               Windows Server 2008                                                                               | Windows Server 2008 R2 |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
|  [/Server： <Server name>]  | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。 |                        |
| [/ContentProvider： <name>] |                                                        僅顯示指定之內容提供者的命名空間。                                                         |                        |
|      [/Show：用戶端]      |                            僅支援 Windows Server 2008。 顯示連接到命名空間之用戶端電腦的相關資訊。                             |                        |
|    [/details：用戶端]     |                           僅支援 Windows Server 2008 R2。 顯示連接到命名空間之用戶端電腦的相關資訊。                           |                        |
|  [/ExcludedeletePending]  |                                                              排除清單中任何已停用的傳輸。                                                              |                        |

## <a name="BKMK_examples"></a>典型
若要查看所有命名空間，請輸入：
```
wdsutil /Get-AllNamespaces
```
若要查看已停用的所有命名空間，請輸入：
- Windows Server 2008
  ```
  wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:"MyContentProv" /Show:Clients /ExcludedeletePending
  ```
- Windows Server 2008 R2
  ```
  wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:"MyContentProv" /details:Clients /ExcludedeletePending
  ```
  #### <a name="additional-references"></a>其他參考
  [命令列語法索引鍵](command-line-syntax-key.md)
  [使用新的命名空間命令](using-the-new-namespace-command.md)
  [使用 remove-Namespace 命令](using-the-remove-namespace-command.md)
  [子命令： start-namespace](subcommand-start-namespace.md)
