---
title: get-Namespace
description: 取得命名空間的參考文章，其會顯示自訂命名空間的相關資訊。
ms.topic: article
ms.assetid: ea641bab-e97b-4909-918e-447730027dc1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e998334b8297b06bf5eb23b9106acd3770504ffb
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896937"
---
# <a name="get-namespace"></a>get-Namespace

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

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
|      命名空間<Namespace name>      | 指定命名空間的名稱。 請注意，這不是易記的名稱，而且必須是唯一的。<p>-部署伺服器：命名空間名稱的語法是/Namspace： WDS： <ImageGroup> / <ImageName> / <Index> 。 例如： **WDS： ImageGroup1/install .wim/1**<br />-傳輸伺服器：此值應符合在伺服器上建立命名空間時所提供的名稱。 |
|        [/Server： <Server name> ]        |                                                                                                             指定伺服器的名稱。 這可以是 NetBIOS 名稱或 (FQDN) 的完整功能變數名稱。 如果未指定伺服器名稱，則會使用本機伺服器。                                                                                                              |
| [/Show： Clients] 或 [/details： Clients] |                                                                                                                                                  顯示連線到指定命名空間之用戶端電腦的相關資訊。                                                                                                                                                  |

## <a name="examples"></a>範例
若要查看命名空間的相關資訊，請輸入：
```
wdsutil /Get-Namespace /Namespace:Custom Auto 1
```
若要查看已連接之命名空間和用戶端的相關資訊，請輸入下列其中一項：
- Windows Server 2008：`wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /Show:Clients`
- Windows Server 2008 R2：`wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /details:Clients`
  ## <a name="additional-references"></a>其他參考資料
  - [命令列語法索引鍵](command-line-syntax-key.md) 
  [使用 AllNamespaces 命令](using-the-get-allnamespaces-command.md) 
  [使用新的-Namespace 命令](using-the-new-namespace-command.md) 
  [使用 remove-Namespace 命令](using-the-remove-namespace-command.md) 
  [子命令： start-Namespace](subcommand-start-namespace.md)
