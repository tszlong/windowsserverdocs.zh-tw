---
title: 建立 BranchCache 雜湊發行群組原則物件
description: 本主題是 Windows Server 2016 的 BranchCache 部署指南的一部分，示範如何在分散式和託管快取模式中部署 BranchCache，以優化分公司的 WAN 頻寬使用量
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: c3d33bed-83ef-4eb8-acf9-0719ecb4a931
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e546705d022bbac2588ace5b3e2c6c807c96da63
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319302"
---
# <a name="create-the-branchcache-hash-publication-group-policy-object"></a>建立 BranchCache 雜湊發行群組原則物件

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用此程式來建立 BranchCache 雜湊發行集群組原則物件（GPO）。  
  
若要執行此程序，至少需要 **Domain Admins** 的成員資格或同等權限。  
  
> [!NOTE]  
> 執行此程式之前，您必須先建立 BranchCache 檔案伺服器組織單位，並將檔案伺服器移到 OU 中。 如需詳細資訊，請參閱[啟用網域成員檔案伺服器的雜湊發行](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md)。  
  
### <a name="to-create-the-branchcache-hash-publication-group-policy-object"></a>若要建立 BranchCache 雜湊發行集群組原則物件  
  
1.  開啟 Windows PowerShell，然後輸入 **mmc**，再按 ENTER 鍵。 此時會開啟 Microsoft Management Console (MMC)。  
  
2.  在 MMC 的 **[檔案]** 功能表上，按一下 **[新增/移除嵌入式管理單元]** 。 [**新增或移除嵌入式管理單元**] 對話方塊隨即開啟。  
  
3.  在 [**新增或移除嵌入式管理單元**] 的 [**可用**的嵌入式管理單元] 中，按兩下 [**群組原則管理**]，然後按一下 **[確定]** 。  
  
4.  在 [群組原則管理] MMC 中，展開您先前建立之 BranchCache 檔案伺服器 OU 的路徑。 例如，如果您的樹系名為 example.com，您的網域會命名為 example1.com，而您的 OU 會命名為 BranchCache 檔案伺服器，請展開下列路徑：**群組原則管理**、**樹系： example.com**、**網域**、 **example1.com**、 **BranchCache 檔案伺服器**。  
  
5.  以滑鼠右鍵按一下 [ **BranchCache 檔案伺服器**]，然後按一下 [**在這個網域中建立 GPO 並連結到**]。 [**新增 GPO** ] 對話方塊隨即開啟。 在 [**名稱**] 中，輸入新 GPO 的名稱。 例如，如果您想要將物件命名為 BranchCache 雜湊發行，請輸入**Branchcache 雜湊發行**。 按一下 [確定]。  
  


