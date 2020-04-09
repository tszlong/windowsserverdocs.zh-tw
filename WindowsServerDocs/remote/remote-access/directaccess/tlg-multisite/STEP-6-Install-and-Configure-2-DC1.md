---
title: 步驟6安裝和設定 2-DC1
description: 本主題屬於測試實驗室指南-示範適用于 Windows Server 2016 的 DirectAccess 多網站部署
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 3d66901a-c40b-474c-9948-f989f399cfea
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 21591de4527cc33857e215d9521f2f1458c1f3c6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861561"
---
# <a name="step-6-install-and-configure-2-dc1"></a>步驟6安裝和設定 2-DC1

>適用於：Windows Server (半年通道)、Windows Server 2016

2-DC1 提供下列服務：  
  
-   Corp2.corp.contoso.com Active Directory Domain Services （AD DS）網域的網域控制站。  
  
-   Corp2.corp.contoso.com DNS 網域的 DNS 伺服器。  
  
2-DC1 設定包含下列各項：  
  
- 在 2-DC1 上安裝作業系統
  
- 設定 TCP/IP 內容

- 將 2-DC1 設定為網域控制站和 DNS 伺服器

- 提供 CORP\User1 的群組原則許可權
  
- 允許 CORP2 電腦取得電腦憑證
  
- 強制在 DC1 和 2-DC1 之間複寫
  
## <a name="install-the-operating-system-on-2-dc1"></a>在 2-DC1 上安裝作業系統  
首先，安裝 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012。  
  
### <a name="to-install-the-operating-system-on-2-dc1"></a>若要在 2-DC1 上安裝作業系統  
  
1.  開始安裝 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012。  
  
2.  依照指示完成安裝，指定 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 （完整安裝）以及本機系統管理員帳戶的強式密碼。 使用本機系統管理員帳戶登入。  
  
3.  將 2-DC1 連線到具有網際網路存取權的網路，並執行 Windows Update 以安裝 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 的最新更新，然後中斷網際網路連線。  
  
4.  將 2-DC1 連線到2公司網路的子網。  
  
## <a name="configure-tcpip-properties"></a>設定 TCP/IP 內容  
以靜態 IP 位址設定 TCP/IP 通訊協定。  
  
### <a name="to-configure-tcpip-on-2-dc1"></a>在 2-DC1 上設定 TCP/IP  
  
1.  在伺服器管理員主控台中，按一下 [**本機伺服器**]，然後在 [**屬性**] 區域中的 [**有線乙太**網路連線] 旁，按一下連結。  
  
2.  在 [網路連線] 中，用滑鼠右鍵按一下 [有線乙太網路連線]，然後按一下 [內容]。  
  
3.  按一下 [網際網路通訊協定第 4 版 (TCP/IPv4)]，然後按一下 [內容]。  
  
4.  按一下 [使用下列的 IP 位址]。 在 [ **IP 位址**] 中，輸入**10.2.0.1**。 在 [子網路遮罩] 中，輸入 **255.255.255.0**。 在 [**預設閘道**] 中，輸入**10.2.0.254**。 按一下 **[使用下列的 dns 伺服器位址**]，在 [**慣用 dns 伺服器**] 中輸入**10.2.0.1**，然後在 [**其他 dns 伺服器**] 中輸入**10.0.0.1**。  
  
5.  按一下 [進階]，然後按一下 [DNS] 索引標籤。  
  
6.  在 [**這個連線的 DNS 尾碼**] 中，輸入**corp2.corp.contoso.com**，然後按兩次 **[確定]** 。  
  
7.  選取 **[網際網路通訊協定第 6 版 (TCP/IPv6)]** ，然後按一下 **[屬性]** 。  
  
8.  按一下 **[使用下列 IPv6 位址**]。 在 [ **IPv6 位址**] 中，輸入**2001： db8：2：： 1**。 在 [**子網前置長度**] 中，輸入**64**。 在 [**預設閘道**] 中，輸入**2001： db8：2：： fe**。 按一下 **[使用下列的 dns 伺服器位址**]，在 [**慣用 dns 伺服器**] 中輸入**2001： db8：2：： 1**，然後在 [**其他 dns 伺服器**] 中輸入**2001： db8：1：： 1**。  
  
9. 按一下 [進階]，然後按一下 [DNS] 索引標籤。  
  
10. 在 [**這個連線的 DNS 尾碼**] 中，輸入**corp2.corp.contoso.com**，然後按兩次 **[確定]** 。  
  
11. 在 [**有線乙太**網路連線屬性] 對話方塊中，按一下 [**關閉**]。  
  
12. 關閉 [網路連線] 視窗。  
  
13. 在伺服器管理員主控台的 [**本機伺服器**] 的 [內容 **] 區域中，按一下 [** **電腦名稱稱**] 旁的連結。  
  
