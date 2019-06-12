---
title: change port
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ced5b9f0198179ab8b388f56aaea848b7a966081
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434501"
---
# <a name="change-port"></a>change port

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

列出或變更相容的 WMS-DOS 應用程式的 COM 連接埠對應。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。
> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要了解最新版本的新功能，請參閱[Windows Server 2012 中的遠端桌面服務在何種新的 s](https://technet.microsoft.com/library/hh831527)在 Windows Server TechNet 文件庫中。
> ## <a name="syntax"></a>語法
> ```
> change port [<PortX>=<PortY> | /d <PortX> | /query]
> ```
> ## <a name="parameters"></a>參數
> 
> |    參數    |              描述               |
> |-----------------|----------------------------------------|
> | <PortX>=<PortY> |    對應 COM <*PortX*> 到 <*PortY*>。    |
> |   /d <PortX>    | 刪除 com 對應 <*PortX*>。 |
> |     /query      |  顯示目前的連接埠對應。   |
> |       /?        |  在命令提示字元顯示說明。  |
> 
> ## <a name="remarks"></a>備註
> - 大部分的 WMS-DOS 應用程式支援僅透過 COM4 序列埠 COM1。 **變更連接埠**命令會將序列連接埠對應至不同的連接埠號碼，讓應用程式不支援高編號的 COM 存取序列埠的連接埠。 重新對應只適用於目前的工作階段，並不會保留，如果您從工作階段登出並再次登入。
> - 使用**變更連接埠**不含任何參數，以顯示可用的 COM 連接埠和其目前的對應。
>   ## <a name="BKMK_examples"></a>範例
> - 若要對應 com12 的連接埠 com1 使用 MS-DOS 型應用程式，請輸入：
>   ```
>   change port com12=com1
>   ```
> - 若要顯示目前的連接埠對應，請輸入：
>   ```
>   change port /query
>   ```
>   #### <a name="additional-references"></a>其他參考資料
>   [命令列語法重點](command-line-syntax-key.md)
>   [變更](change.md)
>   [遠端桌面服務&#40;終端機服務&#41;命令參考](remote-desktop-services-terminal-services-command-reference.md)
