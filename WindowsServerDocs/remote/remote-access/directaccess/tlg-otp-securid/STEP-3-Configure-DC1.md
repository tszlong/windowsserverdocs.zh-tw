---
title: 步驟 3 設定 DC1
description: 本主題是測試實驗室指南-使用 OTP 驗證和 RSA SecurID for Windows Server 2016 的示範 DirectAccess 的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 836a2a08-3d22-48d2-873e-80d7e57ebbd6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 338e214ac10796d3f9864aef74190f2d27f173b7
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281351"
---
# <a name="step-3-configure-dc1"></a>步驟 3 設定 DC1

>適用於：Windows Server （半年通道），Windows Server 2016

DC1 會做為網域控制站、 DNS 伺服器和 corp.contoso.com 網域的 DHCP 伺服器。 設定 DC1 如下所示：  
  
## <a name="verify-user1-has-a-user-principal-name-defined-on-dc1"></a>驗證 User1 具有定義在 DC1 上的使用者主體名稱  
  
1.  在 DC1 上，開啟 [伺服器管理員]，然後按一下**AD DS**的左窗格中。 以滑鼠右鍵按一下**DC1** ，然後選取**Active Directory 使用者和電腦**。 在左窗格中展開**corp.contoso.com\Users**，然後按兩下 User1。  
  
2.  在上**帳號** 索引標籤上，確認**使用者登入名稱**設為 User1。 如果沒有，然後輸入**User1**中**使用者登入名稱**欄位。  
  
3.  按一下 [確定]  。 關閉 [Active Directory 使用者和電腦]  主控台。  
  


