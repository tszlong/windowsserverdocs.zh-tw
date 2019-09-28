---
title: 啟用非網域成員檔案伺服器的雜湊發行
description: 本主題是 Windows Server 2016 的 BranchCache 部署指南的一部分，示範如何在分散式和託管快取模式中部署 BranchCache，以優化分公司的 WAN 頻寬使用量
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 11584b73-f9e2-4530-afa5-b8df970e6b24
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e870863b497c17b4b56265d99d91274e34690767
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356543"
---
# <a name="enable-hash-publication-for-non-domain-member-file-servers"></a>啟用非網域成員檔案伺服器的雜湊發行

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用此程式，在執行 Windows Server 2016 的檔案伺服器上，使用已安裝檔案服務伺服器角色的**BranchCache For Network Files**角色服務來設定群組原則 branchcache 的雜湊發行。  
  
此程式的目的是要在非網域成員檔案伺服器上使用。 如果您在網域成員檔案伺服器上執行此程式，而且也使用網域群組原則來設定 BranchCache，網域群組原則設定會覆寫本機群組原則設定。  
  
若要執行此程式，至少需要**Administrators**的成員資格或同等許可權。  
  
> [!NOTE]  
> 如果您有一或多個網域成員檔案伺服器，您可以將它們新增至 Active Directory Domain Services 中的組織單位（OU），然後使用群組原則一次設定所有檔案伺服器的雜湊發行，而不是個別設定每個檔案伺服器。 如需詳細資訊，請參閱[啟用網域成員檔案伺服器的雜湊發行](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md)。  
  
### <a name="to-enable-hash-publication-for-one-file-server"></a>啟用一部檔案伺服器的雜湊發行  
  
1.  開啟 Windows PowerShell，輸入 **mmc**，然後按 ENTER 鍵。 此時會開啟 Microsoft Management Console (MMC)。  
  
2.  在**MMC 的 [檔案] 功能表上**，按一下 [**新增/移除嵌入式管理單元**]。 [**新增或移除嵌入式管理單元**] 對話方塊隨即開啟。  
  
3.  在 [**新增或移除嵌入式管理單元**] 的 [可用的嵌入式**管理**單元] 中，按兩下 [**群組原則物件編輯器**]。 [群組原則 Wizard] 隨即開啟，並選取 [本機電腦] 物件。 按一下 [完成]，然後按一下 [確定]。  
  
4.  在本機群組原則編輯器 MMC 中，展開下列路徑：**本機電腦原則**、**電腦**設定、**系統管理範本**、**網路**、 **Lanman 伺服器**。 按一下 [ **Lanman 伺服器**]。  
  
5.  在詳細資料窗格中，按兩下 [ **BranchCache 的雜湊發行**]。 [ **BranchCache 的雜湊發行**] 對話方塊隨即開啟。  
  
6.  在 [ **BranchCache 的雜湊發行**] 對話方塊中，按一下 [**已啟用**]。  
  
7.  在 [**選項**] 中，**針對 [所有共用資料夾] 按一下 [允許雜湊發行**]，然後按一下下列其中一項：  
  
    1.  若要為這部電腦上的所有共用資料夾啟用雜湊發行，請按一下 [**允許所有共用資料夾的雜湊發行**]。  
  
    2.  若只要針對已啟用 BranchCache 的共用資料夾啟用雜湊發行，請按一下 [**僅允許啟用 branchcache 之共用資料夾的雜湊發行**]。  
  
    3.  若不允許電腦上所有共用資料夾的雜湊發行，即使已在檔案共用上啟用 BranchCache，也請按一下 [不**允許在所有共用資料夾上進行雜湊發行**]。  
  
8.  按一下 [確定]。  
  


