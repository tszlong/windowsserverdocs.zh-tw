---
title: 移除 Active Directory Domain NPS 的伺服器
description: 您可以使用此主題為隊伍登記執行 Windows Server 2016 NPS 伺服器預設網域或其他網域中的網路原則伺服器的伺服器。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 68a94616-3c29-45bd-bd33-e4c578f119e1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 55c3b00146706831351ce63d1e5b74f45d7b9be1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="unregister-an-nps-server-from-an-active-directory-domain"></a>移除 Active Directory Domain NPS 的伺服器

>適用於：Windows Server（以每年次管道）、Windows Server 2016

過程中管理您的 NPS 伺服器部署，您可能會發現 NPS 伺服器移到另一個網域取代 NPS 伺服器，或淘汰 NPS 伺服器很有幫助。 

當您移動或解除 NPS 伺服器時，您可取消 NPS 伺服器 Active Directory 網域 NPS 伺服器具有讀取的 Active Directory 中帳號屬性權限的位置中。

資格在**系統管理員**，或相當於，才能執行這些程序最小值。

## <a name="to-unregister-an-nps-server"></a>若要移除 NPS 伺服器

1. 網域控制站在伺服器管理員中，按一下 [**工具**，然後按**Active Directory 使用者與電腦**。 Active Directory 使用者和電腦主機開啟。

2. 按一下**使用者**，然後按兩下 [ **RAS 及 IAS 伺服器]**。

3. 按一下**成員**索引標籤，然後選取您想要移除的 NPS 伺服器。

4. 按一下**移除**，按一下 [**是**，然後按一下 [ **[確定]**。

