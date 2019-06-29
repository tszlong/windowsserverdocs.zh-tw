---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Windows Time 服務
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 3233434403594ef9e2555c0329c4791d1fb99709
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469592"
---
# <a name="windows-time-service-w32time"></a>Windows Time 服務 (W32Time)

>適用於：Windows Server 2016 中，Windows Server 2012 R2，Windows Server 2012 中，Windows 10 或更新版本

Windows 時間服務 (W32Time) 同步處理的日期和時間執行 Active Directory 網域服務 (AD DS) 中的所有電腦。 時間同步處理是重要的許多 Windows 服務和的營運 (LOB) 應用程式正常運作。 Windows 時間服務會使用網路時間通訊協定 (NTP)，在網路上的電腦時鐘進行同步處理。 NTP 可確保精確的時鐘值或時間戳記，可以指派給網路驗證和資源的存取要求。

Windows Time 服務 (W32Time) 主題中的下列內容有：
- **[Windows Server 2016 正確時間](accurate-time.md)。** Windows Server 2016 中的時間同步處理精確度具有已持續改善，同時維持完整回溯 NTP 與較舊的 Windows 版本的相容性。 在合理的作業情況下，您可以維護 1 相對於 UTC 或 Windows Server 2016 和 Windows 10 年度更新版的網域成員的更佳的毫秒精確度。
- **[高精確度的環境支援界限](support-boundary.md)。** 本文說明 Windows 時間服務 (W32Time) 的支援界限需要高度精確且穩定的系統時間的環境中。
- **[設定系統的高精確度](configuring-systems-for-high-accuracy.md)。** 在 Windows 10 和 Windows Server 2016 中的時間同步處理已經過大幅改良。  在合理的運作狀況，系統可以設定為維護 1 毫秒 （毫秒） 或 （相對於 UTC) 的更佳的精確度。
- **[Windows 時間可追蹤性](windows-time-for-traceability.md)。** 在許多的磁區的法規要求系統具有可追蹤為 UTC。  這表示，相對於 UTC 證明系統的位移。  若要啟用法規合規性的情況下，Windows 10 和 Server 2016 提供新的事件記錄，讓您能夠了解系統時鐘上所採取的動作的作業系統觀點的圖片。  這些事件記錄檔的 Windows 時間服務持續產生，可檢查或備份起來以供稍後分析。
- **[Windows 時間服務技術參考](windows-time-service-tech-ref.md)。** W32Time 服務會提供網路而不需要大量的設定電腦的時鐘同步處理。 W32Time 服務是不可或缺的 Kerberos V5 驗證成功的作業，因此，AD DS 為基礎的驗證。
    - **[Windows 時間服務的運作方式](How-the-Windows-Time-Service-Works.md)。** 雖然 Windows 時間服務不是實際的實作的網路時間通訊協定 (NTP)，它會使用複雜的演算法套件，以確保在整個網路的電腦上的時鐘會盡可能精確的 NTP 規格中定義。
    - **[Windows 時間服務工具和設定](Windows-Time-Service-Tools-and-Settings.md)。** 大部分的網域成員電腦有時間用戶端類型的 NT5DS，這表示它們同步處理時間與網域階層。 這只是一般例外狀況是做為主要網域控制站 (PDC) 模擬器操作主機，樹系根網域，這通常設定為與外部時間來源同步處理時間的網域控制站。


## <a name="related-topics"></a>相關主題
如需有關計分方法與網域階層的詳細資訊，請參閱[「 什麼是 Windows Time 服務 」？](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) 部落格文章。

Windows 時間提供者外掛程式模型可以[記載於 TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx)。

您可以下載 Windows 2016 正確時間發行項所參考的增補[這裡](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)

Windows 時間服務的快速概觀，請查看本[高層級概觀影片](https://aka.ms/WS2016TimeVideo)。
