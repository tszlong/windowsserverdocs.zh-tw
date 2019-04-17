---
title: 安裝 BranchCache 功能
description: 本主題是 BranchCache 部署節目表適用於 Windows Server 2016 的示範如何將 BranchCache 部署最佳化分公司 WAN 頻寬分散與裝載快取模式中的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 4f31dc61-2dbe-4c7e-b3f9-85ae49a45049
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a69848536b56521da9b5ef07689aba7f8690e888
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-branchcache-feature"></a>安裝 BranchCache 功能

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用此程序安裝 BranchCache 功能，並開始執行 Windows Server 的電腦上的 BranchCache 服務&reg;2016、 Windows Server 2012 R2 或 Windows Server 2012。  
  
資格在**系統管理員**或相當於的最低需求才能執行此程序。  
  
在執行此程序之前，建議您安裝並設定您的位元為基礎的應用程式或 Web 伺服器。  
  
> [!NOTE]  
> 使用 Windows PowerShell，系統管理員的身分，以執行 Windows PowerShell 來執行這個程序，Windows PowerShell 命令提示字元中，輸入下列命令，然後按 ENTER 鍵。  
>   
> `Install-WindowsFeature BranchCache`  
>   
> `Restart-Computer`  
  
### <a name="to-install-and-enable-the-branchcache-feature"></a>安裝以及 BranchCache 功能  
  
1.  在伺服器管理員中，按一下**管理**，然後按**新增角色與功能**。 開啟精靈新增角色與功能]。 按一下**下一步**。  
  
2.  在**選擇安裝類型**，確認**以角色為基礎，或為基礎的功能的安裝**已選取，然後按一下 [**下一步**。  
  
3.  在**選擇目的伺服器**，確定正確的伺服器已選取，然後按**下一步**。  
  
4.  在**選擇伺服器角色**，按一下 [**下**。  
  
5.  在**選取功能**，按一下 [ **BranchCache**，然後按一下 [**下一步**。  
  
6.  在**確認安裝選項**，按一下 [**安裝**。 在**安裝進度**、 進行 BranchCache 功能安裝。 安裝完成時，請按一下**關閉**。  
  
您安裝 BranchCache 功能之後，BranchCache 服務-也稱為 PeerDistSvc-功能，並為 [自動] 的 [開始] 畫面類型。  
  


