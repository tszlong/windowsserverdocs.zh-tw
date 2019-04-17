---
title: "變更訂單和群組] 索引標籤"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 79a417fd-1b3e-47ab-ae33-bb1faf95c86d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 578c5619cfdf076bb2735254494f393d56d35713
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="change-the-order-and-grouping-of-tabs"></a>變更訂單和群組] 索引標籤

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

您可以變更順序儀表板中的索引標籤，讓您的索引標籤中索引標籤列的第一個（左側）。 若要這樣做您加入登錄項目。 您也可以透過將項目新增至登錄影響索引標籤的群組。 索引標籤的順序可以後面 Microsoft 建索引標籤，第三方索引標籤，然後遵循任何額外的索引標籤，接著和 [您的主要索引標籤。  
  
## <a name="change-the-order-of-the-tabs-in-the-dashboard"></a>變更順序儀表板中的索引標籤  
 您必須將新增的識別碼增益集，以定義的順序登錄顯示您的索引標籤。  
  
#### <a name="to-display-your-tab-first-in-the-list-of-tabs"></a>若要在清單中的索引標籤第一次顯示您的索引標籤  
  
1.  參考在電腦上，按一下 [ **[開始]**，輸入**regedit**，然後按**輸入**。  
  
2.  在左窗格中，展開**跳**，展開 [**軟體**，展開 [ **Microsoft**，然後展開 [ **Windows Server**。 如果**OEM**鍵並不存在，您必須先完成下列步驟來建立：  
  
    1.  以滑鼠右鍵按一下**Windows Server**，指向 [**新**，然後按一下 [**鍵**。  
  
    2.  輸入**OEM**鍵的名稱。  
  
3.  以滑鼠右鍵按一下**OEM**，指向 [**新**，然後按一下 [**字串值**。  
  
4.  輸入**DashboardMainTabID**字串名稱，以及然後按**輸入**。  
  
5.  以滑鼠右鍵按一下右窗格中的新字串，然後按一下**修改]**。  
  
6.  輸入 GUID 所定義的最上層的索引標籤，然後按**Enter**。  
  
     如需有關建立並找出最上層的索引標籤，請查看[建立頂層索引標籤](https://msdn.microsoft.com/library/gg513957)Windows Server 方案 sdk。  
  
7.  登錄儲存變更。  
  
8.  您也必須包含 GUID 您識別碼群組的索引標籤清單中的主要最上層索引標籤。 若要這樣做，請執行步驟列在**變更群組儀表板中的索引標籤的**。  
  
## <a name="change-the-grouping-of-tabs-in-the-dashboard"></a>變更儀表板中的索引標籤的群組  
 您可以確保您的索引標籤的一起和包含在清單中建 Microsoft 索引標籤，新增識別碼登錄。  
  
#### <a name="to-change-the-grouping-of-tabs"></a>若要變更的索引標籤的群組  
  
1.  如果 regedit 不開放，請按一下**[開始]**，輸入**regedit**，然後按**輸入**。  
  
2.  在左窗格中，展開**跳**，展開 [**軟體**，展開 [ **Microsoft**，然後展開 [ **Windows Server**。  
  
3.  以滑鼠右鍵按一下**OEM**，指向 [**新**，然後按一下 [**鍵**。  
  
4.  輸入**DashboardAddins**然後按下按鍵名稱，以及**輸入**。  
  
5.  以滑鼠右鍵按一下**DashboardAddins**，指向 [**新**，然後按一下 [**字串值**。  
  
6.  輸入您的索引標籤的 GUID 識別碼做為名稱。 不需要的值。  
  
7.  重複步驟 5 和 6 為每個您的索引標籤和子索引標籤。  
  
8.  儲存登錄變更。  
  
## <a name="see-also"></a>也了  
 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](Additional-Customizations.md)   
 [準備部署映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](Testing-the-Customer-Experience.md)