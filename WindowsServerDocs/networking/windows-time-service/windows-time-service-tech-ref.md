---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Windows 時間服務技術參考
description: W32Time 服務會提供網路而不需要大量的設定電腦的時鐘同步處理。 W32Time 服務是不可或缺的 Kerberos V5 驗證成功的作業，因此，AD DS 為基礎的驗證。
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 904a53797d22d2e06e0cd2f7572b99fc386dae16
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845119"
---
# <a name="windows-time-service-technical-reference"></a>Windows 時間服務技術參考
>適用於：Windows Server 2016 中，Windows Server 2012 R2，Windows Server 2012 中，Windows 10 或更新版本

W32Time 服務會提供網路而不需要大量的設定電腦的時鐘同步處理。 W32Time 服務是不可或缺的 Kerberos V5 驗證成功的作業，因此，AD DS 為基礎的驗證。 任何 Kerberos 感知的應用程式，包括大部分的安全性服務，都依賴參與驗證要求的電腦之間的時間同步處理。 AD DS 網域控制站必須也具有同步的時鐘以協助確保精確的資料複寫。

> [!NOTE]  
> 在 Windows Server 2003 和 Microsoft Windows 2000 Server 中，目錄服務稱為 Active Directory 目錄服務。 在 Windows Server 2008 R2 和 Windows Server 2008 中，目錄服務稱為 Active Directory 網域服務 (AD DS)。 本主題的其餘部分指的是 AD DS 中，但資訊也適用於 Windows Server 2016 中的 Active Directory 網域服務。

W32Time 服務實作中呼叫 W32Time.dll，預設會在已安裝的動態連結程式庫 **%Systemroot%\System32**。 W32Time.dll 原本是所開發以支援一種規格的 Windows 2000 Server 的 Kerberos V5 驗證通訊協定需要在同步處理網路上的時鐘。 從 Windows Server 2003 開始，W32Time.dll 可以提供更高的精確度，網路時鐘同步處理對 Windows Server 2000 作業系統。 此外，在 Windows Server 2003，W32Time.dll 支援各種不同的硬體裝置及使用時間提供者的網路時間通訊協定。

雖然原本設計提供的 Kerberos 驗證的時鐘同步處理時，許多目前的應用程式就會使用時間戳記來確保交易一致性，記錄的時間的重要事件，和其他重要的企業，時間緊迫資訊。  這些應用程式受益於 Windows 時間服務所提供的電腦之間的時間同步處理。

## <a name="importance-of-time-protocols"></a>時間通訊協定的重要性
交換時間資訊，並接著使用該資訊來同步處理其時鐘的兩部電腦之間的通訊時間通訊協定。 與 Windows 時間服務的時間通訊協定，用戶端會要求來自伺服器的時間資訊，並同步處理其時鐘根據收到的資訊。
  