14. 在 [系統內容] 的 [電腦名稱] 索引標籤上，按一下 [變更]。  
  
15. 在 [**電腦名稱稱/網域變更**] 對話方塊的 [**電腦名稱稱**] 中，輸入**2-DC1**，然後按一下 **[確定]** 。  
  
16. 當提示您必須重新啟動電腦時，按一下 [確定]。  
  
17. 按一下 [系統內容] 對話方塊中的 [關閉]。  
  
18. 當提示您重新啟動電腦時，請按一下 [立即重新啟動]。  
  
19. 重新開機之後，使用本機系統管理員帳戶登入。  
  
## <a name="configure-2-dc1-as-a-domain-controller-and-dns-server"></a>將 2-DC1 設定為網域控制站和 DNS 伺服器  
將 2-DC1 設定為 corp2.corp.contoso.com 網域的網域控制站，以及做為 corp2.corp.contoso.com DNS 網域的 DNS 伺服器。  
  
### <a name="to-configure-2-dc1-as-a-domain-controller-and-dns-server"></a>將 2-DC1 設定為網域控制站和 DNS 伺服器  
  
1.  在伺服器管理員主控台的 [**儀表板**] 上，按一下 [**新增角色及功能**]。  
  
2.  按三次 **[下一步]** 以移至伺服器角色選取畫面  
  
3.  在 [**選取伺服器角色**] 頁面上，選取 [ **Active Directory Domain Services**]。 出現提示時，按一下 [**新增功能**]，然後按 **[下一步]** 三次。  
  
4.  在 [確認安裝選項] 頁面上，按一下 [安裝]。  
  
5.  當安裝成功完成時，請按一下 [將**此伺服器升級為網域控制站**]。  
  
6.  在 [Active Directory Domain Services 設定] 嚮導的 [**部署**設定] 頁面上，按一下 [**將新網域新增至現有的樹**系]。  
  
7.  在 [**父系網域名稱**] 中，輸入**corp.contoso.com**，在 [**新功能變數名稱**] 中輸入**corp2**。  
  
8.  在 **[提供認證以執行此**作業] 底下，按一下 [**變更**]。 在 [ **Windows 安全性**] 對話方塊的 [**使用者名稱**] 中輸入**Com\Administrator**，然後在 [**密碼**] 中輸入 corp\Administrator 密碼，按一下 **[確定]** ，然後按 **[下一步**]。  
  
9. 在 [**網域控制站選項**] 頁面上，確定 [**網站名稱**] 為 [**第二個網站**]。 在 **[輸入目錄服務還原模式（DSRM）密碼]** 下的 [**密碼**] 和 [**確認密碼**] 中，輸入強式密碼兩次，然後按 **[下一步]** 五次。  
  
10. 在 [**必要條件檢查**] 頁面上，驗證必要條件之後，按一下 [**安裝**]。  
  
11. 等到 wizard 完成 Active Directory 和 DNS 服務的設定，然後按一下 [**關閉**]。  
  
12. 在電腦重新開機之後，使用系統管理員帳戶登入 CORP2 網域。  
  
## <a name="provide-group-policy-permissions-to-corpuser1"></a>提供 CORP\User1 的群組原則許可權  
使用此程式，為 CORP\User1 使用者提供建立和變更 corp2 群組原則物件的完整許可權。  
  
### <a name="to-provide-group-policy-permissions"></a>提供群組原則許可權  
  
1.  在 [**開始**] 畫面上，輸入**gpmc**，然後按 enter。  
  
2.  在群組原則管理主控台中，開啟 [**樹系： corp.contoso.com/Domains/corp2.corp.contoso.com**]。  
  
3.  在詳細資料窗格中，按一下 [**委派**] 索引標籤。在 [**許可權**] 下拉式清單中，按一下 [**連結 gpo**]。  
  
4.  按一下 **[新增]，然後**在新的 [**選取使用者、電腦或群組**] 對話方塊中，按一下 [**位置**]。  
  
5.  在 [**位置**] 對話方塊的 [**位置**] 樹狀目錄中，按一下 [ **Corp.contoso.com**]，然後按一下 **[確定]** 。  
  
6.  在 [**輸入物件名稱來選取**類型**User1**] 中，按一下 **[確定]** ，然後在 [**新增群組或使用者**] 對話方塊中按一下 **[確定]** 。  
  
7.  在 [群組原則管理主控台] 的樹狀目錄中，按一下 [**群組原則物件**]，然後在詳細資料窗格中按一下 [**委派**] 索引標籤。  
  
8.  按一下 **[新增]，然後**在新的 [**選取使用者、電腦或群組**] 對話方塊中，按一下 [**位置**]。  
  
9. 在 [**位置**] 對話方塊的 [**位置**] 樹狀目錄中，按一下 [ **Corp.contoso.com**]，然後按一下 **[確定]** 。  
  
10. 在 [**輸入物件名稱來選取**類型**User1**] 中，按一下 **[確定]** 。  
  
