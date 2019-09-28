---
title: 啟用虛擬機器中的所有整合服務
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 16e202ad-3795-40c9-8176-7ca319e56d26
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 1984c3d1d6261756bf83f899985b457681537046
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364895"
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
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
  
*一或多個整合服務已停用或在虛擬機器中無法運作。*  
  
## <a name="impact"></a>影響  
  
*下列虛擬機器的服務或整合功能可能無法正常運作：*  
  
@no__t 0list 的虛擬機器名稱 >  
  
## <a name="resolution"></a>解析度  
  
*使用 [服務] 嵌入式管理單元或 sc config 命令列工具，確認服務已設定為自動啟動且不會停止。*  
  
#### <a name="to-configure-how-a-service-is-started-using-the-services-snap-in"></a>使用服務嵌入式管理單元設定服務的啟動方式  
  
1.  使用遠端桌面服務或虛擬機器連線來連線到虛擬機器，並登入客體作業系統。  
  
2.  開啟 [服務]。 （按一下 [**開始**]，在 [**開始搜尋**] 方塊中按一下，輸入**services.msc**，然後按 enter）。  
  
3.  在詳細資料窗格中，以滑鼠右鍵按一下您要設定的服務，然後按一下 [**屬性**]。  
  
4.  在 [**一般**] 索引標籤的 [**啟動**類型] 中，按一下 [**自動**]。  
  
#### <a name="to-configure-how-a-service-is-started-using-sc-config"></a>使用 SC Config 設定服務的啟動方式  
  
1.  開啟 Windows PowerShell。 （從桌面上，按一下 [**開始**]，然後開始鍵入**Windows PowerShell**。）  
  
2.  以滑鼠右鍵按一下 [ **Windows PowerShell** ]，然後按一下 [**以系統管理員身分執行**]。  
  
3.  以服務的名稱取代 < 的服務名稱 >，然後輸入：  
  
    ```  
    sc config <service-name> start=auto  
    ```  
  


