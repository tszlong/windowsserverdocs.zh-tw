---
title: 廣告樹系修復-復原單一網域中的多網域樹
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 267541be-2ea7-4af6-ab34-8b5a3fedee2d
ms.technology: identity-adfs
ms.openlocfilehash: 3a1c9d0671a732eee83aa707e061afdbbf106fa5
ms.sourcegitcommit: f26d2668f57624a3865ca4ffd36a698eea7b503e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/12/2018
---
# <a name="ad-forest-recovery---recovering-a-single-domain-in-a-multidomain-forest"></a>廣告樹系修復-復原單一網域中的多網域樹

>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2

可能會有時復原單一網域中的多個網域，而不是完整的樹系復原樹系所需的時間。 本主題涵蓋考量復原單一網域和修復的可能策略。  
  
 單一網域復原呈現的唯一挑戰重建通用 (GC) 伺服器。 例如，如果的第一個網域控制站 DC 網域還原備份，建立一個星期更早版本，然後在森林中的所有其他 Gc 從將會有更多最新資料該網域比還原網域控制站。 若要重新建立 GC 資料的一致性，有幾個選項：  
  
-   Unhost，再一次重新裝載的樹系以外的復原網域中的所有 Gc 復原的網域磁碟分割。  
  
-   依照復原網域的樹系修復程序，並 Gc 其他網域中的移除延遲物件。  
  
 下列區段會提供一般考量每個選項。 一組完整的需要進行修復的步驟會針對不同的 Active Directory 環境而有所不同。  
  
## <a name="rehost-all-gcs"></a>重新所有 Gc 都裝載  

> [!WARNING]
>  萬一發生問題而無法存取 gc 登入的所有網域網域系統管理員負責密碼必須可供使用。  

 可以完成 rehosting 所有 Gc 使用 repadmin//unhost 和 repadmin /rehost 命令 (repadmin 的一部分 /experthelp)。 您會在每個復原網域中的每個 GC 執行 repadmin 的命令。 它需要可確保所有 Gc 再不包含復原網域複本。 為了這 unhost 網域磁碟分割第一次所有網域控制站跨樹系的所有無-復原網域第一次。 所有 Gc 並不會再磁碟分割都包含之後，您可以重新裝載。 當 rehosting，請考慮將網站和複寫-結構的樹系，例如時，完成 rehost 的一個俠每個之前 rehosting 其他網域控制站該網站的網站。  
  
 此選項可小型的只有少數網域網域控制站每個組織的好處。 所有 Gc 可以星期五之夜 」 上重建，如有需要，週一之前完成所有唯讀網域複寫磁碟分割。 但如果您需要復原全球涵蓋網站大型網域，rehosting 唯讀網域磁碟分割上所有的 Gc 其他網域可大幅影響作業，可能需要向時間。  
  
## <a name="remove-lingering-objects"></a>移除延遲物件  
 類似的樹系的修復程序，一個俠從備份還原的網域中，您需要復原、 執行的清除中繼資料的剩餘 Dc，並重新安裝 AD DS 建置網域。 森林中的所有其他網域 Gc、 在您移除延遲物件唯讀復原網域中的磁碟分割。  
  
 適用於延遲物件清理來源必須 DC 復原網域中。 若要將某些來源 DC 不會有任何延遲物件的任何網域磁碟分割，您可以移除通用是否 GC。  
  
 移除延遲物件的是較大型的組織可能會無法使用的其他選項的相關的向下時間的理想。  
  
 如需詳細資訊，請查看[使用 Repadmin 移除延遲物件](https://technet.microsoft.com/library/cc785298.aspx)。

## <a name="next-steps"></a>後續步驟
-   [廣告樹系修復必要條件](AD-Forest-Recovery-Prerequisties.md)  
-   [廣告樹系修復-設計自訂樹系復原計劃](AD-Forest-Recovery-Devising-a-Plan.md)  
-   [廣告樹系修復-找出問題](AD-Forest-Recovery-Identify-the-Problem.md)
-   [廣告樹系修復-判斷如何復原](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [廣告樹系修復-執行初始復原](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [廣告樹系修復程序](AD-Forest-Recovery-Procedures.md)  
-   [廣告樹系修復-常見問題集](AD-Forest-Recovery-FAQ.md)  
-   [廣告樹系修復-復原 Multidomain 樹系單一網域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [廣告樹系復原與 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md)  
