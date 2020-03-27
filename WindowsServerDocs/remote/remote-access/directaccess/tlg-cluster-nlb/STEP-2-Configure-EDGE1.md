---
title: 步驟2設定 EDGE1
description: 本主題是測試實驗室指南的一部分-示範 windows Server 2016 的 Windows NLB 叢集中的 DirectAccess
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 84457351-1ca7-4e7c-8e2c-53d55b1fcdc0
ms.author: lizross
author: eross-msft
ms.openlocfilehash: eea83bd9788ffd704b6169c140adc68465f9325d
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310846"
---
# <a name="step-2-configure-edge1"></a>步驟2設定 EDGE1

>適用於：Windows Server (半年通道)、Windows Server 2016

下列程式是在 DirectAccess 伺服器上執行：

## <a name="to-configure-directaccess-on-edge1"></a>在 EDGE1 上設定 DirectAccess
  
1.  在 [**開始**] 畫面上，輸入**ramgmtui.exe**，然後按 enter。 如果出現 [使用者帳戶控制] 對話方塊，請確認其顯示的動作為您想要的動作，然後按一下 [是]。  
  
2.  在 [遠端存取管理] 主控台的左窗格中 **，按一下 [** 設定]。  
  
3.  在主控台中間窗格的 [**步驟2遠端存取服務器**] 區域中，按一下 [**編輯**]。  
  
4.  在 [**遠端存取服務器安裝程式**] 中，按一下 [**首碼**設定]。 在 [**首碼**設定] 頁面的 [ **IPv6 首碼指派給 DirectAccess 用戶端電腦**] 中，輸入**2001： db8：1：1000：：/59**，然後按 **[下一步]** 。  
  
5.  按一下 **[完成]** 。  
  
6.  在主控台中間窗格中，按一下 **[完成**]。  
  
7.  在 [**遠端存取審核**] 對話方塊中，檢查設定，**然後按一下 [** 套用]。 在 [套用遠端存取安裝精靈設定] 對話方塊上，按一下 [關閉]。
