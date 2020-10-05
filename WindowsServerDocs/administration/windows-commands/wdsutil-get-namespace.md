---
title: wdsutil 取得命名空間
description: Wdsutil get 命名空間的參考文章，顯示自訂命名空間的相關資訊。
ms.topic: reference
ms.assetid: ea641bab-e97b-4909-918e-447730027dc1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2c88bd26af900950c33b059822c08f5afb40de77
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729891"
---
# <a name="wdsutil-get-namespace"></a>wdsutil 取得命名空間

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示自訂命名空間的相關資訊。

## <a name="syntax"></a>Syntax
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
|      命名空間<Namespace name>      | 指定命名空間的名稱。 請注意，這不是易記的名稱，而且必須是唯一的。<p>-Deployment Server：命名空間名稱的語法為/Namspace： WDS： <ImageGroup> / <ImageName> / <Index> 。 例如： **WDS： ImageGroup1/install .wim/1**<br />-Transport Server：此值應該符合在伺服器上建立命名空間時所提供的名稱。 |
|        [/Server： <Server name> ]        |                                                                                                             指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。                                                                                                              |
| [/Show： Clients] 或 [/details： Clients] |                                                                                                                                                  顯示連接至指定命名空間之用戶端電腦的相關資訊。                                                                                                                                                  |

## <a name="examples"></a>範例
若要查看命名空間的相關資訊，請輸入：
```
wdsutil /Get-Namespace /Namespace:Custom Auto 1
```
若要查看命名空間和所連接用戶端的相關資訊，請輸入下列其中一項：
- Windows Server 2008： `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /Show:Clients`
- Windows Server 2008 R2： `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /details:Clients`

## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil get-allnamespaces 命令](wdsutil-get-allnamespaces.md)
- [wdsutil 新命名空間命令](wdsutil-new-namespace.md)
- [wdsutil remove-namespace 命令](wdsutil-remove-namespace.md)
- [wdsutil 開始-命名空間命令](wdsutil-start-namespace.md)
