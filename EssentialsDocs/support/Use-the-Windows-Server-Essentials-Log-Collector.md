---
title: 使用 Windows Server Essentials 記錄檔收集器
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c6985518-b42d-4cfb-9761-eaa75306b6d7
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d003c6a45159548f7e34d86ca242f74868659d2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877989"
---
# <a name="use-the-windows-server-essentials-log-collector"></a>使用 Windows Server Essentials 記錄檔收集器

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

當您在疑難排解電腦問題時，從 Microsoft 客戶服務及支援服務人員可能會要求您從伺服器、 網路或兩者都使用 Windows Server Essentials 的記錄檔收集器上的電腦收集記錄檔。  
  
 記錄檔收集器會將程式記錄檔、事件檢閱者記錄檔及相關的環境資訊，複製到指定位置的單一 zip 檔案中。 您可直接從伺服器或網路上的任一部電腦執行記錄檔收集器，也可使用與電腦的遠端連線。  
  
> [!NOTE]
>  -   記錄檔收集器不會分析網路問題，或對任何伺服器或網路上的電腦進行變更。 如需有關如何疑難排解網路問題的資訊，請參閱您的伺服器產品的說明文件。  
> -   在本指南中，您的網路，而不是您的伺服器上的電腦稱為網路電腦。  
> -   [下載 Windows Server Essentials 記錄檔收集器安裝套件](https://go.microsoft.com/fwlink/?LinkID=266341)。  
  
 如果要安裝並執行記錄檔收集器，請執行下列主題中的步驟：  
  

1.  [安裝記錄檔收集器](Install-the-Windows-Server-Essentials-Log-Collector.md)  
  
2.  [執行記錄檔收集器](Run-the-Windows-Server-Essentials-Log-Collector.md)  

1.  [安裝記錄檔收集器](../support/Install-the-Windows-Server-Essentials-Log-Collector.md)  
  
2.  [執行記錄檔收集器](../support/Run-the-Windows-Server-Essentials-Log-Collector.md)  

  
## <a name="environment-information-collected"></a>收集的環境資訊  
 對於您指定的每部網路電腦或伺服器，記錄檔收集器會收集下列環境資訊，並將它放入記錄檔集合檔案。  
  
-   作業系統版本  
  
-   CPU 製造商及描述  
  
-   記憶體數量和配置  
  
-   繫結至 TCP/IP 的網路介面卡  
  
-   Locale  
  
-   處理程序  
  
-   存放裝置設定  
  
-   主機檔案資訊  
  
-   包括應用程式、系統、Windows 伺服器和 Media Center 的事件記錄檔  
  
-   服務控制管理員訊息  
  
-   重新開機事件和 Windows Update 事件  
  
-   系統錯誤和應用程式錯誤  
  
## <a name="services-information-collected"></a>收集的服務資訊  
  
### <a name="server-services"></a>伺服器服務  
  
-   Windows Server 用戶端電腦備份服務  
  
-   Windows Server 用戶端電腦備份提供者服務  
  
-   Windows Server 裝置提供者  
  
-   Windows Server 網域名稱管理  
  
-   Windows Server 服務提供者登錄  
  
-   Windows Server 設定提供者  
  
-   Windows Server UPnP 裝置服務  
  
-   Windows Server 遠端 Web 存取管理提供者  
  
-   Windows Server 健康情況服務  
  
-   Windows Server 存放裝置服務  
  
-   Windows Server SQM 服務  
  
### <a name="network-computer-services"></a>網路電腦服務  
  
-   Windows Server 用戶端電腦備份提供者服務  
  
-   Windows Server 健康情況服務  
  
-   Windows Server 服務提供者登錄  
  
-   Windows Server SQM 服務  
  
## <a name="logs-and-registry-information-collected"></a>收集的記錄檔和登錄資訊  
 對於指定的每部網路電腦或伺服器，記錄檔收集器會收集伺服器和網路電腦中的記錄檔與登錄資訊，如下所示。  
  
### <a name="server-logs-and-registry-information"></a>伺服器記錄檔和登錄資訊  
  
-   伺服器產品記錄檔，從 < ProgramData\>server\logs  
  
-   排定的工作  
  
-   安裝程式 API 記錄檔  
  
-   Windows Update 記錄檔  
  
-   健康狀態警示檔案  
  
-   裝置資訊檔案  
  
-   伺服器備份記錄檔  
  
-   夥伴記錄檔  
  
-   服務  
  
-   登錄機碼，來自於：  
  
    -   \\\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\  
  
    -   \\\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DevicesProviderSvc  
  
    -   \\\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DomainManagerProviderSvc  
  
### <a name="network-computer-logs-and-registry-information"></a>網路電腦記錄檔和登錄資訊  
  
-   網路電腦產品記錄檔，位於 < ProgramData\>server\logs  
  
-   健康狀態警示檔案，位於 < ProgramData\>\Microsoft\Windows Server\Data  
  
-   Windows Update 記錄檔  
  
-   安裝程式 API 記錄檔  
  
-   排定的工作資訊  
  
-   登錄機碼從\\\HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows Server\  
  
## <a name="logs-for-computers-that-do-not-run-a-version-of-the-windows-operating-system"></a>不是執行 Windows 作業系統版本的電腦的記錄檔  
 記錄檔收集器不會從不是執行 Windows 作業系統版本的電腦收集記錄檔 對於非 Windows 電腦，請以手動方式將下列記錄檔複製到您儲存記錄檔收集器檔案的相同位置。  
  
-   System.log  
  
-   Library/Logs/Windows Server.log  
  
-   Library/Logs/CrashReporter/LaunchPad-< nnn\> (複製所有 LaunchPad-< nnn\>.crash 檔案)  
  
-   Library/Logs/DiagnosticReports/LaunchPad-< nnn\> (複製所有 LaunchPad-< nnn\>.crash 檔案)  
  
## <a name="see-also"></a>另請參閱  
  

-   [疑難排解記錄檔收集器錯誤](Troubleshoot-Windows-Server-Essentials-Log-Collector-Errors.md)

-   [疑難排解記錄檔收集器錯誤](../support/Troubleshoot-Windows-Server-Essentials-Log-Collector-Errors.md)

