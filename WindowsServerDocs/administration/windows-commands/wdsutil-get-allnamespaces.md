---
title: wdsutil get-allnamespaces
description: Wdsutil allnamespaces 的參考文章，會顯示伺服器上所有命名空間的相關資訊。
ms.topic: reference
ms.assetid: e8fe896d-a69a-4180-923b-9f18185f5941
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ddad534e46c4ca30311ad497a9fa66d60a4cda60
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729911"
---
# <a name="wdsutil-get-allnamespaces"></a>wdsutil get-allnamespaces

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示伺服器上所有命名空間的相關資訊。

## <a name="syntax"></a>Syntax
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

## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil 新命名空間命令](wdsutil-new-namespace.md)
- [wdsutil remove-namespace 命令](wdsutil-remove-namespace.md)
- [wdsutil 開始-nmespace 命令](wdsutil-start-namespace.md)
