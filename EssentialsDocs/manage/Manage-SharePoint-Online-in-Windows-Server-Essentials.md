---
title: 管理 Windows Server Essentials 中的 SharePoint Online
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 282f3634-6de6-4691-803c-df6c3c16660d
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 66e5a2c8781e5c9602ba667e15ac789732767be3
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626095"
---
# <a name="manage-sharepoint-online-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的 SharePoint Online

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

如果您將 Microsoft 365 與 Windows Server Essentials 伺服器整合，就可以從儀表板管理您的 SharePoint Online 文件庫和小組網站，而不需登入 Microsoft 365。 您可以取得 SharePoint Online 文件庫和小組網站，其中包含任何 Microsoft 365 的商務方案。 [瞭解如何將 Microsoft 365 與您的伺服器整合](Manage-Office-365-in-Windows-Server-Essentials.md)

 此外，您的使用者將能夠使用 My Server 2012 R2 應用程式，從任何地方使用行動裝置或 Windows phone 來存取 SharePoint Online 文件庫中的檔案。 [我可以在哪裡取得 My Server 應用程式？](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md)

 尚未嘗試過 SharePoint 嗎？ [您可以立即執行的作業](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/get-started-with-sharepoint-2013-HA102772778.aspx)

## <a name="where-on-the-dashboard-will-i-manage-my-libraries-and-team-sites"></a>我可以在儀表板上何處管理文件庫和小組網站？
 當您將 Microsoft 365 與伺服器整合時，將會使用新的 **SharePoint Online** 索引標籤，此索引標籤會加入至儀表板的 [ **儲存** ] 區域，以管理 SharePoint Online 資源。


## <a name="what-can-i-manage-from-the-dashboard"></a>我可以從儀表板管理什麼？

### <a name="manage-your-online-libraries"></a>管理您的線上文件庫

- **新增程式庫。** 在 [SharePoint 文件庫]**** 索引標籤中，使用 [新增文件庫]****。 您可以執行所有的一般選項：
  - 選擇小組網站和文件庫類別。
  - 決定是否要使用版本控制。
  - 指派存取權限。
     **秘訣：** 若要找出您的文件庫將會繼承哪些小組網站許可權，如果您未指派許可權，請使用 **[流覽網站] 許可權**。
- **開啟文件庫。** 若要使用文件庫的內容，您必須在 Microsoft 365 中開啟。 只要選取文件庫，並按一下 [開啟文件庫]****。 您可以使用內容執行的動作將取決於您用來登入 SharePoint Online 的認證。
- **變更版本控制或存取權限。** 您可以使用 [檢視文件庫內容]**** 以檢視或變更文件庫的版本控制或存取權限。
- **刪除程式庫。** 在您檢查確定文件庫上沒有儲存任何後續還需要的資料後，請選取該文件庫並按一下 [刪除文件庫]****。 **警告：** 在您刪除 SharePoint Online 文件庫之前，請務必將任何想要保留的檔案儲存到另一個位置。 當您從 SharePoint 刪除文件庫時，所有專案都會永久刪除。 沒有任何可以復原的方法。

### <a name="manage-your-team-sites"></a>管理您的小組網站

- **管理 SharePoint 小組網站。** [ **管理小組網站** ] 動作可讓您登入 Microsoft 365 及管理 SharePoint Online 小組網站。 您可以在 Microsoft 365 中進行的動作取決於您用來登入的線上帳戶。 當您關閉 Microsoft 365 並返回儀表板時 **，按一下 [** 重新整理] 以顯示變更。
- **查看或變更小組網站許可權。** 由於程式庫預設會繼承其小組網站的許可權，因此很容易存取小組網站。 若要查看或變更小組網站的許可權，請選取小組網站或其任何文件庫，然後按一下 **[流覽網站許可權**]。 **秘訣：** 需要 SharePoint 小組網站許可權的精確點的協助嗎？ 在小組網站權限中有一個實用的 [深入了解](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/introduction-control-user-access-with-permissions-HA102771919.aspx?CTT=5&origin=HA102771924) 連結。

## <a name="tips"></a>提示

-   **按一下 [重新整理] 以顯示在 Microsoft 365 入口網站中所做的最新變更。** 開啟 Microsoft 365 以管理 SharePoint Online 之後，您必須重新整理顯示。 如果您維持長時間開啟儀表板，請按一下 [重新整理]**** 以確定您看到的是最新的變更。

-   **您可以在 SharePoint Online 中執行的動作，取決於您是在儀表板或 Microsoft 365 中工作。** 在儀表板上，SharePoint Online 的變更是使用系統管理員帳戶進行 Microsoft 365 整合。 但是當您從儀表板登入 Microsoft 365 時，您所使用的線上帳戶存取權限會決定您可以執行的動作。

     若要找出 Microsoft 365 整合的系統管理員帳戶，請開啟儀表板上的 [ **Microsoft 365** ] 索引標籤。

## <a name="other-things-you-might-want-to-do"></a>您可能想要執行的其他動作

-   [從任何地方使用 My Server 搭配 SharePoint Online 文件庫使用](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md)

-   [深入了解指派 SharePoint 小組網站權限的相關資訊](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/introduction-control-user-access-with-permissions-HA102771919.aspx?CTT=5&origin=HA102771924)

-   [了解如何使用 SharePoint 功能](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/get-started-with-sharepoint-2013-HA102772778.aspx)

-   [請查看可用的 Microsoft 365 商務方案](https://office.microsoft.com/business/compare-office-365-for-business-plans-FX102918419.aspx?CR_CC=200061904&WT.srch=1&WT.mc_ID=PS_bing_O365Comm_what-is-office-365-for_Text)

-   [將 Microsoft 365 與您的伺服器整合](Manage-Office-365-in-Windows-Server-Essentials.md)

-   [從儀表板管理其他 Microsoft Online Services](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)
