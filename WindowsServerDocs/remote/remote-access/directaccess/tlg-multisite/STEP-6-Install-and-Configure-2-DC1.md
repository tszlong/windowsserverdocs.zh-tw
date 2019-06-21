---
title: 步驟 6 的安裝和設定 2-DC1
description: 本主題是一部分的 「 測試實驗室指南： 示範 DirectAccess 多站台部署的 Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3d66901a-c40b-474c-9948-f989f399cfea
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8c3e12f9d64619ce00e578c7a8096c764e68ff7d
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283162"
---
# <a name="step-6-install-and-configure-2-dc1"></a>步驟 6 的安裝和設定 2-DC1

>適用於：Windows Server （半年通道），Windows Server 2016

2-DC1 會提供下列服務：  
  
-   Corp2.corp.contoso.com Active Directory 網域服務 (AD DS) 網域的網域控制站。  
  
-   Corp2.corp.contoso.com DNS 網域的 DNS 伺服器。  
  
2-DC1 設定是由下列項目所組成：  
  
- 2-DC1 上安裝作業系統
  
- 設定 TCP/IP 內容

- 2-DC1 設定為網域控制站和 DNS 伺服器

- 以 CORP\User1 提供群組原則權限
  
- 允許 CORP2 電腦，以取得電腦憑證
  
- 強制 DC1 與 2-DC1 之間的複寫
  
## <a name="install-the-operating-system-on-2-dc1"></a>2-DC1 上安裝作業系統  
首先，安裝 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012。  
  
### <a name="to-install-the-operating-system-on-2-dc1"></a>2-DC1 上安裝作業系統  
  
1.  啟動 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 的安裝。  
  
2.  請依照下列指示完成安裝，指定 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 （完整安裝） 和本機系統管理員帳戶的強式密碼。 使用本機 Administrator 帳戶登入。  
  
3.  2-DC1 連接至具有網際網路存取的網路，並執行 Windows Update，以 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012，安裝最新的更新，然後中斷網際網路連線。  
  
4.  2-DC1 接上 2 Corpnet 子網路。  
  
## <a name="configure-tcpip-properties"></a>設定 TCP/IP 內容  
使用靜態 IP 位址設定的 TCP/IP 通訊協定。  
  
### <a name="to-configure-tcpip-on-2-dc1"></a>若要在 2-DC1 設定 TCP/IP  
  
1.  在 伺服器管理員 主控台中，按一下**本機伺服器**，然後在**屬性**區域中下, 一步**有線乙太網路連線**，按一下連結。  
  
2.  在 [網路連線]  中，用滑鼠右鍵按一下 [有線乙太網路連線]  ，然後按一下 [內容]  。  
  
3.  按一下 [網際網路通訊協定第 4 版 (TCP/IPv4)]  ，然後按一下 [內容]  。  
  
4.  按一下 [使用下列的 IP 位址]  。 在  **IP 位址**，型別**10.2.0.1**。 在 [子網路遮罩]  中，輸入 **255.255.255.0**。 在 **預設閘道**，型別**10.2.0.254**。 按一下 **使用下列的 DNS 伺服器位址**，請在**慣用 DNS 伺服器**，型別**10.2.0.1**，然後在**備用 DNS 伺服器**，類型**10.0.0.1**。  
  
5.  按一下 [進階]  ，然後按一下 [DNS]  索引標籤。  
  
6.  在 **此連線的 DNS 尾碼**，型別**corp2.corp.contoso.com**，然後按一下**確定**兩次。  
  
7.  選取 **[網際網路通訊協定第 6 版 (TCP/IPv6)]** ，然後按一下 **[屬性]** 。  
  
8.  按一下 **使用下列 IPv6 位址**。 在  **IPv6 位址**，型別**2001:db8:2::1**。 在 **子網路首碼長度**，型別**64**。 在 **預設閘道**，型別**2001:db8:2::fe**。 按一下 **使用下列的 DNS 伺服器位址**，請在**慣用 DNS 伺服器**，型別**2001:db8:2::1**，然後在**備用 DNS 伺服器**，型別**2001:db8:1::1**。  
  
9. 按一下 [進階]  ，然後按一下 [DNS]  索引標籤。  
  
10. 在 **此連線的 DNS 尾碼**，型別**corp2.corp.contoso.com**，然後按一下**確定**兩次。  
  
11. 在 [**有線乙太網路連線內容**] 對話方塊中，按一下**關閉**。  
  
12. 關閉 [網路連線]  視窗。  
  
