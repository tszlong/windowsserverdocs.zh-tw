---
title: 步驟4安裝及設定 RSA 和 EDGE1
description: 本主題是測試實驗室指南的一部分-示範使用 OTP 驗證的 DirectAccess 和適用于 Windows Server 2016 的 RSA SecurID
manager: brianlic
ms.topic: article
ms.assetid: d46ede6f-1a21-414d-b8c3-6b5c87344b9d
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 591d6051d6c2a754416f48b615fa17c286b822c8
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948964"
---
# <a name="step-4-install-and-configure-rsa-and-edge1"></a>步驟4安裝及設定 RSA 和 EDGE1

>適用於：Windows Server (半年度管道)、Windows Server 2016

RSA 是 RADIUS 和 OTP 伺服器，而且會在設定 RADIUS 和 OTP 之前安裝。

您將執行下列步驟來設定 RSA 部署：

1. 在 RSA 伺服器上安裝作業系統。 在 RSA 伺服器上安裝 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012。

2. 在 RSA 設定 TCP/IP。 在 RSA 伺服器上設定 TCP/IP 設定。

3. 將「驗證管理員」安裝檔案複製到 RSA 伺服器。 在 RSA 上安裝作業系統之後，請將驗證管理員檔案複製到 RSA 電腦。

4. 將 RSA 伺服器加入 CORP 網域。 將 RSA 加入 CORP 網域。

5. 停用 RSA 上的 Windows 防火牆。 停用 RSA 伺服器上的 Windows 防火牆。

6. 在 RSA 伺服器上安裝「RSA 驗證管理員」。 安裝 RSA 驗證管理員。

7. 設定 RSA 驗證管理員。 設定驗證管理員。

8. 建立 DAProbeUser。 建立使用者帳戶以供探查之用。

9. 在 CLIENT1 上安裝 RSA SecurID 軟體權杖。 在 CLIENT1 上安裝 RSA SecurID 軟體權杖。

10. 將 EDGE1 設定為 RSA 驗證代理程式。 在 EDGE1 上設定 RSA 驗證代理程式。

11. 設定 EDGE1 以支援 OTP 驗證。 設定 DirectAccess 的 OTP，並確認設定。

## <a name="install-the-operating-system-on-the-rsa-server"></a><a name="InstallOS"></a>在 RSA 伺服器上安裝作業系統

1.  在 RSA 上，開始安裝 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012。

2.  依照指示完成安裝，指定 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 (完整安裝) ，以及本機系統管理員帳戶的強式密碼。 使用本機 Administrator 帳戶登入。

3.  將 RSA 連接到可存取網際網路的網路並執行 Windows Update，以安裝 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 的最新更新，然後中斷與網際網路的連線。

4.  將 RSA 連接到公司網路子網。

## <a name="configure-tcpip-on-rsa"></a><a name="TCP"></a>在 RSA 設定 TCP/IP

1.  在初始設定工作中，按一下 [ **設定網路**]。

2.  在 [**網路** 連線] 中，以滑鼠 **按右鍵 [****區域** 連線]，然後按一下 [內容]。

3.  按一下 [網際網路通訊協定第 4 版 (TCP/IPv4)]，然後按一下 [內容]。

4.  按一下 [使用下列的 IP 位址]。 在 [IP 位址] 中，輸入 **10.0.0.5**。 在 [子網路遮罩] 中，輸入 **255.255.255.0**。 在 [ **預設閘道**] 中，輸入 **10.0.0.2**。 在 [**慣用 dns 伺服器**] 中，按一下 **[使用下列的 dns 伺服器位址**]，輸入 **10.0.0.1**。

5.  按一下 [進階]，然後按一下 [DNS] 索引標籤。

6.  在 [ **這個連線的 DNS 尾碼**] 中，輸入 **corp.contoso.com**，然後按兩次 **[確定]** 。

7.  在 [ **區域連接屬性** ] 對話方塊中，按一下 [ **關閉**]。

8.  關閉 [網路連線] 視窗。

## <a name="copy-authentication-manager-installation-files-to-the-rsa-server"></a><a name="copyinstfiles"></a>將 Authentication Manager 安裝檔案複製到 RSA 伺服器

1.  在 RSA 伺服器上，建立 C:\RSA 安裝資料夾。

2.  將 RSA Authentication Manager 7.1 SP4 媒體的內容複寫到 C:\RSA 安裝資料夾。

3.  建立 C:\RSA Installation\License 和 Token 的子資料夾。

4.  將 RSA 許可證檔案複製到 C:\RSA Installation\License 和權杖。

