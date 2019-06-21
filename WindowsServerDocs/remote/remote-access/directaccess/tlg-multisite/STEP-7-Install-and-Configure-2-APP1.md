---
title: 步驟 7 的安裝和設定 2-APP1
description: 本主題是一部分的 「 測試實驗室指南： 示範 DirectAccess 多站台部署的 Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1cc0abc6-be4d-4cbe-bd0c-cc448bf294f6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4746ff5118814506d20983d3570881366297322f
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283185"
---
# <a name="step-7-install-and-configure-2-app1"></a>步驟 7 的安裝和設定 2-APP1

>適用於：Windows Server （半年通道），Windows Server 2016

2-APP1 提供網頁及檔案共用服務。 2-APP1 設定包含下列各項：  
  
- 2-APP1 上安裝作業系統  
  
- 設定 TCP/IP 內容  
  
- 2-APP1 加入 CORP2 網域  
  
- 2-APP1 上安裝網頁伺服器 (IIS) 角色  
  
- 2-APP1 上建立共用的資料夾 
  
## <a name="bkmk_InstallOS"></a>2-APP1 上安裝作業系統  
首先，安裝 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012。  
  
#### <a name="to-install-the-operating-system-on-2-app1"></a>2-APP1 上安裝作業系統  
  
1.  開始安裝 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 （完整安裝）。  
  
2.  依指示完成安裝，為本機 Administrator 帳戶指定強式密碼。 使用本機 Administrator 帳戶登入。  
  
3.  2-APP1 連接至具有網際網路存取的網路，並執行 Windows Update，以 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012，安裝最新的更新，然後中斷網際網路連線。  
  
4.  2-APP1 接上 2 Corpnet 子網路。  
  
## <a name="bkmk_TCP"></a>設定 TCP/IP 內容  
2-APP1 上，設定 TCP/IP 內容。  
  
#### <a name="to-configure-tcpip-properties"></a>設定 TCP/IP 內容  
  
1.  在 伺服器管理員 主控台中，按一下**本機伺服器**，然後在**屬性**區域中下, 一步**有線乙太網路連線**，按一下連結。  
  
2.  在 **網路連線** 視窗中，以滑鼠右鍵按一下**有線乙太網路連線**，然後按一下 **屬性**。  
  
3.  按一下 [網際網路通訊協定第 4 版 (TCP/IPv4)]  ，然後按一下 [內容]  。  
  
4.  按一下 [使用下列的 IP 位址]  。 在  **IP 位址**，型別**10.2.0.3**。 在 [子網路遮罩]  中，輸入 **255.255.255.0**。 在 **預設閘道**，型別**10.2.0.254**。  
  
5.  按一下 [使用下列的 DNS 伺服器位址]  。 在 **慣用 DNS 伺服器**，型別**10.2.0.1**。  
  
6.  按一下 [進階]  ，然後按一下 [DNS]  索引標籤。中**此連線的 DNS 尾碼**，型別**corp2.corp.contoso.com**，然後按一下**確定**兩次。  
  
7.  選取 **[網際網路通訊協定第 6 版 (TCP/IPv6)]** ，然後按一下 **[屬性]** 。  
  
8.  按一下 **使用下列 IPv6 位址**。 在  **IPv6 位址**，型別**2001:db8:2::3**。 在 **子網路首碼長度**，型別**64**。 在 **預設閘道**，型別**2001:db8:2::fe**。 按一下 **使用下列的 DNS 伺服器位址**，然後在**慣用 DNS 伺服器**，型別**2001:db8:2::1**。  
  
9. 按一下 [進階]  ，然後按一下 [DNS]  索引標籤。  
  
10. 在 **此連線的 DNS 尾碼**，型別**corp2.corp.contoso.com**，然後按一下**確定**兩次。  
  
11. 在 **有線乙太網路連線內容**對話方塊中，按一下**關閉**。  
  
12. 關閉 [網路連線]  視窗。  
  
