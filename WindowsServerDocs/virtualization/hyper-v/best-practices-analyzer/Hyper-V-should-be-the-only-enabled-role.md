---
title: HYPER-V 應該是唯一啟用的角色
description: 提供指示來解決此 Best Practices Analyzer 規則所回報的問題。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5a0ed176-048f-40b1-b56c-8391b805fd37
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: bd03554396696a43b4821aff0f4ed893933484c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886459"
---
# <a name="hyper-v-should-be-the-only-enabled-role"></a>HYPER-V 應該是唯一啟用的角色

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
  
*在此伺服器上啟用 HYPER-V 以外的角色。*  
  
在大部分情況下，它不是個不錯的主意，在執行 HYPER-V 角色的伺服器上安裝其他角色。 遠端桌面虛擬主機角色服務會是例外狀況，因為它是遠端桌面服務角色的一部分，而且需要安裝在同一部伺服器上的 HYPER-V。  
  
## <a name="impact"></a>影響  
  
*HYPER-V 角色應該是唯一的伺服器上啟用的角色。*  
  
這個最佳做法可協助確保主機作業系統可用的角色、 功能及不需要執行 HYPER-V 的應用程式。 遵循這個最佳做法和 Nano Server 上執行 HYPER-V 可協助減少您將需要的因為只有 Nano 伺服器 HYPER-V 服務元件，以及 Windows hypervisor 會受到軟體更新的更新數目。  
  
## <a name="resolution"></a>解析度  
  
*您可以使用 伺服器管理員來移除 HYPER-V 以外的所有角色。*  
  
伺服器管理員包括 [移除角色精靈]。 此精靈可讓您一次移除多個角色。 移除角色之前, 移除角色精靈 會檢查相依性，以降低移除其他角色所依賴的軟體的風險。 如果找不到相依性，精靈會提示您核准移除其他角色、 角色服務或已安裝的角色所需的軟體。   
  
若要使用伺服器管理員，您必須登入電腦的系統管理員身分。  
  
#### <a name="to-remove-a-role"></a>若要移除的角色  
  
1.  使用快速鍵上開啟 伺服器管理員**啟動**Windows 工作列上或在 系統管理工具 功能表。  
2.   在 **角色摘要**區域中的 伺服器管理員 主視窗中，按一下**移除角色**。 請遵循移除角色精靈 的指示。   
  
  
  


