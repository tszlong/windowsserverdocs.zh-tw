---
title: 步驟2設定 EDGE1
description: 瞭解如何在 EDGE1 伺服器上設定 DirectAccess。
manager: brianlic
ms.topic: article
ms.assetid: 84457351-1ca7-4e7c-8e2c-53d55b1fcdc0
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 52f94c6625286e9243419a668381995a18b28fae
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98039328"
---
# <a name="step-2-configure-edge1"></a>步驟2設定 EDGE1

>適用於：Windows Server (半年度管道)、Windows Server 2016

下列程式是在 DirectAccess 伺服器上執行：

## <a name="to-configure-directaccess-on-edge1"></a>在 EDGE1 上設定 DirectAccess

1.  在 [ **開始** ] 畫面上，輸入 **RAMgmtUI.exe**，然後按 enter。 如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**是**]。

2.  在 [遠端存取管理] 主控台的左窗格中 **，按一下 [** 設定]。

3.  在主控台中間窗格的 [ **步驟2遠端存取服務器** ] 區域中，按一下 [ **編輯**]。

4.  在 [ **遠端存取服務器安裝程式** ] 中，按一下 [ **首碼** 設定]。 在 [ **首碼** 設定] 頁面的 [ **指派給 DirectAccess 用戶端電腦的 IPv6 首碼**] 中，輸入 **2001： db8：1：1000：：/59**，然後按 **[下一步]**。

5.  按一下 [完成] 。

6.  在主控台的中間窗格中，按一下 **[完成]**。

7.  在 [ **遠端存取審核** ] 對話方塊中，檢查設定， **然後按一下 [** 套用]。 在 [套用遠端存取安裝精靈設定] 對話方塊上，按一下 [關閉]。
