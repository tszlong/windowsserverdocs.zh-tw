---
title: "廣告樹系修復重新部署剩餘網域控制站的機會"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 66b0bc65b3b8e5dfbf5f1a85350dab60ac3a11c8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---redeploy-remaining-dcs"></a>廣告樹系修復重新部署剩餘網域控制站的機會

>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2

 目前的步驟適用於所有的樹系︰ 有效備份尋找每個網域、 復原隔離的網域、 重新連接，重設通用，並清除。 在下一個步驟中，您將會重新部署樹系。 執行此動作的方式會大幅而定，您的樹系設計、 層級服務合約，網站結構，可用的頻寬，和許多其他因素而有所不同。 您將需要設計自己重新部署的計劃依據原則和建議在本區段中，最適合您的企業需求的方式。  
  
 安裝所有的樹系復原當時之前讀音網域控制站 AD DS 是下一個步驟。 如果網域控制站仍然存在，必須移除強制，AD DS 服務或網域控制站可以重新安裝。 無法其他任何現有的備份的這些網域控制站，因為已樹系復原期間移除對應中繼資料。 簡單的環境中此重新部署程序可以重新連接復原網域控制站 production 網路，以及視升級新的網域控制站簡單。  
  
 大型企業面臨世界各地的基礎結構，需要更複雜的計劃。 第一階段通常是還原 AD 即服務。這表示策略性安裝放網域控制站的所有重要的商業部門和應用程式可以再次開始工作。 可能可以接受分公司暫時減少根據這效能。 為第二個階段所有剩餘和較不重要網域控制站的部署。  
  
 有兩種方法可以安裝其他 Dc，會自動這兩種：  
  
-   複製  
  
     模擬環境中執行 Windows Server 2012，複製是復原大量的網域控制站的最快速、 最簡單的方式。 從備份還原單一模擬的 DC 之後，您就可以自動網域中的所有模擬 Dc 復原。  
  
     如需有關複製與必要條件的詳細資訊，請[Active Directory Domain Services (AD DS) 模擬 (層級 100) 簡介](https://technet.microsoft.com/library/hh831734.aspx)。  
  
-   使用 Windows PowerShell 伺服器上執行 Windows Server 2012 （或執行舊版 Windows Server 的伺服器上 Dcpromo.exe） 或使用的使用者介面重新安裝 AD DS  
  
     若要加速重新安裝 AD DS，您可以使用安裝媒體 (IFM) 選項，在安裝期間減少複寫流量。 如需有關使用**ntdsutil ifm**命令來建立安裝媒體，請查看[從媒體安裝 AD DS](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx)。  
  
 請考慮將模擬俠複製或安裝 （而不是從備份還原） AD DS 復原森林中每個複本俠下列其他點數︰  
  
-   在適用於複製做為來源網域控制站的所有軟體都必須複製。 應用程式與服務無法複製應該先移除車載機起始 [複製。 如果不能，要尋找替代模擬的俠應該選擇做為來源。  
  
-   如果您要複製的第一次還原模擬 DC 其他模擬的 Dc，將需要來源 DC 關機時將其 VHDX 檔案複製。 然後必須是執行，並提供 online 時複製 virtual 網域控制站的第一次開始。 如果無法接受的第一個復原俠所需的關機當機，來安裝做為來源複製到 AD DS 部署其他模擬的俠。  
  
-   還有複製模擬的俠或伺服器您想要安裝 AD DS 無限制的主機名稱。 您可以使用新的主機名稱或之前所使用的主機名稱。 如需 DNS 名稱的主機語法的詳細資訊，請查看[建立 DNS 名稱電腦](https://technet.microsoft.com/library/cc785282.aspx)([https://go.microsoft.com/fwlink/?LinkId=74564](https://go.microsoft.com/fwlink/?LinkId=74564))。  
  
-   將每個森林 (還原根網域中的第一個 DC) 中的第一個 DNS 伺服器的伺服器設定為慣用 DNS 伺服器中的 [網路介面卡 TCP/IP 屬性。 如需詳細資訊，請查看[設定 TCP/IP 使用 DNS](https://technet.microsoft.com/library/cc779282.aspx)。  
  
-   藉由模擬俠複製如果幾個 Rodc 中央位置，以部署或重新它們移除或重新安裝 AD DS，如果在隔離所在位置，例如分公司排列部署建置的傳統方法，重新網域中的所有 Rodc 都部署。  
  
     重建 Rodc 確保它們不包含任何延遲物件，而且可協助避免發生稍後複寫衝突。 當您移除 AD DS RODC，從*選擇俠中繼資料的保留*。 使用此選項的 RODC 保留 krbtgt account 保留的權限委派的 RODC 管理員和密碼複寫原則 (PRP)，並避免您要移除 RODC 上重新安裝 AD DS 使用網域管理員認證。 它也會保留的 DNS 伺服器，與通用角色如果安裝它們的 RODC 原始。  
  
     當您重新建立網域控制站 （Rodc 或寫入網域控制站），可能會增加複寫資料傳輸期間它們重新安裝。 若要協助降低的影響，您可以交錯排程 RODC 安裝，而您可以使用的安裝的媒體 (IFM) 選項。 如果您使用的 IFM 選項，請執行**ntdsutil ifm**在您信任為免費，損壞資料寫入網域控制站的命令。 這有助於避免可能損壞 AD DS 重新安裝完成後，在 RODC 出現。 如需 IFM 的詳細資訊，請查看[從媒體安裝 AD DS](https://technet.microsoft.com/library/cc770654\(WS.10\).aspx)。  
  
     如需有關重建 Rodc 的詳細資訊，請查看[RODC 移除並重新安裝](https://technet.microsoft.com/library/cc835490\(WS.10\).aspx)。  
  
-   如果 DC 已執行 DNS 伺服器服務之前樹系發生問題，請安裝並 AD DS 在安裝期間設定 DNS 伺服器服務。 否則，設定為先前的 DNS 用與其他 DNS 伺服器。  
  
-   如果您需要額外的全球目錄分享驗證或查詢載入的使用者或應用程式，您可以加入通用來源擬化俠檔案之前，請先複製或可以 AD DS 在安裝期間使 DC 通用伺服器。  
  
## <a name="next-steps"></a>後續步驟
-   [廣告樹系修復必要條件](AD-Forest-Recovery-Prerequisties.md)  
-   [廣告樹系修復-設計自訂樹系復原計劃](AD-Forest-Recovery-Devising-a-Plan.md)  
- [廣告樹系修復-找出問題](AD-Forest-Recovery-Identify-the-Problem.md)
-   [廣告樹系修復-判斷如何復原](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [廣告樹系修復-執行初始復原](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [廣告樹系修復程序](AD-Forest-Recovery-Procedures.md)  
-   [廣告樹系修復-常見問題集](AD-Forest-Recovery-FAQ.md)  
-   [廣告樹系修復-復原 Multidomain 樹系單一網域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [廣告樹系復原與 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md)  