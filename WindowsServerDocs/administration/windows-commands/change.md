---
title: 變更
description: 適用于變更的 Windows 命令主題，會變更登入、COM 埠對應和安裝模式遠端桌面工作階段主機伺服器設定。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90012116-0fb3-4f34-a819-cf4d4b4f8981
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d5d91f8d0941fc96e776c761b9c7037e58588df8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847951"
---
# <a name="change"></a>變更

> 適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更登入、COM 埠對應和安裝模式遠端桌面工作階段主機伺服器設定。

> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要瞭解最新版本的新功能，請參閱 Windows Server TechNet Library 中的[Windows server 2012 遠端桌面服務的新功能](https://technet.microsoft.com/library/hh831527)。

## <a name="syntax"></a>語法

 ```
 change logon
 change port
 change user
 ```
 
 ### <a name="parameters"></a>參數
 
 |            參數            |                                                   描述                                                   |
 |---------------------------------|-----------------------------------------------------------------------------------------------------------------|
 | [change logon](change-logon.md) | 在 rd 工作階段主機伺服器上啟用或停用來自用戶端會話的登入，或顯示目前的登入狀態。 |
 |  [change port](change-port.md)  |                列出或變更 COM 埠對應，以與 MS-DOS 應用程式相容。                |
 |  [change user](change-user.md)  |                            變更 rd 工作階段主機伺服器的安裝模式。                             |
 
 ## <a name="additional-references"></a>其他參考資料
 - [命令列語法金鑰](command-line-syntax-key.md)
 [遠端桌面服務（終端機服務）命令參考](remote-desktop-services-terminal-services-command-reference.md)
