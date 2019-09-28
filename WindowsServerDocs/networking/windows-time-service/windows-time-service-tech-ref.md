---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Windows 時間服務技術參考
description: W32Time 服務提供電腦的網路時鐘同步處理，而不需要進行大量設定。 W32Time 服務對於 Kerberos V5 驗證的成功操作而言是不可或缺的，因此，這是為了 AD DS 型驗證。
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: c45ac44448326ec3a236a685387b7969d21aa607
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395656"
---
# <a name="windows-time-service-technical-reference"></a>Windows 時間服務技術參考
>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows 10 或更新版本

W32Time 服務提供電腦的網路時鐘同步處理，而不需要進行大量設定。 W32Time 服務對於 Kerberos V5 驗證的成功操作而言是不可或缺的，因此，這是為了 AD DS 型驗證。 任何 Kerberos 感知應用程式（包括大部分的安全性服務）都會依賴參與驗證要求的電腦之間的時間同步處理。 AD DS 的網域控制站也必須有同步處理的時鐘，以協助確保資料複寫正確。

> [!NOTE]  
> 在 Windows Server 2003 和 Microsoft Windows 2000 Server 中，目錄服務的名稱 Active Directory 目錄服務。 在 Windows Server 2008 R2 和 Windows Server 2008 中，目錄服務的名稱是 Active Directory Domain Services （AD DS）。 本主題的其餘部分指的是 AD DS，但這項資訊也適用于 Windows Server 2016 中的 Active Directory Domain Services。

W32Time 服務是在名為 W32Time 的動態連結程式庫中執行，預設會安裝在 **%Systemroot%\System32**中。 W32Time 原本是針對 Windows 2000 伺服器所開發，以支援 Kerberos V5 驗證通訊協定的規格，這項要求必須在網路上進行同步處理。 從 Windows Server 2003 開始，W32Time 提供了在 Windows Server 2000 作業系統上，網路時鐘同步處理的準確性。 此外，在 Windows Server 2003 中，W32Time 支援使用時間提供者的各種硬體裝置和網路時間通訊協定。

雖然原先是設計來提供 Kerberos 驗證的時鐘同步處理，但許多目前的應用程式都使用時間戳來確保交易一致性、記錄重要事件的時間，以及其他業務關鍵、時間緊迫的更多資訊.  這些應用程式可受益于 Windows 時間服務所提供電腦之間的時間同步處理。

## <a name="importance-of-time-protocols"></a>時間通訊協定的重要性
時間通訊協定會在兩部電腦之間進行通訊，以交換時間資訊，然後使用該資訊來同步處理其時鐘。 使用 Windows 時間服務時間通訊協定，用戶端會向伺服器要求時間資訊，並根據收到的資訊同步處理其時鐘。
  
