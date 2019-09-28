---
title: 子命令開始-命名空間
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 55fe4a6136fe4f8e886dc62fff746a1e5ff1898f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370754"
---
# <a name="subcommand-start-namespace"></a>子命令： start-Namespace

> 適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 會啟動已排程的轉換命名空間。
> ## <a name="syntax"></a>語法
> ```
> wdsutil /start-Namespace /Namespace:<Namespace name> [/Server:<Server name>]
> ```
> ## <a name="parameters"></a>參數
> 
> |          參數          |                                                                                                                                                                                             描述                                                                                                                                                                                             |
> |-----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | /Namespace： <Namespace name> | 指定命名空間的名稱。 請注意，這不是易記的名稱，而且必須是唯一的。<br /><br />-   **部署伺服器**：命名空間名稱的語法為/Namspace： WDS： <Image group> @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4。 例如: **WDS： ImageGroup1/install .wim/1**<br />-   **傳輸伺服器**：這個名稱必須符合在伺服器上建立命名空間時所指定的名稱。 |
> |   [/Server： <Server name>]   |                                                                                                           指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。                                                                                                           |
> 
> ## <a name="BKMK_examples"></a>典型
> 若要啟動命名空間，請輸入下列其中一項：
> ```
> wdsutil /start-Namespace /Namespace:"Custom Auto 1"
> wdsutil /start-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1"
> ```
> #### <a name="additional-references"></a>其他參考
> [命令列語法索引鍵](command-line-syntax-key.md)
> [使用 AllNamespaces 命令](using-the-get-allnamespaces-command.md)
>  使用[新的-](using-the-new-namespace-command.md)Namespace 命令，
> [使用 remove-namespace 命令](using-the-remove-namespace-command.md)
