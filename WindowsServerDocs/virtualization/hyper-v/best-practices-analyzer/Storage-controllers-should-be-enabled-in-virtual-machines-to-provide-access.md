---
title: 存放裝置控制器應該啟用虛擬機器中，提供附加的儲存體的存取
description: 提供指示來解決此 Best Practices Analyzer 規則所回報的問題。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 532548a1-8ffe-4b5b-902e-ed2f0819012b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 42803a0eef84bf006e9f9e7ed6297ea21b4eb7b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849159"
---
# <a name="storage-controllers-should-be-enabled-in-virtual-machines-to-provide-access-to-attached-storage"></a>存放裝置控制器應該啟用虛擬機器中，提供附加的儲存體的存取

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
  
*一或多個儲存體控制器可能會停用虛擬機器中。*  
  
## <a name="impact"></a>影響  
  
*虛擬機器無法使用連接到已停用的儲存體控制器的儲存體。這會影響下列虛擬機器：*  
  
\<清單中的虛擬機器名稱 >  
  
## <a name="resolution"></a>解析度  
  
*使用在客體作業系統的裝置管理員，讓所有的儲存體控制站。如果儲存體控制器不是必要的使用 HYPER-V 管理員來移除虛擬機器。*  
  
如需有關如何使用裝置管理員的指示，請參閱在客體作業系統中的說明。 如需有關如何移除存放裝置控制器的指示，請參閱下列程序。  
  
#### <a name="to-remove-a-scsi-storage-controller-from-the-virtual-machine"></a>若要從虛擬機器移除 SCSI 存放裝置控制器  
  
1.  開啟 \[Hyper-V 管理員\]。 按一下 [開始]，指向 [系統管理工具]，然後按一下 [Hyper-V 管理員]。  
  
2.  在 [結果] 窗格中，在**虛擬機器**，選取您想要設定的虛擬機器。  
  
3.  如果虛擬機器正在執行，請將虛擬機器關機。 以滑鼠右鍵按一下 虛擬機器，然後按一下 **關閉**。  
  
4.  在 [執行] 窗格的虛擬機器名稱之下，按一下 [設定]。  
  
5.  在左窗格中**設定**對話方塊的 **硬體**，按一下  **SCSI 控制器**。  
  
6.  在右窗格中，按一下**移除**。  
  


