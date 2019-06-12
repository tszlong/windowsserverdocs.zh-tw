---
title: 子命令開始命名空間
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2dd1c11e-6ab7-4129-9e3a-3f80e0ba59c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9a54f849580a139470c2cca43ba57fee60dc81ec
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441153"
---
# <a name="subcommand-start-namespace"></a>子命令： 開始命名空間

> 適用於：Windows Server （半年通道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 開始排程傳送命名空間。
> ## <a name="syntax"></a>語法
> ```
> wdsutil /start-Namespace /Namespace:<Namespace name> [/Server:<Server name>]
> ```
> ## <a name="parameters"></a>參數
> 
> |          參數          |                                                                                                                                                                                             描述                                                                                                                                                                                             |
> |-----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | / 命名空間：<Namespace name> | 指定的命名空間名稱。 請注意，這不是易記的名稱，它必須是唯一的。<br /><br />-   **部署伺服器**:命名空間名稱的語法是 /Namspace:WDS:<Image group>/<Image name>/<Index>。 例如: **WDS:ImageGroup1/install.wim/1**<br />-   **傳輸伺服器**:此名稱必須符合伺服器上建立時指定命名空間的名稱。 |
> |   [/ 伺服器：<Server name>]   |                                                                                                           指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未不指定任何伺服器名稱，則會使用本機伺服器。                                                                                                           |
> 
> ## <a name="BKMK_examples"></a>範例
> 若要啟動的命名空間，請輸入下列其中一項：
> ```
> wdsutil /start-Namespace /Namespace:"Custom Auto 1"
> wdsutil /start-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1"
> ```
> #### <a name="additional-references"></a>其他參考資料
> [命令列語法重點](command-line-syntax-key.md)
> [使用 get AllNamespaces 命令](using-the-get-allnamespaces-command.md)
> [使用 新命名空間命令](using-the-new-namespace-command.md)
> [使用移除命名空間命令](using-the-remove-namespace-command.md)
