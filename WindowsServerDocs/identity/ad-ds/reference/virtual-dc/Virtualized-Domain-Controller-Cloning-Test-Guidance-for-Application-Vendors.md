---
ms.assetid: fde99b44-cb9f-49bf-b888-edaeabe6b88d
title: "模擬的網域控制站複製測試廠商應用程式的指導方針"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 72c4e818f82d3252c45776b26fb59e095893f2c7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="virtualized-domain-controller-cloning-test-guidance-for-application-vendors"></a>模擬的網域控制站複製測試廠商應用程式的指導方針

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題解釋廠商應用程式應該考慮協助確保您自己的應用程式仍會繼續執行如預期般模擬的網域控制站 DC 複製程序完成之後。 其涵蓋的那些層面複製程序，感興趣的應用程式廠商並可能需要額外的測試案例。 已複製有模擬的網域控制站在自己的應用程式，適用於驗證的應用程式廠商是鼓勵清單中社群內容本主題，以及所在的使用者可以深入了解驗證您的組織的網站連結底部的應用程式的名稱。  
  
## <a name="overview-of-virtualized-dc-cloning"></a>模擬俠複製概觀  
複製程序模擬的網域控制站在詳細資料中所述的方式[Active Directory Domain Services (AD DS) 模擬 (層級 100) 簡介](https://technet.microsoft.com/library/hh831734.aspx)和[擬化檔案網域控制站技術參考 (層級 300)](https://technet.microsoft.com/library/jj574214.aspx)。 應用程式廠商觀點，這些都是評估的影響複製到您的應用程式時，請考慮事項：  
  
-   原始電腦不已損壞。 它會保持在網路上，與戶端互動。 重新命名] 移除 DNS 記錄原始電腦的位置，然而來源網域控制站的原始記錄保留。  
  
-   複製過程新電腦一開始執行的時間在舊電腦的身分短暫車載機起始複製程序，而且進行所需的變更。 建立記錄主機相關的應用程式應該確保複製的電腦不會不會覆寫相關原始主機記錄複製程序期間。  
  
-   複製是只模擬的網域控制站的特定部署功能不複製其他伺服器角色通用擴充功能。 尤其是不支援部分伺服器角色複製：  
  
    -   動態主機設定通訊協定」(DHCP)  
  
    -   Active Directory 憑證 Services (AD CS)  
  
    -   Active Directory 輕量 Directory Services (AD LDS)  
  
-   複製程序的一部分，表示原本俠整個 VM 複製時，讓該 VM 上的任何應用程式狀態也複製。 驗證狀態本機主機上複製 DC，這項變更適應應用程式，或如果介入，例如服務重新開機。  
  
-   複製的一部分，新的 DC 取得新電腦的身分和條款本身為複本俠拓撲中。 驗證是否應用程式電腦的身分，如其名稱、 帳號，SID，而定。 它會自動適用於電腦的身分複製上的變更會嗎？ 如果該應用程式快取的資料，請確定它不依賴電腦可能會快取的身分資料。  
  
## <a name="what-is-interesting-for-application-vendors"></a>何謂廠商應用程式的有趣？  
  
### <a name="customdccloneallowlistxml"></a>CustomDCCloneAllowList.xml  
無法執行應用程式或服務的網域控制站複製直到應用程式或服務可能是：  
  
-   使用取得-ADDCCloningExcludedApplicationList Windows PowerShell cmdlet 來新增至 CustomDCCloneAllowList.xml 檔案  
  
-或者-  
  
-   已移除網域控制站  
  
第一次使用者執行取得-ADDCCloningExcludedApplicationList cmdlet，它會傳回服務和應用程式的網域控制站上執行，但並非預設服務和應用程式可以複製支援清單中的清單。 根據預設，您的應用程式或服務將不會列出。 若要新增到清單的應用程式與服務可以放心地的應用程式或服務複製，取得-ADDCCloningExcludedApplicationList cmdlet 再試一次-GenerateXML 選項以將它新增到 CustomDCCloneAllowList.xml 檔案使用者執行。 如需詳細資訊，請查看[步驟 2： 執行取得 ADDCCloningExcludedApplicationList cmdlet](https://technet.microsoft.com/library/hh831734.aspx#bkmk6_run_get_addccloningexcludedapplicationlist_cmdlet)。  
  
### <a name="distributed-system-interactions"></a>分散式的系統交互  
通常是在本機電腦隔離的服務可能通過或失敗時參與複製。 顧慮簡短的一段時間同時有兩個主機電腦的執行個體網路上有分散式的服務。 這可能會顯示為嘗試拉資訊，從系統的夥伴有新的身分廠商為登記完畢複製的服務執行個體。 或兩個服務可能推播資訊到 AD DS 資料庫在同一時間使用不同的結果。 例如，它不確定哪一部電腦將會進行通訊網路的網域控制站的 Windows 進行測試技術 (WTT) 服務的兩部電腦時。  
  
分散式 DNS 伺服器服務，複製程序仔細避免覆寫來源網域控制站的 DNS 記錄時複製網域控制站開始使用新的 IP 位址。  
  
您不應該依賴移除您所有的舊身分結束複製到電腦。 新的網域控制站新操作在升級之後，選取 [Sysprep 清理額外的狀態的電腦執行提供者。 例如，它是電腦的此時移除舊憑證，並變更密碼編譯密碼，可以存取電腦。  
  
有多少物件的從 PDC 複製就是最大倍不同的複製時機。 較舊的媒體增加完成複製所需的時間。  
  
您的應用程式或服務不明，因為它將會繼續執行。 複製程序不會變更非 Windows 服務的狀態。  
  
此外，新的電腦有不同與原始電腦的 IP 位址。 這些行為可能會造成副作用服務或根據服務或應用程式的處理方式此環境中的應用程式。  
  
## <a name="additional-scenarios-suggested-for-testing"></a>其他案例，建議的測試  
  
### <a name="cloning-failure"></a>複製失敗  
服務廠商應該測試此案例，因為當複製失敗電腦開機至 Directory 服務修復模式 (DSRM)，一種安全模式。 此時電腦尚未完成複製。 某些狀態可能會變更，且部分狀態可能仍會是原始的網域控制站的。 本案例，以了解哪些影響它能在您的應用程式測試。  
  
便會失敗複製，嘗試複製網域控制站複製的權限授與它不。 若是如此，電腦將會有只變更的 IP 位址，仍然可以大部分的原始的網域控制站的狀態。 如需有關複製網域控制站權限授與的詳細資訊，請查看[步驟 1： 複製的權限授與的來源模擬的網域控制站](https://technet.microsoft.com/library/hh831734.aspx#bkmk4_grant_source)。  
  
### <a name="pdc-emulator-cloning"></a>複製肯定  
還有其他重新開機時肯定複製因為廠商服務和應用程式應該測試本案例。 此外，大部分的複製身分暫時允許互動肯定複製程序期間的新複製到執行。  
  
### <a name="writable-versus-read-only-domain-controllers"></a>唯讀模式網域控制站與寫入  
服務和應用程式的供應商應測試複製使用相同的網域控制站類型 (也就是寫入或唯讀網域控制站)，才能執行預計服務。  
  


