---
title: 安裝 BranchCache 功能
description: 本主題是 Windows Server 2016 的 BranchCache 部署指南的一部分，示範如何在分散式和託管快取模式中部署 BranchCache，以優化分公司的 WAN 頻寬使用量
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 4f31dc61-2dbe-4c7e-b3f9-85ae49a45049
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5ee438ef57d3355cf19713d8574591aeea6ae06f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406438"
---
# <a name="install-the-branchcache-feature"></a>安裝 BranchCache 功能

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用此程式來安裝 BranchCache 功能，並在執行 Windows Server @ no__t-0 2016、Windows Server 2012 R2 或 Windows Server 2012 的電腦上啟動 BranchCache 服務。  
  
若要執行此程式，至少需要**Administrators**的成員資格或同等許可權。  
  
執行此程式之前，建議您先安裝和設定以 BITS 為基礎的應用程式或 Web 服務器。  
  
> [!NOTE]  
> 若要使用 Windows PowerShell 執行此程式，請以系統管理員身分執行 Windows PowerShell，在 Windows PowerShell 提示字元中輸入下列命令，然後按 ENTER。  
>   
> `Install-WindowsFeature BranchCache`  
>   
> `Restart-Computer`  
  
### <a name="to-install-and-enable-the-branchcache-feature"></a>安裝和啟用 BranchCache 功能  
  
1.  在 [伺服器管理員] 中，按一下 [**管理**]，然後按一下 [**新增角色及功能**]。 [新增角色及功能] wizard 隨即開啟。 按一下 [下一步]。  
  
2.  在 [**選取安裝類型**] 中，確定已選取 [以**角色為基礎] 或 [功能型安裝**]，然後按 **[下一步]** 。  
  
3.  在 [**選取目的地伺服器**] 中，確定已選取正確的伺服器，然後按 **[下一步]** 。  
  
4.  在 [選取伺服器角色] 中按 [下一步]。  
  
5.  在 [**選取功能**] 中，按一下 [ **BranchCache**]，然後按 **[下一步]** 。  
  
6.  在 [確認安裝選項] 中，按一下 [安裝]。 在 [**安裝進度**] 中，BranchCache 功能安裝會繼續進行。 安裝完成時，按一下 [**關閉**]。  
  
安裝 BranchCache 功能之後，BranchCache 服務（也稱為 PeerDistSvc）會啟用，且啟動類型為 [自動]。  
  


