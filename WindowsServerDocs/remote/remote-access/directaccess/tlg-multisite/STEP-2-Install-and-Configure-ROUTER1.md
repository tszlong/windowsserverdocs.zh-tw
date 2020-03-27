---
title: 步驟2安裝和設定 ROUTER1
description: 本主題屬於測試實驗室指南-示範適用于 Windows Server 2016 的 DirectAccess 多網站部署
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc20b1a0-540d-4531-a176-50b87c071600
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 3cd73f1a5e2612f4551be1f16e49e9645c5e12c0
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308737"
---
# <a name="step-2-install-and-configure-router1"></a>步驟2安裝和設定 ROUTER1

>適用於：Windows Server (半年通道)、Windows Server 2016

在此多網站測試實驗室指南中，路由器電腦會在公司網路與2公司網路之間提供 IPv4 和 IPv6 橋接器，並作為 IP-HTTPS 和 Teredo 流量的路由器。  
  
- 在 ROUTER1 上安裝作業系統 
  
- 設定 TCP/IP 屬性並重新命名電腦  
  
- 關閉防火牆
  
- 設定路由和轉送
  
## <a name="install-the-operating-system-on-router1"></a>在 ROUTER1 上安裝作業系統  
首先，安裝 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012。  
  
### <a name="to-install-the-operating-system-on-router1"></a>在 ROUTER1 上安裝作業系統  
  
1.  開始安裝 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 （完整安裝）。  
  
2.  依指示完成安裝，為本機 Administrator 帳戶指定強式密碼。 使用本機系統管理員帳戶登入。  
  
3.  將 ROUTER1 連線至具有網際網路存取權的網路，並執行 Windows Update 以安裝 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 的最新更新，然後中斷網際網路連線。  
  
4.  將 ROUTER1 連接到公司網路和2公司網路的子網。  
  
## <a name="configure-tcpip-properties-and-rename-the-computer"></a>設定 TCP/IP 屬性並重新命名電腦  
在路由器上設定 TCP/IP 設定，並將電腦重新命名為 ROUTER1。  
  
### <a name="to-configure-tcpip-properties-and-rename-the-computer"></a>設定 TCP/IP 內容並重新命名電腦  
  
1.  在伺服器管理員主控台中，按一下 [**本機伺服器**]，然後在 [**屬性**] 區域中的 [**有線乙太**網路連線] 旁，按一下連結。  
  
2.  在 [**網路**連線] 視窗中，以滑鼠右鍵按一下連接到公司網路的網路介面卡，按一下 [**重新命名**]，輸入**公司**網路，然後按 enter。  
  
3.  以滑鼠右鍵按一下 [**公司**網路]，然後按一下 [**屬性**]。  
  
4.  按一下 [網際網路通訊協定第 4 版 (TCP/IPv4)]，然後按一下 [內容]。  
  
5.  按一下 [使用下列的 IP 位址]。 在 [ **IP 位址**] 中，輸入**10.0.0.254**。 在 [**子網路遮罩**] 中輸入**255.255.255.0**，然後按一下 **[確定]** 。  
  
6.  選取 **[網際網路通訊協定第 6 版 (TCP/IPv6)]** ，然後按一下 **[屬性]** 。  
  
7.  按一下 **[使用下列 IPv6 位址**]。 在 [ **IPv6 位址**] 中，輸入**2001： db8：1：： fe**。 在 [**子網前置長度**] 中，輸入**64**，然後按一下 **[確定]** 。  
  
8.  在 [**公司**網路內容] 對話方塊上，按一下 [**關閉**]。  
  
9. 在 [**網路**連線] 視窗的網路介面卡上按一下滑鼠右鍵，然後按一下 [**重新命名**]，再輸入**2-公司**，然後按 enter。  
  
10. 以滑鼠右鍵按一下 [ **2-公司**網路]，然後按一下 [**屬性**]。  
  
11. 按一下 [網際網路通訊協定第 4 版 (TCP/IPv4)]，然後按一下 [內容]。  
  
12. 按一下 [使用下列的 IP 位址]。 在 [ **IP 位址**] 中，輸入**10.2.0.254**。 在 [**子網路遮罩**] 中輸入**255.255.255.0**，然後按一下 **[確定]** 。  
  
