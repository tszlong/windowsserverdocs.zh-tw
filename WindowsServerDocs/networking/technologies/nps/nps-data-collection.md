---
title: 網路原則伺服器使用者資料收集
description: Windows Server 2016 中的網路原則伺服器會使用哪些資訊來協助驗證使用者。
author: MicrosoftGuyJFlo
manager: mtillman
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ''
ms.date: 05/01/2018
ms.openlocfilehash: d393ad4af81ee1c24fa5f28b8a3b05217e7b34dd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396296"
---
# <a name="network-policy-server-user-data-collection"></a>網路原則伺服器使用者資料收集

本檔說明如何在您想要移除網路原則伺服器（NPS）的事件中尋找其所收集的使用者資訊。

>[!Note]
>如果您有興趣查看或刪除個人資料, 請參閱 Microsoft 在[GDPR 的 Windows 資料主體要求](https://docs.microsoft.com/microsoft-365/compliance/gdpr-dsr-windows)網站中的指引。 如果您要尋找有關 GDPR 的一般資訊, 請參閱[服務信任入口網站的 GDPR 一節](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)。

## <a name="information-collected-by-nps"></a>NPS 收集的資訊

- Timestamp
- 事件時間戳記
- 使用者名稱
- 完整的使用者名稱
- 用戶端 IP 位址
- 用戶端廠商
- 用戶端易記名稱
- 驗證類型
- 關於 RADIUS 通訊協定的其他許多欄位

## <a name="gather-data-from-nps"></a>從 NPS 收集資料

如果已啟用並設定計量資料，則可以根據設定，從 SQL Server 或記錄檔中取得使用者的 NPS 驗證嘗試記錄。 

如果已針對 SQL Server 設定計量資料，請查詢使用者名 = `'<username>'` 的所有記錄。

如果已針對記錄檔設定計量資料，請搜尋記錄`<username>`檔中的以尋找所有記錄專案。

網路原則與存取服務的事件記錄檔專案會被視為重複到帳戶處理資料，而不需要收集。

如果未啟用計量資料，則可以藉由搜尋`<username>`，從 [網路原則與存取服務] 事件記錄檔中取得使用者的 NPS 驗證嘗試記錄。
