---
title: change
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 90012116-0fb3-4f34-a819-cf4d4b4f8981
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0a02302c4b99ead3701a966ba2d3fc65f6b078d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434402"
---
# <a name="change"></a>change

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

變更登入、 COM 連接埠對應及安裝模式的遠端桌面工作階段主機 (rd 工作階段主機) 伺服器設定。
> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要了解最新版本的新功能，請參閱[Windows Server 2012 中的遠端桌面服務在何種新的 s](https://technet.microsoft.com/library/hh831527)在 Windows Server TechNet 文件庫中。
> ## <a name="syntax"></a>語法
> ```
> change logon
> change port
> change user
> ```
> ## <a name="parameters"></a>參數
> 
> |            參數            |                                                   描述                                                   |
> |---------------------------------|-----------------------------------------------------------------------------------------------------------------|
> | [change logon](change-logon.md) | 啟用或停用從 rd 工作階段主機伺服器上的用戶端工作階段的登入，或顯示目前的登入狀態。 |
> |  [change port](change-port.md)  |                列出或變更相容的 WMS-DOS 應用程式的 COM 連接埠對應。                |
> |  [change user](change-user.md)  |                            變更 rd 工作階段主機伺服器的安裝模式。                             |
> 
> #### <a name="additional-references"></a>其他參考資料
> [命令列語法重點](command-line-syntax-key.md)
> [遠端桌面服務&#40;終端機服務&#41;命令參考](remote-desktop-services-terminal-services-command-reference.md)
