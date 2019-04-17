---
title: 停用 NAS 通知轉接 NPS 中
description: 本主題提供指示設定 Windows Server 2016 中的網路原則伺服器同時驗證。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: a09bfb03-95fc-4534-bf3c-97078ef6b07e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c25f3b5a94624a35099e84ede3296f7ab860da7c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="disable-nas-notification-forwarding-in-nps"></a>停用 NAS 通知轉接 NPS 中

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用此程序，來停用的 [開始] 畫面與停止從網路存取伺服器 (Nas) 的訊息中 NPS 設定遠端 RADIUS 伺服器群組成員。

當您已經設定遠端 RADIUS 伺服器群組]，然後在 NPS**連接要求原則**，您清除**轉送計量要求這個遠端 RADIUS 伺服器群組到**核取方塊，這些群組仍會傳送 NAS 開始和停止的訊息通知。 

這會建立不必要的網路流量。 若要排除此流量，來停用 NAS 轉寄個人伺服器每個遠端 RADIUS 伺服器群組中的通知。

若要完成此程序，您必須成員的**系統管理員**群組。

### <a name="to-disable-nas-notification-forwarding"></a>若要停用 NAS 通知轉接

1. 在伺服器管理員中，按一下 [**工具**，然後按**的網路原則伺服器**。 NPS 主控台開啟。

2. 在 NPS 主控台中，按兩下 [ **RADIUS 戶端與伺服器**，按一下 [**遠端 RADIUS 伺服器群組**，然後按兩下您想要設定遠端 RADIUS 伺服器群組。 遠端 RADIUS 伺服器群組**屬性**對話方塊。

3. 按兩下您想要設定，然後按一下 [群組成員**驗證日計量**索引標籤。

4. 在**計量**，清除**向前網路存取伺服器 [開始] 畫面與停止此伺服器通知**核取方塊，並再按**[確定]**。

5. 重複步驟 3、4 您想要設定的所有群組成員。

如需有關管理 NPS 的詳細資訊，請查看[管理的網路原則伺服器]](nps-manage-top.md)。

如需 NPS 的詳細資訊，請查看[的網路原則 Server (NPS)](nps-top.md)。
