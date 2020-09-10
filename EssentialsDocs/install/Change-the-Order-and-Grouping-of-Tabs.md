---
title: 變更索引標籤的順序和群組
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 79a417fd-1b3e-47ab-ae33-bb1faf95c86d
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 5a52f4b14423d1ef337cba9c2d18dbf4c196ca93
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623869"
---
# <a name="change-the-order-and-grouping-of-tabs"></a>變更索引標籤的順序和群組

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

您可以變更儀表板中索引標籤的順序，使您的索引標籤變成整排索引標籤中的第一個 (左側) 索引標籤。 若要執行此動作，您可以新增登錄項目。 您也可以新增登錄項目來影響索引標籤的群組。 您可以將索引標籤的順序變成：您的主要索引標籤，然後是 Microsoft 內建索引標籤，接著是您其他的索引標籤，最後是協力廠商索引標籤。

## <a name="change-the-order-of-the-tabs-in-the-dashboard"></a>變更儀表板中索引標籤的順序
 您必須針對會顯示您的索引標籤的增益集，將其識別項新增至登錄以定義順序。

#### <a name="to-display-your-tab-first-in-the-list-of-tabs"></a>若要在索引標籤清單中將您的索引標籤顯示為第一個

1.  在參照電腦上，按一下 [開始]****，輸入 **regedit**，然後按 **ENTER**。

2.  在左窗格中，依序展開 **HKEY_LOCAL_MACHINE**、**SOFTWARE**、**Microsoft**，然後展開 **Windows Server**。 如果 **OEM** 機碼不存在，您必須完成下列步驟來建立機碼：

    1.  以滑鼠右鍵按一下 **Windows Server**，指向 **[新增]**，然後按一下 **[機碼]**。

    2.  輸入 **OEM** 作為機碼名稱。

3.  以滑鼠右鍵按一下 **OEM**，指向 **[新增]**，然後按一下 **[字串值]**。

4.  輸入 **DashboardMainTabID** 作為字串名稱，然後按 **ENTER**。

5.  在右窗格中，以滑鼠右鍵按一下新的字串，然後按一下 **[修改]**。

6.  輸入已為頂層索引標籤定義的 GUID，然後按 **ENTER**。

     如需建立和識別頂層索引標籤的詳細資訊，請參閱 Windows Server 解決方案 SDK 中的 [建立頂層索引標籤](/previous-versions/windows/server-essentials/gg513957(v=msdn.10)) 。

7.  將變更儲存至登錄。

8.  您也必須將您主要頂層索引標籤的 GUID 加到識別項清單中，以將索引標籤群組在一起。 若要執行此動作，請執行**變更儀表板中索引標籤的群組**中所列出的步驟。

## <a name="change-the-grouping-of-tabs-in-the-dashboard"></a>變更儀表板中索引標籤的群組
 您可以新增登錄項目，以確保您的索引標籤會群組在一起，而且會包含在內建 Microsoft 索引標籤的清單中。

#### <a name="to-change-the-grouping-of-tabs"></a>若要變更索引標籤的群組

1.  如果 regedit 尚未開啟，請按一下 [開始]****，輸入 **regedit**，然後按 **ENTER**。

2.  在左窗格中，依序展開 **HKEY_LOCAL_MACHINE**、**SOFTWARE**、**Microsoft**，然後展開 **Windows Server**。

3.  以滑鼠右鍵按一下 **OEM**，指向 **[新增]**，然後按一下 **[機碼]**。

4.  輸入 **DashboardAddins** 作為機碼名稱，然後按 **ENTER**。

5.  以滑鼠右鍵按一下 **DashboardAddins**，指向 **[新增]**，然後按一下 **[字串值]**。

6.  輸入您索引標籤的 GUID 識別項作為字串名稱。 不需要有值。

7.  針對您每個索引標籤和子索引標籤重複步驟 5 和 6。

8.  儲存登錄變更。

## <a name="see-also"></a>另請參閱
 [建立和自訂映射](Creating-and-Customizing-the-Image.md)[其他自訂](Additional-Customizations.md)專案[準備映射以進行部署](Preparing-the-Image-for-Deployment.md)[測試客戶體驗](Testing-the-Customer-Experience.md)