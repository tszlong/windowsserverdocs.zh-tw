---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: "Windows 時間服務"
description: 
author: shortpatti
ms.author: pashort
manager: brianlic
ms.date: 02/01/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c3c53db3839b5f33a87fe3789f2f7958f0bc42c2
ms.sourcegitcommit: 556361fe7c73c75d6cdc46a67dc88679fbe89c61
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/08/2018
---
# <a name="windows-time-service"></a>Windows 時間服務

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012
 
  
## <a name="w2k3tr_times_intro"></a>Windows 時間服務技術參考  
**本指南**  
  
* 如何找出 Windows 時間服務設定的資訊  
* Windows 時間服務為何？  
* 時間通訊協定的重要性  
* Windows 時間服務的運作方式   
* Windows 時間服務工具和設定  
  
> [!NOTE]  
> 在 Windows Server 2003，Microsoft Windows 2000 Server directory 服務名為 Active Directory directory 服務。 Windows Server 2008 R2 和 Windows Server 2008、directory 服務名為 Active Directory Domain Services (AD DS)。 本主題中的其餘部分是指到 AD DS，但也適用於 Windows Server 2016 中的 Active Directory Domain Services 資訊。  
  
Windows 時間服務，也就是 W32Time 同步處理的日期和時間的所有 AD DS 網域中執行的電腦。 同步處理時間是重要的許多 Windows 服務和業務的應用程式的正常運作。 Windows 時間服務使用，以便正確時鐘值或頻率，可指派給網路驗證和資源存取要求同步電腦在網路上的時鐘網路時間通訊協定 (NTP)。 服務整合 NTP 與的時間提供者，讓您的企業系統管理員可擴充時間服務。  
  
