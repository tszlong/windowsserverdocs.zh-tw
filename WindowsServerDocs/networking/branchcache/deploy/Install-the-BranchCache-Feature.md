---
title: 安裝 BranchCache 功能
description: 本主題是 BranchCache 部署指南的 Windows Server 2016 中，示範如何以最佳化 WAN 頻寬使用量，在分公司的分散式和裝載式快取模式部署 BranchCache 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 4f31dc61-2dbe-4c7e-b3f9-85ae49a45049
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8b4aecd9e9355a6c2d5ac485ac77c76428fe295f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872189"
---
# <a name="install-the-branchcache-feature"></a>安裝 BranchCache 功能

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用此程序來安裝 BranchCache 功能，並啟動執行 Windows Server 的電腦上的 BranchCache 服務&reg;2016，Windows Server 2012 R2 或 Windows Server 2012。  
  
中的成員資格**系統管理員**或同等權限才能執行此程序的最小值。  
  
執行此程序之前，建議您安裝並設定您的 BITS 型應用程式或 Web 伺服器。  
  
> [!NOTE]  
> 使用 Windows PowerShell，以系統管理員身分執行 Windows PowerShell 執行此程序的 Windows PowerShell 提示字元中，輸入下列命令，然後按 ENTER 鍵。  
>   
> `Install-WindowsFeature BranchCache`  
>   
> `Restart-Computer`  
  
### <a name="to-install-and-enable-the-branchcache-feature"></a>若要安裝並啟用 BranchCache 功能  
  
1.  在 [伺服器管理員] 中，按一下 [**管理**]，然後按一下 [**新增角色及功能**]。 [新增角色及功能精靈] 隨即開啟。 按一下 [下一步] 。  
  
2.  在 [**選取安裝類型**，請確認**角色型或功能型安裝**已選取，然後按一下**下一步]**。  
  
3.  在 [**選取目的地伺服器**，確定正確的伺服器已選取，然後按一下**下一步]**。  
  
4.  在 [選取伺服器角色] 中按 [下一步]。  
  
5.  在 [**選取的功能**，按一下**BranchCache**，然後按一下**下一步]**。  
  
6.  在 [確認安裝選項] 中，按一下 [安裝]。 在  **průběh instalace**，BranchCache 功能的安裝程式將繼續。 安裝完成時，按一下**關閉**。  
  
安裝 BranchCache 功能之後，BranchCache 服務-也稱為 PeerDistSvc-已啟用，並啟動類型為 自動。  
  


