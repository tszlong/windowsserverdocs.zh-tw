---
title: 將檔案伺服器移到 BranchCache 檔案伺服器單位
description: 本主題是 BranchCache 部署節目表適用於 Windows Server 2016 的示範如何將 BranchCache 部署最佳化分公司 WAN 頻寬分散與裝載快取模式中的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 56c915ec-edb1-43b0-8ad2-c93841bb566f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 09afb4936545cb1f5bb14573261008ff18badd4d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>將檔案伺服器移到 BranchCache 檔案伺服器單位

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用此程序將 BranchCache 檔案伺服器（組織單位）組織單位，在 Active Directory Domain Services (AD DS)。  
  
資格在**網域系統管理員**，或相當於的最低需求才能執行此程序。  
  
> [!NOTE]  
> 之前您組織單位，使用此程序新增電腦帳號，您必須在 Active Directory 使用者及電腦主控台建立 BranchCache 檔案伺服器組織單位。 如需詳細資訊，請查看[建立 BranchCache 檔案伺服器組織單位](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md)。  
  
### <a name="to-move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>將檔案伺服器 BranchCache 檔案伺服器單位  
  
1.  安裝的電腦的 AD DS，在伺服器管理員中，按一下 [**工具**，然後按一下 [ **Active Directory 使用者和電腦**。 Active Directory 使用者和電腦主機開啟。  
  
2.  在 Active Directory 使用者和電腦主控台中，尋找 BranchCache 檔案伺服器、選取帳號，並再拖放在您先前建立組織單位 BranchCache 檔案伺服器上的電腦帳號左鍵。 例如，如果您先前建立組織單位名為**BranchCache 檔案伺服器**、拖放在電腦 account **BranchCache 檔案伺服器]**組織單位。  
  
3.  重複上一個步驟的網域中您想要移動到組織單位，每個 BranchCache 檔案伺服器。  
  


