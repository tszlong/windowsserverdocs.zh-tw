---
title: 啟用虛擬機器中的所有整合服務
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 16e202ad-3795-40c9-8176-7ca319e56d26
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 307e2d407a0defa14a6b57bda95a2f3ab018406d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829429"
---
# <a name="enable-all-integration-services-in-virtual-machines"></a>啟用虛擬機器中的所有整合服務

>適用於：Windows Server 2016

如需最佳做法和掃描的詳細資訊，請參閱 [最佳做法分析程式](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>問題  
  
*一或多個整合服務已停用] 或 [虛擬機器中無法運作。*  
  
## <a name="impact"></a>影響  
  
*對下列虛擬機器，「 服務 」 或 「 整合功能可能無法正常運作：*  
  
\<清單中的虛擬機器名稱 >  
  
## <a name="resolution"></a>解析度  
  
*若要確認服務設定為自動啟動，而且並不會停止使用服務嵌入式管理單元或 sc config 命令列工具。*  
  
#### <a name="to-configure-how-a-service-is-started-using-the-services-snap-in"></a>使用 [服務] 嵌入式管理單元來設定服務如何啟動  
  
1.  使用遠端桌面服務或虛擬機器連線來連線到虛擬機器和記錄檔上的客體作業系統。  
  
2.  開啟 [服務]。 (按一下**開始**，按一下**開始搜尋**方塊中，輸入**services.msc**，然後按 ENTER 鍵。)  
  
3.  在 詳細資料 窗格中，以滑鼠右鍵按一下您想要設定，然後按一下 服務**屬性**。  
  
4.  在 **一般**索引標籤中，於**啟動**輸入，再按一下**自動**。  
  
#### <a name="to-configure-how-a-service-is-started-using-sc-config"></a>使用 SC Config 來設定服務如何啟動  
  
1.  開啟 Windows PowerShell。 (從桌面上，按一下**開始**，並開始輸入**Windows PowerShell**。)  
  
2.  以滑鼠右鍵按一下**Windows PowerShell**然後按一下**系統管理員身分執行**。  
  
3.  取代 < 服務名稱 > 服務的名稱，然後輸入：  
  
    ```  
    sc config <service-name> start=auto  
    ```  
  


