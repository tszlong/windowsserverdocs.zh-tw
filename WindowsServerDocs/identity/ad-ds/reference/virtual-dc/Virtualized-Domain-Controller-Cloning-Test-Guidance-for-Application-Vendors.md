---
ms.assetid: fde99b44-cb9f-49bf-b888-edaeabe6b88d
title: 適用於應用程式廠商的虛擬網域控制站複製測試指導方針
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0b2303bc837cdaf9f6e7ebd4b3ccbf6c66aa7ad2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879339"
---
# <a name="virtualized-domain-controller-cloning-test-guidance-for-application-vendors"></a>適用於應用程式廠商的虛擬網域控制站複製測試指導方針

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

本主題說明應用程式廠商應該考慮的事項以協助確保其應用程式會繼續如預期般虛擬的網域控制站 (DC) 複製程序完成之後運作。 該感興趣的應用程式廠商和可能需要額外測試的案例，其中會涵蓋這些層面複製程序。 已驗證他們的應用程式，適用於已複製的虛擬化的網域控制站上的應用程式廠商會建議清單中社群內容底部的 本主題中，以及連結的應用程式名稱您使用者可以了解更多關於驗證組織的網站。  
  
## <a name="overview-of-virtualized-dc-cloning"></a>虛擬化 DC 複製概觀  
虛擬化的網域控制站複製程序已詳[Active Directory 網域服務 (AD DS) 虛擬化 (等級 100) 簡介](https://technet.microsoft.com/library/hh831734.aspx)和[虛擬化網域控制站技術參考 (技術等級 300)](https://technet.microsoft.com/library/jj574214.aspx)。 從應用程式廠商的觀點來看，這些是評估您的應用程式複製的影響時，需要考量的一些考量：  
  
-   原始的電腦不會損毀。 它會保持在網路上，與用戶端互動。 不同於重新命名，其中會移除原始電腦的 DNS 記錄，保留來源網域控制站的原始記錄。  
  
-   在複製過程中，新的電腦一開始執行短暫的舊電腦的身分識別之下的時間內起始複製程序，並完成必要的變更之前。 建立主機的相關記錄的應用程式應該確定複製的電腦不會在複製程序期間複寫原始的主機記錄。  
  
-   複製是特定的部署功能對於唯一的虛擬的網域控制站，不複製其他伺服器角色的一般用途延伸模組。 特別是不支援複製某些伺服器角色：  
  
    -   動態主機設定通訊協定 (DHCP)  
  
    -   Active Directory 憑證服務 (AD CS)  
  
    -   Active Directory 輕量型目錄服務 (AD LDS)  
  
-   複製程序的一部分，會複製整個 VM，表示原始的 DC，因此也會複製該 VM 上的任何應用程式狀態。 驗證應用程式適應這項變更中複製的 DC 上的本機主機的狀態或任何介入的情況下是必要項目，例如重新啟動服務。  
  
-   在複製時，新的 DC 取得新的機器身分識別和佈建本身為複本 DC 拓樸中。 驗證是否取決於機器識別，例如其名稱、 帳戶、 SID 和等等的應用程式。 沒有它自動調整機器身分識別在複製品上的變更嗎？ 如果該應用程式會快取資料，請確定它不會依賴可能會快取的機器身分識別資料。  
  
## <a name="what-is-interesting-for-application-vendors"></a>什麼是有趣的應用程式廠商？  
  
### <a name="customdccloneallowlistxml"></a>CustomDCCloneAllowList.xml  
無法複製網域控制站執行您的應用程式或服務，直到應用程式或服務是：  
  
-   使用 Get-addccloningexcludedapplicationlist Windows PowerShell cmdlet 新增至 CustomDCCloneAllowList.xml 檔案  
  
- 或者 -  
  
-   移除網域控制站  
  
第一次使用者執行 Get-addccloningexcludedapplicationlist cmdlet，它會傳回一份服務和網域控制站上執行，但不在服務和支援複製的應用程式的預設清單中的應用程式。 根據預設，您的服務或應用程式將不會列出。 若要新增您的服務或應用程式清單的應用程式和服務可以安全地複製，使用者執行 Get-addccloningexcludedapplicationlist cmdlet 一次使用-GenerateXML 選項，以將其新增到 CustomDCCloneAllowList.xml 檔案。 如需詳細資訊，請參閱[步驟 2:執行 Get-addccloningexcludedapplicationlist cmdlet](https://technet.microsoft.com/library/hh831734.aspx#bkmk6_run_get_addccloningexcludedapplicationlist_cmdlet)。  
  
### <a name="distributed-system-interactions"></a>分散式的系統的互動  
通常是隔離到本機電腦的服務傳遞，或是參與複製時失敗。 分散式的服務不必擔心有同時在短暫的時間內的網路上的主機電腦的兩個執行個體。 這可能會顯示為嘗試提取來自夥伴系統的資訊複製為新的廠商身分識別具有註冊所在的服務執行個體。 或服務的兩個執行個體可能會發送到 AD DS 資料庫資訊同時與不同的結果。 比方說，它不具決定性的網域控制站網路上有 Windows 測試技術 (WTT) 服務的兩部電腦時，會與傳送哪一部電腦。  
  
各地的 DNS 伺服器服務中，複製程序仔細可避免複製網域控制站啟動新的 IP 位址時，覆寫的來源網域控制站的 DNS 記錄。  
  
您不應該依賴要移除所有舊的身分識別的結尾複製的電腦。 新的網域控制站升級在新的內容後，選取 Sysprep 提供者會在執行以清除其他電腦的狀態。 比方說，它是在此時會移除舊憑證的電腦，且電腦可存取的密碼編譯密碼已變更。  
  
最重要的因素而異的複製時間會有多少物件會從 PDC 複寫。 較舊的媒體會增加完整複製所需的時間。  
  
因為您的服務或應用程式不明時，它會保持執行。 複製程序不會變更非 Windows 服務的狀態。  
  
此外，新的電腦有不同的 IP 位址，與原始電腦。 這些行為可能會造成副作用，您的服務或應用程式，取決於服務或應用程式在此環境中運作的方式。  
  
## <a name="additional-scenarios-suggested-for-testing"></a>建議用於測試的其他案例  
  
### <a name="cloning-failure"></a>複製失敗  
服務供應商應該測試此案例中，因為當複製失敗時在電腦開機到目錄服務修復模式 (DSRM)，一種安全模式。 此時電腦尚未完成複製。 某些狀態可能已變更，某些狀態可能仍會從原始網域控制站。 測試這個案例，以了解它可以對您的應用程式產生的影響。  
  
若要引發的複製失敗，嘗試複製網域控制站，但不授與它要複製的權限。 在此情況下，電腦將會只變更 IP 位址，且仍有大部分的原始網域控制站的狀態。 如需有關如何授與網域控制站可以被複製的詳細資訊，請參閱[步驟 1:要複製的權限授與來源虛擬的網域控制站](https://technet.microsoft.com/library/hh831734.aspx#bkmk4_grant_source)。  
  
### <a name="pdc-emulator-cloning"></a>複製的 PDC 模擬器  
服務和應用程式的廠商應該測試這個案例，因為當 PDC 模擬器會複製會有額外的重新開機。 此外，以允許新的複製品與 PDC 模擬器互動在複製程序期間的暫時性身分識別下執行大部分的複製。  
  
### <a name="writable-versus-read-only-domain-controllers"></a>唯讀網域控制站與可寫入  
服務和應用程式的廠商應該測試使用相同類型的網域控制站複製 (也就是在可寫入或唯讀網域控制站)，規劃服務上執行。  
  


