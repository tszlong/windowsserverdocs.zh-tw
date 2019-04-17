---
title: "廣告樹系復原步驟還原樹系"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 29988c262897649173e039cc052bb64f38a1a0ca
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/07/2017
---
#<a name="ad-forest-recovery---steps-for-restoring-the-forest"></a>廣告樹系復原步驟還原樹系 

>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2

本章節復原樹系提供建議路徑的概觀。 樹系修復的步驟稍後描述在詳細資料。  
  
 下列清單摘要高階修復的步驟：  
  
1.  [找出問題](AD-Forest-Recovery-Identify-the-Problem.md)  
  
     搭配 IT 與 Microsoft 支援服務來判斷範圍問題的可能原因，並與所有企業關鍵評估可能救濟權利。 通常總樹系修復應該最後一個選項。  
  
2.  [選擇如何復原森林](AD-Forest-Recovery-Determine-how-to-Recover.md)  
  
     完成初稿判斷是必要的樹系修復之後，步驟它的準備： 判斷目前的樹系結構，找出俠每個執行時，可以選擇要俠還原為每個網域中，並確保離線拍攝的所有寫入網域控制站的功能。  
  
3.  [執行初始復原](AD-Forest-Recovery-Perform-initial-recovery.md)  
  
     隔離，復原一個網域控制站的每個網域、 清理，並重新網域。 重設特殊權限的帳號，並修正造成安全性漏洞，在此階段中的問題。  
  
4.  [剩餘的網域控制站重新部署](AD-Forest-Recovery-Restore-Additional-DCs.md)  
  
     重新部署樹系返回為失敗之前的狀態。 這個步驟會需要是適用於您的特定的設計與需求。 模擬的網域控制站複製可協助促進此程序。  
  
5.  [清理](AD-Forest-Recovery-Cleanup.md)  
  
     還原功能後，如有需要重新設定名稱解析，並取得運作 LOB 應用程式。  

  
 最小化 reintroducing 危險資料插入復原樹系的可能性設計本文中的步驟操作。 您可能需要修改因素的步驟執行：  
  
-   擴充性  
-   遠端管理  
-   復原的速度  