13. 在 伺服器管理員 主控台中，在**本機伺服器**，請在**屬性**區域中下, 一步**電腦名稱**，按一下連結。  
  
14. 在 [系統內容]  的 [電腦名稱]  索引標籤上，按一下 [變更]  。  
  
15. 上**電腦名稱/網域變更**對話方塊中，於**電腦名稱**，型別**2-DC1**，然後按一下 **確定**。  
  
16. 當提示您必須重新啟動電腦時，按一下 [確定]  。  
  
17. 按一下 [系統內容]  對話方塊中的 [關閉]  。  
  
18. 當提示您重新啟動電腦時，請按一下 [立即重新啟動]  。  
  
19. 重新啟動之後，使用本機系統管理員帳戶登入。  
  
## <a name="configure-2-dc1-as-a-domain-controller-and-dns-server"></a>2-DC1 設定為網域控制站和 DNS 伺服器  
當做 corp2.corp.contoso.com 網域的網域控制站以及 corp2.corp.contoso.com DNS 網域的 DNS 伺服器，請設定 2-DC1。  
  
### <a name="to-configure-2-dc1-as-a-domain-controller-and-dns-server"></a>若要將 2-DC1 設定為網域控制站和 DNS 伺服器  
  
1.  在 [伺服器管理員] 主控台中，在**儀表板**，按一下**新增角色及功能**。  
  
2.  按一下 [**下一步]** 三次，進入伺服器角色選擇畫面  
  
3.  在 **選取伺服器角色**頁面上，選取**Active Directory 網域服務**。 按一下 [**將功能加入**當出現提示，然後按一下**下一步]** 三次。  
  
4.  在 [確認安裝選項]  頁面上，按一下 [安裝]  。  
  
5.  安裝成功完成時，按一下**此伺服器升級為網域控制站**。  
  
6.  在 Active Directory 網域服務設定精靈，在**部署組態**頁面上，按一下**將新的網域新增至現有的樹系**。  
  
7.  在 **父系網域名稱**，型別**corp.contoso.com**，請在**新網域名稱**，型別**corp2**。  
  
8.  底下**提供的認證來執行這項作業**，按一下**變更**。 在上**Windows 安全性**對話方塊中，於**使用者名稱**，型別**corp.contoso.com\Administrator**，然後在**密碼**，輸入 corp\系統管理員密碼，按一下**確定**，然後按一下**下一步**。  
  
9. 在 **網域控制站選項**頁面上，請確定**站台名稱**會**第二個站台**。 底下**輸入目錄服務還原模式 (DSRM) 密碼**，請在**密碼**並**確認密碼**兩次，輸入強式密碼，然後按一下 [ **下一步]** 五次。  
  
10. 在 **先決條件檢查**頁面上之後會驗證必要元件，, 按一下**安裝**。  
  
11. 等候直到精靈完成 Active Directory 和 DNS 服務的組態，然後按**關閉**。  
  
12. 在電腦重新啟動之後，登入 CORP2 網域使用的系統管理員帳戶。  
  
## <a name="provide-group-policy-permissions-to-corpuser1"></a>以 CORP\User1 提供群組原則權限  
您可以使用此程序，提供 CORP\User1 使用者建立和變更 corp2 群組原則物件的完整權限。  
  
### <a name="to-provide-group-policy-permissions"></a>提供群組原則權限  
  
1.  在 **開始**畫面上，輸入**gpmc.msc**，然後按 ENTER 鍵。  
  
2.  在 [群組原則管理] 主控台中，開啟**樹系： corp.contoso.com/Domains/corp2.corp.contoso.com**。  
  
