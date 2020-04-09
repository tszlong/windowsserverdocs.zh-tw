---
title: AD 樹系復原 - 常見問題
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: ac9e5a3d-8b1e-41b7-8e02-f64b7acf1359
ms.technology: identity-adds
ms.openlocfilehash: f32111cf7cc81f8f49b7b1058cc1a0ccc780da7f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824001"
---
# <a name="ad-forest-recovery---faq"></a>AD 樹系復原 - 常見問題

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2、Windows Server 2003

本檔包含關於樹系復原的常見問題（Faq）：  

## <a name="general-recovery"></a>一般復原

**問：我可以做什麼來加速復原？**

雖然復原的速度不是本指南的主要目標，但是您可以藉由下列方式縮短復原時間：  
  
- 建立詳細的樹系復原計畫、定期更新，並在至少一年的合理大小的模擬測試環境中進行練習  
- 使用虛擬網域控制站（DC）複製  
   - 虛擬化的 DC 複製可加速從每個網域中的備份還原一個 DC 之後，取得額外 Dc 的程式。 可以複製額外的虛擬 Dc，而不是等待可能冗長的 AD DS 安裝完成，並在安裝後完成非關鍵性複寫。  
   - 虛擬 Dc 裝載于相對較少的連線資料中心的樹系，在復原期間可能會有最大的好處。 不過，相同網域的多個虛擬化 Dc 會共置於同一個虛擬機器主機上的任何環境，都應該受益。  
- 部署唯讀網域控制站（Rodc）  
   - Rodc 可以在復原過程中供應商務持續性，因為它們不需要與網路中斷連線，就像可寫入的 Dc 一樣。 Rodc 不會執行輸出複寫。 因此，它們不會帶來與可寫入的 Dc 用來將損毀資料複寫回復原環境相同的風險。  
  
影響樹系復原程式持續時間的其他因素包括下列各項：  
  
- 當您從備份還原 Dc 時，需要一些時間來執行下列動作：  
   - 找出實體備份媒體，例如 [磁帶]。  
   - 重新安裝作業系統。  
   - 從備份媒體還原資料。  
      - 您可以藉由執行完整伺服器復原，而不是系統狀態還原，減少重新安裝作業系統和從備份還原資料所需的時間。 由於完整伺服器復原是以二進位為基礎，因此其完成速度會比系統狀態還原快很多。  
      - 不過，如果伺服器包含從您不想要還原的系統狀態資料中排除的資料，則完整伺服器復原可能不是系統狀態還原的可行替代方案。 請考慮執行完整伺服器復原的優點，而不是明確地針對您的伺服器進行系統狀態還原，並藉由執行您打算稍後還原的適當備份類型來準備。  
- 當您重建 Dc 時，需要時間複寫資料以進行以網路為基礎的升級。  
   - 您可以執行下列步驟來減少還原 Dc 所需的時間：  
- 藉由下列方式減少抓取備份媒體的時間：  
   - 使用 Active Directory 資料庫掛接工具（Dsamain.exe）來識別要用於還原作業的最佳備份。 如需有關使用 Active Directory 資料庫掛接工具的詳細資訊，請參閱[Active Directory 資料庫掛接工具逐步指南](https://go.microsoft.com/fwlink/?LinkId=132577)（ https://go.microsoft.com/fwlink/?LinkId=132577)。  
   - 清楚地標示備份媒體，並以組織的方式將媒體儲存在方便但安全的位置，以允許快速抓取。  
   - 使用磁碟區陰影複製服務搭配存放區域網路（SAN）來維護不同時間點的備份。 如需詳細資訊，請參閱[Windows Server 2003 Active Directory 使用磁碟區陰影複製服務和虛擬磁碟服務的快速](https://go.microsoft.com/fwlink/?LinkId=70781)復原（ https://go.microsoft.com/fwlink/?LinkId=70781)。  
- 強制從 Dc 移除 AD DS，而不是重新安裝作業系統。 如果整個樹系失敗的原因已被識別為純粹在 AD DS 範圍內，您就不需要在 Dc 上重新安裝作業系統。  
   - 如需強制從執行 Windows Server 2008 或更新版本的 DC 移除 AD DS 的詳細資訊，請參閱[強制移除 Windows server 2008 網域控制站](https://go.microsoft.com/fwlink/?LinkId=132627)（ https://go.microsoft.com/fwlink/?LinkId=132627)。 如需強制從執行 Windows Server 2003 的 DC 移除 AD DS 的詳細資訊，請參閱 Microsoft 知識庫中的[文章 332199](https://go.microsoft.com/fwlink/?LinkId=70780) （ https://go.microsoft.com/fwlink/?LinkId=70780)。  
- 使用更快速的磁帶裝置或磁片備份來縮短還原作業所需的時間。  
  
您也可以使用 [從媒體安裝（IFM）] 功能，在每個網域中重建 Dc，以協助加速 AD DS 安裝。 IFM 可減少在每個網域中重建 Dc 時所產生的複寫延遲。  
  
具有更積極服務等級協定（SLA）的企業可能會考慮改變樹系復原程式，以加速復原。  
  
**問：我可以自動化樹系復原程式嗎？**

因為樹系復原程式有複雜且重要的本質，所以目前沒有任何端對端自動化。 樹系復原程式在還原商務持續性方面，比處理自動化的技術問題更具物流和組織的挑戰。 因此，負責管理環境的個人應該建立該環境專屬的樹系復原計畫，然後自動化可成功自動化的區段。  
  
您可以使用命令列工具來執行大部分的樹系修復步驟。 因此，大部分的步驟都是可編寫腳本的。 例如，Ntdsutil.exe 是樹系復原程式中最常使用的工具之一。  
  
雖然腳本可以加速復原，但您必須徹底測試這些腳本，然後才在實際環境中套用它們。 此外，您必須根據 Active Directory 環境中的變更來更新它們，例如加入新的網域或 DC，或 Active Directory 的新版本。

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
