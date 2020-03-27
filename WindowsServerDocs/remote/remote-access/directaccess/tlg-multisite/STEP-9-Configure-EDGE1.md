---
title: 步驟9設定 EDGE1
description: 本主題屬於測試實驗室指南-示範適用于 Windows Server 2016 的 DirectAccess 多網站部署
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6e8d85b-de65-43b3-bf3e-ec84471a1fcc
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 74c4eae329698d33b160ac7180bbabd6d1d8fbad
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314517"
---
# <a name="step-9-configure-edge1"></a>步驟9設定 EDGE1

>適用於：Windows Server (半年通道)、Windows Server 2016

下列程式會在 EDGE1 伺服器上執行：  
  
1. 在 EDGE1 上設定 DNS 伺服器。 必須從 EDGE1 上的 corp2.corp.contoso.com 網域設定 DNS 伺服器。  
  
2. 設定子網之間的路由。 設定 EDGE1 上的路由，以啟用公司網路與2公司網路子網之間的通訊。  
  
## <a name="configure-the-dns-servers-on-edge1"></a><a name="IPv6"></a>在 EDGE1 上設定 DNS 伺服器  
  
1.  在伺服器管理員主控台中，按一下 [**本機伺服器**]，然後在 [**屬性**] 區域的 [**公司**網路] 旁，按一下連結。  
  
2.  在 [網路連線] 視窗中，以滑鼠右鍵按一下 [**公司**網路]，然後按一下 [**屬性**]。  
  
3.  按一下 [網際網路通訊協定第 4 版 (TCP/IPv4)]，然後按一下 [內容]。  
  
4.  在 [**其他 DNS 伺服器**] 中，輸入**10.2.0.1**。 然後按一下 [確定]。  
  
5.  選取 **[網際網路通訊協定第 6 版 (TCP/IPv6)]** ，然後按一下 **[屬性]** 。  
  
6.  在 [**其他 DNS 伺服器**] 中，輸入**2001： db8：2：： 1** ，然後按一下 **[確定]** 。  
  
7.  在 [**公司**網路內容] 對話方塊上，按一下 [**關閉**]。  
  
8.  關閉 [網路連線] 視窗。  
  
## <a name="configure-routing-between-subnets"></a><a name="ConfigRouting"></a>設定子網之間的路由  
  
1.  在 [**開始**] 畫面上，輸入**cmd.exe**，在**cmd**上按一下滑鼠右鍵，按一下 [ **Advanced**]，然後按一下 [以**系統管理員身分執行**]。 如果出現 [使用者帳戶控制] 對話方塊，請確認其顯示的動作為您想要的動作，然後按一下 [是]。  
  
2.  在 [命令提示字元] 視窗中，輸入下列命令。 輸入每個命令之後，按 ENTER 鍵。  
  
    ```  
    netsh interface IPv4 add route 10.2.0.0/24 Corpnet 10.0.0.254  
    netsh interface IPv6 add route 2001:db8:2::/64 Corpnet 2001:db8:1::fe  
    ```  
  
3.  關閉 [命令提示字元] 視窗。  
  


