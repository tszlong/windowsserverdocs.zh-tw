---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Windows Time 服務
author: dcuomo
ms.author: dacuo
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 5dbb0db20f7100ed7dbe99587f201f38abf632ad
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815901"
---
# <a name="windows-time-service-w32time"></a>Windows Time 服務 (W32Time)

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows 10 或更新版本

Windows Time 服務 (W32time) 會為在 Active Directory Domain Services (AD DS) 中執行的所有電腦同步其日期與時間。 若要能夠正常操作許多 Windows 服務和企業營運 (LOB) 應用程式，就必須讓時間保持同步。 Windows Time 服務會使用網路時間通訊協定 (NTP)，協助讓電腦時間與網路同步。 NTP 可確保準確的時鐘值或時間戳記，可以指派給網路驗證和資源存取要求。

Windows Time 服務 (W32Time) 主題中包含下列內容：
- **[Windows Server 2016 準確時間](accurate-time.md)。** Windows Server 2016 中的時間同步準確度已大幅改善，同時維持與舊版 Windows 的完整 NTP 回溯相容性。 在合理的作業條件下，您可以針對 Windows Server 2016 和 Windows 10 年度更新網域成員，維持 1 毫秒的準確度 (以 UTC 為準或更佳)。
- **[高準確度環境的支援界限](support-boundary.md)。** 本文說明 Windows Time 服務 (W32Time) 在需要高度準確且穩定系統時間之環境中的支援界限。
- **[設定高準確度的系統](configuring-systems-for-high-accuracy.md)。** Windows 10 和 Windows Server 2016 中的時間同步功能已大幅改善。  在合理的作業狀況下，系統可以設定為維持 1 ms (毫秒) 或更高的準確度 (以 UTC 為準)。
- **[適用於可追蹤性的 Windows 時間](windows-time-for-traceability.md)。** 在許多磁區中，都必須仰賴 UTC 對系統的追蹤來符合法規。  這表示系統的位移可在 UTC 方面得到證實。  為了支援法規遵循案例，Windows 10 和 Server 2016 提供了新的事件記錄檔，以從作業系統的角度解說對於系統時鐘所應採取的動作。  這些事件記錄檔會針對 Windows Time 服務持續產生，且您可加以查看或封存，以供日後分析之用。
- **[Windows Time 服務技術參考資料](windows-time-service-tech-ref.md)。** W32Time 服務會為電腦提供網路時鐘同步功能，而不需要進行大量設定。 W32Time 服務對於能否成功操作 Kerberos V5 驗證非常重要，因此也對能否成功操作 AD DS 型驗證非常重要。
    - **[Windows 時間設定服務的運作方式](How-the-Windows-Time-Service-Works.md)。** 雖然 Windows Time 服務不是網路時間通訊協定 (NTP) 的確切實作，但其會使用在 NTP 規格中定義的複雜演算法套件，確保網路上電腦的時鐘盡可能準確。
    - **[Windows Time 服務工具和設定](Windows-Time-Service-Tools-and-Settings.md)。** 大部分網域成員電腦的時間用戶端類型都是 NT5DS，這表示其會與網域階層的時間同步。 一般來說，關於這一點的唯一例外是作為樹系根網域主要網域控制站 (PDC) 模擬器操作主機的網域控制站，這個網域控制站通常會設定為與外部時間來源的時間同步。


## <a name="related-topics"></a>[相關主題]
如需有關網域階層和計分系統的詳細資訊，請參閱[「Windows Time 服務是什麼？」](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) 部落格文章。

Windows 時間提供者外掛程式模型已[記載於 TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx) 上。

在[這裡](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)可以下載「Windows 2016 準確時間」一文所參考的附錄

如需 Windows Time 服務的快速概觀，請參閱此[高階概觀影片](https://aka.ms/WS2016TimeVideo)。