Windows 時間服務會使用 NTP 來協助在網路上同步處理時間。 NTP 是網際網路時間通訊協定，其中包含所需的同步處理時鐘的專業領域演算法。 NTP 是更精確的時間通訊協定比簡單網路時間通訊協定 (SNTP)，可在部分版本的 Windows;不過，W32Time 會繼續支援 SNTP 啟用回溯相容性的執行 SNTP 為基礎的時間服務，例如 Windows 2000 的電腦。
<!-- maybe this should be its own topic under the Tech Ref section -->
## <a name="where-to-find-windows-time-service-configuration-related-information"></a>哪裡可以找到 Windows 時間服務與設定相關資訊  
本指南並未**不**討論設定 Windows 時間服務。 有許多不同的主題在 Microsoft TechNet 上及 Microsoft 知識庫中進行說明設定 Windows 時間服務的程序。 如果您需要設定資訊時，下列主題應該協助您找出適當的資訊。  
<!-- should this be an if/then table -->
-   若要設定樹系根網域主控站 (PDC) 模擬器的 Windows 時間服務，請參閱：  
  
    -   [在樹系根網域中的 PDC 模擬器上設定 Windows 時間服務](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29) 
  
    -   [設定樹系的時間來源](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794823%28v%3dws.10%29) 
  
    -   Microsoft 知識庫文件 816042，[如何在 Windows Server 中設定授權時間伺服器](https://go.microsoft.com/fwlink/?LinkID=60402)，其中說明執行 Windows Server 2008 R2，Windows Server 2008，Windows Server 之電腦的組態設定2003 和 Windows Server 2003 R2。  
  
-   若要在任何網域成員用戶端或伺服器或未設定為樹系根 PDC 模擬器的偶數的網域控制站上設定 Windows 時間服務，請參閱[設定用戶端電腦自動網域時間同步處理](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).  
  
    > [!WARNING]  
    > 有些應用程式可能需要他們的電腦來擁有高精確度的時間服務。 如果是這樣，您可以選擇設定手動時間來源，但請注意，Windows 時間服務的設計無法做為高度精確的時間來源。 確保您知道的支援限制的高精確度的 time 環境如所述的 Microsoft 知識庫文章 939322，[支援界限，以設定 Windows 時間服務的高精確度環境](support-boundary.md).  
  
-   若要設定 Windows 時間服務以 Windows 為基礎的用戶端或伺服器的任何電腦上設定工作群組成員，而不是網域成員看到[設定所選的用戶端電腦手動時間來源](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816656%28v%3dws.10%29)。  
  
-   若要設定 Windows 時間服務主機電腦上執行虛擬環境，請參閱 Microsoft 知識庫文件 816042，[如何在 Windows Server 中設定授權時間伺服器](https://go.microsoft.com/fwlink/?LinkID=60402)。 如果您正在使用的非 Microsoft 虛擬化產品，請務必參閱該產品的供應商的文件。  
  
-   若要設定 Windows 時間服務正在執行的虛擬機器中的網域控制站上，建議您部分停用主機系統與客體作業系統做為網域控制站之間的時間同步處理。 這可讓客體網域控制站的網域階層中，同步處理時間，但防止發生時間扭曲如果從已儲存狀態還原。 如需詳細資訊，請參閱 Microsoft 知識庫文章 976924，[接收 Windows 時間服務事件識別碼 24、 29 和 38 之間執行 HYPER-V 的 Windows Server 2008 主機伺服器的虛擬的網域控制站上](https://go.microsoft.com/fwlink/?LinkID=192236)並[虛擬化的網域控制站的部署考量](https://go.microsoft.com/fwlink/?LinkID=192235)。  
  
-   若要設定 Windows 時間服務做為樹系根 PDC 模擬器中的虛擬電腦同時執行的網域控制站上，遵循相同的實體電腦中所述[上設定 Windows 時間服務樹系根網域中的 PDC 模擬器](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29)。  
  
-   若要設定 Windows 時間服務與虛擬電腦執行的成員伺服器上，使用網域的時間階層中所述[設定為自動網域時間同步處理用戶端電腦](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29)。


> [!IMPORTANT]  
> 在 Windows Server 2016 之前, W32Time 服務並非設計時間緊迫的應用程式的需求。  不過，Windows Server 2016 的更新現在可讓您實作的解決方案 1 毫秒精確度您網域中的。  如需詳細資訊，請參閱 < [Windows 2016 正確時間](accurate-time.md)並[來設定 Windows 時間服務的高精確度的環境支援界限](support-boundary.md)如需詳細資訊。

## <a name="related-topics"></a>相關主題
- [Windows 2016 正確時間](accurate-time.md)
- [適用於 Windows Server 2016 的時間精確度改善](windows-server-2016-improvements.md)  
- [Windows 時間服務的運作方式](How-the-Windows-Time-Service-Works.md)  
- [Windows 時間服務工具和設定](Windows-Time-Service-Tools-and-Settings.md)  
- [若要設定 Windows 時間服務的高精確度的環境支援界限](support-boundary.md)
- [Microsoft 知識庫文章 902229](https://go.microsoft.com/fwlink/?LinkId=186066)