---
title: change port
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3d772c90-e849-4e74-b9ec-b6cae1159336 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0e587572acd1af1cc7dbd2550e1eae5244d0d1dd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379603"
---
# <a name="change-port"></a>change port

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

列出或變更 COM 埠對應，以與 MS-DOS 應用程式相容。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要瞭解最新版本的新功能，請參閱 Windows Server TechNet Library 中的[Windows server 2012 遠端桌面服務的新功能](https://technet.microsoft.com/library/hh831527)。
> ## <a name="syntax"></a>語法
> ```
> change port [<PortX>=<PortY> | /d <PortX> | /query]
> ```
> ## <a name="parameters"></a>參數
> 
> |    參數    |              描述               |
> |-----------------|----------------------------------------|
> | <PortX>=<PortY> |    將 COM <*PortX*> 對應至 <*PortY*>。    |
> |   /d <PortX>    | 刪除 COM <*PortX*> 的對應。 |
> |     /query      |  顯示目前的埠對應。   |
> |       /?        |  在命令提示字元顯示說明。  |
> 
> ## <a name="remarks"></a>備註
> - 大部分的 MS-DOS 應用程式僅支援 COM1 到 COM4 的序列埠。 [**變更埠**] 命令會將序列埠對應至不同的埠號碼，讓不支援高編號 COM 埠的應用程式能夠存取序列埠。 重新對應只適用于目前的會話，如果您登出會話然後再次登入，則不會保留。
> - 使用不含任何參數的**變更埠**，以顯示可用的 COM 埠和其目前的對應。
>   ## <a name="BKMK_examples"></a>典型
> - 若要將 COM12 對應到 COM1 以供以 MS-DOS 為基礎的應用程式使用，請輸入：
>   ```
>   change port com12=com1
>   ```
> - 若要顯示目前的埠對應，請輸入：
>   ```
>   change port /query
>   ```
>   #### <a name="additional-references"></a>其他參考
>   [命令列語法索引鍵](command-line-syntax-key.md)
>   [變更](change.md)
>   [遠端桌面服務&#40;終端機&#41;服務命令參考](remote-desktop-services-terminal-services-command-reference.md)
