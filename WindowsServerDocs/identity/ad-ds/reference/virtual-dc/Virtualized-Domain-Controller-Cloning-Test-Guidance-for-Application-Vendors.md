---
ms.assetid: fde99b44-cb9f-49bf-b888-edaeabe6b88d
title: 適用於應用程式廠商的虛擬網域控制站複製測試指導方針
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 2823e761ead46dc5180c540ce6faef8ef604cd18
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86955080"
---
# <a name="virtualized-domain-controller-cloning-test-guidance-for-application-vendors"></a>適用於應用程式廠商的虛擬網域控制站複製測試指導方針

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題說明在虛擬網域控制站（DC）複製程式完成之後，應用程式廠商應考慮哪些事項，以協助確保其應用程式繼續如預期般運作。 其中涵蓋了複製程式的相關層面，也就是可能需要額外測試的應用程式廠商和案例。 已驗證其應用程式是否可在已複製的虛擬網域控制站上運作的應用程式廠商，建議您在本主題底部的 [社區內容] 中列出應用程式的名稱，並附上您組織網站的連結，使用者可以在此深入瞭解驗證。

## <a name="overview-of-virtualized-dc-cloning"></a>虛擬化 DC 複製的總覽
[Active Directory Domain Services （AD DS）虛擬化（等級100）](../../introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100.md)和[虛擬網域控制站技術參考（等級300）](../../deploy/virtual-dc/virtualized-domain-controller-technical-reference--level-300-.md)的簡介中會詳細說明虛擬網域控制站複製程式。 從應用程式廠商的觀點來看，在評估複製到應用程式的影響時，需要考慮下列事項：

-   原始電腦未損毀。 它會保留在網路上，並與用戶端互動。 不像重新命名會移除原始電腦的 DNS 記錄，來源網域控制站的原始記錄仍會保留。

-   在複製過程中，新的電腦一開始會在舊電腦的識別下短暫執行一段時間，直到複製程式起始並進行必要的變更為止。 建立主機相關記錄的應用程式應確保複製的電腦不會覆寫複製程式期間的原始主機相關記錄。

-   複製是僅適用于虛擬網域控制站的特定部署功能，不是用來複製其他伺服器角色的一般用途延伸模組。 某些伺服器角色特別不支援複製：

    -   動態主機設定通訊協定 (DHCP)

    -   Active Directory 憑證服務 (AD CS)

    -   Active Directory 輕量型目錄服務 (AD LDS)

-   作為複製程式的一部分，會複製代表原始 DC 的整個 VM，因此也會複製該 VM 上的任何應用程式狀態。 驗證應用程式是否可在複製的 DC 上以本機主機的狀態調整此變更，或如果需要介入，例如服務重新開機。

-   在複製過程中，新的 DC 會取得新的機器身分識別，並將其本身布建為拓撲中的複本 DC。 驗證應用程式是否相依于電腦身分識別，例如其名稱、帳戶、SID 等等。 它是否會自動適應複製上電腦身分識別的變更？ 如果該應用程式快取資料，請確定它不依賴可能快取的機器識別資料。

## <a name="what-is-interesting-for-application-vendors"></a>應用程式廠商有什麼意義？

### <a name="customdccloneallowlistxml"></a>CustomDCCloneAllowList.xml
執行應用程式或服務的網域控制站，必須等到應用程式或服務可以進行複製：

-   使用 Get-addccloningexcludedapplicationlist Windows PowerShell Cmdlet 新增至 CustomDCCloneAllowList.xml 檔案

-或-

-   已從網域控制站移除

使用者第一次執行 Get-addccloningexcludedapplicationlist 指令程式時，會傳回在網域控制站上執行但不在支援複製之服務和應用程式預設清單中的服務和應用程式清單。 根據預設，您的服務或應用程式將不會列出。 若要將您的服務或應用程式新增至可安全複製的應用程式和服務清單中，使用者可以使用-GenerateXML 選項再次執行 Get-addccloningexcludedapplicationlist Cmdlet，以將它新增至 CustomDCCloneAllowList.xml 檔案。 如需詳細資訊，請參閱[步驟2：執行 get-addccloningexcludedapplicationlist Cmdlet](/powershell/module/addsadministration/get-addccloningexcludedapplicationlist)。

### <a name="distributed-system-interactions"></a>分散式系統互動
通常在參與複製時，與本機電腦隔離的服務可能會通過或失敗。 分散式服務必須在意網路上的兩個主機電腦實例一小段時間。 這可能會以服務實例的形式，嘗試從複製已註冊為身分識別新廠商的夥伴系統提取資訊。 或者，服務的兩個實例可能會以不同的結果同時將資訊推送至 AD DS 資料庫。 例如，當具有 Windows 測試技術（WTT）服務的兩部電腦位於具有網域控制站的網路上時，不具決定性的電腦會與之通訊。

對於分散式 DNS 伺服器服務，複製程式會在複製網域控制站以新的 IP 位址啟動時，謹慎地避免覆寫來源網域控制站的 DNS 記錄。

您不應該依賴電腦移除所有舊的身分識別，直到複製結束為止。 在新的內容中升級新的網域控制站之後，請選取 [Sysprep 提供者]，以清除電腦的其他狀態。 例如，此時會移除電腦的舊憑證，而且電腦可以存取的密碼編譯密碼也會變更。

改變複製時間的最大因素，就是要從 PDC 複寫多少物件。 較舊的媒體會增加完成複製所需的時間。

因為您的服務或應用程式不明，所以它會保持執行狀態。 複製程式並不會變更非 Windows 服務的狀態。

此外，新的電腦與原始電腦的 IP 位址不同。 這些行為可能會對您的服務或應用程式造成副作用，這取決於服務或應用程式在此環境中的行為。

## <a name="additional-scenarios-suggested-for-testing"></a>建議進行測試的其他案例

### <a name="cloning-failure"></a>複製失敗
服務廠商應測試此案例，因為當複製失敗時，電腦會開機進入目錄服務修復模式（DSRM），這是一種安全模式的形式。 此時電腦尚未完成複製。 某些狀態可能已變更，而且某些狀態可能會保留在原始網域控制站中。 測試此案例，以瞭解它對您的應用程式可能造成的影響。

若要引發複製失敗，請嘗試複製網域控制站，而不要授與它要複製的許可權。 在此情況下，電腦只會變更 IP 位址，而且仍然擁有來自原始網域控制站的大部分狀態。 如需有關授與要複製之網域控制站許可權的詳細資訊，請參閱[步驟1：授與來源虛擬網域控制站要複製的許可權](../../get-started/virtual-dc/virtualized-domain-controller-deployment-and-configuration.md)。

### <a name="pdc-emulator-cloning"></a>PDC 模擬器複製
服務和應用程式廠商應測試此案例，因為在複製 PDC 模擬器時，會有額外的重新開機。 此外，大部分的複製都是在暫時的身分識別下執行，以允許新的複本在複製過程中與 PDC 模擬器互動。

### <a name="writable-versus-read-only-domain-controllers"></a>可寫入與唯讀網域控制站
服務和應用程式廠商應使用已計畫在其上執行服務的相同類型網域控制站（也就是在可寫入或唯讀網域控制站上）來測試複製。
