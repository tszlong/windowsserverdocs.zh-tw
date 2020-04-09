---
title: get-Namespace
description: Get-Namespace 的 Windows 命令主題，它會顯示自訂命名空間的相關資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea641bab-e97b-4909-918e-447730027dc1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 58e84ec5c82ea3c2663b38bd2e53f65f2cf47519
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830881"
---
# <a name="get-namespace"></a>get-Namespace

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

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
|      /Namespace：<Namespace name>      | 指定命名空間的名稱。 請注意，這不是易記的名稱，而且必須是唯一的。<p>-部署伺服器：命名空間名稱的語法是/Namspace： WDS：<ImageGroup>/<ImageName>/<Index>。 例如： **WDS： ImageGroup1/install .wim/1**<br />-傳輸伺服器：此值應符合在伺服器上建立命名空間時所提供的名稱。 |
|        [/Server：<Server name>]        |                                                                                                             指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。                                                                                                              |
| [/Show： Clients] 或 [/details： Clients] |                                                                                                                                                  顯示連線到指定命名空間之用戶端電腦的相關資訊。                                                                                                                                                  |

## <a name="examples"></a><a name=BKMK_examples></a>典型
若要查看命名空間的相關資訊，請輸入：
```
wdsutil /Get-Namespace /Namespace:Custom Auto 1
```
若要查看已連接之命名空間和用戶端的相關資訊，請輸入下列其中一項：
- Windows Server 2008： `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /Show:Clients`
- Windows Server 2008 R2： `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /details:Clients`
  ## <a name="additional-references"></a>其他參考資料
  - [命令列語法索引鍵](command-line-syntax-key.md)
  [使用 AllNamespaces 命令](using-the-get-allnamespaces-command.md)
  使用[new namespace](using-the-new-namespace-command.md)命令
  [使用 remove-namespace 命令](using-the-remove-namespace-command.md)
  [子命令： start-namespace](subcommand-start-namespace.md)
