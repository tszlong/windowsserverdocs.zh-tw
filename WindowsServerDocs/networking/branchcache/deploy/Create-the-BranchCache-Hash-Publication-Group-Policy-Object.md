---
title: 建立 BranchCache Hash 發行群組原則物件
description: 本主題是 BranchCache 部署節目表適用於 Windows Server 2016 的示範如何將 BranchCache 部署最佳化分公司 WAN 頻寬分散與裝載快取模式中的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: c3d33bed-83ef-4eb8-acf9-0719ecb4a931
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4510d13c806adc2db46dfccce02a5a1b2814a2c4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="create-the-branchcache-hash-publication-group-policy-object"></a>建立 BranchCache Hash 發行群組原則物件

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用此程序建立 BranchCache hash 發行群組原則物件 (GPO)。  
  
資格在**網域系統管理員**，或相當於的最低需求才能執行此程序。  
  
> [!NOTE]  
> 之前，請先執行此程序，您必須建立 BranchCache 檔案伺服器組織單位，並將檔案伺服器組織單位。 如需詳細資訊，請查看[讓 Hash 發行網域成員檔案伺服器的](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md)。  
  
### <a name="to-create-the-branchcache-hash-publication-group-policy-object"></a>若要建立 BranchCache hash 發行群組原則物件  
  
1.  開放的 Windows PowerShell，輸入**mmc**，然後按 ENTER 鍵。 Microsoft Management Console (MMC) 開啟。  
  
2.  在 MMC，在**檔案**功能表上，按**新增/移除嵌入式管理單元**。 **中新增或移除嵌入式管理單元**對話方塊。  
  
3.  在**中新增或移除嵌入式管理單元**，請在**可用嵌入式管理單元**，按兩下 [**群組原則管理**，然後按一下 [ **[確定]**。  
  
4.  在群組原則管理 MMC 中，展開 BranchCache 檔案伺服器您先前建立組織單位路徑。 例如，如果 example.com 名為您的樹系、您的網域名稱為 example1.com，並 BranchCache 檔案伺服器名為您的組織單位，展開下列路徑：**群組原則管理**，**樹系：example.com**、**網域**、**example1.com**、**BranchCache 檔案伺服器**。  
  
5.  以滑鼠右鍵按一下**BranchCache 檔案伺服器**，然後按**在這個網域中建立 GPO 並連結到**。 **新的 GPO**對話方塊。 在**名稱**，輸入新的 GPO 的名稱。 例如，如果您想要為物件 BranchCache Hash 發行，輸入**BranchCache Hash 發行**。 按一下**[確定]**。  
  


