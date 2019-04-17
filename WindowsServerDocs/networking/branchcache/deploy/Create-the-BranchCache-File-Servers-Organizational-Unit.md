---
title: 建立組織單位 BranchCache 檔案伺服器
description: 本主題是 BranchCache 部署節目表適用於 Windows Server 2016 的示範如何將 BranchCache 部署最佳化分公司 WAN 頻寬分散與裝載快取模式中的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 2cda192f-6b45-4e6c-88d9-70ca179ddb94
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a92cb3110e4aecb1ef09a45ed14173305722c655
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="create-the-branchcache-file-servers-organizational-unit"></a>建立組織單位 BranchCache 檔案伺服器

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用此程序建立組織單位（組織單位）中 BranchCache 檔案伺服器的 Active Directory Domain Services (AD DS)。  
  
資格在**網域系統管理員**，或相當於的最低需求才能執行此程序。  
  
### <a name="to-create-the-branchcache-file-servers-organizational-unit"></a>若要建立 BranchCache 檔案伺服器組織單位  
  
1.  安裝的電腦的 AD DS，在伺服器管理員中，按一下 [**工具**，然後按一下 [ **Active Directory 使用者和電腦**。 Active Directory 使用者和電腦主機開啟。  
  
2.  在 Active Directory 使用者和電腦主控台中，以滑鼠右鍵按一下您要將組織單位加入的網域。 例如，如果您的網域名稱為 example.com，以滑鼠右鍵按一下**example.com**。指向 [**新增]**，然後按一下 [**組織單位**。 **新物件-單位**對話方塊。  
  
3.  在**新物件-單位**對話方塊中，在**名稱**，輸入新的組織單位的名稱。 如果您想要組織單位 BranchCache 檔案伺服器的名稱，例如，輸入**BranchCache 檔案伺服器**，然後按**[確定]**。  
  


