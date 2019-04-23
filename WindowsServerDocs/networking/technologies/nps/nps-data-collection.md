---
title: 網路原則伺服器的使用者資料收集
description: 哪些資訊用來協助驗證使用者，Windows Server 2016 中的網路原則伺服器。
author: MicrosoftGuyJFlo
manager: mtillman
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
ms.date: 05/01/2018
ms.openlocfilehash: 5bddd22c9c2f954435cc6ce37347d18c76ee7de3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888739"
---
# <a name="network-policy-server-user-data-collection"></a>網路原則伺服器的使用者資料收集

本文件說明如何尋找使用者資訊收集網路原則伺服器 (NPS)，萬一您想要將它移除。

>[!Note]
>如果您想要檢視或刪除個人資料，請檢閱中的 Microsoft 的指導方針[Windows 資料主體要求 GDPR](https://docs.microsoft.com/microsoft-365/compliance/gdpr-dsr-windows)站台。 如果您想要尋求 GDPR 的一般資訊，請參閱[GDPR 服務信任入口網站區段](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)。

## <a name="information-collected-by-nps"></a>NPS 所收集的資訊

- 時間戳記
- 事件時間戳記
- Username
- 完整合格的使用者名稱
- 用戶端 IP 位址
- 用戶端廠商
- 用戶端的好記名稱
- 驗證類型
- 關於 RADIUS 通訊協定的許多其他欄位

## <a name="gather-data-from-nps"></a>從 NPS 收集資料

如果啟用計量資料，並設定，使用者的 NPS 驗證嘗試的記錄可以取得從 SQL Server 或記錄檔，視設定而定。 

如果 SQL server 已計量資料，所有的查詢會記錄其中 User_Name = `'<username>'`。

如果記錄檔設定計量資料，然後搜尋記錄檔以取得`<username>`來尋找所有記錄項目。

網路原則與存取服務事件記錄項目會被視為傳銷計量資料，而且不需要收集。

如果未啟用計量資料，則使用者的 NPS 驗證嘗試的記錄，可由網路原則與存取服務事件記錄檔搜尋`<username>`。