## <a name="join-the-rsa-server-to-the-corp-domain"></a><a name="JoinDomain"></a>將 RSA 伺服器加入 CORP 網域

1.  以滑鼠右鍵按一下 **我的電腦**，然後按一下 [ **屬性**]。

2.  在 [系統內容] 對話方塊中的 [電腦名稱] 索引標籤上，按一下 [變更]。

3.  在 [ **電腦名稱稱**] 中，輸入 **RSA**。 在 [ **成員隸屬**] 中，按一下 [ **網域**]，輸入 **Corp.contoso.com**，然後按一下 **[確定]**。

4.  當系統提示您輸入使用者名稱和密碼時，輸入 **User1** 和其密碼，然後按一下 **[確定]**。

5.  在 [歡迎網域] 對話方塊中，按一下 **[確定**]。

6.  當提示您必須重新啟動電腦時，按一下 [確定]。

7.  按一下 [系統內容] 對話方塊中的 [關閉]。

8.  當提示您重新啟動電腦時，請按一下 [立即重新啟動]。

9. 電腦重新開機之後，輸入 **User1** 和密碼，在 [ **登入至：** ] 下拉式清單中選取 CORP，然後按一下 **[確定]**。

## <a name="disable-windows-firewall-on-rsa"></a><a name="BKMK_Firewall"></a>停用 RSA 上的 Windows 防火牆

1.  依序按一下 [ **開始**]、[ **主控台**]、[ **系統及安全性**]，然後按一下 [ **Windows 防火牆**]。

2.  按一下 [ **開啟或關閉 Windows 防火牆**]。

3.  關閉所有設定的 **Windows 防火牆**。

4.  按一下 **[確定]** 並關閉 [Windows 防火牆]。

## <a name="install-rsa-authentication-manager-on-the-rsa-server"></a><a name="install"></a>在 RSA 伺服器上安裝 RSA Authentication Manager

1.  如果在此程式期間任何時候出現安全性警告訊息，請按一下 [ **執行** ] 繼續進行。

2.  開啟 C:\RSA 安裝資料夾，然後按兩下 [ **autorun.exe**]。

3.  按一下 [ **立即安裝**]，按 [ **下一步]**，選取美洲的最上層選項，然後按 **[下一步]**。

4.  選取 [ **我接受授權合約的條款**]，然後按 **[下一步]**。

5.  選取 [ **主要實例**]，然後按 **[下一步]**。

6.  在 [ **目錄名稱：** ] 欄位中輸入 **C:\RSA**，然後按 **[下一步]**。

7.  確認伺服器名稱 (RSA.corp.contoso.com) 和 IP 位址是正確的，然後按 **[下一步]**。

8.  流覽至 C:\RSA Installation\License 和權杖，然後按 **[下一步]**。

9. 在 [ **驗證授權檔案** ] 頁面上，按 **[下一步]**。

10. 在 [ **使用者識別碼** ] 欄位中，輸入 [ **系統管理員**]，然後在 [ **密碼** ] 和 [ **確認密碼** ] 欄位中輸入強式密碼。 按一下 [下一步] 。

11. 在記錄選取畫面上，接受預設值，然後按 **[下一步]**。

12. 在 [摘要] 畫面上，按一下 [ **安裝**]。

13. 安裝完成之後，請按一下 **[完成]**。

## <a name="configure-rsa-authentication-manager"></a><a name="confiauthmgr"></a>設定 RSA 驗證管理員

1.  如果 RSA 安全性主控台未自動開啟，請在 RSA 電腦桌面上按兩下 [RSA Security Console]。

2.  如果出現安全性憑證警告/安全性警示，請按一下 [ **繼續流覽此網站** ]，或按一下 **[是]** 繼續作業，並將此網站新增至 [信任的網站] （如有要求）。

3.  在 [ **使用者識別碼** ] 欄位中輸入 **Administrator** ，然後按一下 **[確定]**。

4.  在 [ **密碼** ] 欄位中，輸入系統管理員帳戶的密碼，然後按一下 [ **登** 入]。

5.  插入權杖資訊。

    1.  在 [ **RSA 安全性] 主控台** 中，按一下 [ **驗證** ]，然後按一下 [ **SecurID 權杖**]。

    2.  按一下 [匯 **入權杖作業**]，然後按一下 [ **加入新** 的]。

    3.  在 [匯 **入選項** ] 區段中，按一下 **[流覽]**。 流覽並選取 C：\ 中的權杖 XML 檔案RSA Installation\License 和 Token 資料夾，然後按一下 [ **開啟**]。

    4.  按一下頁面底部的 [ **提交工作** ]。

