---
title: AD 樹系復原-重新部署剩餘的 Dc
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.openlocfilehash: d3d826e0ec587289671723a4bd78d0d669189428
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93070810"
---
# <a name="ad-forest-recovery---redeploy-remaining-dcs"></a>AD 樹系復原-重新部署剩餘的 Dc

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

目前為止的步驟適用于所有樹系：尋找每個網域的有效備份、隔離網域、將它們重新連線、重設通用類別目錄，以及清除。 在下一個步驟中，您將重新部署樹系。 這樣做的方式將大幅取決於您的樹系設計、服務等級協定、網站結構、可用的頻寬，以及許多其他因素。 您必須根據本節中的原則和建議，以最適合您商務需求的方式來設計您自己的重新部署計畫。

下一步是在發生樹系復原之前的所有 Dc 上安裝 AD DS。 如果 Dc 仍然存在，則需要強制移除 AD DS 服務，或者可以重新安裝 Dc。 因為對應的中繼資料已在樹系復原期間移除，所以無法重複使用這些 Dc 的任何現有備份。 在簡單的環境中，這種重新部署程式可以像將已復原的 Dc 重新連接到生產網路一樣簡單，也可以視需要升級新的 Dc。

在面對全球基礎結構的大型企業中，需要更精密的方案。 第一個階段通常是以服務方式還原 AD;這表示安裝策略性放置的 Dc，讓所有重要的營業單位和應用程式都能再次開始運作。 分公司可能可接受，因為這樣做可能會暫時降低效能。 在第二個階段中，會重新部署所有剩餘和較不重要的 Dc。

 有兩種方法可以安裝額外的 Dc，兩者都可以自動化：

- 複製
   - 針對執行 Windows Server 2012 的虛擬化環境，複製是復原大量 Dc 的最快速且最簡單的方法。 從備份還原單一虛擬化 DC 之後，您可以自動復原網域中的所有虛擬化 dc。
   - 如需有關複製和必要條件的詳細資訊，請參閱 [Active Directory Domain Services (AD DS) 虛擬化 (層級 100) 的簡介 ](./managing-rid-issuance.md)。
- 在執行 Windows Server 2012 (的伺服器上使用 Windows PowerShell 重新安裝 AD DS，或在執行舊版 Windows Server) 或使用使用者介面的伺服器上 Dcpromo.exe
   - 若要加速重新安裝 AD DS，您可以使用 [從媒體安裝] (IFM) 選項，在安裝期間減少複寫流量。 如需使用 **ntdsutil ifm** 命令建立安裝媒體的詳細資訊，請參閱 [從媒體安裝 AD DS](./managing-rid-issuance.md)。

針對在樹系中由虛擬化 DC 複製或安裝 AD DS (（而不是從備份) 還原）復原的每個複本 DC，請考慮下列額外的點：

- DC 上用來作為複製來源的所有軟體都必須能夠複製。 您應該先移除無法複製的應用程式和服務，再開始複製。 如果無法這麼做，則應該選擇替代的虛擬化 DC 作為來源。
- 如果您從第一個要還原的虛擬化 DC 複製額外的虛擬化 dc，則在複製其 VHDX 檔案時，將需要關閉來源 DC。 然後，當複製虛擬 Dc 初次開機時，就必須在線上執行和提供。 如果第一次復原的 DC 無法接受關機所需的停機時間，請安裝 AD DS 來部署額外的虛擬化 DC，以作為複製的來源。
- 所複製之虛擬化 DC 的主機名稱或您想要安裝 AD DS 的伺服器沒有限制。 您可以使用先前使用的新主機名稱或主機名稱。 如需 DNS 主機名稱語法的詳細資訊，請參閱 () [建立 Dns 電腦名稱稱](/previous-versions/windows/it-pro/windows-server-2003/cc785282(v=ws.10)) [https://go.microsoft.com/fwlink/?LinkId=74564](https://go.microsoft.com/fwlink/?LinkId=74564) 。
- 使用樹系中的第一部 DNS 伺服器來設定每部伺服器 (根域中還原的第一個 DC，) 作為其網路介面卡的 TCP/IP 內容中的慣用 DNS 伺服器。 如需詳細資訊，請參閱 [設定 tcp/ip 以使用 DNS](/previous-versions/windows/it-pro/windows-server-2003/cc779282(v=ws.10))。
- 如果有數個 Rodc 部署在中央位置，或藉由移除並重新安裝 AD DS 的傳統方法，在地理位置（例如分公司）個別部署時，重新部署網域中的所有 Rodc （藉由虛擬化 DC 複製）。
   - 重建 Rodc 可確保其不包含任何延遲物件，並可協助防止複寫衝突在稍後發生。 當您從 RODC 移除 AD DS 時，請 *選擇保留 DC 中繼資料的選項* 。 使用這個選項會保留 RODC 的 krbtgt 帳戶，並保留委派的 RODC 系統管理員帳戶和密碼複寫原則 (PRP) 的許可權，並防止您必須使用網域系統管理員認證，才能移除並重新安裝 RODC 上的 AD DS。 如果最初安裝在 RODC 上，它也會保留 DNS 伺服器和通用類別目錄角色。
   - 當您重建 Dc (Rodc 或可寫入 Dc) 時，在重新安裝期間可能會增加複寫流量。 為了降低這項影響，您可以錯開 RODC 安裝的排程，也可以使用 [從媒體安裝] (IFM) 選項。 如果您使用 IFM 選項，請在您信任的可寫入 DC 上執行 **ntdsutil IFM** 命令，以釋放損毀的資料。 這有助於避免在 AD DS 重新安裝完成之後，在 RODC 上出現可能的損毀。 如需 IFM 的詳細資訊，請參閱 [從媒體安裝 AD DS](./managing-rid-issuance.md)。
   - 如需重建 Rodc 的詳細資訊，請參閱 [RODC 移除和重新安裝](/previous-versions/windows/it-pro/windows-server-2003/cc779282(v=ws.10))。
- 如果 DC 在樹系故障之前執行 DNS 伺服器服務，請在安裝 AD DS 期間安裝並設定 DNS 伺服器服務。 否則，請使用其他 DNS 伺服器設定其先前的 DNS 用戶端。
- 如果您需要額外的通用類別目錄來共用使用者或應用程式的驗證或查詢負載，您可以在複製之前將通用類別目錄新增至來源虛擬化 DC，也可以在安裝 AD DS 期間將 DC 設為通用類別目錄伺服器。

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原 - 先決條件](AD-Forest-Recovery-Prerequisties.md)
- [AD 樹系復原-設計自訂樹系復原方案](AD-Forest-Recovery-Devising-a-Plan.md)
- [AD 樹系修復-找出問題](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 樹系復原-決定復原方式](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 樹系復原-執行初始復原](AD-Forest-Recovery-Perform-initial-recovery.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)
- [AD 樹系復原-常見問題](AD-Forest-Recovery-FAQ.md)
- [AD 樹系復原-復原多網域樹系中的單一網域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)
- [AD 樹系復原-含 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md)
