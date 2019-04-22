---
title: 將新的檔案伺服器作為內容伺服器安裝
description: 本主題是 BranchCache 部署指南的 Windows Server 2016 中，示範如何以最佳化 WAN 頻寬使用量，在分公司的分散式和裝載式快取模式部署 BranchCache 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 1f49fc3c-28a6-4d3d-b787-1be9e61e792f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e3a4dbe5339685b385b0157756379e9e545f1964
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812019"
---
# <a name="install-a-new-file-server-as-a-content-server"></a>將新的檔案伺服器作為內容伺服器安裝

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用此程序來安裝檔案服務伺服器角色和**網路檔案的 BranchCache**執行 Windows Server 2016 的電腦上的角色服務。  
  
中的成員資格**系統管理員**，或同等權限才能執行此程序的最小值。  
  
> [!NOTE]  
> 使用 Windows PowerShell，以系統管理員身分執行 Windows PowerShell 執行此程序的 Windows PowerShell 提示字元中，輸入下列命令，然後按 ENTER 鍵。  
>   
> `Install-WindowsFeature FS-BranchCache -IncludeManagementTools`  
>   
> `Restart-Computer`  
>   
> 若要安裝 「 重複資料刪除 」 角色服務，請輸入下列命令，，然後按 ENTER 鍵。  
>   
> `Install-WindowsFeature FS-Data-Deduplication -IncludeManagementTools`  
  
### <a name="to-install-file-services-and-the-branchcache-for-network-files-role-service"></a>若要安裝檔案服務與網路檔案 」 角色服務的 BranchCache  
  
1.  在 [伺服器管理員] 中，按一下 [**管理**]，然後按一下 [**新增角色及功能**]。 [新增角色及功能精靈] 隨即開啟。 在 [在您開始前] 中，按 [下一步]。  
  
2.  在 [**選取安裝類型**，請確認**角色型或功能型安裝**已選取，然後按一下**下一步]**。  
  
3.  在 [**選取目的地伺服器**，確定正確的伺服器已選取，然後按一下**下一步]**。  
  
4.  在**選取伺服器角色**，請在**角色**，注意**檔案與儲存體服務**已安裝角色並按一下 角色名稱，以展開左邊的箭號選取的角色服務，然後按一下左側的箭號**檔案和 iSCSI 服務**。  
  
5.  選取核取方塊**檔案伺服器**並**網路檔案的 BranchCache**。  
  
    > [!TIP]  
    > 建議您也選取核取方塊**重複資料刪除**。
  
    按一下 [下一步] 。  
  
6.  在 [**選取的功能**，按一下**下一步]**。  
  
7.  在 **確認安裝選項**，檢閱您的選擇，然後按一下**安裝**。 **Průběh instalace**窗格會顯示在安裝期間。 安裝完成時，按一下**關閉**。
