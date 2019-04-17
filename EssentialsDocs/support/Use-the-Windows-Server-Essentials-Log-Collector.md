---
title: "使用 Windows Server Essentials 登入行程"
description: "告訴您如何使用 Windows Server Essentials"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="use-the-windows-server-essentials-log-collector"></a>使用 Windows Server Essentials 登入行程

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

當您的電腦問題的疑難排解時，可能會要求您收集登伺服器、 網路，或使用 Windows Server Essentials 登入行程上的電腦的代表 Microsoft 客服和支援。  
  
 登入行程複製程式登、 事件檢閱登和環境相關的資訊到檔案單一 zip 指定的位置。 網路，或使用遠端連接到電腦，您可以直接從的伺服器或任何電腦執行的登入行程。  
  
> [!NOTE]
>  -   登入行程不分析網路問題或做的任何伺服器或網路上的電腦。 如網路問題進行疑難排解的相關資訊，協助您伺服器 product 的文件。  
> -   本指南，稱為網路的電腦不是您的伺服器，您網路上的電腦。  
> -   [下載 Windows Server Essentials 登入行程安裝套件](https://go.microsoft.com/fwlink/?LinkID=266341)。  
  
 若要安裝及執行，登入行程，執行下列主題中的步驟：  
  

1.  [安裝在登入行程](Install-the-Windows-Server-Essentials-Log-Collector.md)  
  
2.  [執行，登入行程](Run-the-Windows-Server-Essentials-Log-Collector.md)  

1.  [安裝在登入行程](../support/Install-the-Windows-Server-Essentials-Log-Collector.md)  
  
2.  [執行，登入行程](../support/Run-the-Windows-Server-Essentials-Log-Collector.md)  

  
## <a name="environment-information-collected"></a>環境所收集的資訊  
 針對每個網路的電腦或指定的伺服器，登入行程會收集下列環境資訊，並將它放入登入收集檔案。  
  
-   作業系統版本  
  
-   CPU 製造商及描述  
  
-   記憶體金額與配置  
  
-   繫結至 TCP/IP 網路介面卡  
  
-   地區設定  
  
-   處理程序  
  
-   儲存空間設定  
  
-   主機檔案資訊  
  
-   事件登包括應用程式，系統、 Windows Server 及 Media Center  
  
-   服務控制項 Manager 的訊息  
  
-   開機和 Windows 更新事件  
  
-   錯誤系統和應用程式錯誤  
  
## <a name="services-information-collected"></a>服務所收集的資訊  
  
### <a name="server-services"></a>伺服器服務  
  
-   Windows Server Client 電腦備份服務  
  
-   Windows Server Client 電腦備份提供者服務  
  
-   Windows Server 的裝置提供者  
  
-   Windows Server 網域名稱管理  
  
-   Windows Server 服務提供者登錄  
  
-   Windows Server 設定提供者  
  
-   Windows Server UPnP 裝置服務  
  
-   Windows Server 遠端存取管理提供者  
  
-   Windows Server Health 服務  
  
-   Windows Server 儲存服務  
  
-   Windows Server SQM 服務  
  
### <a name="network-computer-services"></a>網路電腦服務  
  
-   Windows Server Client 電腦備份提供者服務  
  
-   Windows Server Health 服務  
  
-   Windows Server 服務提供者登錄  
  
-   Windows Server SQM 服務  
  
## <a name="logs-and-registry-information-collected"></a>登和登錄所收集的資訊  
 針對每個網路的電腦或指定的伺服器，登入行程收集登入和登錄資訊伺服器與網路的電腦，如下所示。  
  
### <a name="server-logs-and-registry-information"></a>伺服器登和登錄資訊  
  
-   伺服器 product 登，\Microsoft\Windows Server\Logs < ProgramData\ >  
  
-   排定的工作  
  
-   設定登 API  
  
-   Windows Update 登  
  
-   健康警示檔案  
  
-   裝置資訊檔案  
  
-   伺服器備份登入檔案  
  
-   豹人登入檔案  
  
-   服務  
  
-   登錄按鍵，從  
  
    -   \\\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\  
  
    -   \\\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DevicesProviderSvc  
  
    -   \\\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DomainManagerProviderSvc  
  
### <a name="network-computer-logs-and-registry-information"></a>網路的電腦登和登錄資訊  
  
-   網路電腦 product 登 < ProgramData\ > \Microsoft\Windows Server\Logs。  
  
-   健康警示檔案 < ProgramData\ > \Microsoft\Windows Server\Data。  
  
-   Windows Update 登  
  
-   設定登 API  
  
-   排程的工作資訊  
  
-   從 \\\HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows Server\ 登錄鍵  
  
## <a name="logs-for-computers-that-do-not-run-a-version-of-the-windows-operating-system"></a>登的電腦無法執行的 Windows 作業系統版本  
 登入行程不會從不執行的版本的 Windows 作業系統的電腦收集登入的檔案。 對於非 Windows 的電腦，以手動方式下列登入檔案複製到位置您收集登入將檔案儲存在同一個位置。  
  
-   System.log  
  
-   庫/登 Windows Server.log  
  
-   庫日登日 CrashReporter 日 LaunchPad-< nnn\ > （複製所有 LaunchPad-< nnn\ >.crash 檔案）  
  
-   庫日登日 DiagnosticReports 日 LaunchPad-< nnn\ > （複製所有 LaunchPad-< nnn\ >.crash 檔案）  
  
## <a name="see-also"></a>也了  
  

-   [登入行程錯誤的疑難排解](Troubleshoot-Windows-Server-Essentials-Log-Collector-Errors.md)

-   [登入行程錯誤的疑難排解](../support/Troubleshoot-Windows-Server-Essentials-Log-Collector-Errors.md)

