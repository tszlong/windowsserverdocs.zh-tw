---
title: AD 樹系復原-復原在多網域樹系有單一網域
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 267541be-2ea7-4af6-ab34-8b5a3fedee2d
ms.technology: identity-adds
ms.openlocfilehash: fae2cc40af0b43dd38d72c2622720a6bb17b0a66
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863469"
---
# <a name="ad-forest-recovery---recovering-a-single-domain-in-a-multidomain-forest"></a>AD 樹系復原-復原在多網域樹系有單一網域

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

可以有時復原只有單一網域內具有多個網域，而不是完整的樹系復原的樹系所需的時間。 本主題涵蓋考量復原為單一網域和復原的可能策略。  
  
單一網域復原代表獨特的挑戰，以重建通用類別目錄 (GC) 伺服器。 比方說，如果已建立一週稍早的備份，則其他所有樹系中的 Gc 從還原的第一個網域控制站 (DC) 網域會擁有多個新的資料網域比還原的網域控制站。 若要重新建立 GC 資料一致性，有幾個選項：  
  
- Unhost，然後重新復原的網域磁碟分割，所有的 Gc，在樹系，除了在復原的網域中，從裝載在相同的時間。  
- 若要復原的網域樹系復原程序，然後再移除 其他網域中的 Gc 中的 延遲物件。  
  
下列各節提供每個選項的一般考量。 一組完整的必須完成復原的步驟會針對不同的 Active Directory 環境而有所不同。  
  
## <a name="rehost-all-gcs"></a>重新裝載所有的 Gc  

> [!WARNING]
> 萬一發生問題，讓存取登入為 gc，所有網域的網域系統管理員帳戶的密碼必須是可供使用。  

可以在重新裝載所有 Gc 使用 repadmin / unhost 和 repadmin /rehost 命令 （repadmin /experthelp 的一部分）。 您會在每次 GC 不會復原每個網域中執行 repadmin 命令。 它必須確保，所有的 Gc 未再保存一份已復原的網域。 若要這麼做，unhost 網域分割第一次所有網域控制站在無復原的所有定義域，樹系的第一次。 所有的 Gc 不不再包含資料分割之後，您就可以將它重新裝載。 重新裝載時，請考慮站台-和複寫-結構的樹系，比方說，完成前重新裝載該站台的其他網域控制站的站台每一個網域控制站的重新裝載。  
  
此選項很有幫助，對於小型組織有只有少數的網域控制站的每個網域。 所有的 Gc 可以重建在星期五晚上，如有必要，在星期一早上前完成複寫的所有唯讀網域分割區。 但是，如果您需要復原大型網域，其中涵蓋在全球各地的網站，重新裝載唯讀的網域上的磁碟分割所有的 Gc 其他定義域可大幅影響作業，可能需要停機時間。  
  
## <a name="remove-lingering-objects"></a>移除延遲物件

類似於樹系修復程序，您有一個 DC 從備份還原的網域中，您需要復原，執行其餘的 Dc 的中繼資料清除作業，然後再重新安裝 AD DS 網域建置。 在樹系中的所有其他網域的 Gc，您可以移除延遲物件的唯讀磁碟分割的已復原的網域。  

延遲物件清除的來源必須是復原的網域中的 DC。 特定來源 DC 並沒有任何延遲物件的任何網域分割區，才能，如果它是 GC，可以移除通用類別目錄。  

移除延遲物件是不可能會與其他選項相關聯的停機時間的較大型組織有利的。  

如需詳細資訊，請參閱 <<c0> [ 移除延遲物件的使用 Repadmin](https://technet.microsoft.com/library/cc785298.aspx)。

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
