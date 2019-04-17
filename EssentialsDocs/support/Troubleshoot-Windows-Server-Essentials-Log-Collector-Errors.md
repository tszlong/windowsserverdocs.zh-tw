---
title: "Windows Server Essentials 登入行程錯誤的疑難排解"
description: "告訴您如何使用 Windows Server Essentials"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-windows-server-essentials-log-collector-errors"></a>Windows Server Essentials 登入行程錯誤的疑難排解

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

當您執行的登入行程時，您可能會遇到的錯誤，下列其中一個。 修正問題的相關，請依照下列指導方針提供相關的錯誤。  
  
> [!NOTE]
>  本文件中的電腦不是您的伺服器，您網路上稱為網路的電腦。嗎？  
  
###  <a name="BKMK_TheDestinationFolderIsNotValid"></a>無效的目的地資料夾  
 **原因：**您嘗試登入檔案複製資料夾不存在，或可能不到檔案的儲存空間不足，無法。  
  
 **方案：**請有選取的資料夾，而且有是空間不足可在磁碟機上的檔案。 您也應該確定空間不足剩餘磁碟機的暫存資料夾中。  
  
###  <a name="BKMK_ANetworkErrorHasOccurred"></a>網路錯誤  
 **原因：**可能有伺服器或網路的電腦上的網路相關問題。  
  
 **方案：**確保所有電腦與網路的裝置，都提供及連接到網路。 如果您無法解析問題，請連絡保留您的網路，以取得協助的人員。  
  
###  <a name="BKMK_CannotCollectLogFiles"></a>不會收集登入電腦的檔案  
 **原因：**在登入行程可能無法在電腦上安裝，因為電腦是否已成功連接到連接的電腦使用的伺服器精靈的伺服器。  
  
 **方案：**如需有關如何解析問題有關連接到您的伺服器的資訊，[伺服器連接電腦的「疑難排解」](https://go.microsoft.com/fwlink/p/?LinkID=241492)嗎？ (https://go.microsoft.com/fwlink/p/?LinkID=241492) Microsoft 網站。  
  
 如果您仍然無法伺服器連接電腦時，然後您可以登入手動複製檔案到 USB 快閃磁碟機，如下所示：  
  
-   對於 client 電腦正在執行 Windows 7、Windows 8 或 Windows 單多點伺服器，您可以複製**登**資料夾位於**%sysdir%\programdata\Microsoft\Windows 伺服器**。  
  
###  <a name="BKMK_YouDoNotHavePermission"></a>您尚未登入的檔案儲存至選取的資料夾的權限  
 **原因：**您不一定寫入權限在您選取要儲存的登入檔案的資料夾。  
  
 **方案：**如果您正在使用的預設路徑儲存登入檔案，確保您擁有的共用資料夾寫入權限**\\\ \Logs < ServerName\ >**。 如果您的網路的電腦上儲存登，確保您擁有您儲存的登入檔案所選取的資料夾寫入權限。  
  
###  <a name="BKMK_TheComputerIsNotConfiguredProperly"></a>電腦設定不正確收集登入的檔案  
 **原因：**電腦尚未設定正確登入行程。  
  
 **方案：**重新安裝「登入行程。 查看[重新安裝「登入行程](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall)。  
  
###  <a name="BKMK_AnUnknownErrorOccurred"></a>發生錯誤  
 **原因：**不明。  
  
 **方案 1:**重新執行，登入行程。 如果再次發生錯誤，請確定未連接的問題。 您也可以嘗試重新安裝「登入行程。 查看[重新安裝「登入行程](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall)。 如果您無法解析問題，請連絡保留您的網路，以取得協助的人員。  
  
 **方案 2:**第一次，請打開登入的檔案儲存的資料夾。 如果已經也 zip 檔案的電腦名稱，略過此錯誤，改為使用的登入檔案。 如果有產生不登入檔案，請重新執行，登入行程。 如果再次發生錯誤，請確定未連接的問題。 您也可以嘗試重新安裝「登入行程。 查看[重新安裝「登入行程](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall)。 如果您無法解析問題，請連絡保留您的網路，以取得協助的人員。