---
title: 啟用檔案共用的 BranchCache (選用)
description: 本主題是 BranchCache 部署指南的 Windows Server 2016 中，示範如何以最佳化 WAN 頻寬使用量，在分公司的分散式和裝載式快取模式部署 BranchCache 的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-bc
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9c465a9e-c504-44ec-9ebc-4e06ba54db30
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fd1757f6da011c2f774d8f97f628e5f0e87d3bf7
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284028"
---
# <a name="enable-branchcache-on-a-file-share-optional"></a>啟用檔案共用的 BranchCache (選用)

>適用於：Windows Server （半年通道），Windows Server 2016

若要啟用 BranchCache 的檔案共用上，您可以使用此程序。  
  
> [!IMPORTANT]  
> 您不需要執行此程序，如果您設定具有值的雜湊發行集設定**允許所有共用資料夾的雜湊發行**。  
  
中的成員資格**系統管理員**，或同等權限才能執行此程序的最小值。  
  
### <a name="to-enable-branchcache-on-a-file-share"></a>若要在檔案共用上啟用 BranchCache  
  
1.  開啟 Windows PowerShell，輸入 **mmc**，然後按 ENTER 鍵。 此時會開啟 Microsoft Management Console (MMC)。  
  
2.  在 MMC 中，在**檔案**功能表上，按一下**新增/移除嵌入式管理單元**。 **新增或移除嵌入式管理單元**對話方塊隨即開啟。  
  
3.  在 **新增或移除嵌入式管理單元**，請在**可用的嵌入式管理單元**，連按兩下**共用資料夾**。 共用資料夾精靈 隨即開啟與選取本機電腦物件。 設定您偏好，檢視按一下 **完成**，然後按一下**確定**。  
  
4.  按兩下**共用資料夾 （本機）** ，然後按一下**共用**。  
  
5.  在 [詳細資料] 窗格中，請在共用中，按一下滑鼠右鍵，，然後按一下**屬性**。 共用的**屬性**對話方塊隨即開啟。  
  
6.  在 **屬性**對話方塊的 **一般**索引標籤上，按一下 **離線設定**。 **離線設定**對話方塊隨即開啟。  
  
7.  請確認**只可以使用的檔案和使用者指定的程式離線**已選取，然後按一下**啟用 BranchCache**。  
  
8.  按兩次 **[確定]** 。  
  

