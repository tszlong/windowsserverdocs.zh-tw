---
title: 設定訂閱摘要您 RDS 電子探索
description: 了解如何將 Azure AD 網域服務整合到您的 RDS 部署。
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 3/27/2018
ms.localizationpriority: medium
ms.topic: article
author: christianmontoya
ms.openlocfilehash: 5b3f162b8eee70fbc452b7400b737454c3fffb59
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/01/2018
ms.locfileid: "1691667"
---
# <a name="set-up-email-discovery-to-subscribe-to-your-rds-feed"></a>設定訂閱摘要您 RDS 電子探索

您是否有問題您的使用者連線至饋、 可以是因為在摘要中的單一遺失字元其發佈 RDS URL 或因為其遺失 url 的電子郵件吗？ 幾乎所有的遠端桌面用戶端應用程式支援可以輸入電子郵件地址，使其成為比屬於若要取得您連線至其 RemoteApps 和桌面的使用者更容易尋找您的訂閱。

>[!IMPORTANT]
>Microsoft 存放區中的 Microsoft 遠端桌面應用程式不支援在此階段的電子郵件地址訂閱。

設定電子郵件探索之前，請執行下列作業：

- 請確定您已為您的電子郵件相關聯的網域新增 TXT 記錄的權限 (例如，如果您的使用者必須@contoso.com電子郵件地址，您就需要的權限 contoso.com 網域)
- 建立摘要 URL RD Web (https://\ < rdweb-dns-name\ >.domain/RDWeb/Feed/webfeed.aspx，例如：https://rdweb.contoso.com/RDWeb/Feed/webfeed.aspx)

現在，使用下列步驟來設定電子郵件探索：

1. 在瀏覽器中，連線至其中已註冊您的網域的網域名稱註冊機構的網站。
2. 瀏覽至您已註冊的網域檢視、 新增和編輯 DNS 記錄的適當頁面。
3. 輸入新的 DNS 記錄具有下列內容：
   - **主機：** _msradc
   - **文字：** \ < RD Web 摘要 URL\ >
   - **TTL：** 300

   DNS 記錄欄位名稱不同的網域名稱註冊機構，但此程序會導致名為 _msradc。 TXT 記錄 \ < domain_name\ > （例如 _msradc.contoso.com) 具有完整 RD Web 摘要值。

這是它 ！ 現在，啟動裝置上的遠端桌面應用程式和訂閱自行 ！