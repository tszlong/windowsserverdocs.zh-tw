---
title: 步驟3設定 DC1
description: 本主題是測試實驗室指南的一部分-示範使用 OTP 驗證的 DirectAccess 和適用于 Windows Server 2016 的 RSA SecurID
manager: brianlic
ms.topic: article
ms.assetid: 836a2a08-3d22-48d2-873e-80d7e57ebbd6
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 642449e9c0c6524aa053cfb73c57b69507e5f4eb
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950454"
---
# <a name="step-3-configure-dc1"></a>步驟3設定 DC1

>適用於：Windows Server (半年度管道)、Windows Server 2016

DC1 作為 corp.contoso.com 網域的網域控制站、DNS 伺服器和 DHCP 伺服器。 將 DC1 設定如下：

## <a name="verify-user1-has-a-user-principal-name-defined-on-dc1"></a>確認 User1 具有在 DC1 上定義的使用者主體名稱

1.  在 DC1 上，開啟伺服器管理員，然後按一下左窗格中的 [ **AD DS** ]。 以滑鼠右鍵按一下 **DC1** ，然後選取 [ **Active Directory 消費者和電腦**]。 在左窗格中，展開 [ **com\Users**]，然後按兩下 [User1]。

2.  在 [ **帳戶** ] 索引標籤上，確認 [ **使用者登入名稱** ] 設定為 [User1]。 如果沒有，請在 [**使用者登入名稱**] 欄位中輸入 **User1** 。

3.  按一下 [確定]。 關閉 [Active Directory 使用者和電腦] 主控台。



