---
title: 建立 BranchCache 檔案伺服器組織單位
description: 本主題是 BranchCache 部署指南的 Windows Server 2016 中，示範如何以最佳化 WAN 頻寬使用量，在分公司的分散式和裝載式快取模式部署 BranchCache 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 2cda192f-6b45-4e6c-88d9-70ca179ddb94
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b7b26ec5808f5b11141e81dc5e738c83c94ef6b8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874079"
---
# <a name="create-the-branchcache-file-servers-organizational-unit"></a>建立 BranchCache 檔案伺服器組織單位

>適用於：Windows Server （半年通道），Windows Server 2016

在 BranchCache 的檔案伺服器的 Active Directory 網域服務 (AD DS) 中，您可以建立組織單位 (OU) 使用此程序。  
  
若要執行此程序，至少需要 **Domain Admins** 的成員資格或同等權限。  
  
### <a name="to-create-the-branchcache-file-servers-organizational-unit"></a>若要建立 BranchCache 的檔案伺服器的組織單位  
  
1.  在 AD DS 安裝的位置，在 [伺服器管理員] 中，電腦上按一下**工具**，然後按一下**Active Directory 使用者和電腦**。 [Active Directory 使用者和電腦] 主控台隨即開啟。  
  
2.  在 [Active Directory 使用者和電腦] 主控台中，以滑鼠右鍵按一下您要新增 OU 的網域。 例如，如果您的網域名稱為 example.com，以滑鼠右鍵按一下**example.com**。 指向 [新增]，然後按一下 [組織單位]。 **新增物件-組織單位**對話方塊隨即開啟。  
  
3.  在 **新增物件-組織單位**對話方塊中，於**名稱**，輸入新 OU 的名稱。 例如，如果您想要命名 OU BranchCache 的檔案伺服器，輸入**BranchCache 的檔案伺服器**，然後按一下**確定**。  
  


