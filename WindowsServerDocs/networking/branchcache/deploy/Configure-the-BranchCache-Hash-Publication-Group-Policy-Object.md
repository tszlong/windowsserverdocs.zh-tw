---
title: 設定 BranchCache Hash 發行群組原則物件
description: 本主題是 BranchCache 部署節目表適用於 Windows Server 2016 的示範如何將 BranchCache 部署最佳化分公司 WAN 頻寬分散與裝載快取模式中的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da74fea7-52b2-4d6d-9d21-19184eedbe3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6f528c9f0a8a39b286ad7ac4fa728d93c311f779
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-branchcache-hash-publication-group-policy-object"></a>設定 BranchCache Hash 發行群組原則物件

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用此程序，讓您新增到您的組織單位的所有檔案伺服器都已經套用到他們的相同 hash 發行原則設定設定 BranchCache hash 發行群組原則物件 (GPO)。  
  
資格在**網域系統管理員**，或相當於的最低需求才能執行此程序。  
  
> [!NOTE]  
> 之前，請先執行此程序，BranchCache 檔案伺服器組織單位，您必須先建立組織單位，將檔案伺服器，並建立 GPO BranchCache hash 發行。 如需詳細資訊，請查看[讓 Hash 發行網域成員檔案伺服器的](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md)。  
  
### <a name="to-configure-the-branchcache-hash-publication-group-policy-object"></a>若要設定 BranchCache hash 發行群組原則物件  
  
1.  Windows PowerShell 系統管理員身分執行，輸入**mmc**，然後按 ENTER 鍵。 Microsoft Management Console (MMC) 開啟。  
  
2.  在 MMC，在**檔案**功能表上，按**新增/移除嵌入式管理單元**。 **中新增或移除嵌入式管理單元**對話方塊。  
  
3.  在**中新增或移除嵌入式管理單元**，請在**可用嵌入式管理單元**，按兩下 [**群組原則管理**，然後按一下 [ **[確定]**。  
  
4.  在群組原則管理 MMC 中，展開 BranchCache hash 發行 GPO 您先前建立的路徑。 例如樹系稱為 example.com，如果您的網域名稱為 example1.com，，為您 GPO **BranchCache Hash 發行**，展開下列路徑：**群組原則管理**、**樹系：example.com**、**網域**，**example1.com**，**群組原則物件**、**BranchCache Hash 發行**。  
  
5.  以滑鼠右鍵按一下**BranchCache Hash 發行**GPO 和**編輯**。 群組原則編輯器] 管理主控台開啟。  
  
6.  在群組原則編輯器] 管理主控台中，展開下列路徑：**電腦設定**，**原則**、**系統管理範本]**、**網路**，**Lanman 伺服器**。  
  
7.  在群組原則編輯器] 管理主控台中，按一下 [ **Lanman 伺服器**。 在詳細資料窗格中，按兩下 [**適用於 BranchCache Hash 發行**。 **適用於 BranchCache Hash 發行**對話方塊。  
  
8.  在**Hash 發行 BranchCache 的**對話方塊中，按**啟用**。  
  
9. 在**選項**，按一下 [**允許 hash 發行的所有共用資料夾**，然後按一下 [下列其中一個動作：  
  
    1.  若要的所有檔案伺服器您新增到組織單位 hash 發行的所有共用資料夾，按一下 [**允許 hash 發行的所有共用資料夾**。  
  
    2.  若要讓 hash 發行只 BranchCache 支援的共用資料夾，按一下 [**允許 hash 發行 BranchCache 支援的共用資料夾的僅適用於**。  
  
    3.  若要禁止 hash 發行，針對所有共用即使 BranchCache 支援的檔案共用的電腦上的資料夾，按一下 [**不允許 hash 發行，所有的共用資料夾**。  
  
10. 按一下**[確定]**。  
  
> [!NOTE]  
> 在大部分案例中，您必須儲存 MMC 主控台並重新整理] 檢視顯示您所做的設定變更。  
  


