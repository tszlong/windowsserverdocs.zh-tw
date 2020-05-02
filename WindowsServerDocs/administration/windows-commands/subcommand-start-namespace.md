---
title: 子命令開始-命名空間
description: 子命令開始-命名空間的參考主題，它會啟動已排程的轉換命名空間。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2dd1c11e-6ab7-4129-9e3a-3f80e0ba59c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1562fcb6c61533fcc9994e9011bf7d61154c06f7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721662"
---
# <a name="subcommand-start-namespace"></a>子命令： start-Namespace

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟動已排程的轉換命名空間。

## <a name="syntax"></a>語法
```
wdsutil /start-Namespace /Namespace:<Namespace name[/Server:<Server name>]
```
### <a name="parameters"></a>參數

|          參數          |                                                                                                                                                                                             描述                                                                                                                                                                                             |
|-----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Namespace： <命名空間名稱| 指定命名空間的名稱。 請注意，這不是易記的名稱，而且必須是唯一的。<p>-   **部署伺服器**：命名空間名稱的語法是/NAMSPACE： WDS：<Image group>/<Image name>/<Index>。 例如： **WDS： ImageGroup1/install .wim/1**<br />-   **傳輸伺服器**：此名稱必須符合在伺服器上建立命名空間時所指定的名稱。 |
|   [/Server：<Server name>]   |                                                                                                           指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。                                                                                                           |

## <a name="examples"></a>範例
若要啟動命名空間，請輸入下列其中一項：
```
wdsutil /start-Namespace /Namespace:Custom Auto 1
wdsutil /start-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1
```
## <a name="additional-references"></a>其他參考
- [使用](command-line-syntax-key.md)
[AllNamespaces 命令](using-the-get-allnamespaces-command.md)
的命令列語法索引鍵使用[new-](using-the-new-namespace-command.md)
[namespace 命令](using-the-remove-namespace-command.md)
