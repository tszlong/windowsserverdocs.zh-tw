---
title: 步驟 3 驗證部署
description: 本主題是指南 Windows Server 2016 中，從遠端管理 DirectAccess 用戶端的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a78a078-d2e7-4cbd-b8d5-20cfb6d1524b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8d2b0b27d4b1d33971564672954667b49a87a4e0
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282772"
---
# <a name="step-3-verify-the-deployment"></a>步驟 3 驗證部署

>適用於：Windows Server （半年通道），Windows Server 2016

本主題描述如何確認您已正確設定您的遠端管理 DirectAccess 用戶端部署。  
  
### <a name="to-verify-proper-deployment"></a>若要確認正確的部署  
  
1.  DirectAccess 用戶端電腦連線到公司網路，並取得群組原則物件。  
  
2.  用戶端電腦上，按一下**網路連線**圖示來存取 DirectAccess 媒體管理員的通知區域中。  
  
3.  按一下  **DirectAccess 連線**，，您會看到的狀態是 **本機連線**。  
  
4.  將電腦從公司網路移除，並將它連接到公用網路。  
  
5.  在命令提示字元中，輸入**nltest /dsgetdc: [完整的網域名稱]** 。 此命令會確認公司網路是用戶端可以存取。 如果無法存取網域控制站，下列的錯誤訊息會顯示報告網域不存在：ERROR_NO_SUCH_DOMAIN。  
  


