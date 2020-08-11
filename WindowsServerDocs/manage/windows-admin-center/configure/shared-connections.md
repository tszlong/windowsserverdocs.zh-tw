---
title: 為 Windows Admin Center 閘道的所有使用者設定共用連線
description: 了解系統管理員如何只需設定其 Windows Admin Center (Honolulu 專案) 閘道一次，就能讓所有使用者都能共用單一連線清單。
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/28/2019
ms.localizationpriority: medium
ms.openlocfilehash: eea0e467629a6110d38ed3081a1320e002496cfb
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87970875"
---
# <a name="configure-shared-connections-for-all-users-of-the-windows-admin-center-gateway"></a>為 Windows Admin Center 閘道的所有使用者設定共用連線

> 適用於：Windows Admin Center 預覽版、Windows Admin Center

透過設定共用連線的能力，閘道管理員可以針對指定之 Windows Admin Center 閘道的所有使用者，一次設定連線清單。 這項功能僅適用於 Windows Admin Center 服務模式。

在 Windows Admin Center 閘道設定的 [共用連線]  索引標籤中，閘道管理員可以從所有連線頁面新增伺服器、叢集和電腦連線，包括標記連線的能力。 在共用連線清單中新增的任何連線和標籤，此 Windows Admin Center 閘道所有 [連線] 頁面上的所有使用者都會看見。

![Windows Admin Center - 共用連線頁面](../media/shared-cnxns-1.png)

若任何 Windows Admin Center 使用者在已設定共用連線之後存取「所有連線」頁面時，會看到其連線分為兩個區段：個人和共用連線。 個人群組是特定使用者的連線清單，並會在該使用者的瀏覽器工作階段中保存。 共用連線群組在所有使用者中都相同，而且無法從所有連線頁面修改。

![Windows Admin Center - 所有連線頁面](../media/shared-cnxns-2.png)