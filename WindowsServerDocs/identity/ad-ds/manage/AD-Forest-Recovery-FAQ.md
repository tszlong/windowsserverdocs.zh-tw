---
title: "廣告樹系修復常見問題集"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: ac9e5a3d-8b1e-41b7-8e02-f64b7acf1359
ms.technology: identity-adfs
ms.openlocfilehash: 839e01c88b7ced22501154d8752c6c71cc52b696
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---faq"></a>廣告樹系修復常見問題集

>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2、 Windows Server 2003

本文件包含有關樹系修復常見問題的解答 (Faq):  
 
## <a name="general-recovery"></a>一般復原
  
 
**問： 項目執行加速修復？** 
 
雖然復原的速度並不本書的主要目標，一種由短復原時間：  
  
-   建立詳細的樹系修復計劃，定期更新，以及練習模擬的測試環境合理的大小的每年至少一次  
  
-   使用複製的模擬的網域控制站 (DC)  
  
     可以模擬俠複製加速取得其他執行一個從每個網域中的備份還原俠之後的網域控制站的程序。 除了等待長 AD DS 安裝完成後，以及完成安裝之後嚴重的複寫可以複製其他模擬網域控制站。  
  
     森林 virtual 網域控制站放置在較小許多連接良好的資料中心可能前提最複製復原期間。 不過，應該前提任何環境多個模擬網域控制站在相同的網域一起位置 hypervisor 主機。  
  
-   部署唯讀網域控制站 (Rodc)  
  
     Rodc 可以提供業務持續性的修復程序期間，因為它們不需要像寫入 Dc 中斷網路。 Rodc 並執行輸出複寫。 因此，它們不會存在寫入 Dc 造成損害資料複寫回修復環境相同風險。  
  
 其他因素而有所不同影響的樹系的修復程序的持續時間，包含下列類型：  
  
-   從備份還原 Dc 時, 所需的時間：  
  
    -   找出實體備份媒體，例如磁帶。  
  
    -   重新安裝作業系統。  
  
    -   從媒體備份還原資料。  
  
     您可以減少重新安裝作業系統，並從備份還原資料，來執行完整伺服器修復系統狀態還原而所需的時間。 完整伺服器復原是二進位為基礎，因為它是比系統狀態還原更快。  
  
     不過，如果伺服器包含排除系統狀態資料，您不想要還原的資料，完整伺服器復原可能無法系統狀態還原可行替代物取代。 考慮優點具體而言，執行而不是狀態系統還原完整伺服器復原您的伺服器，並依此準備執行適當想要稍後還原備份的類型。  
  
-   當您重新建立網域控制站時，它花一些時間複寫網路促銷活動的資料。  
  
 您可以減少還原 Dc 下列步驟來執行所需的時間：  
  
-   減少正在透過備份的媒體擷取的時間：  
  
    -   使用 Active Directory 資料庫裝載工具 (Dsamain.exe) 找出最佳備份使用還原操作。 如需有關如何使用 Active Directory 資料庫裝載工具的詳細資訊，請[Active Directory 資料庫裝載工具 Step-by-Step 指南](https://go.microsoft.com/fwlink/?LinkId=132577)(https://go.microsoft.com/fwlink/?LinkId=132577)。  
  
    -   清楚標記備份媒體，並將媒體存放在便利的方式整理尚未安全，可讓快速擷取的位置。  
  
    -   使用不同的點備份維護時間存放區網路 （舊） 音量陰影複製服務。 如需詳細資訊，請查看[Windows Server 2003 Active Directory（fast ring）修復磁碟區陰影複製服務 Virtual 磁碟服務與](https://go.microsoft.com/fwlink/?LinkId=70781)(https://go.microsoft.com/fwlink/?LinkId=70781)。  
  
-   強制 AD DS 移除的網域控制站而不是重新安裝作業系統。 如果完全中 AD DS 的範圍是已知的樹系失敗的原因，您不必網域控制站上重新安裝作業系統。  
  
     如需有關移除 AD DS 強迫從 DC 執行 Windows Server 2008，或更新版本，請查看[強迫移除 Windows Server 2008 網域控制站的](https://go.microsoft.com/fwlink/?LinkId=132627)(https://go.microsoft.com/fwlink/?LinkId=132627)。 如需有關從執行 Windows Server 2003 DC 強迫移除 AD DS，請查看[文章 332199](https://go.microsoft.com/fwlink/?LinkId=70780)中「Microsoft 知識庫 (https://go.microsoft.com/fwlink/?LinkId=70780)。  
  
-   使用更快速地磁帶裝置或減少所需的時間磁碟備份還原操作。  
  
 您也可以協助加快 AD DS 安裝重建網域控制站在每個網域中的使用安裝媒體 (IFM) 功能。 IFM 減少當您重新建立網域控制站在每個網域收取複寫延遲。  
  
 企業有更多積極層級服務合約 (SLA)，可能會考慮改變快速修復的樹系修復程序。  
  

**問： 樹系修復程序可以自動化嗎？**  
 複雜和重要的樹系的修復程序的性質，因為有是目前的任何端點-自動化。 樹系的修復程序是更邏輯及組織挑戰還原比的程序自動化技術問題的業務持續性。 因此，管理環境人才應該建立特定的環境樹系復原計畫，再將它可以成功自動化的各節。  
  
 您可以使用命令列工具來執行大多數的樹系修復的步驟。 因此，這些步驟是編寫指令碼。 例如，Ntdsutil.exe 是最常使用的樹系的修復程序的工具之一。  
  
 雖然指令碼可以速度復原，您必須在先真實的環境中所套用完全測試這些指令碼。 同時，您必須更新這些根據 Active Directory 環境，例如加入新的網域或俠或新版的 Active Directory 中變更。

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
