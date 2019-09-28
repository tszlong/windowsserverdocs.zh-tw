---
title: 步驟3驗證部署
description: 本主題是在 Windows Server 2016 中遠端系統管理 DirectAccess 用戶端指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a78a078-d2e7-4cbd-b8d5-20cfb6d1524b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 81ac8bf7321df915330d8d706fa5ba3912b8f54c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367292"
---
# <a name="step-3-verify-the-deployment"></a>步驟3驗證部署

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題說明如何確認您已正確設定部署，以進行 DirectAccess 用戶端的遠端系統管理。  
  
### <a name="to-verify-proper-deployment"></a>確認適當的部署  
  
1.  將 DirectAccess 用戶端電腦連線到公司網路，並取得群組原則物件。  
  
2.  在用戶端電腦上，按一下通知區域中的 [**網路**連線] 圖示，以存取 DirectAccess 媒體管理員。  
  
3.  按一下 [ **DirectAccess**連線]，您會看到狀態為 [**本機連接**]。  
  
4.  從公司網路移除電腦，並將它連線到公用網路。  
  
5.  在命令提示字元中，輸入**nltest/dsgetdc： [完整功能變數名稱]** 。 此命令會確認用戶端可以存取公司網路。 如果無法存取網域控制站，下列錯誤訊息將會顯示網域不存在的報告：ERROR_NO_SUCH_DOMAIN.  
  


