---
title: 步驟 2 設定 EDGE1
description: 本主題是測試實驗室指南-示範在具備 Windows NLB 的 Windows Server 2016 的叢集中的 DirectAccess 的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 84457351-1ca7-4e7c-8e2c-53d55b1fcdc0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6c31e65fc7210849564bd0541085322a7a6c284e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838579"
---
# <a name="step-2-configure-edge1"></a>步驟 2 設定 EDGE1

>適用於：Windows Server （半年通道），Windows Server 2016

DirectAccess 伺服器上執行下列程序：

## <a name="to-configure-directaccess-on-edge1"></a>在 EDGE1 上設定 DirectAccess
  
1.  在 **開始**畫面上，輸入**RAMgmtUI.exe**，然後按 ENTER 鍵。 如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**是**]。  
  
2.  在遠端存取管理主控台中，在左窗格中，按一下**組態**。  
  
3.  在主控台的中間窗格中**步驟 2 遠端存取伺服器**區域中，按一下**編輯**。  
  
4.  在 **遠端存取伺服器安裝**精靈中，按一下**首碼設定**。 上**前置詞組態**頁面上，於**指派給 DirectAccess 用戶端電腦的 IPv6 首碼**，輸入**2001:db8:1:1000:: / 59**，然後按一下 [**下一步]**.  
  
5.  按一下 **[完成]**。  
  
6.  在主控台中間窗格中，按一下**完成**。  
  
7.  在 [**遠端存取權檢閱**] 對話方塊中，檢閱組態設定，然後按一下**套用**。 在 [套用遠端存取安裝精靈設定]  對話方塊上，按一下 [關閉] 。
