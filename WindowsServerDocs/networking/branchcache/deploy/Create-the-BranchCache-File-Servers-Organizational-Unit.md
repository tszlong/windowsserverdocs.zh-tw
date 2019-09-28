---
title: 建立 BranchCache 檔案伺服器組織單位
description: 本主題是 Windows Server 2016 的 BranchCache 部署指南的一部分，示範如何在分散式和託管快取模式中部署 BranchCache，以優化分公司的 WAN 頻寬使用量
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 2cda192f-6b45-4e6c-88d9-70ca179ddb94
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f058dbec42997f22106666b014508191a8861694
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406491"
---
# <a name="create-the-branchcache-file-servers-organizational-unit"></a>建立 BranchCache 檔案伺服器組織單位

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用此程式，在適用于 BranchCache 檔案伺服器的 Active Directory Domain Services （AD DS）中建立組織單位（OU）。  
  
若要執行此程序，至少需要 **Domain Admins** 的成員資格或同等權限。  
  
### <a name="to-create-the-branchcache-file-servers-organizational-unit"></a>建立 BranchCache 檔案伺服器組織單位  
  
1.  在安裝 AD DS 的電腦上，按一下伺服器管理員中的 [**工具**]，然後按一下 [ **Active Directory 使用者和電腦**]。 [Active Directory 使用者和電腦] 主控台隨即開啟。  
  
2.  在 [Active Directory 使用者和電腦] 主控台中，以滑鼠右鍵按一下您要新增 OU 的網域。 例如，如果您的網功能變數名稱為 example.com，請以滑鼠右鍵按一下 [ **example.com**]。 指向 [新增]，然後按一下 [組織單位]。 [**新物件-組織單位**] 對話方塊隨即開啟。  
  
3.  在 [**新物件-組織單位**] 對話方塊的 [**名稱**] 中，輸入新 OU 的名稱。 例如，如果您想要將 OU BranchCache 檔案伺服器命名為，請輸入**BranchCache 檔案伺服器**，然後按一下 **[確定]** 。  
  


