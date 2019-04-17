---
title: 安裝新的檔案伺服器為內容伺服器
description: 本主題是 BranchCache 部署節目表適用於 Windows Server 2016 的示範如何將 BranchCache 部署最佳化分公司 WAN 頻寬分散與裝載快取模式中的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 1f49fc3c-28a6-4d3d-b787-1be9e61e792f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 53937cfc139efc6df5facfa872e63609229a548c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="install-a-new-file-server-as-a-content-server"></a>安裝新的檔案伺服器為內容伺服器

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用此程序安裝檔案服務伺服器角色與**BranchCache 網路檔案的**執行 Windows Server 2016 的電腦上的角色服務。  
  
資格在**系統管理員**，或相當於的最低需求才能執行此程序。  
  
> [!NOTE]  
> 使用 Windows PowerShell，系統管理員的身分，以執行 Windows PowerShell 來執行這個程序，Windows PowerShell 命令提示字元中，輸入下列命令，然後按 ENTER 鍵。  
>   
> `Install-WindowsFeature FS-BranchCache -IncludeManagementTools`  
>   
> `Restart-Computer`  
>   
> 若要安裝資料 Deduplication 角色服務，輸入下列命令，，然後按 ENTER 鍵。  
>   
> `Install-WindowsFeature FS-Data-Deduplication -IncludeManagementTools`  
  
### <a name="to-install-file-services-and-the-branchcache-for-network-files-role-service"></a>若要安裝檔案服務與 BranchCache 網路的檔案角色服務  
  
1.  在伺服器管理員中，按一下**管理**，然後按**新增角色與功能**。 新增角色與功能精靈開啟。 在**在您開始之前**，按一下 [**下**。  
  
2.  在**選擇安裝類型**，確認**以角色為基礎，或為基礎的功能的安裝**已選取，然後按一下 [**下一步**。  
  
3.  在**選擇目的伺服器**，確定正確的伺服器已選取，然後按**下一步**。  
  
4.  在**選擇伺服器角色**，請在**角色**，請注意，**檔案與儲存空間服務**已安裝的角色。按一下箭頭左邊的角色名稱，以展開角色服務，在選取範圍，然後按一下 [左邊的箭頭**檔案和 iSCSI 服務**。  
  
5.  選取核取方塊**檔案伺服器**和**的檔案網路 BranchCache**。  
  
    > [!TIP]  
    > 我們建議您，您也可以選取核取方塊的**資料 Deduplication**。
  
    按一下**下一步**。  
  
6.  在**選擇功能**，按一下 [**下**。  
  
7.  在**確認安裝選項**，檢視您的選項，然後按**安裝**。 **安裝進度**窗格中會顯示在安裝期間。 安裝完成時，請按一下**關閉**。
