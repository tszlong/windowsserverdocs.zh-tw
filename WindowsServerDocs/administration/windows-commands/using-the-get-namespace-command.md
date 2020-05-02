---
title: get-Namespace
description: 取得命名空間的參考主題，其會顯示自訂命名空間的相關資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea641bab-e97b-4909-918e-447730027dc1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 76980d2add9ee9b7584812c9d366408f8770b681
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719746"
---
# <a name="get-namespace"></a>get-Namespace

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示自訂命名空間的相關資訊。

## <a name="syntax"></a>語法
Windows Server 2008 R2
```
wdsutil /Get-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/Show:Clients]
```
Windows Server 2008 R2
```
wdsutil /Get-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/details:Clients]
```
### <a name="parameters"></a>參數

|               參數               |                                                                                                                                                                                         描述                                                                                                                                                                                          |
|---------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      命名空間<Namespace name>      | 指定命名空間的名稱。 請注意，這不是易記的名稱，而且必須是唯一的。<p>-部署伺服器：命名空間名稱的語法是/Namspace： WDS：<ImageGroup>/<ImageName>/<Index>。 例如： **WDS： ImageGroup1/install .wim/1**<br />-傳輸伺服器：此值應符合在伺服器上建立命名空間時所提供的名稱。 |
|        [/Server：<Server name>]        |                                                                                                             指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。                                                                                                              |
| [/Show： Clients] 或 [/details： Clients] |                                                                                                                                                  顯示連線到指定命名空間之用戶端電腦的相關資訊。                                                                                                                                                  |

## <a name="examples"></a>範例
若要查看命名空間的相關資訊，請輸入：
```
wdsutil /Get-Namespace /Namespace:Custom Auto 1
```
若要查看已連接之命名空間和用戶端的相關資訊，請輸入下列其中一項：
- Windows Server 2008：`wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /Show:Clients`
- Windows Server 2008 R2：`wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /details:Clients`
  ## <a name="additional-references"></a>其他參考
  - [Command-Line Syntax Key](command-line-syntax-key.md)使用
  [AllNamespaces 命令](using-the-get-allnamespaces-command.md)
  的命令列語法索引鍵使用[新的-](using-the-new-namespace-command.md)
  [Using the remove-Namespace Command](using-the-remove-namespace-command.md)
  namespace 命令[子命令： start-namespace](subcommand-start-namespace.md)
