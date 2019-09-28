---
title: 為 Windows Admin Center 閘道的所有使用者設定共用連線
description: 瞭解系統管理員如何設定其 Windows 系統管理中心 (Project 檀香山) 閘道一次, 讓所有使用者都能共用單一連線清單。
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/28/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: c632c178e91b92e0a80d8c72e8ea0ce3ce37b502
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357294"
---
# <a name="configure-shared-connections-for-all-users-of-the-windows-admin-center-gateway"></a>為 Windows Admin Center 閘道的所有使用者設定共用連線

> 適用於：Windows 管理中心預覽, Windows 系統管理中心

透過設定共用連線的能力, 閘道系統管理員可以針對指定的 Windows 管理中心閘道的所有使用者, 設定連線清單一次。 

在 Windows 管理中心閘道設定的 [**共用**連線] 索引標籤中, 閘道系統管理員可以從 [所有連線] 頁面新增伺服器、叢集和電腦連線, 包括標記連線的能力。 在 [共用連線] 清單中新增的任何連線和標籤, 都會出現在此 Windows 管理中心閘道所有 [連線] 頁面上的所有使用者。
    ![](../media/shared-cnxns-1.png)

當任何 Windows 系統管理中心使用者在已設定共用連線之後存取「所有連線」頁面時, 他們會看到其連線分為兩個區段:個人和共用的連接。 個人群組是特定使用者的連接清單, 並會在該使用者的瀏覽器會話中保存。 共用連接群組在所有使用者中都相同, 而且無法從 [所有連接] 頁面修改。
![](../media/shared-cnxns-2.png)