> [!IMPORTANT]  
> Windows Server 2016 之前 W32Time 服務設計時效性的應用程式需求。  不過，Windows Server 2016 的更新現在可讓您實作的 1ms 方案網域中的準確度。  查看[Windows 2016 正確時間](accurate-time.md)和[設定為高不正確的環境 Windows 時間服務支援邊界](https://go.microsoft.com/fwlink/?LinkID=179459)的詳細資訊。  
  
## <a name="BKMK_Config"></a>如何找出 Windows 時間服務設定的資訊  
本指南會**未**討論設定 Windows 時間服務。 有數個不同的主題 Microsoft TechNet 上與「Microsoft 知識庫執行解釋設定 Windows 時間服務的程序。 如果您需要設定資訊，下列主題可以協助您尋找適當的資訊。  
  
-   若要設定的樹系根主要網域控制站 (PDC) 模擬器 Windows 時間服務，查看：  
  
    -   [Windows 時間服務設定肯定森林根網域中](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29) 
  
    -   [設定樹系的時間的來源](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794823%28v%3dws.10%29) 
  
    -   Microsoft 知識庫文章 816042，[如何設定 Windows Server 授權時間伺服器](https://go.microsoft.com/fwlink/?LinkID=60402)，描述您的電腦執行的 Windows Server 2008 R2、Windows Server 2008、Windows Server 2003，設定與 Windows Server 2003 R2。  
  
-   若要在任何網域成員 client 伺服器或甚至網域控制站的不設定為 [樹系根肯定設定 Windows 時間服務，請查看[設定為自動網域時間同步 client 電腦](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29)。  
  
    > [!WARNING]  
    > 某些應用程式可能需要他們的電腦有服務高不正確的時間。 如果這是如此，您可以選擇設定手動的時間來源，但留意 Windows 時間服務設計為高準確的時間來源運作。 確定您知道的支援 Microsoft 知識庫文章 939322 中, 所述的環境高不正確的時間限制的[設定為高不正確的環境 Windows 時間服務支援邊界](https://go.microsoft.com/fwlink/?LinkID=179459)。  
  
-   若要在任何 windows client 或伺服器設定電腦群組成員，而不是網域成員看到設定 Windows 時間服務[設定手動的時間來源 client 選取的電腦的](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816656%28v%3dws.10%29)。  
  
-   若要設定 Windows 時間服務執行 virtual 環境主機電腦上，看到 Microsoft 知識庫文章 816042，[如何設定 Windows Server 授權時間伺服器](https://go.microsoft.com/fwlink/?LinkID=60402)。 如果您正在使用非 Microsoft 模擬 product，請務必連絡，該 product 廠商的文件。  
  
-   若要設定 Windows 時間服務網域控制站執行中一樣，建議您部分停用主機系統與來賓作業系統做為網域控制站之間同步處理時間。 這可讓您來賓網域控制站網域階層、進行同步處理時間，但避免發生傾斜如果還原為已儲存的狀態的時間。 如需詳細資訊，請查看 Microsoft 知識庫文章 976924，[您會收到 Windows 時間服務事件 Id 24、29，以及 38 執行的 Windows Server 2008 主機伺服器 HYPER-V 模擬的網域控制站在](https://go.microsoft.com/fwlink/?LinkID=192236)和[部署考量模擬的網域控制站的](https://go.microsoft.com/fwlink/?LinkID=192235)。  
  
-   要設定的網域控制站做為森林根肯定也 virtual 的電腦是執行 Windows 時間服務，請遵循相同的實體電腦中所述[上 PDC 設定 Windows 時間服務森林根網域中的模擬器](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29)。  
  
-   若要設定 Windows 時間服務成員伺服器執行 virtual 的電腦上，使用中所述的網域時間層 ([設定為自動網域時間同步 client 電腦](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29)。  
  
## <a name="BKMK_WTS"></a>Windows 時間服務為何？  
Windows 時間服務 (W32Time) 提供的網路時鐘同步處理的電腦，而不需要廣泛設定。  
  
Windows 時間服務務必 F:kerberos 版本 5 驗證順利運作，因此，到 AD DS 為基礎的驗證。 任何 Kerberos 感知應用程式，包括最安全的服務，依賴參與驗證要求電腦之間同步處理時間。 AD DS 網域控制站必須也同步時鐘來協助確保複寫正確的資料。  
  
Windows 時間服務係稱為 W32Time.dll 動態連結媒體櫃中。 在 [預設會安裝 W32Time.dll **%Systemroot%\System32**資料夾期間，作業系統設定和安裝。  
  
時鐘上同步處理的網路所需的 Kerberos V5 驗證通訊協定來原始 W32Time.dll 開發適用於 Windows 2000 Server 支援規格。 開始使用 Windows Server 2003，W32Time.dll 提供透過 Windows 2000 Server 作業系統的網路時鐘同步提高的正確性，此外，支援各種不同的硬體裝置和網路時間通訊協定的時間提供者使用。 雖然最初設計目的是提供時鐘同步處理的 F:kerberos 驗證，目前的許多應用程式使用時間戳記確保交易的一致性，錄製重要活動，以及其他關鍵性、即時資訊的時間。 這些應用程式可以從 Windows 時間服務提供的電腦之間同步處理時間。  
  
## <a name="BKMK_TimeProtocols"></a>時間通訊協定的重要性  
時間通訊協定進行通訊交換時間資訊和使用這些資訊同步時鐘兩部電腦之間。 Windows 時間服務時間通訊協定，以 client 要求來自伺服器的時間資訊和同步時鐘依據收到的資訊。  
  
Windows 時間服務使用 NTP 可協助您在網路上同步處理時間。 NTP 是包含訓練演算法所需的同步時鐘網際網路時間通訊協定。 NTP 是更加準確的時間通訊協定比簡單網路時間通訊協定 (SNTP) 用於某些版本的 Windows。不過，繼續支援，可讓電腦執行的時間 SNTP 為基礎的服務，例如 Windows 2000 的回溯相容性 SNTP W32Time。  
  
## <a name="see-also"></a>也了  
[Windows 時間服務的運作方式](How-the-Windows-Time-Service-Works.md)  
[Windows 時間服務工具和設定](Windows-Time-Service-Tools-and-Settings.md)  
[Microsoft 知識庫文章 902229](https://go.microsoft.com/fwlink/?LinkId=186066)