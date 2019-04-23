---
title: HYPER-V 虛擬機器管理服務應該設定為自動啟動
description: 提供指示來解決此 Best Practices Analyzer 規則所回報的問題。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 222bbe76-c514-4a3f-b61b-860a4dc2826a
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c33f81678d7fdc71e81834a002fd3d7917a6f632
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833249"
---
# <a name="the-hyper-v-virtual-machine-management-service-should-be-configured-to-start-automatically"></a>HYPER-V 虛擬機器管理服務應該設定為自動啟動

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
  
*HYPER-V 虛擬機器管理服務未設定為自動啟動。*  
  
## <a name="impact"></a>影響  
  
*無法管理虛擬機器，直到服務已啟動。*  
  
正在執行的虛擬機器將會繼續執行。 不過，您無法再管理虛擬機器，或建立或刪除它們，直到服務正在執行。  
  
## <a name="resolution"></a>解析度  
  
*您可以使用服務嵌入式管理單元或 sc config 命令列工具來重新設定為自動啟動服務。*  
  
> [!TIP]  
> 如果您找不到服務中的傳統型應用程式或命令列工具會報告該服務不存在，HYPER-V 管理工具可能未安裝。 若要安裝它們：  
>   
> - 在 Windows 伺服器上，開啟 伺服器管理員，並使用 新增角色及功能精靈。 如需詳細資訊，請參閱 < [Windows Server 2016 上安裝 HYPER-V 角色](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md)。  
> - 在 Windows 中，從 Desktop，開始輸入**程式**，按一下**程式和功能**（控制台） >**開啟或關閉開啟的 Windows 功能** >  **Hyper V** > **HYPER-V 管理工具**。 然後，按一下**確定**。  
  
#### <a name="to-reconfigure-the-service-to-start-automatically-using-the-services-desktop-app"></a>若要重新設定來啟動服務的傳統型應用程式會使用自動服務  
  
1.  開啟服務的傳統型應用程式。 (按一下**開始**、 按一下 搜尋 方塊中，開始鍵入**Services**，然後按一下結果清單中的 服務。  
  
2.  在 [詳細資料] 窗格中，以滑鼠右鍵按一下**HYPER-V 虛擬機器管理**，然後按一下**屬性**。  
  
3.  在 **一般**索引標籤中，於**啟動**輸入，再按一下**自動**。  
  
#### <a name="to-reconfigure-the-service-to-start-automatically-using-the-sc-config-command"></a>若要重新設定此服務可以自動使用 SC Config 命令  
  
1.  開啟 Windows PowerShell。  
  
2.  輸入：  
  
    ```  
    set-service  vmms -startuptype automatic  
    ```  
  
3.  如果服務不正在執行，輸入：  
  
    ```  
    start-service -name vmms  
    ```  
  


