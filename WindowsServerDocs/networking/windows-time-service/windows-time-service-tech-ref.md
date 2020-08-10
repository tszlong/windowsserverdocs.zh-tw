---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Windows Time 服務技術參考資料
description: W32Time 服務會為電腦提供網路時鐘同步功能，而不需要進行大量設定。 W32Time 服務對於能否成功操作 Kerberos V5 驗證非常重要，因此也對能否成功操作 AD DS 型驗證非常重要。
author: dcuomo
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.openlocfilehash: 01dc71fb483dfa426d7c7239145391260f2534e0
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87939879"
---
# <a name="windows-time-service-technical-reference"></a>Windows Time 服務技術參考資料
>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows 10 或更新版本

W32Time 服務會為電腦提供網路時鐘同步功能，而不需要進行大量設定。 W32Time 服務對於能否成功操作 Kerberos V5 驗證非常重要，因此也對能否成功操作 AD DS 型驗證非常重要。 任何使用 Kerberos 的應用程式 (包括大部分的安全性服務) 都依賴參與驗證要求之電腦間的時間同步。 AD DS 網域控制站也必須具有同步的時鐘以協助確保精確的資料複寫。

> [!NOTE]
> 在 Windows Server 2003 和 Microsoft Windows 2000 Server 中，目錄服務稱為 Active Directory 目錄服務。 在 Windows Server 2008 R2 和 Windows Server 2008 中，目錄服務則稱為 Active Directory Domain Services (AD DS)。 本主題的其餘部分請參閱 AD DS，但這項資訊也適用於 Windows Server 2016 中的 Active Directory Domain Services。

W32Time 服務會在名為 W32Time.dll 的動態連結程式庫中實作，其預設會安裝在 **%Systemroot%\System32** 中。 W32Time 原本是為了 Windows 2000 Server 所開發的，目的是要支援 Kerberos V5 驗證通訊協定的規範，這項規範要求網路上的所有時鐘都必須保持同步。 從 Windows Server 2003 開始，W32Time.dll 便已提供比 Windows Server 2000 作業系統更高的網路時鐘同步準確度。 此外，在 Windows Server 2003 中，W32Time.dll 也已支援使用了時間提供者的各種硬體裝置和網路時間通訊協定。

雖然原先是設計來為 Kerberos 驗證提供時鐘同步功能的，但許多現行的應用程式都會使用時間戳記來確保交易一致性、記錄重要事件的時間，以及其他業務關鍵、有時效性的資訊。  這些應用程式可受益於 Windows Time 服務所提供的電腦間時間同步特性。

## <a name="importance-of-time-protocols"></a>時間通訊協定的重要性
時間通訊協定會在兩部電腦之間通訊以交換時間資訊，然後使用該資訊來同步其時鐘。 使用 Windows Time 服務時間通訊協定時，用戶端會向伺服器要求時間資訊，並根據收到的資訊來同步其時鐘。

