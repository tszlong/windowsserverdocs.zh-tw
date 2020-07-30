---
title: 遠端桌面服務（終端機服務）命令列工具參考
description: 遠端桌面服務（RDS）命令列工具的參考文章。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2f371848-5c48-470c-908c-afbc95d3a805
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 36204940c339b937817457aa993ead6ed6644891
ms.sourcegitcommit: 145cf75f89f4e7460e737861b7407b5cee7c6645
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/29/2020
ms.locfileid: "87409689"
---
# <a name="remote-desktop-services-terminal-services-command-line-tools-reference"></a>遠端桌面服務（終端機服務）命令列工具參考

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

深入瞭解可用的遠端桌面服務（終端機服務）命令列工具，並提供詳細資訊的說明和連結。

> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要瞭解最新版本的新功能，請參閱[Windows Server 中遠端桌面服務的新功能](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11))。

| Command | 描述 |
|--|--|--|
| [change](change.md) | 變更 [登入]、[COM 埠對應] 和 [安裝模式] 的遠端桌面工作階段主機伺服器設定。 |
| [change logon](change-logon.md) | 啟用或停用遠端桌面工作階段主機伺服器上用戶端會話的登入，或顯示目前的登入狀態。 |
| [change port](change-port.md) | 列出或變更 COM 埠對應，以與 MS-DOS 應用程式相容。 |
| [change user](change-user.md) | 變更遠端桌面工作階段主機伺服器的安裝模式。 |
| [chglogon](chglogon.md) | 啟用或停用遠端桌面工作階段主機伺服器上用戶端會話的登入，或顯示目前的登入狀態。 |
| [chgport](chgport.md) | 列出或變更 COM 埠對應，以與 MS-DOS 應用程式相容。 |
| [chgusr](chgusr.md) | 變更遠端桌面工作階段主機伺服器的安裝模式。 |
| [flattemp](flattemp.md) | 啟用或停用單層暫存資料夾。 |
| [logoff](logoff.md) | 從遠端桌面工作階段主機伺服器上的會話登出使用者，並刪除伺服器上的會話。 |
| [msg](msg.md) | 將訊息傳送給遠端桌面工作階段主機伺服器上的使用者。 |
| [mstsc](mstsc.md) | 建立遠端桌面工作階段主機伺服器或其他遠端電腦的連接。 |
| [qappsrv](qappsrv.md) | 顯示網路上所有遠端桌面工作階段主機伺服器的清單。 |
| [qprocess](qprocess.md) | 顯示在遠端桌面工作階段主機伺服器上執行之進程的相關資訊。 |
| [查詢](query.md) | 顯示進程、會話和遠端桌面工作階段主機伺服器的相關資訊。 |
| [query process](query-process.md) | 顯示在遠端桌面工作階段主機伺服器上執行之進程的相關資訊。 |
| [query session](query-session.md) | 顯示遠端桌面工作階段主機伺服器上會話的相關資訊。 |
| [query termserver](query-termserver.md) | 顯示網路上所有遠端桌面工作階段主機伺服器的清單。 |
| [query user](query-user.md) | 顯示遠端桌面工作階段主機伺服器上的使用者會話相關資訊。 |
| [quser](quser.md) | 顯示遠端桌面工作階段主機伺服器上的使用者會話相關資訊。 |
| [qwinsta](qwinsta.md) | 顯示遠端桌面工作階段主機伺服器上會話的相關資訊。 |
| [rdpsign](rdpsign.md) | 可讓您以數位方式簽署遠端桌面通訊協定（.rdp）檔案。 |
| [reset session](reset-session.md) | 可讓您在遠端桌面工作階段主機伺服器上重設（刪除）會話。 |
| [rwinsta](rwinsta.md) | 可讓您在遠端桌面工作階段主機伺服器上重設（刪除）會話。 |
| [shadow](shadow.md) | 可讓您在遠端桌面工作階段主機伺服器上，遠端控制另一位使用者的使用中會話。 |
| [tscon](tscon.md) | 連接到遠端桌面工作階段主機伺服器上的另一個會話。 |
| [tsdiscon](tsdiscon.md) | 中斷會話與遠端桌面工作階段主機伺服器的連線。 |
| [tskill](tskill.md) | 結束在遠端桌面工作階段主機伺服器上的會話中執行的處理常式。 |
| [tsprof](tsprof.md) | 將遠端桌面服務的使用者設定資訊從一個使用者複製到另一個。 |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)