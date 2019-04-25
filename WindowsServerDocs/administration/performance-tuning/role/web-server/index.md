---
title: 網頁伺服器的效能調整
description: Windows Server 16 上網頁伺服器的效能調整建議
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: DavSo; Ericam; YaShi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 72a7a8c2bc7e90a24e8c47c9296aa6670010cd0a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891819"
---
# <a name="performance-tuning-web-servers"></a>網頁伺服器的效能調整


此主題說明 Windows Server 2016 網頁伺服器的效能調整方法與建議。


## <a name="selecting-the-proper-hardware-for-performance"></a>選取適當的硬體以獲得效能


您必須選取適當的硬體，以滿足預期的 Web 負載，並考量平均負載、尖峰負載、容量、成長計畫和回應時間。 硬體瓶頸限會制軟體調整的效果。

[伺服器硬體的效能調整](../../hardware/index.md)提供有關硬體的建議，以避免下列效能限制：

-   緩慢的 CPU 為 CPU 密集工作負載 (例如 ASP、ASP.NET 與 TLS 案例) 提供有限的處理能力。

-   小型 L2 或 L3/LLC 處理器快取可能會對效能造成負面影響。

-   有限的記憶體數量會影響可裝載的站台數目、可儲存的動態內容指令碼 (例如 ASP.NET) 數目，以及應用程式集區或背景工作處理序數目。

-   網路會因為網路介面卡不足而成為瓶頸。

-   檔案系統會因為磁碟子系統或儲存裝置介面卡不足而成為瓶頸。

## <a name="operating-system-best-practices"></a>作業系統最佳做法


如果可能的話，請從作業系統全新安裝開始。 升級軟體會留下過時的、不必要的或效能欠佳的的登錄設定，而且先前安裝的服務與應用程式若設定為自動啟動，還會取用資源。 若已安裝另一個作業系統而且您必須保留它，您應該在不同的磁碟分割上安裝新作業系統。 否則，新安裝會覆寫 %Program Files%\\Common Files 下的設定。

為減少磁碟存取干擾，請將系統分頁檔、作業系統、Web 資料、ASP 範本快取與 Internet Information Services (IIS) 記錄放在不同的實體硬碟上 (如果可能的話)。

為減少系統資源爭用情況，請將 Microsoft SQL Server 與 IIS 安裝在不同的伺服器上 (如果可能的話)。

避免安裝非必要服務與應用程式。 在某些案例中，停用系統上不需要的服務可能有幫助。

## <a name="ntfs-file-system-settings"></a>NTFS 檔案系統設定

系統全域切換參數 **NtfsDisableLastAccessUpdate** (REG\_DWORD) 1 位於 **HKLM\\System\\CurrentControlSet\\Control\\FileSystem** 下，而且已預設為 1。 此切換參數可透過針對上次檔案或目錄存取停用日期和時間戳記更新，以減少磁碟 I/O 負載與延遲。 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 與 Windows Server 2008 的全新安裝預設會啟用此設定，因此您不需要調整它。 舊版 Windows 未設定此機碼。 若您的伺服器執行舊版 Windows，或它已升級到 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008，您應該啟用此設定。

當您使用包含數千個目錄的大型資料集 (或許多主機) 時，停用更新非常有效。 若您只針對 Web 系統管理目的維護此資訊，我們建議您改為使用 IIS　記錄。

>[!Warning]
> 某些應用程式 (例如增量備份公用程式) 仰賴此更新資訊，而且若沒有此資訊，它們將無法運作。

## <a name="see-also"></a>另請參閱
- [IIS 10.0 效能調整](tuning-iis-10.md)
- [HTTP 1.1/2 調整](http-performance.md)


