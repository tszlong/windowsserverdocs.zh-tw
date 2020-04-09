---
title: AD 樹系復原-重新部署剩餘的 Dc
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 17e5ceec74277c888232d17adca5c2bbb305af97
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823621"
---
# <a name="ad-forest-recovery---redeploy-remaining-dcs"></a>AD 樹系復原-重新部署剩餘的 Dc

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

到目前為止的步驟適用于所有樹系：尋找每個網域的有效備份、獨立復原網域、重新連線、重設通用類別目錄，以及清除。 在下一個步驟中，您將重新部署樹系。 執行此動作的方式很明顯取決於您的樹系設計、服務等級協定、網站結構、可用的頻寬，以及許多其他因素。 您必須根據本節中的原則和建議，以最適合您商務需求的方式來設計您自己的重新部署計畫。  
  
下一個步驟是將 AD DS 安裝在樹系復原發生之前的所有 Dc 上。 如果 Dc 仍然存在，就必須強制移除 AD DS 服務，或者可以重新安裝 Dc。 無法重複使用這些 Dc 的任何現有備份，因為對應的中繼資料已在樹系復原期間移除。 在不復雜的環境中，這個重新部署程式可以像將復原的 Dc 重新連接到生產網路一樣簡單，並視需要升級新的 Dc。  
  
在面臨全球基礎結構的大型企業中，需要更複雜的計畫。 第一個階段通常是還原 AD 即服務;這表示要安裝策略性放置的 Dc，讓所有重要的營業單位和應用程式都能再次開始工作。 因此，分公司可能會因為此而暫時降低效能。 在第二個階段中，會重新部署所有剩餘和較不重要的 Dc。  
  
 有兩種方法可安裝額外的 Dc，這兩種方式都可以自動化：  
  
- 複製  
   - 對於執行 Windows Server 2012 的虛擬化環境，複製是復原大量 Dc 的最快速且最簡單的方式。 從備份還原單一虛擬化 DC 之後，您可以自動復原網域中所有虛擬化的 dc。  
   - 如需複製和必要條件的詳細資訊，請參閱[Active Directory Domain Services （AD DS）虛擬化的簡介（層級100）](https://technet.microsoft.com/library/hh831734.aspx)。  
- 在執行 Windows Server 2012 （或執行舊版 Windows Server 的伺服器上的 Dcpromo.exe）或使用使用者介面的伺服器上，使用 Windows PowerShell 重新安裝 AD DS  
   - 若要加速重新安裝 AD DS，您可以使用 [從媒體安裝（IFM）] 選項來減少安裝期間的複寫流量。 如需使用**ntdsutil ifm**命令建立安裝媒體的詳細資訊，請參閱[從媒體安裝 AD DS](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx)。  

藉由虛擬化的 DC 複製或安裝 AD DS （而不是從備份還原），針對每個在樹系中復原的複本 DC 考慮下列額外的點：  
  
- DC 上用來作為複製來源的所有軟體都必須能夠複製。 在開始複製之前，應該先移除無法複製的應用程式和服務。 如果無法這麼做，則應該選擇替代的虛擬化 DC 做為來源。  
- 如果您從第一個要還原的虛擬化 DC 複製其他虛擬化 dc，則在複製其 VHDX 檔案時，來源 DC 將需要關閉。 然後，在第一次啟動複製虛擬 Dc 時，它就必須在執行中並可供線上使用。 如果第一個復原的 DC 無法接受關機所需的停機時間，請安裝 AD DS 作為複製的來源，以部署額外的虛擬化 DC。  
- 複製的虛擬化 DC 的主機名稱或您想要安裝 AD DS 的伺服器不會有任何限制。 您可以使用先前使用的新主機名稱或主機名稱。 如需 DNS 主機名稱語法的詳細資訊，請參閱[建立 Dns 電腦名稱稱](https://technet.microsoft.com/library/cc785282.aspx)（[https://go.microsoft.com/fwlink/?LinkId=74564](https://go.microsoft.com/fwlink/?LinkId=74564)）。  
- 使用樹系中的第一部 DNS 伺服器（在根域中還原的第一個 DC）來設定每部伺服器，做為其網路介面卡的 TCP/IP 屬性中慣用的 DNS 伺服器。 如需詳細資訊，請參閱[CONFIGURE tcp/ip to USE DNS](https://technet.microsoft.com/library/cc779282.aspx)。  
- 如果在中央位置部署了數個 Rodc，或藉由移除並重新安裝 AD DS，如果將它們個別部署在獨立的位置（例如分公司），請在網域中重新部署所有 Rodc （藉由虛擬化 DC 複製）。  
   - 重建 Rodc 可確保它們不包含任何延遲物件，並有助於防止複寫衝突之後發生。 當您從 RODC 移除 AD DS 時，請*選擇保留 DC 中繼資料的選項*。 使用此選項會保留 RODC 的 krbtgt 帳戶，並保留委派的 RODC 系統管理員帳戶和密碼複寫原則（PRP）的許可權，並防止您必須使用網域系統管理員認證來移除和重新安裝 RODC 上的 AD DS。 它也會保留 DNS 伺服器和通用類別目錄角色（如果原先是安裝在 RODC 上）。  
   - 當您重建 Dc （Rodc 或可寫入的 Dc）時，可能會在重新安裝時增加複寫流量。 為協助降低這項影響，您可以錯開 RODC 安裝的排程，而且可以使用 [從媒體安裝（IFM）] 選項。 如果您使用 IFM 選項，請在您信任的可寫入 DC 上執行**ntdsutil IFM**命令，以釋放損毀的資料。 這有助於防止在 AD DS 重新安裝完成後，可能會出現在 RODC 上的損毀。 如需 IFM 的詳細資訊，請參閱[從媒體安裝 AD DS](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx)。  
   - 如需重建 Rodc 的詳細資訊，請參閱[Rodc 移除和重新安裝](https://technet.microsoft.com/library/cc835490\(WS.10\).aspx)。  
- 如果 DC 在樹系故障前執行 DNS 伺服器服務，請在安裝 AD DS 期間安裝和設定 DNS 伺服器服務。 否則，請使用其他 DNS 伺服器設定其先前的 DNS 用戶端。  
- 如果您需要額外的通用類別目錄來共用使用者或應用程式的驗證或查詢負載，您可以在複製之前將通用類別目錄新增至來源虛擬化 DC，或在安裝 AD DS 期間，讓 DC 成為通用類別目錄伺服器。  
  
## <a name="next-steps"></a>後續步驟

- [AD 樹系復原 - 先決條件](AD-Forest-Recovery-Prerequisties.md)  
- [AD 樹系復原-設計自訂樹系復原計畫](AD-Forest-Recovery-Devising-a-Plan.md)  
- [AD 樹系復原-識別問題](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 樹系復原-決定如何復原](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 樹系復原-執行初始復原](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)  
- [AD 樹系復原-常見問題](AD-Forest-Recovery-FAQ.md)  
- [AD 樹系復原-修復多網域樹系中的單一網域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [AD 樹系復原-具有 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md)
