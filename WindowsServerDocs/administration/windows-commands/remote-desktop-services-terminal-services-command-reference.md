---
title: 遠端桌面服務 (終端機服務) 命令參考資料
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2f371848-5c48-470c-908c-afbc95d3a805
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 630b2274e96e446af07f7630d95056b40858209f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722436"
---
# <a name="remote-desktop-services-terminal-services-command-reference"></a>遠端桌面服務 (終端機服務) 命令參考資料

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

以下是遠端桌面服務命令列工具的清單。
> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要瞭解最新版本的新功能，請參閱 Windows Server TechNet Library 中的[Windows server 2012 遠端桌面服務的新功能](https://technet.microsoft.com/library/hh831527)。
> 
> |                 Command                 |                                                      描述                                                       |
> |-----------------------------------------|------------------------------------------------------------------------------------------------------------------------|
> |           [change](change.md)           | 變更登入、COM 埠對應和安裝模式遠端桌面工作階段主機（rd 工作階段主機）伺服器設定。 |
> |     [change logon](change-logon.md)     |    在 rd 工作階段主機伺服器上啟用或停用來自用戶端會話的登入，或顯示目前的登入狀態。     |
> |      [change port](change-port.md)      |                   列出或變更 COM 埠對應，以與 MS-DOS 應用程式相容。                    |
> |      [change user](change-user.md)      |                                變更 rd 工作階段主機伺服器的安裝模式。                                |
> |         [chglogon](chglogon.md)         |    在 rd 工作階段主機伺服器上啟用或停用來自用戶端會話的登入，或顯示目前的登入狀態。     |
> |          [chgport](chgport.md)          |                   列出或變更 COM 埠對應，以與 MS-DOS 應用程式相容。                    |
> |           [chgusr](chgusr.md)           |                                變更 rd 工作階段主機伺服器的安裝模式。                                |
> |         [flattemp](flattemp.md)         |                                      啟用或停用單層暫存資料夾。                                       |
> |           [logoff](logoff.md)           |          將使用者從 rd 工作階段主機伺服器上的會話登出，並刪除伺服器的會話。          |
> |              [msg](msg.md)              |                                將訊息傳送給 rd 工作階段主機伺服器上的使用者。                                 |
> |            [mstsc](mstsc.md)            |                       建立與 rd 工作階段主機伺服器或其他遠端電腦的連接。                        |
> |          [qappsrv](qappsrv.md)          |                             顯示網路上所有 rd 工作階段主機伺服器的清單。                             |
> |         [qprocess](qprocess.md)         |                  顯示在 rd 工作階段主機伺服器上執行之處理常式的相關資訊。                   |
> |            [查詢](query.md)            |                      顯示處理常式、會話和 rd 工作階段主機伺服器的相關資訊。                      |
> |    [查詢處理序](query-process.md)    |                  顯示在 rd 工作階段主機伺服器上執行之處理常式的相關資訊。                   |
> |    [query session](query-session.md)    |                           顯示 rd 工作階段主機伺服器上會話的相關資訊。                            |
> | [查詢 termserver](query-termserver.md) |                             顯示網路上所有 rd 工作階段主機伺服器的清單。                             |
> |       [query user](query-user.md)       |                         顯示 rd 工作階段主機伺服器上使用者會話的相關資訊。                         |
> |            [quser](quser.md)            |                         顯示 rd 工作階段主機伺服器上使用者會話的相關資訊。                         |
> |          [qwinsta](qwinsta.md)          |                           顯示 rd 工作階段主機伺服器上會話的相關資訊。                            |
> |          [rdpsign](rdpsign.md)          |                          可讓您以數位方式簽署遠端桌面通訊協定（.rdp）檔案。                          |
> |    [reset session](reset-session.md)    |                         可讓您在 rd 工作階段主機伺服器上重設（刪除）會話。                          |
> |          [rwinsta](rwinsta.md)          |                         可讓您在 rd 工作階段主機伺服器上重設（刪除）會話。                          |
> |           [shadow](shadow.md)           |            可讓您在 rd 工作階段主機伺服器上，遠端控制另一位使用者的使用中會話。             |
> |            [tscon](tscon.md)            |                               連接到 rd 工作階段主機伺服器上的另一個會話。                                |
> |         [tsdiscon](tsdiscon.md)         |                                 中斷會話與 rd 工作階段主機伺服器的連線。                                  |
> |           [tskill](tskill.md)           |                           結束在 rd 工作階段主機伺服器上的會話中執行的處理常式。                            |
> |           [tsprof](tsprof.md)           |              將遠端桌面服務的使用者設定資訊從一個使用者複製到另一個。               |
