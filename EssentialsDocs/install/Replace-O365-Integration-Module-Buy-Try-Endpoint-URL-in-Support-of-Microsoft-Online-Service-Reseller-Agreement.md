---
title: 在 Microsoft Online Service 經銷商合約的支援下，取代 O365 整合模組並購買/嘗試端點 URL
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 9860a6b9-baea-4bf0-9a9f-6f1a288f996e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 12f4013f84c9fbfe8fc9529ea90b62f5fc5416d5
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181124"
---
# <a name="replace-o365-integration-module-buy-try-endpoint-url-in-support-of-microsoft-online-service-reseller-agreement"></a>在 Microsoft Online Service 經銷商合約的支援下，取代 O365 整合模組並購買/嘗試端點 URL

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

##  <a name="BKMK_O365"></a>
 如果您是 Microsoft Online Service 轉銷商合約（MOSRA）合作夥伴，若要確保客戶註冊交易是透過您的入口網站處理，您必須取代 Windows Server Essentials Office 365 整合模組所使用的端點 Url。

 整合模組使用下列四個端點 URL：

1.  Office 365 Enterprise 訂閱購買端點。

    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\

    -   類型 = REG-SZ

    -   機碼名稱 = MOSRASTDBUY

    -   值 = *xxxxx*，其中 xxxxx 是您的企業訂閱購買 URL。 例如，值 =http://syndicatepartner.office365.com/enterprisebuy.html

2.  Office 365 Enterprise 訂閱試用端點。

    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\

    -   類型 = REG-SZ

    -   機碼名稱 = MOSRASTDTRY

    -   值 = *xxxxx*，其中 xxxxx 是您的企業訂閱購買 URL。 例如，值 =http://syndicatepartner.office365.com/enterprisetry.html

3.  Office 365 Small Business Premium 訂閱購買端點。

    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\

    -   類型 = REG-SZ

    -   機碼名稱 = MOSRALITEBUY

    -   值 = *xxxxx*，其中 xxxxx 是您的企業訂閱購買 URL。 例如，值 =http://syndicatepartner.office365.com/smallbizbuy.html

4.  Office 365 Small Business Premium 訂用帳戶試用端點。

    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\

    -   類型 = REG-SZ

    -   機碼名稱 = MOSRALITETRY

    -   值 = *xxxxx*，其中 xxxxx 是您的企業訂閱購買 URL。 例如，值 =http://syndicatepartner.office365.com/smallbiztry.html

#### <a name="to-add-an-endpoint-url-key-to-the-registry"></a>將端點 URL 機碼新增到登錄

1.  在參照電腦上，按一下 [開始]****，輸入 **regedit**，然後按 ENTER。

2.  在左窗格中，依序展開 **HKEY_LOCAL_MACHINE**、**SOFTWARE**、**Microsoft**、**Windows Server** 及 **MSO**。

3.  如果 MSO 不存在，請在 [Windows Server]**** 上按一下滑鼠右鍵，指向 [新增]****，按一下 [機碼]****，然後輸入 **MSO** 作為機碼的名稱。

4.  以滑鼠右鍵按一下 MSO，然後按一下 **[字串值]**。 輸入下列其中一個端點字串名稱，當做字串的名稱：

    -   MOSRASTDBUY

    -   MOSRASTDTRY

    -   MOSRALITEBUY

    -   MOSRALITETRY

5.  在右窗格中，以滑鼠右鍵按一下新的字串，然後按一下 **[修改]**。

6.  在 **[數值資料]** 文字方塊中輸入您的新端點 URL，然後按一下 **[確定]**。

7.  針對在步驟 4 中列出的每個字串名稱重複步驟 4-6。

## <a name="see-also"></a>另請參閱

 [建立和自訂映射額外的](Creating-and-Customizing-the-Image.md)[自訂](Additional-Customizations.md)[準備映射以進行部署](Preparing-the-Image-for-Deployment.md)[測試客戶體驗](Testing-the-Customer-Experience.md)