13. 選取 **[網際網路通訊協定第 6 版 (TCP/IPv6)]** ，然後按一下 **[屬性]** 。  
  
14. 按一下 **[使用下列 IPv6 位址**]。 在 [ **IPv6 位址**] 中，輸入**2001： db8：2：： fe**。 在 [**子網前置長度**] 中，輸入**64**，然後按一下 **[確定]** 。  
  
15. 在 [ **2-公司**內容] 對話方塊上，按一下 [**關閉**]。  
  
16. 關閉 [網路連線] 視窗。  
  
17. 在伺服器管理員主控台的 [**本機伺服器**] 的 [內容 **] 區域中，按一下 [** **電腦名稱稱**] 旁的連結。  
  
18. 在 [系統內容] 的 [電腦名稱] 索引標籤上，按一下 [變更]。  
  
19. 在 [**電腦名稱稱/網域變更**] 對話方塊的 [**電腦名稱稱**] 中，輸入**ROUTER1**，然後按一下 **[確定]** 。  
  
20. 當提示您必須重新啟動電腦時，按一下 [確定]。  
  
21. 按一下 [系統內容] 對話方塊中的 [關閉]。  
  
22. 當提示您重新啟動電腦時，請按一下 [立即重新啟動]。  
  
23. 電腦重新開機之後，使用本機系統管理員帳戶登入。  
  
## <a name="turn-off-the-firewall"></a>關閉防火牆  
這部電腦僅設定為提供公司網路與2公司網路子網之間的路由;因此，必須關閉防火牆。  
  
### <a name="to-turn-off-the-firewall"></a>關閉防火牆  
  
1.  在 [**開始**] 畫面上，輸入**wf**，然後按 enter。  
  
2.  在 [具有 Advanced Security 的 Windows 防火牆] 的 [**動作**] 窗格中 **，按一下 [** 內容]。  
  
3.  在 [**具有高級安全性的 Windows 防火牆**] 對話方塊的 [**網域設定檔**] 索引標籤上，于 [**防火牆狀態**] 中按一下 [**關閉**]。  
  
4.  在 [**具有高級安全性的 Windows 防火牆**] 對話方塊的 [**私人設定檔**] 索引標籤上，于 [**防火牆狀態**] 中按一下 [**關閉**]。  
  
5.  在 [**具有高級安全性的 Windows 防火牆**] 對話方塊的 [**公用設定檔**] 索引標籤上，按一下 [**防火牆狀態**] 中的 [**關閉**]，然後按一下 **[確定]** 。  
  
6.  接著並關閉 [具有進階安全性的 Windows 防火牆]。  
  
## <a name="configure-routing-and-forwarding"></a>設定路由和轉送  
若要在公司網路與2公司網之間提供路由和轉送服務，您必須在網路介面上啟用轉送，並設定子網之間的靜態路由。  
  
### <a name="to-configure-static-routes"></a>若要設定靜態路由  
  
1.  在 [**開始**] 畫面上，輸入**cmd.exe**，然後按 enter。  
  
2.  使用下列命令，啟用兩個網路介面卡的 IPv4 和 IPv6 介面上的轉送。 輸入每個命令之後，按 ENTER 鍵。  
  
    ```  
    netsh interface IPv4 set interface Corpnet forwarding=enabled  
    netsh interface IPv4 set interface 2-Corpnet forwarding=enabled  
    netsh interface IPv6 set interface Corpnet forwarding=enabled  
    netsh interface IPv6 set interface 2-Corpnet forwarding=enabled  
    ```  
  
3.  在公司網路與2公司網路的子網之間啟用 IP-HTTPS 路由。  
  
    ```  
    netsh interface IPv6 add route 2001:db8:1:1000::/59 Corpnet 2001:db8:1::2  
    netsh interface IPv6 add route 2001:db8:2:2000::/59 2-Corpnet 2001:db8:2::20  
    ```  
  
4.  在公司網路與2公司網路的子網之間啟用 Teredo 路由。  
  
    ```  
    netsh interface IPv6 add route 2001:0:836b:2::/64 Corpnet 2001:db8:1::2  
    netsh interface IPv6 add route 2001:0:836b:14::/64 2-Corpnet 2001:db8:2::20  
    ```  
  
5.  關閉 [命令提示字元] 視窗。
