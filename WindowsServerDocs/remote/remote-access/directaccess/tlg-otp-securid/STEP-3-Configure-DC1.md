---
title: 步驟3設定 DC1
description: 本主題屬於測試實驗室指南-示範使用 OTP 驗證的 DirectAccess 和適用于 Windows Server 2016 的 RSA SecurID
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 836a2a08-3d22-48d2-873e-80d7e57ebbd6
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 07167f2bc68ab8c465a96ce00552339d04dbb198
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856071"
---
# <a name="step-3-configure-dc1"></a>步驟3設定 DC1

>適用於：Windows Server (半年通道)、Windows Server 2016

DC1 作為 corp.contoso.com 網域的網域控制站、DNS 伺服器和 DHCP 伺服器。 設定 DC1，如下所示：  
  
## <a name="verify-user1-has-a-user-principal-name-defined-on-dc1"></a>確認 User1 已在 DC1 上定義使用者主體名稱  
  
1.  在 DC1 上，開啟伺服器管理員，然後按一下左窗格中的 [ **AD DS** ]。 以滑鼠右鍵按一下 [ **DC1** ]，然後選取 [ **Active Directory 使用者和電腦**]。 在左窗格中，展開 [ **com\Users**]，然後按兩下 [User1]。  
  
2.  在 [**帳戶**] 索引標籤上，確認 [**使用者登入名稱**] 設定為 [User1]。 如果沒有，請在 [**使用者登入名稱**] 欄位中輸入**User1** 。  
  
3.  按一下 [確定]。 關閉 [Active Directory 使用者和電腦] 主控台。  
  


