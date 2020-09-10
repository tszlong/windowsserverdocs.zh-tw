---
title: 取代 Microsoft 365 整合模組購買-Try 端點 URL 支援 Microsoft Online Service 轉售商合約
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 9860a6b9-baea-4bf0-9a9f-6f1a288f996e
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: a8a5cf91c6de2971bc8270cc3c7ea92327b71224
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623377"
---
# <a name="replace-microsoft-365-integration-module-buy-try-endpoint-url-in-support-of-microsoft-online-service-reseller-agreement"></a>取代 Microsoft 365 整合模組購買-Try 端點 URL 支援 Microsoft Online Service 轉售商合約

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

##  <a name="BKMK_O365"></a>
 如果您是 Microsoft Online Service 轉售商合約 (MOSRA) 合作夥伴，若要確保透過您的入口網站處理客戶註冊交易，您必須將 Windows Server Essentials Microsoft 365 整合模組使用的端點 Url 取代。

 整合模組使用下列四個端點 URL：

1.  Microsoft 365 企業版訂用帳戶購買端點。

    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\

    -   類型 = REG-SZ

    -   機碼名稱 = MOSRASTDBUY

    -   值 = *xxxxx*，其中 xxxxx 是您的企業訂閱購買 URL。 例如，值 = http://syndicatepartner.office365.com/enterprisebuy.html

2.  Microsoft 365 企業版訂用帳戶試用端點。

    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\

    -   類型 = REG-SZ

    -   機碼名稱 = MOSRASTDTRY

    -   值 = *xxxxx*，其中 xxxxx 是您的企業訂閱購買 URL。 例如，值 = http://syndicatepartner.office365.com/enterprisetry.html

3.  Microsoft 365 Small Business Premium 訂用帳戶購買端點。

    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\

    -   類型 = REG-SZ

    -   機碼名稱 = MOSRALITEBUY

    -   值 = *xxxxx*，其中 xxxxx 是您的企業訂閱購買 URL。 例如，值 = http://syndicatepartner.office365.com/smallbizbuy.html

4.  Microsoft 365 Small Business Premium 訂用帳戶試用端點。

    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\

    -   類型 = REG-SZ

    -   機碼名稱 = MOSRALITETRY

    -   值 = *xxxxx*，其中 xxxxx 是您的企業訂閱購買 URL。 例如，值 = http://syndicatepartner.office365.com/smallbiztry.html

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

 [建立和自訂映射](Creating-and-Customizing-the-Image.md)[其他自訂](Additional-Customizations.md)專案[準備映射以進行部署](Preparing-the-Image-for-Deployment.md)[測試客戶體驗](Testing-the-Customer-Experience.md)

