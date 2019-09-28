---
title: 設定 BranchCache 雜湊發行群組原則物件
description: 本主題是 Windows Server 2016 的 BranchCache 部署指南的一部分，示範如何在分散式和託管快取模式中部署 BranchCache，以優化分公司的 WAN 頻寬使用量
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da74fea7-52b2-4d6d-9d21-19184eedbe3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bae0d3dff22205f3311020e233a26835527186f5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406494"
---
# <a name="configure-the-branchcache-hash-publication-group-policy-object"></a>設定 BranchCache 雜湊發行群組原則物件

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用這個程式來設定 BranchCache 雜湊發行集群組原則物件（GPO），讓您新增到 OU 的所有檔案伺服器都具有相同的雜湊發行集原則設定。  
  
若要執行此程序，至少需要 **Domain Admins** 的成員資格或同等權限。  
  
> [!NOTE]  
> 執行此程式之前，您必須先建立 BranchCache 檔案伺服器組織單位、將檔案伺服器移到 OU 中，然後建立 BranchCache 雜湊發行 GPO。 如需詳細資訊，請參閱[啟用網域成員檔案伺服器的雜湊發行](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md)。  
  
### <a name="to-configure-the-branchcache-hash-publication-group-policy-object"></a>設定 BranchCache 雜湊發行集群組原則物件  
  
1.  以系統管理員身分執行 Windows PowerShell，輸入**mmc**，然後按 enter。 此時會開啟 Microsoft Management Console (MMC)。  
  
2.  在**MMC 的 [檔案] 功能表上**，按一下 [**新增/移除嵌入式管理單元**]。 [**新增或移除嵌入式管理單元**] 對話方塊隨即開啟。  
  
3.  在 [**新增或移除嵌入式管理單元**] 的 [**可用**的嵌入式管理單元] 中，按兩下 [**群組原則管理**]，然後按一下 **[確定]** 。  
  
4.  在群組原則管理 MMC 中，展開您先前建立的 BranchCache 雜湊發行 GPO 的路徑。 例如，如果您的樹系名為 example.com，您的網域會命名為 example1.com，而您的 GPO 名為**BranchCache 雜湊發行**，請展開下列路徑：**群組原則管理**、**樹系： example.com**、**網域**、 **Example1.com**、**群組原則物件**、 **BranchCache 雜湊發行**。  
  
5.  以滑鼠右鍵按一下**BranchCache 雜湊發行**GPO，然後按一下 [**編輯**]。 群組原則管理編輯器主控台隨即開啟。  
  
6.  在群組原則管理編輯器主控台中，展開下列路徑：**電腦**設定、**原則**、**系統管理範本**、**網路**、 **Lanman 伺服器**。  
  
7.  在群組原則管理編輯器主控台中，按一下 [ **Lanman 伺服器**]。 在詳細資料窗格中，按兩下 [ **BranchCache 的雜湊發行**]。 [ **BranchCache 的雜湊發行**] 對話方塊隨即開啟。  
  
8.  在 [ **BranchCache 的雜湊發行**] 對話方塊中，按一下 [**已啟用**]。  
  
9. 在 [**選項**] 中，**針對 [所有共用資料夾] 按一下 [允許雜湊發行**]，然後按一下下列其中一項：  
  
    1.  若要針對您新增至 OU 的所有檔案伺服器，啟用所有共用資料夾的雜湊發行，請按一下 [**允許所有共用資料夾的雜湊發行**]。  
  
    2.  若只要針對已啟用 BranchCache 的共用資料夾啟用雜湊發行，請按一下 [**僅允許啟用 branchcache 之共用資料夾的雜湊發行**]。  
  
    3.  若不允許電腦上所有共用資料夾的雜湊發行，即使已在檔案共用上啟用 BranchCache，也請按一下 [不**允許在所有共用資料夾上進行雜湊發行**]。  
  
10. 按一下 [確定]。  
  
> [!NOTE]  
> 在大部分的情況下，您必須儲存 MMC 主控台，並重新整理此視圖，以顯示您所做的設定變更。  
  


