---
title: 應該在虛擬機器中啟用存放控制器，以提供對附加存放裝置的存取權
description: 提供解決此最佳做法分析程式規則所回報之問題的指示。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 532548a1-8ffe-4b5b-902e-ed2f0819012b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: f0d10ab4c419a6014a9edb4b7f721714dc92798d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393487"
---
# <a name="storage-controllers-should-be-enabled-in-virtual-machines-to-provide-access-to-attached-storage"></a>應該在虛擬機器中啟用存放控制器，以提供對附加存放裝置的存取權

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
  
*虛擬機器中可能已停用一或多個存放裝置控制器。*  
  
## <a name="impact"></a>影響  
  
@no__t 0Virtual 機器無法使用已連線至已停用存放裝置控制器的存放裝置。這會影響下列虛擬機器： *  
  
@no__t 0list 的虛擬機器名稱 >  
  
## <a name="resolution"></a>解析度  
  
@no__t 0Use Device Manager 在客體作業系統中，以啟用所有存放控制器。如果儲存體控制器不是必要的，請使用 Hyper-v 管理員將其從虛擬機器中移除。 *  
  
如需如何使用 Device Manager 的指示，請參閱客體作業系統中的說明。 如需有關如何移除存放裝置控制器的指示，請參閱下列程式。  
  
#### <a name="to-remove-a-scsi-storage-controller-from-the-virtual-machine"></a>從虛擬機器移除 SCSI 存放裝置控制器  
  
1.  開啟 \[Hyper-V 管理員\]。 按一下 [開始]，指向 [系統管理工具]，然後按一下 [Hyper-V 管理員]。  
  
2.  在 [結果] 窗格的 [**虛擬機器**] 底下，選取您要設定的虛擬機器。  
  
3.  如果虛擬機器正在執行，請關閉虛擬機器。 以滑鼠右鍵按一下虛擬機器，然後按一下 [**關機**]。  
  
4.  在 [執行] 窗格的虛擬機器名稱之下，按一下 [設定]。  
  
5.  在 [**設定**] 對話方塊的左窗格中，按一下 [**硬體**] 底下的 [ **SCSI 控制器**]。  
  
6.  在右窗格中，按一下 [**移除**]。  
  


