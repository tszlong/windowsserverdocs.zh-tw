---
title: 疑難排解 Windows Server Essentials 記錄檔收集器錯誤
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fa2e1685-31c0-4d4f-a10a-6c8885dfc493
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e92236c8e5d956b50f657ebcbe1a942b5841fded
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836659"
---
# <a name="troubleshoot-windows-server-essentials-log-collector-errors"></a>疑難排解 Windows Server Essentials 記錄檔收集器錯誤

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

執行記錄檔收集器時，可能會遇到下列其中一種錯誤。 如果要解決問題，請依照為相關錯誤提供的指導方針進行。  
  
> [!NOTE]
>  在本文件中，您的網路，而不是您的伺服器上的電腦稱為網路電腦。 嗎？  
  
###  <a name="BKMK_TheDestinationFolderIsNotValid"></a> 無效的目的地資料夾  
 **原因：** 您嘗試複製記錄檔的目標資料夾可能不存在，或可能沒有足夠空間可存放檔案。  
  
 **解決方案：** 確定選取的資料夾存在，且該磁碟機上有足夠的可用空間可存放該檔案。 您也應該確定該磁碟機上的臨時資料夾中有足夠的可用空間。  
  
###  <a name="BKMK_ANetworkErrorHasOccurred"></a> 發生網路錯誤  
 **原因：** 網路電腦或伺服器上可能有網路相關問題。  
  
 **解決方案：** 確定所有電腦和網路裝置的電源都已開啟，並且已連線至網路。 如果無法解決問題，請連絡維護您網路的人員以取得協助。  
  
###  <a name="BKMK_CannotCollectLogFiles"></a> 無法收集電腦的記錄檔  
 **原因：** 記錄檔收集器可能未安裝在電腦上，因為電腦並未成功使用「將電腦連線到伺服器」精靈連線到伺服器。  
  
 **解決方案：** 如需如何解決問題，連線到您的伺服器資訊，請參閱[電腦連線到伺服器的疑難排解](https://go.microsoft.com/fwlink/p/?LinkID=241492)嗎？ (https://go.microsoft.com/fwlink/p/?LinkID=241492) Microsoft 網站。  
  
 如果仍然無法將電腦連線到伺服器，您可以手動將記錄檔複製到 USB 快閃磁碟機，如下所示：  
  
-   對於執行 Windows 7、Windows 8 或 Windows Multipoint Server 的用戶端電腦，您可以複製位於 **%sysdir%\programdata\Microsoft\Windows Server** 的 [記錄檔] 資料夾。  
  
###  <a name="BKMK_YouDoNotHavePermission"></a> 您沒有將記錄檔儲存到所選資料夾的權限  
 **原因：** 您可能沒有權限寫入您選取用來儲存記錄檔的資料夾。  
  
 **解決方案：** 如果您用來儲存記錄檔的預設路徑，請確定您擁有共用資料夾的寫入權限 **\\ \\< 伺服器名稱\>\Logs**。 如果您要將記錄檔儲存到網路電腦上，請確定您有權寫入您選取用於儲存記錄檔的資料夾。  
  
###  <a name="BKMK_TheComputerIsNotConfiguredProperly"></a> 電腦未正確設定來收集記錄檔  
 **原因：** 電腦未正確設定記錄檔收集器設定  
  
 **解決方案：** 重新安裝記錄檔收集器。 請參閱 [Reinstalling the Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall)。  
  
###  <a name="BKMK_AnUnknownErrorOccurred"></a> 發生未知的錯誤  
 **原因：** 不明。  
  
 **解決方案 1:** 重新執行記錄檔收集器。 如果錯誤再次發生，請確定沒有連線問題。 您也可以嘗試重新安裝記錄檔收集器。 請參閱 [Reinstalling the Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall)。 如果無法解決問題，請連絡維護您網路的人員以取得協助。  
  
 **解決方案 2:** 首先，嘗試開啟儲存記錄檔的資料夾。 如果已經產生包含電腦名稱的 zip 檔案，請忽略此錯誤並改用該記錄檔。 如果未產生任何記錄檔，請重新執行記錄檔收集器。 如果錯誤再次發生，請確定沒有連線問題。 您也可以嘗試重新安裝記錄檔收集器。 請參閱 [Reinstalling the Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall)。 如果無法解決問題，請連絡維護您網路的人員以取得協助。