---
title: Hyper-v 虛擬機器管理服務必須正在執行
description: 瞭解當管理虛擬機器所需的服務未執行時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: f44d6887-6458-4438-9d93-574587e3f7d1
ms.date: 10/03/2016
ms.openlocfilehash: 1fc629002f17861c496e6aa6574cf2aead4baf46
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834343"
---
# <a name="the-hyper-v-virtual-machine-management-service-must-be-running"></a>Hyper-v 虛擬機器管理服務必須正在執行

>適用於：Windows Server 2016

如需最佳做法和掃描的詳細資訊，請參閱[最佳做法分析程式](https://go.microsoft.com/fwlink/?LinkId=122786)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|錯誤|
|**類別**|必要條件|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題

*管理虛擬機器所需的服務不在執行中。*

## <a name="impact"></a>影響

*無法執行任何虛擬機器管理作業。*

正在執行的虛擬機器將會繼續執行。 不過，在服務執行之前，您將無法管理虛擬機器，也無法建立或刪除虛擬機器。

## <a name="resolution"></a>解決方案

*使用 [服務] 嵌入式管理單元或 Sc config 命令列工具，將服務重新設定為自動啟動。*

> [!TIP]
> 如果您在桌面應用程式中找不到服務，或命令列工具報表服務不存在，可能是未安裝 Hyper-v 管理工具。
如果您看不到 [開始] 功能表的 Hyper-v MMC 主控台，您應該安裝 Hyper-v 管理工具。

若要安裝 Hyper-v 管理工具：
>
> - 在 Windows Server 上，開啟伺服器管理員，然後使用 [新增角色及功能] wizard。 如需詳細資訊，請參閱 [在 Windows Server 2016 上安裝 hyper-v 角色](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md)。  您也可以使用 PowerShell 來安裝工具 (`Install-WindowsFeature -Name Hyper-V-Tools, Hyper-V-PowerShell`) 
> - 在 Windows 上，從桌面開始輸入 [**程式**]，按一下 [**程式和功能**] (控制台) > 開啟或關閉 hyper-v   >    >  **hyper-v 管理工具** 上的 Windows 功能]。 然後按一下 **[確定]**。

### <a name="to-reconfigure-the-service-to-start-automatically-using-the-services-desktop-app"></a>若要將服務重新設定為使用服務桌面應用程式自動啟動

1.  開啟服務桌面應用程式。  (按一下 [ **開始**]，在 [ **開始搜尋** ] 方塊中按一下，輸入 **services.msc**，然後按 enter。 ) 

2.  在詳細資料窗格中，以滑鼠 **按右鍵 [** **Hyper-v 虛擬機器管理**]，然後按一下 [內容]。

3.  在 [ **一般** ] 索引標籤的 [ **啟動** 類型] 中，按一下 [ **自動**]。

4.  若要啟動服務，請按一下 [ **開始**]。

### <a name="to-reconfigure-the-service-to-start-automatically-using-sc-config"></a>若要將服務重新設定為使用 SC Config 自動啟動

1.  開啟 Windows PowerShell。 從桌面 (中，按一下 [ **開始** ]，然後開始輸入 **Windows PowerShell**。 ) 

2.  以滑鼠右鍵按一下 **Windows PowerShell** ，然後按一下 [以 **系統管理員身分執行**]。

3.  若要重新設定服務，請輸入：

    ```
    sc config vmms start=auto
    ```

4.  若要啟動服務，請輸入：

    ```
    sc start vmms
    ```

如果服務已設定為自動啟動，而您只需要重新開機服務，您可以從 Hyper-v 管理員或從上面顯示的 sc start vmms 命令來進行。

#### <a name="to-restart-the-service-from-hyper-v-manager"></a>從 Hyper-v 管理員重新開機服務

1.  開啟 Hyper-V 管理員。 按一下 [開始]，指向 [系統管理工具]，然後按一下 [Hyper-V 管理員]。

2.  在流覽窗格中，按一下伺服器的名稱（如果尚未選取）。

3.  在 [ **動作** ] 窗格中，按一下 [ **啟動服務**]。



