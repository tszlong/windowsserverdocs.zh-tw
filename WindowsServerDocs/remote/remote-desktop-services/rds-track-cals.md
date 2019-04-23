---
title: 追蹤您的遠端桌面服務用戶端存取使用權 (RDS Cal)
description: 了解如何在您的 RDS 部署中追蹤 Cal。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ed7490b2b61eb516d43b280b67fcefaeedb3e225
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890619"
---
# <a name="track-your-remote-desktop-services-client-access-licenses-rds-cals"></a>追蹤您的遠端桌面服務用戶端存取使用權 (RDS Cal)

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用遠端桌面授權管理員工具來建立追蹤的 RDS 每個使用者 Cal，遠端桌面授權伺服器已發行的報表。

> [!NOTE]
>  如果您使用 Azure AD Domain Services 環境中，「 遠端桌面授權管理員 」 工具將無法運作以取得每個使用者 Cal。 相反地，您需要追蹤透過登入事件、 輪詢使用中的遠端桌面連線透過連接訊息代理程式或另一項機制可讓您以手動的方式，授權。 

使用下列步驟來產生每個使用者 Cal 報表：

1. 在 遠端桌面授權管理員中以滑鼠右鍵按一下授權伺服器，請按一下**建立報表**，然後按一下**每個使用者 CAL 使用量**。
2. 設定報表的範圍-選取下列其中一項：
   - 整個網域為授權伺服器所屬的網域。
   - 組織單位-授權伺服器所屬的網域內的任何 OU。
   - 整個網域和所有信任的網域-可以包含其他樹系中的網域。 選取此選項，可以增加建立報表所花費的時間。

   您所做的選擇會決定 RDS 每個使用者 CAL 資訊來產生報表中搜尋 AD DS 中的哪些使用者帳戶。
3. 按一下 **建立報表**。 建立報表，並會顯示訊息，確認已成功建立報表。 按一下 **[確定]** 以關閉訊息。

您所建立的報表會出現在 [授權伺服器] 節點下的 [報表] 區段中。 此報告提供下列資訊：

- 建立報表的日期和時間
- 報表的範圍 (例如網域、 OU = Sales 或所有信任的網域)
- RDS 每個使用者 Cal 授權伺服器所安裝的數字
- RDS 每個使用者 Cal 數量已由特定的授權伺服器發行至報表的範圍

您也可以儲存為 CSV 檔案的資料夾位置，在電腦上的報表。 若要儲存報表，以滑鼠右鍵按一下您想要儲存，按一下 另存新檔，然後指定 檔案名稱和位置來儲存報表的報表。

您所建立的報表會列在 [遠端桌面授權管理員中的授權伺服器] 節點下的 [報告] 節點。 如果您不再需要一份報表，您可以將它刪除。
