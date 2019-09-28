---
title: 追蹤您的遠端桌面服務用戶端存取使用權 (RDS CAL)
description: 了解如何在您的 RDS 部署中追蹤 CAL。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 80d82d30-3ad0-4a8c-9a9b-2773c47eee19
author: lizap
ms.author: elizapo
ms.date: 05/11/2017
manager: dongill
ms.openlocfilehash: 278aa7a2d35aeacfaee8deddcd64e668a320853f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387116"
---
# <a name="track-your-remote-desktop-services-client-access-licenses-rds-cals"></a>追蹤您的遠端桌面服務用戶端存取使用權 (RDS CAL)

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

您可以使用遠端桌面授權管理員工具，建立報表來追蹤 RDS 每個使用者的 CAL (已由遠端桌面授權伺服器發行)。

> [!NOTE]
>  如果您在環境中使用 Azure AD Domain Services，遠端桌面授權管理員工具將無法取得每個使用者的 CAL。 您需要改為手動追蹤授權，不論是透過登入事件、透過連線代理人輪詢使用中的遠端桌面連線，或是其他適合您的機制均可。 

使用下列步驟來產生每個使用者的 CAL 報表：

1. 在遠端桌面授權管理員中，以滑鼠右鍵按一下授權伺服器、按一下 [建立報表]  ，然後按一下 [每個使用者的 CAL 使用量]  。
2. 設定報表的範圍 - 選取下列其中一項：
   - 整個網域：授權伺服器所屬的網域。
   - 組織單位：授權伺服器所屬網域內的任何 OU。
   - 整個網域和所有受信任的網域：可以包含其他樹系中的網域。 選取此選項，可以增加建立報表所花費的時間。

   您所做的選擇會決定要針對 RDS 每個使用者的 CAL 資訊，搜尋 AD DS 中的哪些使用者帳戶來產生報表。
3. 按一下 [建立報表]  。 報表隨即建立，並顯示一則訊息來確認已成功建立報表。 按一下 **[確定]** 關閉訊息。

您所建立的報表會出現在授權伺服器節點下方的 [報表] 區段中。 此報表會提供下列資訊：

- 建立報表的日期和時間
- 報表的範圍 (例如，網域、OU=Sales 或所有受信任的網域)
- 授權伺服器上所安裝的 RDS 每個使用者的 CAL 數量
- RDS 每個使用者的 CAL 數量，已由報表範圍特有的授權伺服器所發行

您也可以將報表當成 CSV 檔案儲存至電腦上的資料夾位置。 若要儲存報表，以滑鼠右鍵按一下您想要儲存的報表、按一下 [另存新檔]，然後指定檔案名稱和位置來儲存報表。

您所建立的報表會列在遠端桌面授權管理員內，授權伺服器節點下方的 [報表] 節點中。 如果您不再需要報表，即可將它刪除。
