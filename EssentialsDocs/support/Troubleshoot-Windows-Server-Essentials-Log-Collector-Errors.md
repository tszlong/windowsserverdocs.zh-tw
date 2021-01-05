---
title: 疑難排解 Windows Server Essentials 記錄檔收集器錯誤
description: 瞭解如何針對執行記錄檔收集器時遇到的問題進行疑難排解。
ms.date: 10/03/2016
ms.topic: article
ms.assetid: fa2e1685-31c0-4d4f-a10a-6c8885dfc493
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 0da27272f3daf5159e5f6ac868788382b1bf11a7
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/29/2020
ms.locfileid: "97810223"
---
# <a name="troubleshoot-windows-server-essentials-log-collector-errors"></a>疑難排解 Windows Server Essentials 記錄檔收集器錯誤

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

執行記錄檔收集器時，可能會遇到下列其中一種錯誤。 如果要解決問題，請依照為相關錯誤提供的指導方針進行。

> [!NOTE]
> 在本檔中，您網路上的電腦（而不是伺服器）則稱為網路電腦。

###  <a name="the-destination-folder-is-not-valid"></a><a name="BKMK_TheDestinationFolderIsNotValid"></a> 目的資料夾無效
 **原因：** 您嘗試複製記錄檔的目標資料夾可能不存在，或可能沒有足夠空間可存放檔案。

 **解決方案：** 確定選取的資料夾存在，且該磁碟機上有足夠的可用空間可存放該檔案。 您也應該確定該磁碟機上的臨時資料夾中有足夠的可用空間。

###  <a name="a-network-error-has-occurred"></a><a name="BKMK_ANetworkErrorHasOccurred"></a> 發生網路錯誤
 **原因：** 網路電腦或伺服器上可能有網路相關問題。

 **解決方案：** 確定所有電腦和網路裝置的電源都已開啟，並且已連線至網路。 如果無法解決問題，請連絡維護您網路的人員以取得協助。

###  <a name="cannot-collect-log-files-for-the-computer"></a><a name="BKMK_CannotCollectLogFiles"></a> 無法收集電腦的記錄檔
 **原因：** 記錄檔收集器可能未安裝在電腦上，因為電腦並未成功使用「將電腦連線到伺服器」精靈連線到伺服器。

 **解決方案：** 如需有關如何解決連接至伺服器之問題的詳細資訊，請參閱 [疑難排解將電腦連線到伺服器](https://go.microsoft.com/fwlink/p/?LinkID=241492)。

 如果仍然無法將電腦連線到伺服器，您可以手動將記錄檔複製到 USB 快閃磁碟機，如下所示：

-   對於執行 Windows 7、Windows 8 或 Windows Multipoint Server 的用戶端電腦，您可以複製位於 **%sysdir%\programdata\Microsoft\Windows Server** 的 [記錄檔] 資料夾。

###  <a name="you-do-not-have-permission-to-save-the-log-files-to-the-selected-folder"></a><a name="BKMK_YouDoNotHavePermission"></a> 您沒有將記錄檔儲存到所選資料夾的許可權
 **原因：** 您可能沒有權限寫入您選取用來儲存記錄檔的資料夾。

 **解決方案：** 如果您使用預設路徑來儲存記錄檔，請確定您擁有共用資料夾 **\\ \\<ServerName \> \Logs** 的寫入權限。 如果您要將記錄檔儲存到網路電腦上，請確定您有權寫入您選取用於儲存記錄檔的資料夾。

###  <a name="the-computer-is-not-configured-properly-to-collect-the-log-files"></a><a name="BKMK_TheComputerIsNotConfiguredProperly"></a> 電腦未正確設定，無法收集記錄檔
 **原因：** 電腦未正確設定記錄檔收集器設定

 **解決方案：** 重新安裝記錄檔收集器。 請參閱[重新安裝記錄檔收集器](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall)。

###  <a name="an-unknown-error-occurred"></a><a name="BKMK_AnUnknownErrorOccurred"></a> 發生未知的錯誤
 **原因：** 不明。

 **解決方案 1：** 重新執行記錄檔收集器。 如果錯誤再次發生，請確定沒有連線問題。 您也可以嘗試重新安裝記錄檔收集器。 請參閱[重新安裝記錄檔收集器](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall)。 如果無法解決問題，請連絡維護您網路的人員以取得協助。

 **解決方案 2：** 首先，嘗試開啟儲存記錄檔的資料夾。 如果已經產生包含電腦名稱的 zip 檔案，請忽略此錯誤並改用該記錄檔。 如果未產生任何記錄檔，請重新執行記錄檔收集器。 如果錯誤再次發生，請確定沒有連線問題。 您也可以嘗試重新安裝記錄檔收集器。 請參閱[重新安裝記錄檔收集器](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall)。 如果無法解決問題，請連絡維護您網路的人員以取得協助。