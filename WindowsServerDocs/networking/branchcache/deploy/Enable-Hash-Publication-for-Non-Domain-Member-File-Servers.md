---
title: 啟用非網域成員檔案伺服器的雜湊發行
description: 本主題是 BranchCache 部署指南的 Windows Server 2016 中，示範如何以最佳化 WAN 頻寬使用量，在分公司的分散式和裝載式快取模式部署 BranchCache 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 11584b73-f9e2-4530-afa5-b8df970e6b24
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 00be97abbd583e4c5e762775ea563ba0720d5142
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814949"
---
# <a name="enable-hash-publication-for-non-domain-member-file-servers"></a>啟用非網域成員檔案伺服器的雜湊發行

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用此程序來設定執行 Windows Server 2016 與檔案伺服器上使用本機電腦群組原則的 BranchCache 的雜湊發行**網路檔案的 BranchCache**檔案服務角色服務安裝伺服器角色。  
  
此程序適合使用非網域成員的檔案伺服器上。 如果您的網域成員的檔案伺服器上執行此程序，您也會設定 BranchCache 使用網域群組原則，網域群組原則設定會覆寫本機群組原則設定。  
  
中的成員資格**系統管理員**，或同等權限才能執行此程序的最小值。  
  
> [!NOTE]  
> 如果您有一或多個網域成員的檔案伺服器，您可以將它們新增至 Active Directory 網域服務中的組織單位 (OU)，然後使用 設定雜湊的發行集的所有檔案伺服器一次，而不是個別設定的 群組原則每個檔案伺服器。 如需詳細資訊，請參閱 <<c0> [ 啟用網域成員的檔案伺服器的雜湊發行](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md)。  
  
### <a name="to-enable-hash-publication-for-one-file-server"></a>若要啟用雜湊發行一個檔案伺服器  
  
1.  開啟 Windows PowerShell，輸入 **mmc**，然後按 ENTER 鍵。 此時會開啟 Microsoft Management Console (MMC)。  
  
2.  在 MMC 中，在**檔案**功能表上，按一下**新增/移除嵌入式管理單元**。 **新增或移除嵌入式管理單元**對話方塊隨即開啟。  
  
3.  在 **新增或移除嵌入式管理單元**，請在**可用的嵌入式管理單元**，連按兩下**群組原則物件編輯器**。 群組原則精靈 隨即開啟與選取本機電腦物件。 按一下 [完成] ，然後按一下 [確定] 。  
  
4.  在本機群組原則編輯器 MMC 中，展開下列路徑：**本機電腦原則**，**電腦設定**，**系統管理範本**，**網路**， **Lanman 伺服器**。 按一下  **Lanman 伺服器**。  
  
5.  在 [詳細資料] 窗格中，按兩下**BranchCache 的雜湊發行**。 **BranchCache 的雜湊發行**對話方塊隨即開啟。  
  
6.  在 [ **BranchCache 的雜湊發行**] 對話方塊中，按一下**已啟用**。  
  
7.  在 **選項**，按一下**允許所有共用資料夾的雜湊發行**，然後按一下下列其中之一：  
  
    1.  若要啟用這部電腦上的所有共用資料夾的雜湊發行，按一下**允許所有共用資料夾的雜湊發行**。  
  
    2.  若要啟用雜湊發行 只能用於已啟用 BranchCache 的共用資料夾，請按一下**允許只能用於共用資料夾啟用 BranchCache 的雜湊發行**。  
  
    3.  若要針對所有共用的電腦上的資料夾，即使在檔案共用上啟用 BranchCache，不允許雜湊發行，請按一下**不允許在所有共用資料夾上的雜湊發行**。  
  
8.  按一下 [確定] 。  
  


