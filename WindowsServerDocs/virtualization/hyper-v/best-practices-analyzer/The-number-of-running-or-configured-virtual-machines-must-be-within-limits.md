---
title: 執行數目，或者設定的虛擬機器必須是支援的限制範圍內
description: 提供指示來解決此 Best Practices Analyzer 規則所回報的問題。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9d3c4aa3-8416-46ec-a253-26dc98088d7b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8a971a48b2d8199a6c279f1bd3f1715039fa6e0d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855349"
---
# <a name="the-number-of-running-or-configured-virtual-machines-must-be-within-supported-limits"></a>執行數目，或者設定的虛擬機器必須是支援的限制範圍內

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|錯誤  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的文字。  
  
## <a name="issue"></a>問題  
*更多虛擬機器是執行中或已設定多於受支援。*  
  
## <a name="impact"></a>影響  
*Microsoft 不支援目前的執行，或設定此伺服器上的虛擬機器數目。*  
  
## <a name="resolution"></a>解析度  
*一或多個虛擬機器移到另一部伺服器。*  
  
如需 HYPER-V，例如執行中的虛擬機器，數目最大支援的組態詳細資訊，請參閱[規劃 Windows Server 2016 中的 HYPER-V 延展性](../plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md)。  
  
若要將虛擬機器移到另一部伺服器中，您可以：  
  
- 匯出虛擬機器從目前的伺服器，再匯入至新的伺服器，如下所述。   
- 執行即時移轉：   
    - 如果此伺服器屬於容錯移轉叢集，使用 [容錯移轉叢集] 功能提供的工具。 如需相關指示，請參閱 <<c0> [ 即時移轉、 快速移轉或移動的虛擬機器從節點到節點](https://go.microsoft.com/fwlink/?LinkID=181519)。  
    - 如果這是獨立伺服器，請參閱指示[設定即時移轉和移轉虛擬機器，而不需要容錯移轉叢集](https://technet.microsoft.com//library/jj134199(v=ws.11).aspx)  
  
### <a name="to-export-a-virtual-machine"></a>若要匯出虛擬機器  
  
   > [!IMPORTANT]  
   > 如果您要匯出從 HYPER-V 主機隸屬於網域，而且您想要儲存匯出的檔案，在遠端位置，就必須設定 HYPER-V 主機進行限制委派。 在共用的網路資料夾或您打算匯入至主機上的資料夾，可能是遠端位置。 限制的委派可讓委派的認證為 Common Internet File System (CIFS) 服務提供到遠端電腦為 HYPER-V 主機的電腦帳戶。 如需設定限制的委派的指示，請參閱下列匯出一節並匯入的指示，如下。  
  
1.  開啟 \[Hyper-V 管理員\]。 按一下 [開始]，指向 [系統管理工具]，然後按一下 [Hyper-V 管理員]。  
  
2.  在 結果 窗格中，在**虛擬機器**，以滑鼠右鍵按一下 虛擬機器，然後按一下**匯出**。  
  
3.  在 [**匯出虛擬機器**] 對話方塊中，輸入或瀏覽至具有足夠的可用空間來儲存所有虛擬機器資源的位置。 當您匯出虛擬機器時，所有的虛擬硬碟 （.vhd 檔案或.vhdx 檔案）、 檢查點 （.avhd 檔） 和虛擬機器相關聯的儲存的狀態檔案會複製到指定的資料夾中。  
  
4.  按一下 **匯出**。  
  
匯出虛擬機器之後, 匯入到其他伺服器的虛擬機器。  
  
### <a name="to-import-a-virtual-machine-to-another-server"></a>若要匯入至另一部伺服器的虛擬機器  
  
1.  連接到執行 HYPER-V 的伺服器，並開啟 [HYPER-V 管理員]。  
  
2.  在 **動作**窗格中，按一下**匯入虛擬機器**。  
  
3.  在 **匯入虛擬機器**對話方塊方塊中，指定您要匯出虛擬機器的所在位置。 除非您想要重新匯入此虛擬機器，因為它們是保留匯入設定。  
  
4.  按一下 **匯入**。  
  
### <a name="to-configure-constrained-delegation"></a>設定限制委派  
  
中的成員資格**網域系統管理員**群組，才能完成此程序。  
  
1.  已安裝，在 Active Directory 網域服務工具功能的電腦上**系統管理工具**，開啟**Active Directory 使用者和電腦**，然後瀏覽至的電腦帳戶執行 HYPER-V 的電腦。  
  
    > [!NOTE]  
    > 若未列出 [Active Directory 使用者和電腦]，請安裝「Active Directory 網域服務工具」功能。 如需相關指示，請參閱 <<c0> [ 安裝遠端伺服器管理工具適用於 AD DS](https://go.microsoft.com/fwlink/?LinkId=140463) (https://go.microsoft.com/fwlink/?LinkId=140463)。  
  
2.  以滑鼠右鍵按一下執行 HYPER-V 之電腦的電腦帳戶，然後按一下**屬性**。  
  
3.  在 [委派] 索引標籤上，按一下 [選取這台電腦，但只委派指定的服務]，然後再按一下 [使用任何授權通訊協定]。  
  
4.  若要允許 HYPER-V 電腦帳戶，以顯示遠端電腦的委派的認證：  
  
    1.  按一下 **\[新增\]**。  
  
    2.  在 [**新增服務**] 對話方塊中，按一下**使用者或電腦**，選取 [遠端的電腦]，然後按一下 **[確定]**。  
  
    3.  在 **可用的服務**清單中，選取**cifs**通訊協定 （也稱為伺服器訊息區塊 (SMB) 通訊協定），，然後按一下**新增**。  
  
  
  


