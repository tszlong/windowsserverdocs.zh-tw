---
title: 新增 Microsoft Online Service 合作夥伴合約列名的合作夥伴資訊
description: 瞭解如何建立登錄機碼，其中包含您的 Microsoft Online Service 合作夥伴合約夥伴記錄識別碼)  (POR 識別碼。
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 9bd191d6-ecc5-4230-a88e-f3fc281cb956
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 458bcfd3642cf2fceb485d57927708635e494c17
ms.sourcegitcommit: d2224cf55c5d4a653c18908da4becf94fb01819e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/21/2020
ms.locfileid: "97711643"
---
# <a name="add-microsoft-online-service-partner-agreement-partner-of-record-information"></a>新增 Microsoft Online Service 合作夥伴合約列名的合作夥伴資訊

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

##  <a name="BKMK_3rdLevelDomanNames"></a>
 如果您是 Microsoft Online Service 合作夥伴合約 (MOSPA) 合作夥伴提供 Microsoft 365，以確保當訂用帳戶要求是從 Windows Server Essentials 透過 Microsoft 365 整合模組產生時，您需要建立一個登錄機碼，其中包含記錄的夥伴識別碼 (POR 識別碼) 。 下列資訊會透過 Microsoft 365 的註冊 Url 讀取並傳遞給服務提供者。

-   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO

-   類型 = 字串值

-   機碼名稱 = Partner

-   值 = xxxxx，其中 xxxxx 是 POR ID

#### <a name="to-add-the-por-id-key-to-the-registry"></a>若要將 POR ID 機碼新增至登錄

1.  在參照電腦上，按一下 [開始]，輸入 **regedit**，然後按 ENTER。

2.  在左窗格中，依序展開 **HKEY_LOCAL_MACHINE**、**SOFTWARE**、**Microsoft**，然後展開 **Windows Server**。

3.  以滑鼠右鍵按一下 **Windows Server**，指向 **[新增]**，然後按一下 **[機碼]**。

4.  輸入 **MSO** 做為機碼的名稱。

5.  以滑鼠右鍵按一下您剛才建立的機碼，然後按一下 **[字串值]**。

6.  輸入 **Partner** 做為字串的名稱，然後按 ENTER。

7.  在右窗格中，以滑鼠右鍵按一下新的 **Partner** 字串，然後按一下 **[修改]**。

8.  在 **[數值資料]** 文字方塊中輸入您的 POR ID，然後按一下 **[確定]**。

## <a name="see-also"></a>另請參閱

 [建立和自訂映射](Creating-and-Customizing-the-Image.md)[其他自訂](Additional-Customizations.md)專案[準備映射以進行部署](Preparing-the-Image-for-Deployment.md)[測試客戶體驗](Testing-the-Customer-Experience.md)

