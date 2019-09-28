---
title: 執行中或已設定的虛擬機器數目必須在支援的限制內
description: 提供解決此最佳做法分析程式規則所回報之問題的指示。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9d3c4aa3-8416-46ec-a253-26dc98088d7b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 56d7fd528d7fda20dbdbb16a6262bb072f053ef0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364634"
---
# <a name="the-number-of-running-or-configured-virtual-machines-must-be-within-supported-limits"></a>執行中或已設定的虛擬機器數目必須在支援的限制內

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|Error  
|**分類**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的文字。  
  
## <a name="issue"></a>問題  
*有更多虛擬機器正在執行或設定為支援。*  
  
## <a name="impact"></a>影響  
*Microsoft 不支援在這部伺服器上執行或設定的目前虛擬機器數目。*  
  
## <a name="resolution"></a>解析度  
*將一或多部虛擬機器移至另一部伺服器。*  
  
如需 Hyper-v 支援的最大設定（例如執行中虛擬機器的數目）的詳細資訊，請參閱[規劃 Windows Server 2016 中的 hyper-v 擴充性](../plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md)。  
  
若要將虛擬機器移至另一部伺服器，您可以：  
  
- 從目前的伺服器匯出虛擬機器，然後將它匯入到新的伺服器，如下所述。   
- 進行即時移轉：   
    - 如果此伺服器屬於容錯移轉叢集，請使用容錯移轉叢集功能所提供的工具。 如需指示，請參閱[即時移轉、快速遷移或將虛擬機器從節點移至節點](https://go.microsoft.com/fwlink/?LinkID=181519)。  
    - 如果這是獨立伺服器，請參閱[設定即時移轉和遷移虛擬機器](https://technet.microsoft.com//library/jj134199(v=ws.11).aspx)中的指示，而不使用容錯移轉叢集  
  
### <a name="to-export-a-virtual-machine"></a>若要匯出虛擬機器  
  
   > [!IMPORTANT]  
   > 如果您要匯出的來源 Hyper-v 主機屬於網域，而您想要將匯出的檔案儲存在遠端位置，則必須設定 Hyper-v 主機以進行限制委派。 遠端位置可能是共用網路資料夾，或是您要匯入之主機上的資料夾。 限制委派可讓 Hyper-v 主機的電腦帳戶在遠端電腦上提供一般網際網路檔案系統（CIFS）服務的委派認證。 如需設定限制委派的指示，請參閱下面的匯出和匯入指示一節。  
  
1.  開啟 \[Hyper-V 管理員\]。 按一下 [開始]，指向 [系統管理工具]，然後按一下 [Hyper-V 管理員]。  
  
2.  在 [結果] 窗格的 [**虛擬機器**] 底下，以滑鼠右鍵按一下虛擬機器，然後按一下 [**匯出**]。  
  
3.  在 [**匯出虛擬機器**] 對話方塊中，輸入或流覽至具有足夠可用空間的位置，以儲存所有虛擬機器資源。 當您匯出虛擬機器時，與虛擬機器相關聯的所有虛擬硬碟（.vhd 檔案或 .vhdx 檔案）、檢查點（.avhd 檔案）和儲存的狀態檔案都會複製到指定的資料夾。  
  
4.  按一下 [匯出]。  
  
匯出虛擬機器之後，將虛擬機器匯入到其他伺服器。  
  
### <a name="to-import-a-virtual-machine-to-another-server"></a>將虛擬機器匯入至另一部伺服器  
  
1.  連線到執行 Hyper-v 的伺服器，並開啟 [Hyper-v 管理員]。  
  
2.  在 [**動作**] 窗格中，按一下 [匯**入虛擬機器**]。  
  
3.  在 [匯**入虛擬機器**] 對話方塊中，指定您匯出虛擬機器的位置。 除非您想要重新匯入此虛擬機器，否則請保留 [匯入設定]。  
  
4.  按一下 [匯入]。  
  
### <a name="to-configure-constrained-delegation"></a>設定限制委派  
  
需要網域系統**管理員**群組中的成員資格，才能完成此程式。  
  
1.  在安裝了 [Active Directory Domain Services 工具] 功能的電腦上，在 [系統**管理工具**] 中開啟 [ **Active Directory 使用者和電腦**]，然後流覽至執行 hyper-v 之電腦的電腦帳戶。  
  
    > [!NOTE]  
    > 若未列出[Active Directory 使用者和電腦]，請安裝「Active Directory 網域服務工具」功能。 如需指示，請參閱[安裝 AD DS 的遠端伺服器管理工具](https://go.microsoft.com/fwlink/?LinkId=140463)（ https://go.microsoft.com/fwlink/?LinkId=140463) 。  
  
2.  在執行 Hyper-v 之電腦的電腦帳戶上按一下滑鼠右鍵，然後按一下 [內容 **]。**  
  
3.  在 [委派] 索引標籤上，按一下 [選取這台電腦，但只委派指定的服務]，然後再按一下 [使用任何授權通訊協定]。  
  
4.  若要允許 Hyper-v 電腦帳戶向遠端電腦出示委派的認證：  
  
    1.  按一下 [新增]。  
  
    2.  在 [**新增服務**] 對話方塊中，按一下 [**使用者或電腦**]，選取遠端電腦，然後按一下 **[確定]** 。  
  
    3.  在 [**可用的服務**] 清單中，選取 [ **cifs** ] 通訊協定（也稱為伺服器訊息區（SMB）通訊協定），然後按一下 [**新增**]。  
  
  
  


