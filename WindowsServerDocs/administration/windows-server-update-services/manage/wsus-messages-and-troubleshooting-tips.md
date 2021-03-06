---
title: WSUS 訊息和疑難排解秘訣
description: Windows Server Update Services (WSUS) 主題-使用 WSUS 訊息進行疑難排解
ms.topic: article
ms.assetid: 9f6317f7-bfe0-42d9-87ce-d8f038c728ca
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8abb77727139652f7d3d1ef2e4ce8dd8320db47a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89624235"
---
# <a name="wsus-messages-and-troubleshooting-tips"></a>WSUS 訊息和疑難排解秘訣

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題包含下列 WSUS 訊息的相關資訊：

-   電腦未回報狀態

-   訊息識別碼 6703-WSUS 同步處理失敗

-   錯誤0x80070643：安裝期間發生嚴重錯誤

-   某些服務不在執行中。 檢查下列服務 [...]

## <a name="computer-has-not-reported-status"></a>電腦未回報狀態
當 WSUS 用戶端電腦未將資訊傳送至 WSUS 伺服器，以指出其目前的更新狀態時，就會在 WSUS 主控台中產生此訊息。 此問題通常是由 WSUS 用戶端電腦所造成，而不是 WSUS 伺服器。

最常見的原因包括：

-   電腦已失去網路連線能力：
    -   網路纜線已插上。
    -   中間的網路線有問題。
    -   電腦有故障的網路介面卡。
    -   電腦連接的網路埠已停用。
    -   無線介面卡無法與建立關聯，並聯機到公司的無線存取點。
-   電腦已關閉。  (已關閉或處於睡眠或睡眠模式。 ) 

## <a name="message-id-6703---wsus-synchronization-failed"></a>訊息識別碼 6703-WSUS 同步處理失敗
> 訊息：要求失敗，HTTP 狀態503：服務無法使用。
>
> 來源： UpdateServices. AdminProxy. createUpdateServer。

當您嘗試在 WSUS 伺服器上開啟更新服務時，您會收到下列錯誤：

> 錯誤：連接錯誤
>
> 嘗試連接到 WSUS 伺服器時發生錯誤。 發生此錯誤的原因有很多。 如果問題持續發生，請洽詢您的網路系統管理員。 按一下 [重設伺服器] 節點，以再次連接到伺服器。

除了上述內容，嘗試存取 WSUS 系統管理網站的 URL (例如， `http://CM12CAS:8530`) 失敗，並出現下列錯誤：

> HTTP 錯誤 503。 服務無法使用

在這種情況下，最可能的原因是 IIS 中的 WsusPool 應用程式集區處於停止狀態。

此外，應用程式集區 (KB) 的私用記憶體限制可能會設定為預設值 1843200 KB。 如果您遇到此問題，請將私用記憶體限制增加至 4GB (4000000 KB) 並重新啟動應用程式集區。 若要增加私用記憶體限制，請選取 WsusPool 應用程式集區，然後按一下 [編輯應用程式集區] 下的 [設定]。 然後，將私用記憶體限制設為 4 GB (4000000 KB) 。 重新開機應用程式集區之後，請監視 SMS_WSUS_SYNC_MANAGER 元件狀態、wcm. log 和 wsyncmgr，以瞭解失敗。 請注意，視環境而定，可能需要將私用記憶體限制增加為 8GB (8000000 KB) 或更高的版本。

如需其他詳細資訊，請參閱： [ConfigMgr 2012 中的 WSUS 同步處理失敗，並出現 HTTP 503 錯誤](https://blogs.technet.com/b/sus/archive/2015/03/23/configmgr-2012-support-tip-wsus-sync-fails-with-http-503-errors.aspx)

## <a name="error-0x80070643-fatal-error-during-installation"></a>錯誤0x80070643：安裝期間發生嚴重錯誤
WSUS 安裝程式會使用 Microsoft SQL Server 來執行安裝。 發生此問題的原因是執行 WSUS 安裝程式的使用者沒有 SQL Server 的系統管理員許可權。

若要解決此問題，請將系統管理員許可權授與使用者帳戶或 SQL Server 中的群組帳戶，然後再次執行 WSUS 安裝程式。

## <a name="some-services-are-not-running-check-the-following-services"></a>某些服務不在執行中。 檢查下列服務：

- **Selfupdate：** 如 [需](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc708554(v=ws.10)) Selfupdate 服務疑難排解的相關資訊，請參閱自動更新。

- **WSSUService.exe：** 這種服務有助於進行同步處理。 如果您有同步處理的問題，請按一下 [ **開始**]，指向 [系統 **管理工具**]，按一下 [ **服務**]，然後在服務清單中尋找 **Windows Server Update Service** ，以存取 WSUSService.exe。 執行下列動作：

    -   確認此服務正在執行。 如果停止或**重新開機以重新**整理服務，請按一下 [**啟動**]。

    -   使用事件檢視器來檢查 **應用程式**、 **Securit**y 和 **系統** 事件記錄檔，以查看是否有任何可能表示有問題的事件。

    -   您也可以檢查 SoftwareDistribution，以查看是否有可能表示有問題的事件。

- **Web ServicesSQL 服務：** Web 服務是裝載在 IIS 中。 如果未執行，請確定 IIS 正在執行 (或已啟動) 。 您也可以在命令提示字元中輸入 **iisreset** ，嘗試重設 Web 服務。

- **SQL 服務：** 除了 selfupdate 服務以外的每個服務都需要 SQL 服務正在執行。 如果有任何記錄檔指出 SQL 連接問題，請先檢查 SQL 服務。 若要存取 SQL 服務，請按一下 [ **開始**]，指向 [系統 **管理工具**]，按一下 [ **服務**]，然後尋找下列其中一項：

  - **MSSQLSERver** (如果您使用 WMSDE 或 MSDE，或如果您使用的是 SQL Server，而且使用實例名稱的預設實例名稱) 

  - **MSSQL $ WSUS** (如果您使用 SQL Server 資料庫，並已將您的資料庫實例 WSUS 命名) 

    以滑鼠右鍵按一下服務，然後按一下 [ **啟動** ] （如果服務並未執行），或 **重新開機以重新** 整理服務（如果服務正在執行）。