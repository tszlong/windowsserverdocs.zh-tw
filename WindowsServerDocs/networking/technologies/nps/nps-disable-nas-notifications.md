---
title: 停用 NPS 中的 NAS 通知轉送
description: 本主題提供在 Windows Server 2016 中設定網路原則伺服器並行驗證的指示。
manager: brianlic
ms.topic: article
ms.assetid: a09bfb03-95fc-4534-bf3c-97078ef6b07e
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 72ac7d13005fef609df437ce3fcb4e12f617a400
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87946972"
---
# <a name="disable-nas-notification-forwarding-in-nps"></a>停用 NPS 中的 NAS 通知轉送

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用此程式來停用從網路存取伺服器轉送啟動和停止訊息的 (Nas) 至 NPS 中設定的遠端 RADIUS 伺服器群組的成員。

當您設定遠端 RADIUS 伺服器群組，而且在 NPS 連線**要求原則**中，您清除 [將**要求轉寄到此遠端 RADIUS 伺服器群組**] 核取方塊時，這些群組仍會傳送至 NAS 啟動和停止通知訊息。

這會產生不必要的網路流量。 若要排除此流量，請停用每個遠端 RADIUS 伺服器群組中個別伺服器的 NAS 通知轉送。

若要完成此程序，您必須是 **Administrators** 群組的成員。

### <a name="to-disable-nas-notification-forwarding"></a>停用 NAS 通知轉送

1. 在 [伺服器管理員] 中按一下 [工具]****，然後按一下 [網路原則伺服器]****。 NPS 主控台隨即開啟。

2. 在 NPS 主控台中，按兩下 [ **RADIUS 用戶端和伺服器**]，按一下 [**遠端 radius 伺服器群組**]，然後按兩下您要設定的遠端 radius 伺服器群組。 [遠端 RADIUS 伺服器群組**屬性**] 對話方塊隨即開啟。

3. 按兩下您想要設定的群組成員，然後按一下 [**驗證/帳戶**處理] 索引標籤。

4. 在 [**帳戶**處理] 中，清除 [**轉寄網路存取伺服器啟動和停止通知到這部伺服器**] 核取方塊，然後按一下 **[確定]**。

5. 針對您想要設定的所有群組成員，重複步驟3和4。

如需管理 NPS 的詳細資訊，請參閱[管理網路原則伺服器](nps-manage-top.md)。

如需有關 NPS 的詳細資訊，請參閱[網路原則伺服器 (NPS) ](nps-top.md)。