## <a name="bkmk_JoinDomain"></a>2-APP1 加入 CORP2 網域  
加入 corp2.corp.contoso.com 網域 2-APP1。  
  
#### <a name="to-join-2-app1-to-the-corp2-domain"></a>2-APP1 加入 CORP2 網域  
  
1.  在 伺服器管理員 主控台中，在**本機伺服器**，請在**屬性**區域中下, 一步**電腦名稱**，按一下連結。  
  
2.  在 [系統內容]  的 [電腦名稱]  索引標籤上，按一下 [變更]  。  
  
3.  在 **電腦名稱**，型別**2-APP1**。 中**隸屬**，按一下**網域**，型別**corp2.corp.contoso.com**，然後按一下 **確定**。  
  
4.  當系統提示您輸入使用者名稱和密碼時，輸入**系統管理員**、 其密碼，以及然後按一下**確定**。  
  
5.  當您看見歡迎您加入 corp2.corp.contoso.com 網域 對話方塊中時，按一下**確定**。  
  
6.  當提示您必須重新啟動電腦時，按一下 [確定]  。  
  
7.  按一下 [系統內容]  對話方塊中的 [關閉]  。  
  
8.  當提示您重新啟動電腦時，請按一下 [立即重新啟動]  。  
  
9. 在電腦重新啟動之後，請按一下**切換使用者**，然後按一下**其他使用者**登入 CORP2 網域，以系統管理員帳戶。  
  
## <a name="bkmk_IIS"></a>2-APP1 上安裝網頁伺服器 (IIS) 角色  
安裝網頁伺服器 (IIS) 角色，才能將 2 APP1 變成網頁伺服器。  
  
#### <a name="to-install-the-web-server-iis-role"></a>若要安裝網頁伺服器 (IIS) 角色  
  
1.  在 [伺服器管理員] 主控台中，在**儀表板**，按一下**新增角色及功能**。  
  
2.  按一下 [**下一步]** 三次，進入伺服器角色選擇畫面  
  
3.  在上**選取伺服器角色**頁面上，選取**網頁伺服器 (IIS)** ，然後按一下**下一步**四次。  
  
4.  在 [確認安裝選項]  頁面上，按一下 [安裝]  。  
  
5.  確認安裝是否成功，然後再按一下**關閉**。  
  
## <a name="bkmk_Share"></a>2-APP1 上建立共用的資料夾  
2-APP1 上，建立共用的資料夾和文字檔案的資料夾內。  
  
#### <a name="to-create-a-shared-folder"></a>若要建立共用的資料夾  
  
1.  在 **開始**畫面上，輸入**explorer.exe**，然後按 ENTER 鍵。  
  
2.  按一下 **電腦**，然後按兩下**本機磁碟 （c:）** 。  
  
3.  按一下 **新的資料夾**，型別**檔案**，然後按 ENTER 鍵。 離開**本機磁碟**視窗中開啟。  
  
4.  上**開始**畫面上，輸入**notepad.exe**，以滑鼠右鍵按一下**記事本**，按一下**進階**，然後按一下 **身分執行系統管理員**。  
  
5.  在 **未命名-記事本**視窗中，輸入**這是 2-APP1 上的共用的檔案**。  
  
6.  按一下 **檔案**，按一下**儲存**，按一下 **電腦**，連按兩下**本機磁碟 （c:）** ，然後按兩下**檔案**資料夾。  
  
7.  在 **檔案名稱**，型別**example.txt**，然後按一下**儲存**。 關閉 [記事本]。  
  
8.  在**本機的磁碟** 視窗中，以滑鼠右鍵按一下**檔案**資料夾，指向**分享**，然後按一下**特定人員**。  
  
9. 上**檔案共用**對話方塊中，在下拉式清單中，按一下**Everyone**，然後按一下**新增**。 中**權限層級**for **Everyone**，按一下 **讀取/寫入**。  
  
10. 按一下 **共用**，然後按一下**完成**。  
  
11. 關閉**本機磁碟**視窗。  
  


