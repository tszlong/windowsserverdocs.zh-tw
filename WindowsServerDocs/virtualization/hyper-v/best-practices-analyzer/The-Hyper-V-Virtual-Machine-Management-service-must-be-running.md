---
title: 必須執行的 HYPER-V 虛擬機器管理服務
description: 提供指示來解決此 Best Practices Analyzer 規則所回報的問題。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: f44d6887-6458-4438-9d93-574587e3f7d1
author: KBDAzure
ms.date: 10/03/2016
ms.openlocfilehash: 58886b68ca30ddeb064fc12c6cb4c00183399715
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826109"
---
# <a name="the-hyper-v-virtual-machine-management-service-must-be-running"></a>必須執行的 HYPER-V 虛擬機器管理服務

>適用於：Windows Server 2016
  
如需最佳做法和掃描的詳細資訊，請參閱 [最佳做法分析程式](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|錯誤|  
|**分類**|先決條件|  

在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。

## <a name="issue"></a>問題  
  
*管理虛擬機器所需的服務不在執行中。*  
  
## <a name="impact"></a>影響  
  
*可以不執行任何虛擬機器管理作業。*  
  
正在執行的虛擬機器將會繼續執行。 不過，您無法再管理虛擬機器，或建立或刪除它們，直到服務正在執行。  
  
## <a name="resolution"></a>解析度  
  
*使用服務嵌入式管理單元或 Sc config 命令列工具來重新設定為自動啟動服務。*  
  
> [!TIP]  
> 如果您找不到服務中的傳統型應用程式或命令列工具會報告該服務不存在，HYPER-V 管理工具可能未安裝。 所以，如果您沒有看到 [HYPER-V] MMC 主控台，從 [開始] 功能表，您應該安裝 HYPER-V 管理工具。

若要安裝 HYPER-V 管理工具：  
>   
> - 在 Windows 伺服器上，開啟 伺服器管理員，並使用 新增角色及功能精靈。 如需詳細資訊，請參閱 < [Windows Server 2016 上安裝 HYPER-V 角色](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md)。  您也可以使用 PowerShell 來安裝工具 (`Install-WindowsFeature -Name Hyper-V-Tools, Hyper-V-PowerShell`) 
> - 在 Windows 中，從 Desktop，開始輸入**程式**，按一下**程式和功能**（控制台） >**開啟或關閉開啟的 Windows 功能** >  **Hyper V** > **HYPER-V 管理工具**。 然後，按一下**確定**。  
  
### <a name="to-reconfigure-the-service-to-start-automatically-using-the-services-desktop-app"></a>若要重新設定來啟動服務的傳統型應用程式會使用自動服務  
  
1.  開啟服務的傳統型應用程式。 (按一下**開始**，按一下**開始搜尋**方塊中，輸入**services.msc**，然後按 ENTER 鍵。)  
  
2.  在 [詳細資料] 窗格中，以滑鼠右鍵按一下**HYPER-V 虛擬機器管理**，然後按一下**屬性**。  
  
3.  在 **一般**索引標籤中，於**啟動**輸入，再按一下**自動**。  
  
4.  若要啟動服務，請按一下**啟動**。  
  
### <a name="to-reconfigure-the-service-to-start-automatically-using-sc-config"></a>若要重新設定此服務可以自動使用 SC Config  
  
1.  開啟 Windows PowerShell。 (從桌面上，按一下**開始**，並開始輸入**Windows PowerShell**。)  
  
2.  以滑鼠右鍵按一下**Windows PowerShell**然後按一下**系統管理員身分執行**。  
  
3.  若要重新設定服務，請輸入：  
  
    ```  
    sc config vmms start=auto  
    ```  
  
4.  若要啟動服務，請輸入：  
  
    ```  
    sc start vmms  
    ```  
  
如果服務已設定為自動啟動，而且您只需要重新啟動服務，您可以從 HYPER-V 管理員 中，或如上所示的"sc start vmms"命令的執行。  
  
#### <a name="to-restart-the-service-from-hyper-v-manager"></a>重新啟動服務，從 HYPER-V 管理員  
  
1.  開啟 \[Hyper-V 管理員\]。 按一下 [開始]，指向 [系統管理工具]，然後按一下 [Hyper-V 管理員]。  
  
2.  如果尚未選取，請在 [導覽] 窗格中，按一下伺服器的名稱。  
  
3.  在 **動作**窗格中，按一下**啟動服務**。  
  


