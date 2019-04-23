---
title: 將檔案伺服器移至 BranchCache 檔案伺服器組織單位
description: 本主題是 BranchCache 部署指南的 Windows Server 2016 中，示範如何以最佳化 WAN 頻寬使用量，在分公司的分散式和裝載式快取模式部署 BranchCache 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 56c915ec-edb1-43b0-8ad2-c93841bb566f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 037b354bb6725ac7f91fc323b81bbdf15d03ac15
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885899"
---
# <a name="move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>將檔案伺服器移至 BranchCache 檔案伺服器組織單位

>適用於：Windows Server （半年通道），Windows Server 2016

Active Directory 網域服務 (AD DS) 中，您可以加入組織單位 (OU) 中的 BranchCache 的檔案伺服器使用此程序。  
  
若要執行此程序，至少需要 **Domain Admins** 的成員資格或同等權限。  
  
> [!NOTE]  
> 您將電腦帳戶新增至的 OU 進行此程序之前，您必須建立 BranchCache 的檔案伺服器組織單位 [Active Directory 使用者和電腦] 主控台中。 如需詳細資訊，請參閱 <<c0> [ 建立 BranchCache 的檔案伺服器的組織單位](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md)。  
  
### <a name="to-move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>移至 BranchCache 的檔案伺服器檔案伺服器組織單位  
  
1.  在 AD DS 安裝的位置，在 [伺服器管理員] 中，電腦上按一下**工具**，然後按一下**Active Directory 使用者和電腦**。 [Active Directory 使用者和電腦] 主控台隨即開啟。  
  
2.  在 [Active Directory 使用者和電腦] 主控台中，找出 BranchCache 的檔案伺服器，以滑鼠左鍵按一下來選取帳戶，然後將拖曳並卸除 BranchCache 的檔案伺服器，您先前建立的 OU 上的電腦帳戶的電腦帳戶。 例如，如果您先前已建立名為 OU **BranchCache 的檔案伺服器**、 拖放的電腦帳戶上**BranchCache 的檔案伺服器**OU。  
  
3.  每個您想要移到 OU 的網域中的 BranchCache 檔案伺服器，重複上述步驟。  
  