6.  建立 OTP new 使用者。

    1.  在 [ **RSA 安全性] 主控台** 中，按一下 [ **識別** ] 索引標籤，按一下 [ **使用者**]，然後按一下 [ **新增**]。

    2.  在 [ **姓氏：** ] 區段中輸入 [ **使用者**]，然後在 [ **使用者識別碼：** ] 區段中輸入 **User1** (USERID 必須與用於此實驗室) 的 AD 使用者名稱相同。  在 [ **密碼：** ] 和 [ **確認密碼：** ] 區段中，輸入強式密碼。 清除 [ **要求使用者在下次登入時變更密碼]** 核取方塊，然後按一下 [ **儲存**]。

7.  將 User1 指派給其中一個匯入的權杖。

    1.  在 [ **使用者** ] 頁面上，按一下 [ **User1** ]，然後按一下 [ **SecurID 權杖**]。

    2.  按一下 [ **SecurID 權杖** ]，然後按一下 [ **指派權杖**]。

    3.  在 [ **序號** ] 標題下，按一下列出的第一個數位，然後按一下 [ **指派**]。

    4.  按一下指派的權杖，然後按一下 [ **編輯**]。 在 **使用者驗證需求** 的 [ **SecurID pin 管理**] 區段中，選取 [**不需要 PIN (僅限權杖)**。

    5.  按一下 [ **儲存並散發權杖**]。

    6.  在 [**基本**] 區段的 [**發佈軟體權杖**] 頁面上，按一下 [**發行權杖檔案 (SDTID)**。

    7.  在 [ **發佈軟體權杖** ] 頁面的 [ **權杖檔案選項** ] 區段中，清除 [ **啟用禁止複製** ] 核取方塊。 按一下 [ **無密碼** **] 和 [下一步]**。

    8.  在 [ **發佈軟體權杖** ] 頁面的 [ **下載檔案** ] 區段中，按一下 [ **立即下載**]。 按一下 [儲存]。 流覽至 C:\RSA 安裝，然後按一下 [ **儲存** 後 **關閉**]。

    9. 將 **RSA 安全性主控台** 最小化，以供稍後使用。

8.  將驗證管理員設定為 RADIUS 伺服器。

    1.  在 RSA 電腦桌面上，按兩下 **[Rsa 安全性 Operations 主控台]**。

    2.  如果出現安全性憑證警告/安全性警示，請按一下 [ **繼續流覽此網站** ] 或按一下 **[是]** 繼續作業，並在要求時將此網站新增至信任的網站。

    3.  輸入使用者識別碼和密碼，然後按一下 [ **登** 入]。

    4.  按一下 [ **部署設定-RADIUS-設定伺服器**]。

    5.  在 [ **需要其他認證** ] 頁面上，輸入系統管理員使用者識別碼和密碼，然後按一下 **[確定]**。

    6.  在 [ **設定 RADIUS 伺服器** ] 頁面上，輸入系統管理員使用者用於 **秘密** 和 **主要密碼** 的相同密碼。 輸入系統管理員使用者識別碼和密碼，然後按一下 [ **設定**]。

    7.  確認已顯示 [ **已成功設定 RADIUS 伺服器** ] 訊息。 按一下 [完成]。 關閉 [ **RSA Operations 主控台**]。

    8.  切換回「 **RSA 安全性主控台**」。

    9. 在 [ **radius** ] 索引標籤上，按一下 [ **radius 伺服器**]。 確認已列出 rsa.corp.contoso.com。

9. 將 RSA 伺服器設定為 RSA 驗證用戶端。

    1.  在 [ **radius** ] 索引標籤上，按一下 [ **radius 用戶端** 並 **新增**]。

    2.  按一下 [ **任何 RADIUS 用戶端** ] 核取方塊。

    3.  在 [ **共用密碼** ] 欄位中，輸入您選擇的強式密碼。 您稍後在設定 OTP 的 EDGE1 時，將會使用這個相同的密碼。

    4.  將 [ **IP 位址** ] 欄位保留空白，並將 **Make/Model** 專案保留為 **標準半徑**。

    5.  按一下 [ **不儲存 RSA 代理程式**]。

10. 建立將 EDGE1 設定為 RSA 驗證代理程式所需的檔案。

    1.  在 [ **存取** ] 索引標籤中，反白顯示 **驗證代理** 程式，然後按一下 [ **新增**]。

    2.  在 [**主機名稱**] 欄位中輸入 **EDGE1** ，然後按一下 [**解析 IP**]。

    3.  請注意，EDGE1 的 IP 位址現在會顯示在 [ **Ip 位址** ] 欄位中。 按一下 [儲存]。

11. 產生 EDGE1 伺服器 ( # A0) 的設定檔。

    1.  在 [ **存取** ] 索引標籤中，反白顯示 **驗證代理** 程式，然後按一下 [ **產生設定檔**]

    2.  在 [ **產生設定檔** ] 頁面上，按一下 [ **產生設定檔**]，然後按一下 [ **立即下載**]。

    3.  按一下 [ **儲存**]，流覽至 C：\RSA 安裝，然後按一下 [ **儲存**]。

    4.  按一下 [**下載完成**] 對話方塊中的 [**關閉**]。

12. 產生 EDGE1 伺服器 ( # A0) 的節點秘密檔案。

    1.  在 [**存取**] 索引標籤上反白顯示 [**驗證代理** 程式]，然後按一下 [**管理現有**

    2.  按一下目前設定的節點 EDGE1，然後按一下 [ **管理節點秘密**]。

    3.  核取 [ **建立新的隨機節點秘密，並將節點秘密匯出至** 檔案] 核取方塊。

    4.  在 [ **加密密碼** ] 和 [ **確認加密密碼** ] 欄位中，輸入系統管理員使用者所使用的相同密碼，然後按一下 [ **儲存**]。

    5.  在 [ **產生的節點秘密** 檔案] 頁面上，按一下 [ **立即下載**]。

    6.  在 [檔案 **下載** ] 對話方塊中，按一下 [ **儲存**]，流覽至 C:\RSA 安裝，然後按一下 [ **儲存**]。 按一下 [**下載完成**] 對話方塊中的 [**關閉**]。

    7.  從「RSA 驗證管理員」媒體複製 \auth_mgr\windows-x86_64\am\rsa-ace_nsload\win32-5.0-x86\agent_nsload.exe 到 C:\RSA 安裝。

## <a name="create-daprobeuser"></a><a name="BKMK_DAProbeUser"></a>建立 DAProbeUser

1.  在 [ **RSA 安全性] 主控台** 中，按一下 [ **識別** ] 索引標籤，按一下 [ **使用者**]，然後按一下 [ **新增**]。

2.  在 [ **姓氏：** ] 區段類型 [ **探查**] 中，在 [ **使用者識別碼：** ] 區段中輸入 **DAProbeUser**。 在 [ **密碼：** ] 和 [ **確認密碼：** ] 區段中，輸入強式密碼。 清除 [ **要求使用者在下次登入時變更密碼]** 核取方塊，然後按一下 [ **儲存**]。

## <a name="install-rsa-securid-software-token-on-client1"></a><a name="InstToken"></a>在 CLIENT1 上安裝 RSA SecurID 軟體權杖
使用此程式在 CLIENT1 上安裝 SecurID 軟體權杖。

#### <a name="install-securid-software-token"></a>安裝 SecurID 軟體權杖

1.  在 CLIENT1 電腦上建立資料夾 C:\RSA 檔。 將檔案 Software_Tokens.zip 從 RSA 電腦上的 C:\RSA 安裝複製到 C:\RSA 檔案。 將檔案 User1_000031701832 SDTID 解壓縮至 CLIENT1 上的 C:\RSA 檔案。

2.  存取 RSA SecurID 軟體權杖媒體來源，然後按兩下 [ **SecurID SoftwareToken 用戶端應用程式** ] 資料夾中的 [RSASECURIDTOKEN410]，啟動 RSA SecurID 安裝。 如果出現 [ **開啟檔案-安全性] 警告** 訊息，請按一下 [ **執行**]。

3.  在 [ **RSA SecurID 軟體權杖-InstallShield Wizard** ] 對話方塊上，按兩次 **[下一步** ]。

4.  接受授權合約，然後按 **[下一步]**。

5.  在 [**安裝類型**] 對話方塊中選取 [**一般**]，按一下 **[下一步]**，然後按一下 [**安裝**

6.  如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**是**]。

7.  選取 [ **啟動 RSA SecurID 軟體權杖** ] 核取方塊，然後按一下 **[完成]**。

8.  按一下 [從檔案匯 **入**]。

9. 按一下 **[流覽]**，選取 [C:\RSA Files \ USER1_000031701832. SDTID]，然後按一下 [ **開啟**]。

10. 按兩次 **[確定]**。

## <a name="configure-edge1-as-an-rsa-authentication-agent"></a><a name="configAuthAgt"></a>將 EDGE1 設定為 RSA 驗證代理程式
您可以使用此程式設定 EDGE1 來執行 RSA 驗證。

#### <a name="configure-the-rsa-authentication-agent"></a>設定 RSA 驗證代理程式

1. 在 EDGE1 開啟 Windows 檔案總管並建立資料夾 C:\RSA 檔。 流覽至 RSA ACE 安裝媒體。

2. 從 RSA 媒體將檔案 agent_nsload.exe、AM_Config.zip 和 EDGE1_NodeSecret.zip 複製到 C:\RSA 檔案。

3. 將兩個 zip 檔案的內容解壓縮至下列位置：

   1.  C：\Windows\system32\

   2.  C:\Windows\SysWOW64\

4. 將 agent_nsload.exe 複製到 C:\Windows\SysWOW64 \\ 。

5. 開啟提升許可權的命令提示字元並流覽至 C:\windows\syswow64。

6. 輸入 **agent_nsload.exe-f nodesecret-p <password>** ，其中 <password> 是您在初始 RSA 設定期間所建立的強式密碼。 按 Enter。

7. 將 C:\Windows\SysWOW64\securid 複製到 C:\windows\system32。

## <a name="configure-edge1-to-support-otp-authentication"></a><a name="configOTP"></a>設定 EDGE1 以支援 OTP 驗證
您可以使用此程式來設定 DirectAccess 的 OTP，並確認設定。

#### <a name="configure-otp-for-directaccess"></a>設定 DirectAccess 的 OTP

1.  在 EDGE1 上，開啟伺服器管理員]，然後按一下左窗格中的 [ **遠端存取** ]。

2.  以滑鼠右鍵按一下 [伺服器] 窗格中的 [ **EDGE1** ]，然後選取 [ **遠端存取管理**]。

3.  按一下 [設定]。

4.  在 [ **DirectAccess 設定** ] 視窗的 [ **步驟 2-遠端存取服務器**] 下，按一下 [ **編輯**]。

5.  按三次 [ **下一步]** ，然後在 [ **驗證** ] 區段中選取 [ **雙因素驗證** ] 和 [ **使用 OTP**]，並確定已勾選 [ **使用電腦憑證** ]。 確認根 CA 設定為 **CN = corp-APP1-ca**。 按一下 [下一步] 。

6.  在 [ **OTP RADIUS 伺服器** ] 區段中，按兩下 [空白 **伺服器名稱** ] 欄位。

7.  在 [**新增 RADIUS 伺服器**] 對話方塊的 [**伺服器名稱**] 欄位中輸入 **RSA** 。 按一下 [**共用密碼**] 欄位旁的 [**變更**]，然後輸入您在 **新** 密碼中于 RSA 伺服器上設定 RADIUS 用戶端時所使用的相同密碼，然後 **確認新的秘密** 欄位。 按兩次 **[確定]** ，然後按 **[下一步**]。

    > [!NOTE]
    > 如果 RADIUS 伺服器位於與遠端存取服務器不同的網域中，則 [ **伺服器名稱** ] 欄位必須指定 RADIUS 伺服器的 FQDN。

8.  在 [ **OTP CA 伺服器** ] 區段中，選取 [APP1.corp.contoso.com]，然後按一下 [ **新增**]。 按一下 [下一步] 。

9. 在 [ **OTP 憑證範本** ] 頁面上，按一下 **[流覽]** 以選取用來註冊 OTP 驗證所發行之憑證的憑證範本，然後在 [ **憑證範本** ] 對話方塊中選取 [ **DAOTPLogon**]。 按一下 [確定]。 按一下 **[流覽]** ，選取用來註冊遠端存取服務器用來簽署 OTP 憑證註冊要求之憑證的憑證範本，然後在 [ **憑證範本** ] 對話方塊中選取 [ **DAOTPRA**]。 按一下 [確定]  。 按 [下一步]  。

10. 在 [**遠端存取服務器設定**] 頁面上，按一下 **[完成]**，然後按一下 [ **DirectAccess 專家 Wizard]** 上的 **[完成]** 。

11. 在 [ **遠端存取審核** ] 對話方塊中 **，按一下**[套用]，等候 DirectAccess 原則更新，然後按一下 [ **關閉**]。

12. 在 [ **開始** ] 畫面上，輸入 **powershell.exe**，以滑鼠右鍵按一下 **powershell**，按一下 [ **Advanced**]，然後按一下 [以 **系統管理員身分執行**]。 如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**是**]。

13. 在 [Windows PowerShell] 視窗中，輸入 **gpupdate/force** ，然後按 enter 鍵。

14. 關閉並重新開啟 [遠端存取管理] 主控台，並確認所有的 OTP 設定都正確。



