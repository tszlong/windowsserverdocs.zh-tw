---
title: 設定 BranchCache 雜湊發行群組原則物件
description: 本主題是 BranchCache 部署指南的 Windows Server 2016 中，示範如何以最佳化 WAN 頻寬使用量，在分公司的分散式和裝載式快取模式部署 BranchCache 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da74fea7-52b2-4d6d-9d21-19184eedbe3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6e9a338dfebfb1dfadb258bcbdfcc8d75bd3ea86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862959"
---
# <a name="configure-the-branchcache-hash-publication-group-policy-object"></a>設定 BranchCache 雜湊發行群組原則物件

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用此程序來設定 BranchCache 的雜湊發行群組原則物件 (GPO)，以便您新增到您的組織單位的所有檔案伺服器都具有相同雜湊發行集原則設定套用至它們。  
  
若要執行此程序，至少需要 **Domain Admins** 的成員資格或同等權限。  
  
> [!NOTE]  
> 然後再執行此程序，您必須先建立 BranchCache 的檔案伺服器組織單位，將檔案伺服器移到 OU，並建立 BranchCache 的雜湊發行 GPO。 如需詳細資訊，請參閱 <<c0> [ 啟用網域成員的檔案伺服器的雜湊發行](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md)。  
  
### <a name="to-configure-the-branchcache-hash-publication-group-policy-object"></a>若要設定 BranchCache 的雜湊發行群組原則物件  
  
1.  Windows PowerShell 系統管理員身分執行，型別**mmc**，然後按 ENTER 鍵。 此時會開啟 Microsoft Management Console (MMC)。  
  
2.  在 MMC 中，在**檔案**功能表上，按一下**新增/移除嵌入式管理單元**。 **新增或移除嵌入式管理單元**對話方塊隨即開啟。  
  
3.  中**新增或移除嵌入式管理單元**，請在**可用的嵌入式管理單元**，按兩下**群組原則管理**，然後按一下**確定**。  
  
4.  在群組原則管理 MMC 中，展開 BranchCache 的雜湊發行集您先前建立的 GPO 的路徑。 比方說，如果您的樹系名稱為 example.com，您的網域名稱為 example.com、example1.com 和名為您的 GPO **BranchCache 的雜湊發行**，展開下列路徑：**群組原則管理**，**樹系： example.com**，**網域**， **example1.com**，**群組原則物件**， **BranchCache 的雜湊發行**。  
  
5.  以滑鼠右鍵按一下**BranchCache 的雜湊發行**GPO，然後按一下**編輯**。 群組原則管理編輯器主控台隨即開啟。  
  
6.  在 [群組原則管理編輯器] 主控台中，展開下列路徑：**電腦設定**，**原則**，**系統管理範本**，**網路**， **Lanman 伺服器**。  
  
7.  在 [群組原則管理編輯器] 主控台中，按一下**Lanman 伺服器**。 在 [詳細資料] 窗格中，按兩下**BranchCache 的雜湊發行**。 **BranchCache 的雜湊發行**對話方塊隨即開啟。  
  
8.  在 [ **BranchCache 的雜湊發行**] 對話方塊中，按一下**已啟用**。  
  
9. 在 **選項**，按一下**允許所有共用資料夾的雜湊發行**，然後按一下下列其中之一：  
  
    1.  若要啟用所有共用資料夾的雜湊發行，為所有檔案伺服器新增至 OU，請按一下**允許所有共用資料夾的雜湊發行**。  
  
    2.  若要啟用雜湊發行 只能用於已啟用 BranchCache 的共用資料夾，請按一下**允許只能用於共用資料夾啟用 BranchCache 的雜湊發行**。  
  
    3.  若要針對所有共用的電腦上的資料夾，即使在檔案共用上啟用 BranchCache，不允許雜湊發行，請按一下**不允許在所有共用資料夾上的雜湊發行**。  
  
10. 按一下 [確定] 。  
  
> [!NOTE]  
> 在大部分情況下，您必須儲存 MMC 主控台，並重新整理檢視來顯示您所做的組態變更。  
  


