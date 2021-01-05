---
title: 使用 Windows Server Essentials 記錄檔收集器
description: 瞭解如何使用 Windows Server Essentials 記錄檔收集器，從伺服器、網路上的電腦或兩者收集記錄。
ms.date: 10/03/2016
ms.topic: article
ms.assetid: c6985518-b42d-4cfb-9761-eaa75306b6d7
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 4a540c52ee4cb84455f125c0d9f5c2f4ad0f3bf1
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/29/2020
ms.locfileid: "97810125"
---
# <a name="use-the-windows-server-essentials-log-collector"></a>使用 Windows Server Essentials 記錄檔收集器

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

當您疑難排解電腦問題時，Microsoft 客戶服務和支援人員可能會要求您使用 Windows Server Essentials 記錄檔收集器，從伺服器、網路上的電腦或兩者收集記錄。

 記錄檔收集器會將程式記錄檔、事件檢閱者記錄檔及相關的環境資訊，複製到指定位置的單一 zip 檔案中。 您可直接從伺服器或網路上的任一部電腦執行記錄檔收集器，也可使用與電腦的遠端連線。

> [!NOTE]
>記錄檔收集器不會分析網路問題，或對任何伺服器或網路上的電腦進行變更。 如需有關如何疑難排解網路問題的資訊，請參閱您的伺服器產品的說明文件。
>在本指南中，您的網路上的電腦（而不是您的伺服器）則稱為網路電腦。
>
>[下載 Windows Server Essentials 記錄檔收集器安裝套件](https://www.microsoft.com/download/details.aspx?id=34821)。

 如果要安裝並執行記錄檔收集器，請執行下列主題中的步驟：

1. [安裝記錄檔收集器](../support/Install-the-Windows-Server-Essentials-Log-Collector.md)

2. [執行記錄檔收集器](../support/Run-the-Windows-Server-Essentials-Log-Collector.md)


## <a name="environment-information-collected"></a>收集的環境資訊
 對於您指定的每部網路電腦或伺服器，記錄檔收集器會收集下列環境資訊，並將它放入記錄檔集合檔案。

-   作業系統版本

-   CPU 製造商及描述

-   記憶體數量和配置

-   繫結至 TCP/IP 的網路介面卡

-   地區設定

-   處理程序

-   儲存體組態

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

-   伺服器產品記錄檔，從 <ProgramData \> \Microsoft\Windows Server\Logs

-   排定的工作

-   安裝程式 API 記錄檔

-   Windows Update 記錄檔

-   健康狀態警示檔案

-   裝置資訊檔案

-   伺服器備份記錄檔

-   夥伴記錄檔

-   服務

-   登錄機碼，來自於：

    -   \\\ HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\

    -   \\\ HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DevicesProviderSvc

    -   \\\ HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DomainManagerProviderSvc

### <a name="network-computer-logs-and-registry-information"></a>網路電腦記錄檔和登錄資訊

-   網路電腦產品記錄檔，位於 <ProgramData \> \Microsoft\Windows Server\Logs

-   健康情況警示檔案位於 <ProgramData \> \Microsoft\Windows Server\Data

-   Windows Update 記錄檔

-   安裝程式 API 記錄檔

-   排定的工作資訊

-   來自 \\ \ HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows 伺服器 \ 的登錄機碼

## <a name="logs-for-computers-that-do-not-run-a-version-of-the-windows-operating-system"></a>不是執行 Windows 作業系統版本的電腦的記錄檔
 記錄檔收集器不會從不是執行 Windows 作業系統版本的電腦收集記錄檔 對於非 Windows 電腦，請以手動方式將下列記錄檔複製到您儲存記錄檔收集器檔案的相同位置。

-   System.log

-   Library/Logs/Windows Server.log

-   Library/Logs/CrashReporter/啟動控制板-<nnn \> (複製所有啟動控制板-<nnn \> . 損毀檔案) 

-   Library/Logs/DiagnosticReports/啟動控制板-<nnn \> (複製所有啟動控制板-<nnn \> . 損毀檔案) 

## <a name="see-also"></a>請參閱

-   [疑難排解記錄檔收集器錯誤](../support/Troubleshoot-Windows-Server-Essentials-Log-Collector-Errors.md)

