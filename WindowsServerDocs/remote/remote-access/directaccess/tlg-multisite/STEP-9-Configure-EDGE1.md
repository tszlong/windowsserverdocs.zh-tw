---
title: 步驟 9 設定 EDGE1
description: 本主題是一部分的 「 測試實驗室指南： 示範 DirectAccess 多站台部署的 Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6e8d85b-de65-43b3-bf3e-ec84471a1fcc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b0a47a436bbd11c795caa8b402054ae0d2c3282f
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281369"
---
# <a name="step-9-configure-edge1"></a>步驟 9 設定 EDGE1

>適用於：Windows Server （半年通道），Windows Server 2016

下列程序會執行 EDGE1 伺服器上：  
  
1. 在 EDGE1 上設定的 DNS 伺服器。 必須在 EDGE1 上設定從 corp2.corp.contoso.com 網域的 DNS 伺服器。  
  
2. 設定子網路之間的路由。 設定路由，在 EDGE1 能夠在公司網路和 2 Corpnet 的子網路之間的通訊。  
  
## <a name="IPv6"></a>在 EDGE1 上設定的 DNS 伺服器  
  
1.  在 伺服器管理員 主控台中，按一下**本機伺服器**，然後在**屬性**區域中下, 一步 **Corpnet**，按一下連結。  
  
2.  在 [網路連線] 視窗中，以滑鼠右鍵按一下**Corpnet**，然後按一下**屬性**。  
  
3.  按一下 [網際網路通訊協定第 4 版 (TCP/IPv4)]  ，然後按一下 [內容]  。  
  
4.  在 **備用 DNS 伺服器**，型別**10.2.0.1**。 然後按一下 [確定]  。  
  
5.  選取 **[網際網路通訊協定第 6 版 (TCP/IPv6)]** ，然後按一下 **[屬性]** 。  
  
6.  在 **備用 DNS 伺服器**，型別**2001:db8:2::1** ，然後按一下 **確定**。  
  
7.  在 [ **Corpnet 屬性**] 對話方塊中，按一下**關閉**。  
  
8.  關閉 [網路連線]  視窗。  
  
## <a name="ConfigRouting"></a>設定子網路之間的路由  
  
1.  上**開始**畫面上，輸入**cmd.exe**，以滑鼠右鍵按一下**cmd**，按一下 **進階**，然後按一下 **身分執行系統管理員**。 如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**是**]。  
  
2.  在 [命令提示字元] 視窗中，輸入下列命令。 輸入每個命令之後, 按 ENTER 鍵。  
  
    ```  
    netsh interface IPv4 add route 10.2.0.0/24 Corpnet 10.0.0.254  
    netsh interface IPv6 add route 2001:db8:2::/64 Corpnet 2001:db8:1::fe  
    ```  
  
3.  關閉 [命令提示字元] 視窗。  
  


