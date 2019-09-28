---
title: 設定電子郵件探索以訂閱您的 RDS 摘要
description: 了解如何將 Azure AD Domain Services 與您的 RDS 部署整合。
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 3/27/2018
ms.localizationpriority: medium
ms.topic: article
author: christianmontoya
ms.openlocfilehash: c56a233adf28270aac809dc960e32b5363e4b8ab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387514"
---
# <a name="set-up-email-discovery-to-subscribe-to-your-rds-feed"></a>設定電子郵件探索以訂閱您的 RDS 摘要

您是否曾經因為摘要 URL 中遺漏了單一字元，或因為終端使用者遺失了含有 URL 的電子郵件，而無法讓使用者連線至其已發佈的 RDS 摘要呢？ 幾乎所有的遠端桌面用戶端應用程式都支援藉由輸入電子郵件地址來尋找您的訂閱，因此您的使用者可比以往更輕鬆地連線至其 RemoteApp 和桌面。

>[!IMPORTANT]
>Microsoft Store 中的 Microsoft 遠端桌面應用程式目前不支援電子郵件地址訂閱。

在設定電子郵件探索之前，請執行下列作業：

- 確定您有權將 TXT 記錄新增至與您的電子郵件相關聯的網域 (例如，如果您的使用者具有 @contoso.com 電子郵件地址，您就需要 contoso.com 網域的權限)
- 建立 RD Web 摘要 URL (https://\<rdweb-dns-name\>.domain/RDWeb/Feed/webfeed.aspx，例如 https://rdweb.contoso.com/RDWeb/Feed/webfeed.aspx)

接著，請使用下列步驟設定電子郵件探索：

1. 在瀏覽器中，連線至您的網域註冊所在的網域名稱註冊機構網站。
2. 瀏覽至您已註冊的網域可供檢視、新增和編輯 DNS 記錄的適當頁面。
3. 輸入具有下列屬性的新 DNS 記錄：
   - **Host：** _msradc
   - **文字：** \<RD Web 摘要 URL\>
   - **TTL：** 300

   DNS 記錄欄位的名稱會隨著網域名稱註冊機構而不同，但此程序將產生名為 _msradc.\<domain_name\> (例如 _msradc.contoso.com)、具有完整 RD Web 摘要值的 TXT 記錄。

這樣就完成了！ 現在，在您的裝置上啟動遠端桌面應用程式，並自行訂閱！