---
title: 設定共用的連接的所有使用者的 Windows Admin Center 閘道
description: 了解如何系統管理員可以設定一次其 Windows Admin Center (Project Honolulu) 閘道，讓所有使用者共用單一的連線清單。
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/28/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 1bdafa0d23934666fc0f6985249502e4960dc38e
ms.sourcegitcommit: 74d9eee13386a039186a70cdc97dc0b82e74bbf4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/29/2019
ms.locfileid: "9271001"
---
# 設定共用的連接的所有使用者的 Windows Admin Center 閘道

> 適用於： Windows Admin Center 預覽版

若要設定共用的連線的能力，閘道系統管理員可以設定一次為所有使用者特定的 Windows Admin Center 閘道連線清單。 

Windows Admin Center 閘道設定**共用連線**] 索引標籤，閘道系統管理員可以新增伺服器、 叢集及電腦連線就像從所有連線的頁面，包括能夠在標記連線。 這個 Windows Admin Center 閘道，從其所有連線 \] 頁面上的所有使用者將會都顯示任何連線和共用連線] 清單中新增的標記。
    ![](../media/shared-cnxns-1.png)

當 Windows Admin Center 中的任何使用者存取 「 所有連線 」 頁面共用連線已經設定之後時，他們將會看到他們的連線分成兩個區段： 個人和共用的連線。 個人群組是特定使用者的連線清單和跨該使用者的瀏覽器工作階段會持續存在。 共用的連線群組相同跨所有使用者，且無法修改從所有連線 \] 頁面。
![](../media/shared-cnxns-2.png)