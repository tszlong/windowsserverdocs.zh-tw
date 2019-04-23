---
title: 設定電子郵件探索訂閱摘要您 RDS
description: 了解如何將 Azure AD Domain Services 整合到您的 RDS 部署。
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 3/27/2018
ms.localizationpriority: medium
ms.topic: article
author: christianmontoya
ms.openlocfilehash: 5b3f162b8eee70fbc452b7400b737454c3fffb59
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878319"
---
# <a name="set-up-email-discovery-to-subscribe-to-your-rds-feed"></a>設定電子郵件探索訂閱摘要您 RDS

您是否曾經無法讓您的終端使用者連線到摘要，摘要中的單一遺漏字元因為其已發行 RDS URL，或因為未包含 URL 的電子郵件？ 幾乎所有的遠端桌面用戶端應用程式支援尋找您的訂用帳戶，輸入您的電子郵件地址，方便您比以往若要取得您的 Remoteapp 和桌面連線的使用者。

>[!IMPORTANT]
>在 Microsoft Store 中的 Microsoft 遠端桌面應用程式不支援這一次的電子郵件地址訂用帳戶。

您可以設定電子郵件探索之前，請執行下列作業：

- 請確定您有與您的電子郵件相關的網域中新增 TXT 記錄的權限 (例如，如果您的使用者具有@contoso.com電子郵件地址，您會需要權限 contoso.com 網域)
- 建立 RD Web 摘要 URL (https://\<rdweb dns 名稱\>.domain/RDWeb/Feed/webfeed.aspx，例如 https://rdweb.contoso.com/RDWeb/Feed/webfeed.aspx)

若要設定電子郵件的探索，現在，使用下列步驟：

1. 在瀏覽器中，連接到您的網域註冊所在的網域名稱註冊機構的網站。
2. 瀏覽至適當的頁面，您已註冊的網域，您可以用它來檢視、 新增，，和編輯 DNS 記錄。
3. 輸入新的 DNS 記錄，具有下列屬性：
   - **主機：** _msradc
   - **文字：**\<RD Web 摘要的 URL\>
   - **TTL:** 300

   DNS 記錄欄位的名稱會因網域名稱註冊機構，但此程序將產生名為 _msradc 的 TXT 記錄。\<domain_name\> （例如 _msradc.contoso.com)，其完整的 RD Web 摘要的值。

這樣就行了！ 現在，啟動您的裝置上的遠端桌面應用程式，並自行訂閱 ！