3.  在 [詳細資料] 窗格中，按一下**委派**] 索引標籤。在 [**權限**下拉式清單中，按一下**連結 Gpo**。  
  
4.  按一下 **新增**，並在新**選取使用者、 電腦或群組** 對話方塊中，按一下 **位置**。  
  
5.  上**位置**對話方塊中，於**位置**樹狀結構中，按一下  **corp.contoso.com**，然後按一下**確定**。  
  
6.  中**輸入要選取的物件名稱**型別**User1**，按一下**確定**，然後在**新增群組或使用者**對話方塊中，按一下  **確定**.  
  
7.  在 群組原則管理主控台中，在樹狀目錄中，按一下**群組原則物件**，並在 詳細資料窗格中按一下**委派** 索引標籤。  
  
8.  按一下 **新增**，並在新**選取使用者、 電腦或群組** 對話方塊中，按一下 **位置**。  
  
9. 上**位置**對話方塊中，於**位置**樹狀結構中，按一下  **corp.contoso.com**，然後按一下**確定**。  
  
10. 在 **輸入要選取的物件名稱**型別**User1**，按一下  **確定**。  
  
11. 在 群組原則管理主控台中，在樹狀目錄中，按一下**WMI 篩選器**，並在 詳細資料窗格中按一下**委派** 索引標籤。  
  
12. 按一下 **新增**，並在新**選取使用者、 電腦或群組** 對話方塊中，按一下 **位置**。  
  
13. 上**位置**對話方塊中，於**位置**樹狀結構中，按一下  **corp.contoso.com**，然後按一下**確定**。  
  
14. 在 **輸入要選取的物件名稱**型別**User1**，按一下  **確定**。 上**新增群組或使用者**對話方塊方塊中，請確定**權限**設為**完全控制**，然後按一下**確定**。  
  
15. 關閉 [群組原則管理] 主控台。  
  
## <a name="allow-corp2-computers-to-obtain-computer-certificates"></a>允許 CORP2 電腦，以取得電腦憑證 

CORP2 網域中的電腦必須從在 APP1 上的憑證授權單位取得電腦憑證。 在 APP1 上執行此程序。  
  
### <a name="to-allow-corp2-computers-to-automatically-obtain-computer-certificates"></a>若要允許 CORP2 電腦自動取得電腦憑證  
  
1.  在 APP1 上，按一下**開始**，型別**certtmpl.msc**，然後按 ENTER 鍵。  
  
2.  在 **憑證範本主控台**，在中間窗格中，按兩下**Client-server Authentication**。  
  
3.  在 **用戶端-伺服器驗證屬性** 對話方塊中，按一下**安全性** 索引標籤。  
  
4.  按一下 **新增**，然後在**選取使用者、 電腦、 服務帳戶或群組** 對話方塊中，按一下 **位置**。  
  
5.  在上**位置**對話方塊中，於**位置**，展開**corp.contoso.com**，按一下**corp2.corp.contoso.com**，然後按一下**確定**。  
  
6.  在 **輸入要選取的物件名稱**，型別**Domain Admins;網域電腦**，然後按一下  **確定**。  
  
7.  上**用戶端-伺服器驗證屬性**對話方塊中，於**群組或使用者名稱**，按一下  **Domain Admins （CORP2\Domain 系統管理員）** ，然後在**Domain Admins 的權限**，請在**允許**欄中，選取**撰寫**並**註冊**。  
  
8.  在 **群組或使用者名稱**，按一下**網域電腦 （CORP2\Domain 電腦）** ，然後在**網域電腦的權限**，請在**允許**欄中，選取**註冊**並**自動註冊**，然後按一下**確定**。  
  
9. 關閉 [憑證範本主控台]。  
  
## <a name="replication"></a>強制 DC1 與 2-DC1 之間的複寫  
您可以註冊 2 EDGE1 上的憑證之前，您必須強制 2-DC1 設定從 DC1 的複寫。 在 DC1 上，應該執行此作業。  
  
### <a name="to-force-replication"></a>若要強制複寫  
  
1.  在 DC1 上，按一下**開始**，然後按一下**Active Directory 站台及服務**。  
  
2.  在 Active Directory 站台及服務 主控台樹狀目錄中，展開**站台間傳輸**，然後按一下**IP**。  
  
3.  在 [詳細資料] 窗格中，按兩下**DEFAULTIPSITELINK**。  
  
4.  上**DEFAULTIPSITELINK 屬性**對話方塊中，於**成本**，型別**1**中**複寫每個**，型別**15**，然後按一下 **[確定]** 。 等候 15 分鐘才能完成複寫。  
  
5.  若要強制複寫現在在主控台樹狀目錄中，依序展開 **\ 預設第一個站台-name\Servers\DC1\NTDS 設定**，在 詳細資料 窗格中，以滑鼠右鍵按一下 **<automatically generated>** ，按一下  **立即複寫**，然後在**立即複寫** 對話方塊中，按一下 **確定**。  
  
6.  若要確保已成功完成複寫，執行下列動作：  
  
    1.  在 **開始**畫面上，輸入**cmd.exe**，然後按 ENTER 鍵。  
  
    2.  輸入下列命令，然後按 ENTER。  
  
        ```  
        repadmin /syncall /e /A /P /d /q  
        ```  
  
    3.  請確定所有資料分割會同步處理而未產生錯誤。 如果沒有，然後重新執行命令，直到未不報告任何錯誤後再繼續。  
  
7.  關閉 [命令提示字元] 視窗。  
  