Windows Time 服務會使用 NTP 來協助同步網路內的時間。 NTP 是一種網際網路時間通訊協定，其中包含同步時鐘所需的約束演算法。 相較於在某些 Windows 版本中使用的簡易網路時間通訊協定 (SNTP)，NTP 是更精確的時間通訊協定。不過，W32Time 會繼續支援 SNTP，以提供與執行 SNTP 型時間服務的電腦 (例如 Windows 2000) 的回溯相容性。
## <a name="where-to-find-windows-time-service-configuration-related-information"></a>可在何處找到 Windows Time 服務的設定相關資訊
本指南**不會**討論如何設定 Windows Time 服務。 在 Microsoft TechNet 和 Microsoft 知識庫中，有好幾個不同的主題會說明 Windows Time 服務的設定程序。 如果您需要設定資訊，下列主題應可協助您找到適當資訊。
-   若要為樹系根主要網域控制站 (PDC) 模擬器設定 Windows Time 服務，請參閱：

    -   [在樹系根網域中的 PDC 模擬器上設定 Windows Time 服務](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29)

    -   [設定樹系的時間來源](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794823%28v%3dws.10%29)

    -   Microsoft 知識庫文章 816042 [如何在 Windows Server 中設定授權時間伺服器](https://go.microsoft.com/fwlink/?LinkID=60402)，此文章會說明執行 Windows Server 2008 R2、Windows Server 2008、Windows Server 2003 和 Windows Server 2003 R2 的電腦所具有的組態設定。

-   若要在任何網域成員用戶端或伺服器，或甚至未設定為樹系根 PDC 模擬器的網域控制站上設定 Windows Time 服務，請參閱[設定用戶端電腦以便自動同步網域時間](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29)。

    > [!WARNING]
    > 某些應用程式可能會要求其電腦必須具有高準確度的時間服務。 如果是這種情況，您可以選擇設定手動時間來源，但請注意，Windows Time 服務的設計本意並非是為了作為高精確度的時間來源。 請確定您知道 Microsoft 知識庫文章 939322 [可為高準確度環境設定 Windows Time 服務的支援界限](support-boundary.md)中所述的高準確度時間環境支援限制。

-   若要在設定為工作組成員 (而非網域成員) 的任何 Windows 型用戶端或伺服器電腦上設定 Windows Time 服務，請參閱[為選取的用戶端電腦設定手動時間來源](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816656%28v%3dws.10%29)。

-   若要在執行虛擬環境的主機電腦上設定 Windows Time 服務，請參閱 Microsoft 知識庫文章 816042 [如何在 Windows Server 中設定授權時間伺服器](https://go.microsoft.com/fwlink/?LinkID=60402)。 如果您要使用非 Microsoft 的虛擬化產品，請務必參閱該產品的廠商文件。

-   若要在執行於虛擬機器中的網域控制站上設定 Windows Time 服務，建議您在主機系統和作為網域控制站的客體作業系統之間，局部停用時間同步功能。 這可讓客體網域控制站為該網域階層同步時間，但又不會在從「已儲存」狀態還原時產生時間誤差。 如需詳細資訊，請參閱 Microsoft 知識庫文章 976924 [您在執行於 Windows Server 2008 型主機伺服器 (含 Hyper-V) 上的虛擬網域控制站上收到 Windows Time 服務事件識別碼 24、29 和 38](https://go.microsoft.com/fwlink/?LinkID=192236) 和[虛擬網域控制站的部署考量](https://go.microsoft.com/fwlink/?LinkID=192235)。

-   若要在作為樹系根 PDC 模擬器並且也執行於虛擬電腦中的網域控制站上設定 Windows Time 服務，請遵循[在樹系根網域中的 PDC 模擬器上設定 Windows Time 服務](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29)中所述的實體電腦相同指示。

-   若要在以虛擬電腦形式執行的成員伺服器上設定 Windows Time 服務，請使用[設定用戶端電腦以便自動同步網域時間](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29)中所述的網域時間階層。


> [!IMPORTANT]
> 在 Windows Server 2016 之前，W32Time 服務並非設計來滿足有時效性應用程式的需求。  但現在，Windows Server 2016 的更新可讓您在網域中實作需要 1 毫秒準確度的解決方案。  如需詳細資訊，請參閱 [Windows 2016 的精確時間](accurate-time.md)和[可為高準確度環境設定 Windows Time 服務的支援界限](support-boundary.md)。

## <a name="related-topics"></a>相關主題
- [Windows 2016 的精確時間](accurate-time.md)
- [Windows Server 2016 的時間準確度改善](windows-server-2016-improvements.md)
- [Windows Time 服務的運作方式](How-the-Windows-Time-Service-Works.md)
- [Windows 時間服務工具和設定](Windows-Time-Service-Tools-and-Settings.md)
- [可為高準確度環境設定 Windows Time 服務的支援界限](support-boundary.md)
- [Microsoft 知識庫文章 902229](https://go.microsoft.com/fwlink/?LinkId=186066)
