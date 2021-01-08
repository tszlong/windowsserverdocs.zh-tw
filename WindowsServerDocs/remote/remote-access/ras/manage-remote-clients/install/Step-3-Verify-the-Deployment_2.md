---
title: 步驟3驗證部署
description: 瞭解如何確認您已正確設定部署以進行 DirectAccess 用戶端的遠端系統管理。
manager: brianlic
ms.topic: article
ms.assetid: 6a78a078-d2e7-4cbd-b8d5-20cfb6d1524b
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 7b59a37eb6d24c6de61acdff9fa544f1ec6f0fce
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040008"
---
# <a name="step-3-verify-the-deployment"></a>步驟3驗證部署

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題說明如何確認您已正確設定部署以進行 DirectAccess 用戶端的遠端系統管理。

### <a name="to-verify-proper-deployment"></a>確認適當的部署

1.  將 DirectAccess 用戶端電腦連線到公司網路，並取得群組原則物件。

2.  在用戶端電腦上，按一下通知區域中的 [ **網路** 連線] 圖示，以存取 DirectAccess 媒體管理員。

3.  按一下 [ **DirectAccess 連接**]，您會看到狀態為 [ **本機連線**]。

4.  從公司網路移除電腦，並將它連線到公用網路。

5.  在命令提示字元中，輸入 **nltest/dsgetdc： [完整功能變數名稱]**。 此命令會確認用戶端可以存取公司網路。 如果無法存取網域控制站，下列錯誤訊息將會顯示網域不存在的報告： ERROR_NO_SUCH_DOMAIN。