Windows 時間服務會使用 NTP 來協助跨網路同步處理時間。 NTP 是一種網際網路時間通訊協定，其中包含同步處理時鐘所需的專業演算法。 NTP 是比在某些 Windows 版本中使用的簡易網路時間通訊協定（SNTP）更精確的時間通訊協定;不過，W32Time 會繼續支援 SNTP，使其能夠與執行 SNTP 型時間服務（如 Windows 2000）的電腦回溯相容。
<!-- maybe this should be its own topic under the Tech Ref section -->
## <a name="where-to-find-windows-time-service-configuration-related-information"></a>尋找 Windows 時間服務設定相關資訊的位置  
本**指南不會討論如何**設定 Windows Time 服務。 在 Microsoft TechNet 和 Microsoft 知識庫中，有幾個不同的主題會說明設定 Windows 時間服務的程式。 如果您需要設定資訊，下列主題應可協助您找出適當的資訊。  
<!-- should this be an if/then table -->
-   若要設定樹系根主域控制站（PDC）模擬器的 Windows 時間服務，請參閱：  
  
    -   [在樹系根域的 PDC 模擬器上設定 Windows 時間服務](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29) 
  
    -   [設定樹系的時間來源](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794823%28v%3dws.10%29) 
  
    -   Microsoft 知識庫文章816042，[如何在 Windows server 中設定授權時間伺服器](https://go.microsoft.com/fwlink/?LinkID=60402)，其中描述執行 Windows Server 2008 R2、windows server 2008、windows server 2003 和 windows 之電腦的設定伺服器 2003 R2。  
  
-   若要在任何網域成員用戶端或伺服器，或甚至未設定為樹系根 PDC 模擬器的網域控制站上設定 Windows 時間服務，請參閱[設定用戶端電腦以進行自動網域時間同步](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29)處理。  
  
    > [!WARNING]  
    > 有些應用程式可能需要其電腦具有高準確度的時間服務。 如果是這種情況，您可以選擇設定手動時間來源，但請注意，Windows Time 服務的設計並不是以高精確度的時間來源的方式運作。 請確定您知道 Microsoft 知識庫文章939322中所述的高精確度時間環境支援限制，如為[高準確度環境設定 Windows 時間服務的支援界限](support-boundary.md)中所述。  
  
-   若要在設定為工作組成員而非網域成員的任何 Windows 型用戶端或伺服器電腦上設定 Windows 時間服務，請參閱為[選取的用戶端電腦設定手動時間來源](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816656%28v%3dws.10%29)。  
  
-   若要在執行虛擬環境的主機電腦上設定 Windows 時間服務，請參閱 Microsoft 知識庫檔816042，[如何在 Windows server 中設定授權時間伺服器](https://go.microsoft.com/fwlink/?LinkID=60402)。 如果您使用非 Microsoft 虛擬化產品，請務必參閱該產品廠商的檔。  
  
-   若要在虛擬機器中執行的網域控制站上設定 Windows 時間服務，建議您在主機系統和作為網域控制站的客體作業系統之間，部分停用時間同步處理。 這可讓您的來賓網域控制站同步處理網域階層的時間，但如果從儲存狀態還原，則會保護它免于時間偏差。 如需詳細資訊，請參閱 Microsoft 知識庫檔976924，[您可以在使用 hyper-v 和部署的 Windows Server 2008 主機伺服器上執行的虛擬網域控制站上，收到 Windows 時間服務事件識別碼24、29和 38](https://go.microsoft.com/fwlink/?LinkID=192236) 。 [虛擬網域控制站的考慮](https://go.microsoft.com/fwlink/?LinkID=192235)。  
  
-   若要在網域控制站上設定 Windows 時間服務，做為也在虛擬電腦上執行的樹系根 PDC 模擬器，請依照在[PDC 上設定 Windows 時間服務中所述的相同實體電腦指示進行操作。樹系根域中的模擬器](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29)。  
  
-   若要在執行為虛擬電腦的成員伺服器上設定 Windows 時間服務，請使用網域時間階層，如[設定自動網域時間同步處理的用戶端電腦](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29)中所述。


> [!IMPORTANT]  
> 在 Windows Server 2016 之前，W32Time 服務的設計不是為了符合時間緊迫的應用程式需求。  不過，Windows Server 2016 的更新現在可讓您在網域中執行1毫秒精確度的解決方案。  如需的詳細資訊，請參閱[windows 2016 精確時間](accurate-time.md)和[支援界限，為高準確度環境設定 windows 時間服務](support-boundary.md)以取得詳細資訊。

## <a name="related-topics"></a>相關主題
- [Windows 2016 精確時間](accurate-time.md)
- [Windows Server 2016 的時間準確度改善](windows-server-2016-improvements.md)  
- [Windows 時間服務的運作方式](How-the-Windows-Time-Service-Works.md)  
- [Windows 時間服務工具和設定](Windows-Time-Service-Tools-and-Settings.md)  
- [為高準確度環境設定 Windows 時間服務的支援界限](support-boundary.md)
- [Microsoft 知識庫文章902229](https://go.microsoft.com/fwlink/?LinkId=186066)