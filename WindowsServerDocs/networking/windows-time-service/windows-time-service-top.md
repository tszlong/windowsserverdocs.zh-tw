---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Windows Time 服務
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: e3dbaa188426ac81073e706db3adc6ab0a655c01
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405188"
---
# <a name="windows-time-service-w32time"></a>Windows 時間服務（W32Time）

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows 10 或更新版本

Windows 時間服務（W32Time）會同步處理在 Active Directory Domain Services （AD DS）中執行之所有電腦的日期和時間。 時間同步處理對於許多 Windows 服務和企業營運（LOB）應用程式的適當操作而言非常重要。 Windows 時間服務會使用網路時間通訊協定（NTP）來同步處理網路上的電腦時鐘。 NTP 可確保正確的時鐘值（或時間戳記）可以指派給網路驗證和資源存取要求。

在 Windows Time 服務（W32Time）主題中，可以使用下列內容：
- **[Windows Server 2016 準確的時間](accurate-time.md)。** Windows Server 2016 中的時間同步處理精確度已大幅改善，同時維持與舊版 Windows 的完整回溯 NTP 相容性。 在合理的作業條件下，您可以針對 Windows Server 2016 和 Windows 10 年度更新網域成員，維持1毫秒的精確度（相對於 UTC 或更佳）。
- **[高精確度環境的支援界限](support-boundary.md)。** 本文說明 Windows 時間服務（W32Time）在需要高度精確且穩定系統時間的環境中的支援界限。
- **設定[系統的高準確度](configuring-systems-for-high-accuracy.md)。** 已大幅改善 Windows 10 和 Windows Server 2016 中的時間同步處理。  在合理的作業狀況下，系統可以設定為維持1毫秒（毫秒）精確度或更佳（相對於 UTC）。
- **追蹤[的 Windows 時間](windows-time-for-traceability.md)。** 許多磁區中的法規都需要系統能夠追蹤到 UTC。  這表示系統的位移可以與 UTC 相對證明。  為了實現法規遵循案例，Windows 10 和伺服器2016提供新的事件記錄檔，以從作業系統的角度提供一張圖片，以構成對系統時鐘所採取之動作的瞭解。  這些事件記錄檔會持續針對 Windows 時間服務產生，並可加以檢查或封存以供日後分析。
- **[Windows 時間服務技術參考](windows-time-service-tech-ref.md)。** W32Time 服務提供電腦的網路時鐘同步處理，而不需要進行大量設定。 W32Time 服務對於 Kerberos V5 驗證的成功操作而言是不可或缺的，因此，這是為了 AD DS 型驗證。
    - **[Windows 時間服務的運作方式](How-the-Windows-Time-Service-Works.md)。** 雖然 Windows 時間服務不是網路時間通訊協定（NTP）的精確執行，但它會使用在 NTP 規格中定義的複雜演算法套件，以確保網路上電腦的時鐘盡可能精確。
    - **[Windows 時間服務工具和設定](Windows-Time-Service-Tools-and-Settings.md)。** 大部分網域成員電腦的時間用戶端類型為 NT5DS，這表示它們會同步處理網域階層的時間。 唯一的例外是，網域控制站是做為樹系根域的主域控制站（PDC）模擬器操作主機，這通常是設定為與外部時間來源同步處理時間。


## <a name="related-topics"></a>相關主題
如需有關網域階層和計分系統的詳細資訊，請參閱「[什麼是 Windows 時間服務？](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) 」 blog 文章。

Windows 時間提供者外掛程式模型已[記載于 TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx)。

您可以在[這裡](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)下載 Windows 2016 精確時間文章所參考的增補

如需 Windows Time 服務的快速總覽，請參閱此[高階總覽影片](https://aka.ms/WS2016TimeVideo)。
