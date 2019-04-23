---
title: AD 樹系復原還原的樹系的步驟
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 1712d3a636160f82495539afdd42ab2ee85ffae2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861469"
---
# <a name="ad-forest-recovery---steps-for-restoring-the-forest"></a>AD 樹系復原還原的樹系的步驟

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

本節概述建議的途徑來復原樹系。 樹系復原步驟稍後會說明在 詳細資料。  
  
下列清單摘要說明在高層級的復原步驟：  
  
1. [找出問題](AD-Forest-Recovery-Identify-the-Problem.md)  

   使用 IT 和 Microsoft 支援服務以判斷問題和可能的原因的範圍和評估所有的商務專案關係人可能的補救方法。 在許多情況下總計的樹系復原應該是最後一個選項。  
  
2. [決定如何復原樹系](AD-Forest-Recovery-Determine-how-to-Recover.md)  

   判斷需要樹系復原時之後，完整的初稿步驟，準備： 判斷目前的樹系結構，可識別每個 DC 執行時，決定哪些 DC 為每個網域中，還原的函式，並確定所有的可寫入網域控制站會使其離線。  

3. [執行初始的復原](AD-Forest-Recovery-Perform-initial-recovery.md)  

   在隔離中，復原每個網域一個網域控制站、 清除它們，並重新連線的網域。 重設特殊權限的帳戶，並修正在這個階段中的 安全性破壞所造成的問題。  
  
4. [重新部署剩餘網域控制站](AD-Forest-Recovery-Restore-Additional-DCs.md)  

   重新部署的樹系，以還原為在失敗前的狀態。 此步驟中需要特定的設計和需求進行調整。 複製虛擬的網域控制站，可協助加速此程序。  

5. [清除](AD-Forest-Recovery-Cleanup.md)  

   還原功能之後，視需要重新設定名稱解析，並取得工作的 LOB 應用程式。  

本指南中的步驟被設計來重新將危險資料到已復原的樹系的可能性降至最低。 您可能必須修改這些步驟來處理這類的因素：  
  
- 延展性  
- 遠端管理能力  
- 復原的速度  
