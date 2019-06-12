---
title: WSUS 訊息和疑難排解秘訣
description: Windows Server Update Service (WSUS) 主題-使用 WSUS 訊息進行疑難排解
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9f6317f7-bfe0-42d9-87ce-d8f038c728ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4acc5e284d5ca7a62335a1c52f341cda3dfb547e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439737"
---
# <a name="wsus-messages-and-troubleshooting-tips"></a>WSUS 訊息和疑難排解秘訣

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

本主題包含下列 WSUS 訊息的相關資訊：

-   「 電腦沒有回報狀態 」

-   「 訊息識別碼 6703-WSUS 同步處理失敗 」

-   「 錯誤 0x80070643:在安裝期間的嚴重錯誤 」

-   「 未執行某些服務。 檢查下列服務 [...]

## <a name="computer-has-not-reported-status"></a>電腦沒有回報狀態
此訊息會產生在 WSUS 主控台中，當 WSUS 用戶端電腦不會傳送資訊至 WSUS 伺服器，以表示其目前的更新狀態。 WSUS 用戶端電腦，而不是在 WSUS 伺服器，通常會產生此問題。

最常見的原因如下：

-   電腦已遺失網路連線能力：
    -   網路纜線已拔除。
    -   中間的網路纜線有問題。
    -   電腦會有故障的網路介面卡。
    -   已停用電腦所連接的網路連接埠。
    -   無線網路介面卡，才能夠與產生關聯，並連接到公司的無線存取點。
-   電腦已關閉。 （它已關閉或處於睡眠或休眠模式。）

## <a name="message-id-6703---wsus-synchronization-failed"></a>失敗的訊息識別碼 6703-WSUS 同步處理
> 訊息：要求失敗，HTTP 狀態 503:服務無法使用。
> 
> 來源：Microsoft.UpdateServices.Administration.AdminProxy.createUpdateServer.

當您嘗試開啟 Update Services WSUS 伺服器上您會收到下列錯誤：

> 錯誤：連線錯誤
> 
> 嘗試連線到 WSUS 伺服器時，發生錯誤。 多種原因所造成的就會發生此錯誤。 請如果問題持續發生，連絡網路系統管理員。 按一下 重設嘗試連線到伺服器的伺服器節點。

除了上述項目，再嘗試存取 WSUS 系統管理網站的 URL (亦即， `http://CM12CAS:8530`) 失敗，錯誤：

> HTTP 錯誤 503。 無法使用服務。

在此情況下，最可能的原因是 IIS 中 WsusPool 應用程式集區處於停止狀態。

此外，應用程式集區的私用記憶體限制 (KB) 可能設為預設值為 1843200 KB。 如果您遇到這個問題，請將私用記憶體限制增加到 4 GB (4000000 KB)，然後重新啟動應用程式集區。 若要增加專用記憶體限制，請選取 WsusPool 應用程式集區，按一下 編輯應用程式集區的 進階設定。 然後設定專用記憶體限制為 4 GB (4000000 KB)。 已重新啟動應用程式集區之後，監視 SMS_WSUS_SYNC_MANAGER 元件狀態、 wcm.log 和失敗的 wsyncmgr.log。 請注意，它可能需要增加為 8 GB (8000000 KB) 或更高版本時，私用記憶體限制的環境而定。

如需詳細資訊，請參閱：[ConfigMgr 2012 中的 WSUS 同步處理會失敗，HTTP 503 錯誤](http://blogs.technet.com/b/sus/archive/2015/03/23/configmgr-2012-support-tip-wsus-sync-fails-with-http-503-errors.aspx)

## <a name="error-0x80070643-fatal-error-during-installation"></a>錯誤 0x80070643:安裝時發生嚴重錯誤
WSUS 安裝程式會使用 Microsoft SQL Server 來執行安裝。 因為 SQL Server 中執行 WSUS 安裝程式的使用者沒有系統管理員權限，就會發生此問題。

若要解決此問題，和授與系統管理員權限的使用者帳戶或群組帳戶在 SQL Server，然後再次執行 WSUS 安裝程式。

## <a name="some-services-are-not-running-check-the-following-services"></a>某些服務未執行。 請檢查下列服務：

- **自動更新：** 請參閱[自動更新必須更新](https://technet.microsoft.com/library/cc708554(v=ws.10).aspx)疑難排解自動更新服務相關資訊。

- **WSSUService.exe:** 此服務可同步處理。 如果您有同步處理發生問題時，即可存取 WSUSService.exe**開始**，指向**系統管理工具**，再按一下**Services**，再尋找**Windows Server Update Service**在服務清單中。 執行下列動作：
    
    -   確認此服務正在執行。 按一下 **開始**已停止或**重新啟動**重新整理服務。
    
    -   使用事件檢視器檢查**應用程式**， **Securit**y 並**系統**事件記錄檔以查看是否有任何事件，可能表示有問題。
    
    -   您也可以檢查以查看是否有可能表示有問題的事件參見。

- **Web servicesSQL 服務：** 在 IIS 中裝載的 web 服務。 如果它們並未執行，請確定 IIS 是在執行中 （或已啟動）。 您也可以嘗試重設 Web 服務，輸入**iisreset**在命令提示字元。

- **SQL 服務：** 除了自動更新服務的每個服務需要 SQL 服務正在執行。 如果任何記錄檔指出 SQL 連線問題，請先檢查 SQL 服務。 若要存取 SQL 服務，請按一下**開始**，指向**系統管理工具**，按一下 **服務**，然後尋找下列其中之一：
    
  - **MSSQLSERver** （如果您使用 WMSDE 或 MSDE，或如果您使用 SQL Server，並使用執行個體名稱的預設執行個體名稱）
    
  - **MSSQL$ WSUS** （如果您使用 SQL Server 資料庫，且已 「 WSUS"命名您的資料庫執行個體）
    
    服務，以滑鼠右鍵按一下，然後按一下**開始**如果服務未執行，或是**重新啟動**重新整理服務，如果它正在執行。
