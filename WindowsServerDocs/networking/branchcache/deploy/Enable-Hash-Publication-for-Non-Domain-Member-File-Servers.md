---
title: 讓非網域成員檔案伺服器 Hash 發行
description: 本主題是 BranchCache 部署節目表適用於 Windows Server 2016 的示範如何將 BranchCache 部署最佳化分公司 WAN 頻寬分散與裝載快取模式中的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 11584b73-f9e2-4530-afa5-b8df970e6b24
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4cb3d217115cff3a9b30ee11acb7ba0de6672b24
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="enable-hash-publication-for-non-domain-member-file-servers"></a>讓非網域成員檔案伺服器 Hash 發行

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用此程序，設定 hash 發行 BranchCache 使用本機群組原則的電腦執行 Windows Server 2016 的檔案伺服器上的**BranchCache 網路檔案的**的安裝檔案服務伺服器角色角色服務。  
  
此程序適用於非網域成員檔案伺服器上。 如果您的網域成員檔案伺服器上執行此程序，您也可以設定使用群組原則網域 BranchCache 網域群組原則設定覆寫本機群組原則設定。  
  
資格在**系統管理員**，或相當於的最低需求才能執行此程序。  
  
> [!NOTE]  
> 如果您有一或多個網域成員檔案伺服器，您可以將它們新增到 Active Directory Domain Services 組織單位（組織單位），然後使用群組原則設定 hash 發行的所有檔案伺服器一次，而不是排列設定的每個檔案伺服器。 如需詳細資訊，請查看[讓 Hash 發行網域成員檔案伺服器的](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md)。  
  
### <a name="to-enable-hash-publication-for-one-file-server"></a>若要讓 hash 發行一個檔案伺服器  
  
1.  開放的 Windows PowerShell，輸入**mmc**，然後按 ENTER 鍵。 Microsoft Management Console (MMC) 開啟。  
  
2.  在 MMC，在**檔案**功能表上，按**新增/移除嵌入式管理單元**。 **中新增或移除嵌入式管理單元**對話方塊。  
  
3.  在**中新增或移除嵌入式管理單元**，請在**可用嵌入式管理單元**，按兩下 [**群組原則物件編輯器]**。 群組原則精靈開啟選取的本機電腦物件。 按一下**完成**，然後按**[確定]**。  
  
4.  在本機群組原則編輯器 MMC 中，展開下列路徑：**本機電腦原則**，**電腦設定**、**系統管理範本**、**網路**，**Lanman 伺服器**。 按一下**Lanman 伺服器**。  
  
5.  在詳細資料窗格中，按兩下 [**適用於 BranchCache Hash 發行**。 **適用於 BranchCache Hash 發行**對話方塊。  
  
6.  在**Hash 發行 BranchCache 的**對話方塊中，按**啟用**。  
  
7.  在**選項**，按一下 [**允許 hash 發行的所有共用資料夾**，然後按一下 [下列其中一個動作：  
  
    1.  若要讓 hash 發行的所有共用資料夾，這台電腦上，按一下 [**允許 hash 發行的所有共用資料夾**。  
  
    2.  若要讓 hash 發行只 BranchCache 支援的共用資料夾，按一下 [**允許 hash 發行 BranchCache 支援的共用資料夾的僅適用於**。  
  
    3.  若要禁止 hash 發行，針對所有共用即使 BranchCache 支援的檔案共用的電腦上的資料夾，按一下 [**不允許 hash 發行，所有的共用資料夾**。  
  
8.  按一下**[確定]**。  
  


