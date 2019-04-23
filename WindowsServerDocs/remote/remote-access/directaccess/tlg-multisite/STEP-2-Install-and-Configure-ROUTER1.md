---
title: 步驟 2 的安裝和設定 「 路由器 1 」
description: 本主題是一部分的 「 測試實驗室指南： 示範 DirectAccess 多站台部署的 Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc20b1a0-540d-4531-a176-50b87c071600
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6a771b5eb8587d23bc67a7e7769264251afdb5bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838509"
---
# <a name="step-2-install-and-configure-router1"></a>步驟 2 的安裝和設定 「 路由器 1 」

>適用於：Windows Server （半年通道），Windows Server 2016

在這個多站台的測試實驗室指南中，路由器電腦提供的 IPv4 和 IPv6 的橋接器，在公司網路和 2 公司網路子網路之間，並做為 IP-HTTPS 和 Teredo 流量的路由器。  
  
- 在 「 路由器 1 」 上安裝作業系統 
  
- 設定 TCP/IP 內容，並將電腦重新命名  
  
- 關閉防火牆
  
- 設定路由和轉送
  
## <a name="install-the-operating-system-on-router1"></a>在 「 路由器 1 」 上安裝作業系統  
首先，安裝 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012。  
  
### <a name="to-install-the-operating-system-on-router1"></a>若要在 「 路由器 1 」 上安裝作業系統  
  
1.  開始安裝 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 （完整安裝）。  
  
2.  依指示完成安裝，為本機 Administrator 帳戶指定強式密碼。 使用本機 Administrator 帳戶登入。  
  
3.  連線到網路具有網際網路存取的 「 路由器 1 」，並執行 Windows Update，以針對 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012，安裝最新的更新，並再中斷網際網路連線。  
  
4.  連線到公司網路和 2 Corpnet 的子網路的 「 路由器 1 」。  
  
## <a name="configure-tcpip-properties-and-rename-the-computer"></a>設定 TCP/IP 內容，並將電腦重新命名  
路由器上的 TCP/IP 設定，然後重新命名為 「 路由器 1 」 的電腦。  
  
### <a name="to-configure-tcpip-properties-and-rename-the-computer"></a>若要設定 TCP/IP 內容，及重新命名電腦  
  
1.  在 伺服器管理員 主控台中，按一下**本機伺服器**，然後在**屬性**區域中下, 一步**有線乙太網路連線**，按一下連結。  
  
2.  中**網路連線**視窗中，以滑鼠右鍵按一下 連線到公司網路的網路介面卡，按一下**重新命名**，型別**Corpnet**，然後按 ENTER。  
  
3.  以滑鼠右鍵按一下**Corpnet**，然後按一下**屬性**。  
  
4.  按一下 [網際網路通訊協定第 4 版 (TCP/IPv4)]，然後按一下 [內容]。  
  
5.  按一下 [使用下列的 IP 位址]。 在  **IP 位址**，型別**10.0.0.254**。 在 **子網路遮罩**，型別**255.255.255.0**，然後按一下**確定**。  
  
6.  選取 **[網際網路通訊協定第 6 版 (TCP/IPv6)]**，然後按一下 **[屬性]**。  
  
7.  按一下 **使用下列 IPv6 位址**。 在  **IPv6 位址**，型別**2001:db8:1::fe**。 在 **子網路首碼長度**，型別**64**，然後按一下 **確定**。  
  
8.  在  **Corpnet 屬性**對話方塊中，按一下**關閉**。  
  
9. 中**網路連線**視窗中，以滑鼠右鍵按一下 連線到公司網路的 2 的網路介面卡，按一下**重新命名**，型別**2 Corpnet**，然後按 ENTER。  
  
10. 以滑鼠右鍵按一下**2 Corpnet**，然後按一下**屬性**。  
  
11. 按一下 [網際網路通訊協定第 4 版 (TCP/IPv4)]，然後按一下 [內容]。  
  
12. 按一下 [使用下列的 IP 位址]。 在  **IP 位址**，型別**10.2.0.254**。 在 **子網路遮罩**，型別**255.255.255.0**，然後按一下**確定**。  
  
