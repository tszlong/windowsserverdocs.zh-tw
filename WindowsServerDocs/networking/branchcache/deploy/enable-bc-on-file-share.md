---
title: 讓 BranchCache 檔案共用（選擇性）
description: 本主題是 BranchCache 部署節目表適用於 Windows Server 2016 的示範如何將 BranchCache 部署最佳化分公司 WAN 頻寬分散與裝載快取模式中的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-bc
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9c465a9e-c504-44ec-9ebc-4e06ba54db30
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 33ed40ef91d9389bb7940dcf928cba43f0c9dbd2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="enable-branchcache-on-a-file-share-optional"></a>讓 BranchCache 檔案共用（選擇性）

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用此程序，以便 BranchCache 檔案共用。  
  
> [!IMPORTANT]  
> 您不需要執行此程序，如果您設定 hash 發行值的**允許 hash 發行的所有共用資料夾**。  
  
資格在**系統管理員**，或相當於的最低需求才能執行此程序。  
  
### <a name="to-enable-branchcache-on-a-file-share"></a>若要讓 BranchCache 檔案共用  
  
1.  開放的 Windows PowerShell，輸入**mmc**，然後按 ENTER 鍵。 Microsoft Management Console (MMC) 開啟。  
  
2.  在 MMC，在**檔案**功能表上，按**新增/移除嵌入式管理單元**。 **中新增或移除嵌入式管理單元**對話方塊。  
  
3.  在**中新增或移除嵌入式管理單元**，請在**可用嵌入式管理單元**，按兩下 [**共用資料夾**。 選取 [本機電腦物件開啟共用資料夾精靈。 設定您想要的話，檢視按一下**完成**，然後按一下 [ **[確定]**。  
  
4.  按兩下**共用資料夾（本機）**，然後按**共用**。  
  
5.  在詳細資料窗格中，以滑鼠右鍵按一下 [共用]，然後按一下 [**屬性**。 分享的**屬性**對話方塊。  
  
6.  在**屬性**對話方塊中，於**一般**索引標籤上，按一下 [ **Offline 設定**。 **Offline 設定**對話方塊。  
  
7.  確認**的檔案和程式的使用者指定的提供 offline 僅**已選取，然後按一下 [**可讓 BranchCache**。  
  
8.  按一下**[確定]**兩次。  
  

