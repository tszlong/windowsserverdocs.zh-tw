---
title: 遠端桌面服務 (終端機服務) 命令參考資料
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2f371848-5c48-470c-908c-afbc95d3a805
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b7524973c50b597b8fd8798c5268746dd1d991d7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441831"
---
# <a name="remote-desktop-services-terminal-services-command-reference"></a>遠端桌面服務 (終端機服務) 命令參考資料

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

以下是一份遠端桌面服務的命令列工具。
> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要了解最新版本的新功能，請參閱[Windows Server 2012 中的遠端桌面服務在何種新的 s](https://technet.microsoft.com/library/hh831527)在 Windows Server TechNet 文件庫中。
> 
> |                 命令                 |                                                      描述                                                       |
> |-----------------------------------------|------------------------------------------------------------------------------------------------------------------------|
> |           [change](change.md)           | 變更登入、 COM 連接埠對應及安裝模式的遠端桌面工作階段主機 (rd 工作階段主機) 伺服器設定。 |
> |     [change logon](change-logon.md)     |    啟用或停用從 rd 工作階段主機伺服器上的用戶端工作階段的登入，或顯示目前的登入狀態。     |
> |      [change port](change-port.md)      |                   列出或變更相容的 WMS-DOS 應用程式的 COM 連接埠對應。                    |
> |      [change user](change-user.md)      |                                變更 rd 工作階段主機伺服器的安裝模式。                                |
> |         [chglogon](chglogon.md)         |    啟用或停用從 rd 工作階段主機伺服器上的用戶端工作階段的登入，或顯示目前的登入狀態。     |
> |          [chgport](chgport.md)          |                   列出或變更相容的 WMS-DOS 應用程式的 COM 連接埠對應。                    |
> |           [chgusr](chgusr.md)           |                                變更 rd 工作階段主機伺服器的安裝模式。                                |
> |         [flattemp](flattemp.md)         |                                      啟用或停用一般暫存資料夾。                                       |
> |           [logoff](logoff.md)           |          登出 rd 工作階段主機伺服器上的工作階段的使用者，並從伺服器刪除的工作階段。          |
> |              [msg](msg.md)              |                                若要在 rd 工作階段主機伺服器上傳送訊息。                                 |
> |            [mstsc](mstsc.md)            |                       建立 rd 工作階段主機伺服器或其他遠端電腦的連線。                        |
> |          [qappsrv](qappsrv.md)          |                             顯示在網路上的所有 rd 工作階段主機伺服器的清單。                             |
> |         [qprocess](qprocess.md)         |                  顯示在 rd 工作階段主機伺服器執行的處理序的相關資訊。                   |
> |            [查詢](query.md)            |                      顯示處理程序、 工作階段和 rd 工作階段主機伺服器的相關資訊。                      |
> |    [查詢處理序](query-process.md)    |                  顯示在 rd 工作階段主機伺服器執行的處理序的相關資訊。                   |
> |    [查詢工作階段](query-session.md)    |                           在 rd 工作階段主機伺服器上，會顯示有關工作階段的資訊。                            |
> | [查詢 termserver](query-termserver.md) |                             顯示在網路上的所有 rd 工作階段主機伺服器的清單。                             |
> |       [查詢使用者](query-user.md)       |                         在 rd 工作階段主機伺服器上，會顯示關於使用者工作階段的資訊。                         |
> |            [quser](quser.md)            |                         在 rd 工作階段主機伺服器上，會顯示關於使用者工作階段的資訊。                         |
> |          [qwinsta](qwinsta.md)          |                           在 rd 工作階段主機伺服器上，會顯示有關工作階段的資訊。                            |
> |          [rdpsign](rdpsign.md)          |                          可讓您以數位方式簽署遠端桌面通訊協定 (.rdp) 檔案。                          |
> |    [reset session](reset-session.md)    |                         可讓您在 rd 工作階段主機伺服器上的工作階段重設 （刪除）。                          |
> |          [rwinsta](rwinsta.md)          |                         可讓您在 rd 工作階段主機伺服器上的工作階段重設 （刪除）。                          |
> |           [shadow](shadow.md)           |            可讓您從遠端控制使用中工作階段的 rd 工作階段主機伺服器上的另一位使用者。             |
> |            [tscon](tscon.md)            |                               會連線到 rd 工作階段主機伺服器上的另一個工作階段。                                |
> |         [tsdiscon](tsdiscon.md)         |                                 工作階段中斷連線的 rd 工作階段主機伺服器。                                  |
> |           [tskill](tskill.md)           |                           結束在 rd 工作階段主機伺服器上執行的工作階段中的程序。                            |
> |           [tsprof](tsprof.md)           |              複製一位使用者的遠端桌面服務使用者的設定資訊儲存到另一個。               |