13. 選取 **[網際網路通訊協定第 6 版 (TCP/IPv6)]**，然後按一下 **[屬性]**。  
  
14. 按一下 **使用下列 IPv6 位址**。 在  **IPv6 位址**，型別**2001:db8:2::fe**。 在 **子網路首碼長度**，型別**64**，然後按一下 **確定**。  
  
15. 在  **2 公司網路內容**對話方塊中，按一下**關閉**。  
  
16. 關閉 [網路連線]  視窗。  
  
17. 在 伺服器管理員 主控台中，在**本機伺服器**，請在**屬性**區域中下, 一步**電腦名稱**，按一下連結。  
  
18. 在 [系統內容] 的 [電腦名稱] 索引標籤上，按一下 [變更]。  
  
19. 上**電腦名稱/網域變更**對話方塊中，於**電腦名稱**，型別 **「 路由器 1 」**，然後按一下 **確定**。  
  
20. 當提示您必須重新啟動電腦時，按一下 [確定] 。  
  
21. 按一下 [系統內容]  對話方塊中的 [關閉] 。  
  
22. 當提示您重新啟動電腦時，請按一下 [立即重新啟動] 。  
  
23. 重新啟動電腦之後，使用本機系統管理員帳戶登入。  
  
## <a name="turn-off-the-firewall"></a>關閉防火牆  
此電腦已設定只是要提供的公司網路和 2 Corpnet 子網路; 之間的路由因此，防火牆必須關閉。  
  
### <a name="to-turn-off-the-firewall"></a>若要關閉防火牆  
  
1.  在 **開始**畫面上，輸入**wf.msc**，然後按 ENTER 鍵。  
  
2.  在 具有進階安全性的 Windows 防火牆中**動作**窗格中，按一下**屬性**。  
  
3.  在 **具有進階安全性的 Windows 防火牆**對話方塊的 **網域設定檔**索引標籤**防火牆狀態**，按一下 **關閉**。  
  
4.  在上**具有進階安全性的 Windows 防火牆**對話方塊的 **私人設定檔**索引標籤**防火牆狀態**，按一下 **關閉**。  
  
5.  在上**具有進階安全性的 Windows 防火牆**對話方塊的 **公用設定檔**索引標籤**防火牆狀態**，按一下 **關閉**，然後按一下 **確定**。  
  
6.  關閉具有進階安全性的 Windows 防火牆。  
  
## <a name="configure-routing-and-forwarding"></a>設定路由和轉送  
若要提供路由和轉送服務之間的公司網路和 2 Corpnet 子網路，您必須啟用網路介面上的轉送，並設定靜態路由子網路之間。  
  
### <a name="to-configure-static-routes"></a>若要設定靜態路由  
  
1.  在 **開始**畫面上，輸入**cmd.exe**，然後按 ENTER 鍵。  
  
2.  啟用使用下列命令，這兩個網路介面卡的 IPv4 和 IPv6 介面上的轉送。 輸入每個命令之後, 按 ENTER 鍵。  
  
    ```  
    netsh interface IPv4 set interface Corpnet forwarding=enabled  
    netsh interface IPv4 set interface 2-Corpnet forwarding=enabled  
    netsh interface IPv6 set interface Corpnet forwarding=enabled  
    netsh interface IPv6 set interface 2-Corpnet forwarding=enabled  
    ```  
  
3.  啟用路由的公司網路和 2 Corpnet 子網路之間的 IP-HTTPS。  
  
    ```  
    netsh interface IPv6 add route 2001:db8:1:1000::/59 Corpnet 2001:db8:1::2  
    netsh interface IPv6 add route 2001:db8:2:2000::/59 2-Corpnet 2001:db8:2::20  
    ```  
  
4.  啟用 Teredo 路由之間的公司網路和 2 Corpnet 子網路。  
  
    ```  
    netsh interface IPv6 add route 2001:0:836b:2::/64 Corpnet 2001:db8:1::2  
    netsh interface IPv6 add route 2001:0:836b:14::/64 2-Corpnet 2001:db8:2::20  
    ```  
  
5.  關閉 [命令提示字元] 視窗。
