---
description: 深入瞭解：應用程式廠商的虛擬網域控制站複製測試指南
ms.assetid: fde99b44-cb9f-49bf-b888-edaeabe6b88d
title: 適用於應用程式廠商的虛擬網域控制站複製測試指導方針
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 328bef8456a5b6d4955bf03463bb4a6fecf2201e
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97045076"
---
# <a name="virtualized-domain-controller-cloning-test-guidance-for-application-vendors"></a>適用於應用程式廠商的虛擬網域控制站複製測試指導方針

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題說明當虛擬網域控制站 (DC) 複製程式完成之後，應用程式廠商應考慮的事項，以協助確保其應用程式能如預期般繼續運作。 它涵蓋了複製程式中，有興趣的應用程式廠商和案例可能需要額外測試的部分。 已驗證應用程式在已複製之虛擬網域控制站上運作的應用程式廠商，建議您在本主題底部的「社區內容」中列出應用程式的名稱，並提供您組織網站的連結，讓使用者可以深入瞭解驗證。

## <a name="overview-of-virtualized-dc-cloning"></a>虛擬化 DC 複製的總覽
[Active Directory Domain Services (AD DS) 虛擬化 (等級 100) ](../../introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100.md)和[虛擬網域控制站技術參考 (層級 300) ](../../deploy/virtual-dc/virtualized-domain-controller-technical-reference--level-300-.md)的簡介中，會詳細說明虛擬網域控制站複製程式。 從應用程式廠商的觀點來看，在評估複製到應用程式的影響時，必須考慮下列事項：

-   原始電腦未損毀。 它會保留在網路上，與用戶端互動。 不同于將原始電腦的 DNS 記錄移除的重新命名，來源網域控制站的原始記錄仍會保留。

-   在複製過程中，新的電腦一開始會在舊電腦的身分識別下執行一小段時間，直到起始複製程式並進行必要的變更為止。 建立主機相關記錄的應用程式應該確保複製的電腦在複製過程中不會覆寫原始主機的相關記錄。

-   複製是特定的部署功能，僅適用于虛擬網域控制站，而不是用來複製其他伺服器角色的一般用途延伸模組。 某些伺服器角色特別不支援進行複製：

    -   動態主機設定通訊協定 (DHCP)

    -   Active Directory 憑證服務 (AD CS)

    -   Active Directory 輕量型目錄服務 (AD LDS)

-   在複製過程中，會複製代表原始 DC 的整個 VM，因此也會複製該 VM 上的任何應用程式狀態。 驗證應用程式是否會在複製的 DC 上以本機主機的狀態進行調整，或者是否需要任何介入，例如服務重新開機。

-   在複製過程中，新的 DC 會取得新的電腦身分識別，並將其本身布建為拓撲中的複本 DC。 驗證應用程式是否相依于電腦身分識別，例如其名稱、帳戶、SID 等等。 它會自動調整複製上電腦身分識別的變更嗎？ 如果該應用程式會快取資料，請確定它不依賴可能快取的電腦身分識別資料。

## <a name="what-is-interesting-for-application-vendors"></a>應用程式廠商有哪些有趣之處？

### <a name="customdccloneallowlistxml"></a>CustomDCCloneAllowList.xml
無法複製執行應用程式或服務的網域控制站，直到應用程式或服務可能是：

-   使用 Get-ADDCCloningExcludedApplicationList Windows PowerShell Cmdlet 新增至 CustomDCCloneAllowList.xml 檔案

-或者-

-   已從網域控制站移除

使用者第一次執行 Get-ADDCCloningExcludedApplicationList Cmdlet 時，會傳回在網域控制站上執行的服務和應用程式清單，但不在支援複製的預設服務和應用程式清單中。 根據預設，將不會列出您的服務或應用程式。 若要將您的服務或應用程式新增至可安全複製的應用程式和服務清單，使用者會再次使用-GenerateXML 選項執行 Get-ADDCCloningExcludedApplicationList Cmdlet，以便將它新增至 CustomDCCloneAllowList.xml 檔案。 如需詳細資訊，請參閱 [步驟2：執行 Get-ADDCCloningExcludedApplicationList Cmdlet](/powershell/module/addsadministration/get-addccloningexcludedapplicationlist)。

### <a name="distributed-system-interactions"></a>分散式系統互動
在參與複製時，通常會在本機電腦上隔離或失敗的服務。 分散式服務必須在意網路上的兩個主機電腦實例一小段時間。 這可能會以服務實例的形式出現，該實例會嘗試從夥伴系統提取資訊，其中複製已註冊為身分識別的新廠商。 或服務的兩個實例可能會以不同的結果同時將資訊推送至 AD DS 資料庫。 例如，當兩部具有 Windows 測試技術 (WTT) service 位於網路上具有網域控制站的電腦時，就不具決定性。

若為分散式 DNS 伺服器服務，複製程式會在複製網域控制站以新的 IP 位址啟動時，謹慎地避免覆寫來源網域控制站的 DNS 記錄。

在複製結束之前，您不應該依賴電腦移除所有舊的身分識別。 在新的內容中升級新的網域控制站之後，請選取 [Sysprep 提供者]，以清除電腦的其他狀態。 例如，此時會移除電腦的舊憑證，並變更電腦可以存取的密碼編譯密碼。

改變複製時間的最大因素就是要從 PDC 複寫多少物件。 較舊的媒體會增加完成複製所需的時間。

因為您的服務或應用程式未知，所以它會保持執行狀態。 複製進程不會變更非 Windows 服務的狀態。

此外，新的電腦與原始電腦具有不同的 IP 位址。 這些行為可能會對您的服務或應用程式造成副作用，這取決於服務或應用程式在此環境中的運作方式。

## <a name="additional-scenarios-suggested-for-testing"></a>建議用於測試的其他案例

### <a name="cloning-failure"></a>複製失敗
服務廠商應該測試此案例，因為當複製失敗時，電腦會以安全模式的形式 (DSRM) 來開機進入目錄服務修復模式。 此時電腦尚未完成複製。 某些狀態可能已變更，而某些狀態可能會保留在原始網域控制站中。 測試此案例，以瞭解它對您的應用程式有何影響。

若要引發複製失敗，請嘗試複製網域控制站，而不將複製的許可權授與它。 在此情況下，電腦只會變更 IP 位址，而且仍會有來自原始網域控制站的大部分狀態。 如需授與網域控制站許可權以進行複製的詳細資訊，請參閱 [步驟1：將複製的許可權授與來源虛擬網域控制站](../../get-started/virtual-dc/virtualized-domain-controller-deployment-and-configuration.md)。

### <a name="pdc-emulator-cloning"></a>PDC 模擬器複製
服務和應用程式廠商應該測試此案例，因為在複製 PDC 模擬器時，會有額外的重新開機。 此外，大部分的複製作業都是在暫時的身分識別下執行，以允許新的複製品在複製程式期間與 PDC 模擬器互動。

### <a name="writable-versus-read-only-domain-controllers"></a>可寫入與唯讀網域控制站
服務和應用程式廠商應該使用相同類型的網域控制站來測試複製 (也就是在可寫入或唯讀的網域控制站上，) 該服務已規劃要在其上執行。
