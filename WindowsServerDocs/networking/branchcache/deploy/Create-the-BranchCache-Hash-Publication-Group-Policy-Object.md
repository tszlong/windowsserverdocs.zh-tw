---
title: 建立 BranchCache 雜湊發行群組原則物件
description: 本主題是 BranchCache 部署指南的 Windows Server 2016 中，示範如何以最佳化 WAN 頻寬使用量，在分公司的分散式和裝載式快取模式部署 BranchCache 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: c3d33bed-83ef-4eb8-acf9-0719ecb4a931
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 15e89b961d20b6f14065392553e413374358062d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865429"
---
# <a name="create-the-branchcache-hash-publication-group-policy-object"></a>建立 BranchCache 雜湊發行群組原則物件

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用此程序來建立 BranchCache 的雜湊發行群組原則物件 (GPO)。  
  
若要執行此程序，至少需要 **Domain Admins** 的成員資格或同等權限。  
  
> [!NOTE]  
> 然後再執行此程序，您必須建立 BranchCache 的檔案伺服器的組織單位，並將檔案伺服器移到 OU。 如需詳細資訊，請參閱 <<c0> [ 啟用網域成員的檔案伺服器的雜湊發行](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md)。  
  
### <a name="to-create-the-branchcache-hash-publication-group-policy-object"></a>若要建立 BranchCache 的雜湊發行群組原則物件  
  
1.  開啟 Windows PowerShell，輸入 **mmc**，然後按 ENTER 鍵。 此時會開啟 Microsoft Management Console (MMC)。  
  
2.  在 MMC 中，在**檔案**功能表上，按一下**新增/移除嵌入式管理單元**。 **新增或移除嵌入式管理單元**對話方塊隨即開啟。  
  
3.  中**新增或移除嵌入式管理單元**，請在**可用的嵌入式管理單元**，按兩下**群組原則管理**，然後按一下**確定**。  
  
4.  在群組原則管理 MMC 中，展開 BranchCache 的檔案伺服器，您先前建立的 OU 的路徑。 例如，如果您的樹系名稱為 example.com、 您的網域名稱為 example.com、example1.com 和 BranchCache 的檔案伺服器名稱為您的 OU，請展開下列路徑：**群組原則管理**，**樹系： example.com**，**網域**， **example1.com**， **BranchCache 的檔案伺服器**。  
  
5.  以滑鼠右鍵按一下**BranchCache 的檔案伺服器**，然後按一下**在這個網域中建立 GPO 並連結到**。 **新的 GPO**對話方塊隨即開啟。 在 **名稱**，輸入新的 GPO 的名稱。 如果您想要命名 BranchCache 的雜湊發行的物件，例如，輸入**BranchCache 的雜湊發行**。 按一下 [確定] 。  
  


