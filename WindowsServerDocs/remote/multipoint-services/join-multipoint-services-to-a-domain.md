---
title: '將 MultiPoint 服務加入網域 (選擇性) '
description: 提供將 MultiPoint 服務加入網域的步驟
ms.date: 07/22/2016
ms.topic: article
ms.assetid: 623b7c21-dcbb-402e-8b5a-8e434cd225bd
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: f4e70f60a2e7c0d29b2b008c53ef751429fc868b
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97039425"
---
# <a name="join-the-multipoint-services-computer-to-a-domain-optional"></a>將 MultiPoint 服務電腦加入網域 (選用) 
如果您要透過 Active Directory 網域存取 MultiPoint 服務電腦，下一個步驟是將電腦新增至網域。

> [!IMPORTANT]
> 將電腦加入網域之前，您必須先驗證您的時區。 如需相關指示，請參閱 [設定日期、時間和時區](./set-the-date-time.md)。

1.  從 [開始] 畫面，開啟 [控制台]。 按一下 [ **系統及安全性**]，然後按一下 [ **系統**]。

2.  在 [電腦名稱、網域及工作群組設定] 底下，按一下 [變更設定]。

3.  在 [ **電腦名稱稱** ] 索引標籤上，按一下 [ **變更**]。

4.  在 [ **電腦名稱稱/網域變更** ] 對話方塊中，選取 [ **網域**]，輸入網域的名稱，然後按一下 **[確定**]，然後依照嚮導中的步驟完成程式。

5.  電腦重新開機之後，以系統管理員身分登入，並等候 MultiPoint 管理員開啟。

> [!IMPORTANT]
> 為了確保您的 MultiPoint 服務網域部署能正常運作，您需要設定幾個群組原則，並更新登錄。 如需詳細資訊，請參閱 [設定網域部署的群組原則](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265982(v=ws.11))。
