---
title: 步驟3確認遠端存取 (VPN) 部署
description: 本主題屬於將 DirectAccess 新增至現有的遠端存取 (VPN) 部署的 Windows Server 2016 指南
manager: brianlic
ms.topic: article
ms.assetid: 43ac612e-2e77-418c-8171-ebb2086b7cb6
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 9da14f076f177e7e819c1529f9de647b5dde0500
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969215"
---
# <a name="step-3-verify-the-remote-access-vpn-deployment"></a>步驟3確認遠端存取 (VPN) 部署

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題說明如何確認您已正確設定您的 DirectAccess 部署。

### <a name="to-verify-access-to-internal-resources-through-directaccess"></a>確認透過 DirectAccess 存取內部資源

1.  將 DirectAccess 用戶端電腦連線到公司網路並取得群組原則。

2.  按一下通知區域中的 [網路連線]**** 圖示來存取 DA 媒體管理員。

3.  按一下 [DirectAccess 連線]****，您會看到狀態為 [本機連接]****。

4.  將用戶端電腦連線到外部網路，並嘗試存取內部資源。

    您應該能夠存取所有公司資源。



