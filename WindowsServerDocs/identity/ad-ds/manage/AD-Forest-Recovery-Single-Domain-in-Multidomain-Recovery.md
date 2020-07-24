---
title: AD 樹系復原-修復多網域樹系中的單一網域
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 267541be-2ea7-4af6-ab34-8b5a3fedee2d
ms.technology: identity-adds
ms.openlocfilehash: 84ce874ea85cce7ea562fcd5169902086732b4c4
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86960840"
---
# <a name="ad-forest-recovery---recovering-a-single-domain-in-a-multidomain-forest"></a>AD 樹系復原-修復多網域樹系中的單一網域

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

有時候只需要復原具有多個網域的樹系中的單一網域，而不是完整的樹系復原。 本主題涵蓋復原單一網域的考慮，以及修復的可能策略。  
  
單一網域復原提供重建通用類別目錄（GC）伺服器的獨特挑戰。 例如，如果網域的第一個網域控制站（DC）是從稍早建立的備份中還原，則樹系中的所有其他 Gc 將會擁有該網域的最新資料，而不是還原的 DC。 若要重新建立 GC 資料一致性，有幾個選項：  
  
- Unhost，然後從樹系中的所有 Gc 重新裝載已復原的網域磁碟分割，但已復原的網域中的資料分割也一樣。  
- 遵循樹系復原程式來復原網域，然後從其他網域中的 Gc 移除延遲物件。  
  
下列各節提供每個選項的一般考慮。 針對復原所需完成的一組完整步驟會因不同的 Active Directory 環境而異。  
  
## <a name="rehost-all-gcs"></a>重新裝載所有 Gc  

> [!WARNING]
> 所有網域的網域系統管理員帳戶的密碼都必須準備好使用，以防發生問題而無法存取 GC 進行登入。  

重新裝載所有 Gc 都可以使用 repadmin/unhost 和 repadmin/rehost 命令（repadmin/experthelp 的一部分）來完成。 您會在每個未復原的網域中的每個 GC 上執行 repadmin 命令。 它必須確保，所有 Gc 都不會再保存復原的網域複本。 若要達到此目的，請先從樹系所有無復原網域的所有網域控制站 unhost 網域分割。 在所有 Gc 不再包含分割區之後，您可以重新裝載它。 在重新裝載時，請考慮您樹系的網站和複寫結構，例如，在重新裝載該網站的其他 Dc 之前，先完成每個網站的一個 DC 重新裝載。  
  
對於每個網域只有幾個網域控制站的小型組織而言，此選項很有利。 所有 Gc 都可以在星期五晚上重建，並視需要在星期一早上之前完成所有唯讀網域分割區的複寫。 但是，如果您需要復原涵蓋全球網站的大型網域，則在其他網域的所有 Gc 上重新裝載唯讀網域分割區可能會大幅影響作業，而且可能需要停機時間。  
  
## <a name="remove-lingering-objects"></a>移除延遲物件

類似于樹系復原程式，您可以從需要復原的網域中的備份還原一個 DC、執行其餘 Dc 的中繼資料清除，然後重新安裝 AD DS 以建立網域。 在樹系中所有其他網域的 Gc 上，您可以移除已復原網域的唯讀磁碟分割的延遲物件。  

延遲物件清除的來源必須是已復原之網域中的 DC。 若要確定來源 DC 沒有任何網域分割區的延遲物件，您可以移除通用類別目錄（如果它是 GC）。  

移除延遲物件對於不會造成與其他選項相關聯之停機時間的大型組織很有利。  

如需詳細資訊，請參閱[使用 Repadmin 移除延遲物件](/previous-versions/windows/it-pro/windows-server-2003/cc785298(v=ws.10))。

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
