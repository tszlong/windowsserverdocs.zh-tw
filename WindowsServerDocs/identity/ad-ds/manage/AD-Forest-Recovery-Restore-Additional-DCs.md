---
title: AD 樹系復原的剩餘網域控制站重新部署
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: af9ec02a480b35e573edebcdd5928451174b6c1d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59811999"
---
# <a name="ad-forest-recovery---redeploy-remaining-dcs"></a>AD 樹系復原的剩餘網域控制站重新部署

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

截至目前為止的步驟適用於所有的樹系： 針對每個網域中尋找有效的備份、 復原中隔離的網域、 重新連接它們、 重設通用類別目錄中，並清除。 在下一個步驟中，您將會重新部署樹系。 若要這樣做的方式，可大幅取決於您的樹系設計、 服務等級協定、 網站結構、 可用頻寬和許多其他因素。 您必須設計自己的原則和建議，在此區段中，最適合您的業務需求的方式為基礎的重新部署計劃。  
  
下一個步驟是樹系復原發生之前存在的所有網域控制站上安裝 AD DS。 如果網域控制站仍然存在，必須強制，移除 AD DS 服務，或可以重新安裝網域控制站。 這些網域控制站的任何現有的備份不能重複使用，因為對應的中繼資料已移除樹系復原。 在簡單的環境中此重新部署程序可以是簡單，只要重新連線到生產環境網路，復原網域控制站，並視需要升級新的網域控制站。  
  
在大型企業中面臨的全球基礎結構，需要更複雜的計劃。 第一個階段通常是要還原的 AD 服務;這表示若要安裝策略性地放網域控制站，使所有重要的業務部門和應用程式可以再次開始工作。 可能是可接受的分公司會暫時降低效能，最後的結果。 為第二個階段中，所有剩餘和較不重要的網域控制站重新部署。  
  
 有兩種方法可以安裝其他網域控制站，這兩者都可以自動化：  
  
- 複製  
   - 對於執行 Windows Server 2012 的虛擬化環境，複製會復原大量網域控制站的最快、 最簡單的方法。 從備份還原單一的虛擬化的 DC 之後，您可以自動化網域中的所有虛擬化 Dc 的復原。  
   - 如需有關複製和必要條件的詳細資訊，請參閱 < [Active Directory 網域服務 (AD DS) 虛擬化 (等級 100) 簡介](https://technet.microsoft.com/library/hh831734.aspx)。  
- 重新安裝 AD DS，在執行 Windows Server 2012 （或在執行舊版 Windows Server 的伺服器上的 Dcpromo.exe） 的伺服器上使用 Windows PowerShell，或使用使用者介面  
   - 若要加速重新安裝 AD DS，您可以使用安裝媒體 (IFM) 選項，在安裝期間減少複寫流量。 如需使用詳細資訊**ntdsutil ifm**命令來建立安裝媒體，請參閱[從媒體安裝 AD DS](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx)。  

請考慮下列的其他點為單位的每個複本 DC 虛擬化 DC 複製或安裝 （而不是從備份還原） 的 AD DS 樹系中復原：  
  
- 做為複製來源 DC 上的所有軟體必須都能夠都被複製。 複製起始之前，應該移除應用程式和服務在無法複製。 如果不可行，替代的虛擬化的 DC 應該選擇作為來源。  
- 如果您複製要還原之第一個虛擬化 dc 的其他虛擬化的 Dc 時，來源 DC 必須關閉時其 VHDX 檔案複製。 則它必須是執行及使用線上時複製虛擬網域控制站是第一次啟動。 不接受針對第一個復原網域控制站關機所需的停機時間時，部署其他虛擬化網域控制站安裝 AD DS 做為複製的來源。  
- 複製的虛擬化的 DC 或您想要安裝 AD DS 所在伺服器的主機名稱沒有限制。 您可以使用新的主機名稱或先前已在使用中的主機名稱。 如需有關 DNS 主機名稱語法的詳細資訊，請參閱[建立 DNS 電腦名稱](https://technet.microsoft.com/library/cc785282.aspx)([https://go.microsoft.com/fwlink/?LinkId=74564](https://go.microsoft.com/fwlink/?LinkId=74564))。  
- (已還原根網域中的第一個 DC) 的樹系中的第一個 DNS 伺服器的每個伺服器設定為慣用的 DNS 伺服器，在其網路介面卡的 TCP/IP 內容中。 如需詳細資訊，請參閱 <<c0> [ 設定為使用 DNS 的 TCP/IP](https://technet.microsoft.com/library/cc779282.aspx)。  
- 重新部署在網域中的所有 Rodc，虛擬化 DC 複製如果數個 Rodc 部署在中央位置，或藉由重建它們來移除並重新安裝 AD DS，如果它們個別部署在隔離的所在位置的傳統方法例如分公司辦公室。  
   - 重建 Rodc 可確保它們不包含任何延遲物件，可協助防止複寫衝突之後發生。 當您從 RODC 移除 AD DS*選擇要保留的 DC 中繼資料選項*。 使用此選項會保留在 RODC 的 krbtgt 帳戶和保留的權限，委派的 RODC 系統管理員帳戶和密碼複寫原則 (PRP)，並防止您必須使用網域系統管理員認證來移除並重新安裝上的 AD DSRODC。 它也會保留的 DNS 伺服器和通用類別目錄角色如果原先安裝在 RODC 上。  
   - 當您重建網域控制站 （Rodc 或可寫入網域控制站），可能會增加其重新安裝期間的複寫流量。 為了降低這個影響，您可以錯開 RODC 安裝的排程，以及您可以使用從媒體安裝 (IFM) 選項。 如果您使用 IFM 選項時，執行**ntdsutil ifm**命令在您信任是免費的損毀資料可寫入網域控制站。 這有助於防止在 AD DS 重新安裝完成後，出現在 RODC 上可能已損毀。 如需 IFM 的詳細資訊，請參閱[從媒體安裝 AD DS](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx)。  
   - 如需有關如何重建 Rodc 的詳細資訊，請參閱 < [RODC 移除和重新安裝](https://technet.microsoft.com/library/cc835490\(WS.10\).aspx)。  
- 如果 DC 正在執行 DNS 伺服器服務，在樹系故障之前，安裝和設定 AD DS 安裝期間的 DNS 伺服器服務。 否則，設定其先前的 DNS 用戶端與其他 DNS 伺服器。  
- 如果您需要其他驗證] 或 [使用者或應用程式的查詢負載共用的通用類別目錄時，您可以加入至來源的通用類別目錄虛擬 DC 複製之前，或可以在 AD ds 安裝期間做出 DC 通用類別目錄伺服器。  
  
## <a name="next-steps"></a>後續步驟

- [AD 樹系復原-必要條件](AD-Forest-Recovery-Prerequisties.md)  
- [AD 樹系復原-設計自訂的樹系復原計劃](AD-Forest-Recovery-Devising-a-Plan.md)  
- [AD 樹系復原-找出問題](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 樹系復原-判斷如何復原](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 樹系復原-執行初始的復原](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [AD 樹系修復程序](AD-Forest-Recovery-Procedures.md)  
- [AD 樹系復原-常見問題集](AD-Forest-Recovery-FAQ.md)  
- [AD 樹系復原-復原 Multidomain 樹系內的單一網域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [AD 樹系復原與 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md)