11. 在 [群組原則管理主控台] 的樹狀結構中，按一下 [ **WMI 篩選器**]，然後在詳細資料窗格中按一下 [**委派**] 索引標籤。  
  
12. 按一下 **[新增]，然後**在新的 [**選取使用者、電腦或群組**] 對話方塊中，按一下 [**位置**]。  
  
13. 在 [**位置**] 對話方塊的 [**位置**] 樹狀目錄中，按一下 [ **Corp.contoso.com**]，然後按一下 **[確定]** 。  
  
14. 在 [**輸入物件名稱來選取**類型**User1**] 中，按一下 **[確定]** 。 在 [**新增群組或使用者**] 對話方塊中，確認 [**許可權**] 設定為 [**完全控制**]，然後按一下 **[確定]** 。  
  
15. 關閉 [群組原則管理] 主控台。  
  
## <a name="allow-corp2-computers-to-obtain-computer-certificates"></a>允許 CORP2 電腦取得電腦憑證 

CORP2 網域中的電腦必須從 APP1 上的憑證授權單位單位取得電腦憑證。 在 APP1 上執行此程式。  
  
### <a name="to-allow-corp2-computers-to-automatically-obtain-computer-certificates"></a>允許 CORP2 電腦自動取得電腦憑證  
  
1.  在 APP1 上，按一下 [**開始**]，輸入**certtmpl.msc**，然後按 enter。  
  
2.  在 [**憑證範本] 主控台**的中間窗格中，按兩下 [**用戶端-伺服器驗證**]。  
  
3.  在 [**用戶端-伺服器驗證**內容] 對話方塊上，按一下 [**安全性**] 索引標籤。  
  
4.  按一下 [**新增**]，然後在 [**選取使用者、電腦、服務帳戶或群組**] 對話方塊中，按一下 [**位置**]。  
  
5.  **在 [位置**] 對話方塊的 [**位置**] 中，展開 [ **corp.contoso.com**]，按一下 [ **Corp2.corp.contoso.com**]，然後按一下 **[確定]** 。  
  
6.  在**輸入要選取的物件名稱** 中，輸入**Domain Admins;網域電腦**，然後按一下**確定**。  
  
7.  在 [**用戶端-伺服器驗證**內容] 對話方塊的 [**群組或使用者名稱**] 中，按一下 [**網域系統管理員（CORP2\Domain Admins）** ]，然後在 [**網域管理員的許可權**] 的 [**允許**] 欄位中，選取 [**寫入**並**註冊**]。  
  
8.  在 **[群組或使用者名稱**] 中，按一下 [**網域電腦（CORP2\Domain 電腦）** ]，然後在 [**網域電腦的許可權**] 的 [**允許**] 欄中，選取 [**註冊**並**自動註冊**]，然後按一下 **[確定]** 。  
  
9. 關閉 [憑證範本主控台]。  
  
## <a name="force-replication-between-dc1-and-2-dc1"></a><a name="replication"></a>強制在 DC1 和 2-DC1 之間複寫  
您必須先強制將設定從 DC1 複寫至 2-DC1，才可以在 2-EDGE1 上註冊憑證。 此作業應該在 DC1 上完成。  
  
### <a name="to-force-replication"></a>強制複寫  
  
1.  在 DC1 上，按一下 [**開始**]，然後按一下 [ **Active Directory 網站和服務**]。  
  
2.  在 [Active Directory 網站和服務] 主控台的樹狀目錄中，展開 [**網站間傳輸**]，然後按一下 [ **IP**]。  
  
3.  在詳細資料窗格中，按兩下 [ **DEFAULTIPSITELINK**]。  
  
4.  在 [ **DEFAULTIPSITELINK 屬性**] 對話方塊的 [**成本**] 中，輸入**1**，在 [複寫**間隔**] 中輸入**15**，然後按一下 **[確定]** 。 等候15分鐘的時間完成複寫。  
  
5.  若要立即在主控台樹中強制複寫，請展開 [ **Sites\Default-First-Site-name\Servers\DC1\NTDS 設定**]，在詳細資料窗格中，以滑鼠右鍵按一下 [ **<automatically generated>** ]，按一下 [**立即**複寫]，然後在 [**立即**複寫] 對話方塊中按一下 **[確定]** 。  
  
6.  若要確保複寫順利完成，請執行下列動作：  
  
    1.  在 [**開始**] 畫面上，輸入**cmd.exe**，然後按 enter。  
  
    2.  輸入下列命令，然後按 ENTER。  
  
        ```  
        repadmin /syncall /e /A /P /d /q  
        ```  
  
    3.  請確定所有分割區都已同步處理，而且沒有任何錯誤。 如果沒有，請重新執行命令，直到未回報任何錯誤之後再繼續。  
  
7.  關閉 [命令提示字元] 視窗。  
  


