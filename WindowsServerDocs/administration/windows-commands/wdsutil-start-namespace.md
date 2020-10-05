---
title: wdsutil 開始-命名空間
description: 子命令開始-命名空間的參考文章，它會啟動已排程的轉換命名空間。
ms.topic: reference
ms.assetid: 2dd1c11e-6ab7-4129-9e3a-3f80e0ba59c0
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7463b062bf3322d6cf4f9c481effde825cb2979a
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730542"
---
# <a name="wdsutil-start-namespace"></a>wdsutil 開始-命名空間

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟動已排程的轉換命名空間。

## <a name="syntax"></a>語法
```
wdsutil /start-Namespace /Namespace:<Namespace name[/Server:<Server name>]
```
### <a name="parameters"></a>參數

|          參數          |                                                                                                                                                                                             描述                                                                                                                                                                                             |
|-----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Namespace： <命名空間名稱| 指定命名空間的名稱。 請注意，這不是易記的名稱，而且必須是唯一的。<p>-   **部署伺服器**：命名空間名稱的語法為/NAMSPACE： WDS： <Image group> / <Image name> / <Index> 。 例如： **WDS： ImageGroup1/install .wim/1**<br />-   **傳輸伺服器**：此名稱必須符合在伺服器上建立命名空間時所提供的名稱。 |
|   [/Server： <Server name> ]   |                                                                                                           指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。                                                                                                           |

## <a name="examples"></a>範例
若要啟動命名空間，請輸入下列其中一項：
```
wdsutil /start-Namespace /Namespace:Custom Auto 1
wdsutil /start-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil get-allnamespaces 命令](wdsutil-get-allnamespaces.md)
- [wdsutil 新命名空間命令](wdsutil-new-namespace.md)
- [wdsutil remove-namespace 命令](wdsutil-remove-namespace.md)
