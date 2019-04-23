---
title: AD 樹系復原 - 常見問題
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: ac9e5a3d-8b1e-41b7-8e02-f64b7acf1359
ms.technology: identity-adds
ms.openlocfilehash: 36c9560b490cc28f006770c869bdcdf2b152dd74
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878449"
---
# <a name="ad-forest-recovery---faq"></a>AD 樹系復原 - 常見問題

>適用於：Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2 中，Windows Server 2003

這份文件包含常見問題集 (Faq) 與樹系復原相關：  

## <a name="general-recovery"></a>一般復原

**問：可以做什麼來加速復原？**

雖然復原的速度不是本指南的主要目標，就能夠藉由縮短復原時間：  
  
- 建立詳細的樹系復原計畫時，更新定期執行，並練習在模擬的測試環境中適當大小的至少一次一年  
- 使用 複製虛擬的網域控制站 (DC)  
   - 虛擬化 DC 複製，可以加速程序來取得其他 DC 從每個網域中的備份還原一次之後執行的網域控制站。 其他的虛擬網域控制站可以被複製，而不是等待可能冗長但 AD DS 安裝完成並完成安裝後的非關鍵性複寫。  
   - 樹系虛擬網域控制站會裝載在相對較少的連線狀態良好的資料中心可能獲得修復期間複製的最大效益。 不過，其中多個虛擬的網域控制站相同的網域共置於相同的 hypervisor 主機的任何環境應該獲益。  
- 部署唯讀網域控制站 (Rodc)  
   - Rodc 可以在復原程序期間提供商務持續性，因為他們不需要可寫入網域控制站一樣，與網路中斷。 Rodc 未執行輸出複寫。 因此，它們不會顯示同一個可寫入網域控制站會造成破壞性的資料複寫回復原環境的風險。  
  
其他會影響樹系修復程序的持續時間的因素包括：  
  
- 當您從備份還原網域控制站時，要花的時間：  
   - 找出實體備份媒體，例如磁帶。  
   - 重新安裝作業系統。  
   - 從備份媒體還原資料。  
      - 您可以減少重新安裝作業系統，並從備份還原資料，藉由執行完整伺服器復原，而不是系統狀態還原所需的時間。 完整伺服器復原會是二進位檔為基礎，因為它完成速度，比系統狀態還原。  
      - 不過，如果伺服器包含將排除您不想要還原系統狀態資料的資料，完整伺服器復原可能不是可行的替代方案，以系統狀態還原。 請考慮明確地說，您的伺服器執行完整伺服器復原，而不是系統狀態還原的優點，並據此來準備執行的備份，您打算稍後還原適當的型別。  
- 當您重建 Dc 時，需要將網路為基礎的促銷活動的資料複寫的時間。  
   - 您可以減少還原網域控制站上執行下列步驟所需的時間：  
- 減少擷取備份媒體的時間：  
   - 您可以使用 Active Directory 資料庫掛接工具 (Dsamain.exe) 來識別要用於還原作業的最佳備份。 如需使用 Active Directory 資料庫掛接工具的詳細資訊，請參閱[Active Directory 資料庫掛接工具逐步指南](https://go.microsoft.com/fwlink/?LinkId=132577)(https://go.microsoft.com/fwlink/?LinkId=132577)。  
   - 清楚地標記備份媒體與組織的方式，在一個方便儲存媒體尚未保護，可讓您快速擷取的位置。  
   - 使用存放區域網路 (SAN) 的磁碟區陰影複製服務來維護時間從不同的時間點備份。 如需詳細資訊，請參閱 < [Windows Server 2003 Active Directory 快速復原磁碟區陰影複製服務和虛擬磁碟服務](https://go.microsoft.com/fwlink/?LinkId=70781)(https://go.microsoft.com/fwlink/?LinkId=70781)。  
- 從強制移除 AD DS 的網域控制站，而不是重新安裝作業系統。 如果已經為 AD DS 的範圍內的純識別全樹系失敗的原因，您不必在網域控制站上重新安裝作業系統。  
   - 如需有關如何從執行 Windows Server 2008 或更新版本的 DC 強制移除 AD DS 的詳細資訊，請參閱 <<c0> [ 強制移除 Windows Server 2008 網域控制站](https://go.microsoft.com/fwlink/?LinkId=132627)(https://go.microsoft.com/fwlink/?LinkId=132627)。 如需有關如何從執行 Windows Server 2003 DC 強制移除 AD DS 的詳細資訊，請參閱 <<c0> [ 文章 332199](https://go.microsoft.com/fwlink/?LinkId=70780)在 Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=70780)。  
- 使用更快速的磁帶裝置或磁碟備份，以減少所需的時間還原作業。  
  
您也可以協助加快使用從安裝媒體 (IFM) 功能來重建每個網域中的網域控制站的 AD DS 安裝。 IFM 可減少您重建每個網域中的網域控制站時，即會產生複寫延遲。  
  
有更積極的服務等級協定 (SLA) 的企業可能要考慮修改速度復原的樹系修復程序。  
  
**問：我可以自動化樹系修復程序嗎？**

因為樹系修復程序的複雜且重要本質，目前沒有它的任何端對端自動化。 樹系修復程序是更邏輯和組織挑戰的還原比技術問題的程序自動化的商務持續性。 因此，管理環境的人應該建立該環境特有的樹系復原計劃，然後再自動執行它的區段可以成功地自動化。  
  
您可以使用命令列工具來執行大部分的樹系復原步驟。 因此，大部分的步驟都可編寫指令碼。 比方說，Ntdsutil.exe 是樹系修復程序中最常使用的工具之一。  
  
雖然指令碼可以加速復原，您必須徹底測試這些指令碼，然後再套用在實際環境中。 此外，您必須更新這些值根據 Active Directory 環境，例如加上新的網域或 DC、 或 Active Directory 的新版本中的變更。

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
