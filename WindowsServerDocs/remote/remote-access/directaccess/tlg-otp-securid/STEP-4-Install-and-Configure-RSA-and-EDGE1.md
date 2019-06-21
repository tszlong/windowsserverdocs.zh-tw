---
title: 步驟 4 的安裝和設定 RSA 和 EDGE1
description: 本主題是測試實驗室指南-使用 OTP 驗證和 RSA SecurID for Windows Server 2016 的示範 DirectAccess 的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d46ede6f-1a21-414d-b8c3-6b5c87344b9d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5280a02568305512f868f559fe35d11dc618f2b4
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283065"
---
# <a name="step-4-install-and-configure-rsa-and-edge1"></a>步驟 4 的安裝和設定 RSA 和 EDGE1

>適用於：Windows Server （半年通道），Windows Server 2016

RSA 是 RADIUS 和 OTP 伺服器，而且在設定 RADIUS 和 OTP 之前已安裝。  
  
您將執行下列步驟來設定 RSA 部署：  
  
1. RSA 伺服器上安裝作業系統。 RSA 伺服器上安裝 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012。  
  
2. 設定 RSA 的 TCP/IP。 RSA 伺服器上，設定 TCP/IP 設定。  
  
3. 驗證管理員 」 安裝檔案複製到 RSA 伺服器。 安裝之後的作業系統上 RSA，驗證管理員將檔案複製到 RSA 電腦。  
  
4. 將 RSA 伺服器加入 CORP 網域。 將 RSA 加入 CORP 網域。  
  
5. 停用 RSA 上的 Windows 防火牆。 停用 Windows 伺服器上的防火牆 RSA。  
  
6. RSA 驗證管理員伺服器上安裝 RSA。 安裝 「 RSA 驗證管理員 」。  
  
7. 設定 RSA 驗證管理員。 設定驗證管理員。  
  
8. 建立 DAProbeUser。 建立探查用途的使用者帳戶。  
  
9. 在 CLIENT1 上安裝 RSA SecurID 軟體權杖。 在 CLIENT1 上安裝 RSA SecurID 軟體權杖。  
  
10. 將 EDGE1 設定為 RSA Authentication Agent。 在 EDGE1 上設定 RSA Authentication Agent。  
  
11. 若要支援 OTP 驗證設定 EDGE1。 對於 DirectAccess，設定 OTP，並確認組態。  
  
## <a name="InstallOS"></a>RSA 伺服器上安裝作業系統  
  
1.  Rsa，啟動 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 的安裝。  
  
2.  請依照下列指示完成安裝，指定 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 （完整安裝） 和本機系統管理員帳戶的強式密碼。 使用本機 Administrator 帳戶登入。  
  
3.  RSA 連接至具有網際網路存取的網路，並執行 Windows Update，以 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012，安裝最新的更新，然後中斷網際網路連線。  
  
4.  連線到公司網路子網路的 RSA。  
  
## <a name="TCP"></a>在 RSA 設定 TCP/IP  
  
1.  在初始組態工作中，按一下**設定網路功能**。  
  
2.  在 **網路連線**，以滑鼠右鍵按一下**Local Area Connection**，然後按一下**屬性**。  
  
3.  按一下 [網際網路通訊協定第 4 版 (TCP/IPv4)]  ，然後按一下 [內容]  。  
  
4.  按一下 [使用下列的 IP 位址]  。 在 [IP 位址]  中，輸入 **10.0.0.5**。 在 [子網路遮罩]  中，輸入 **255.255.255.0**。 在 **預設閘道**，型別**10.0.0.2**。 按一下 **使用下列的 DNS 伺服器位址**，請在**慣用 DNS 伺服器**，型別**10.0.0.1**。  
  
5.  按一下 [進階]  ，然後按一下 [DNS]  索引標籤。  
  
6.  在 **此連線的 DNS 尾碼**，型別**corp.contoso.com**，然後按一下**確定**兩次。  
  
7.  在 [**本機區域連線內容**] 對話方塊中，按一下**關閉**。  
  
8.  關閉 [網路連線]  視窗。  
  
## <a name="copyinstfiles"></a>驗證管理員 」 安裝檔案複製到 RSA 伺服器  
  
1.  RSA 在伺服器上建立資料夾 C:\RSA 安裝。  
  
2.  將 RSA 驗證管理員 7.1 SP4 媒體的內容複製到 C:\RSA 安裝資料夾。  
  
3.  建立子資料夾 C:\RSA Installation\License 和語彙基元。  
  
4.  RSA 授權將檔案複製到 C:\RSA Installation\License 和權杖。  
  
## <a name="JoinDomain"></a>將 RSA 伺服器加入 CORP 網域  
  
1.  以滑鼠右鍵按一下**我的電腦**，然後按一下**屬性**。  
  
2.  在 [系統內容]  對話方塊中的 [電腦名稱]  索引標籤上，按一下 [變更]  。  
  
3.  在 **電腦名稱**，型別**RSA**。 中**隸屬**，按一下**網域**，型別**corp.contoso.com**，然後按一下 **[確定]** 。  
  
4.  當系統提示您輸入使用者名稱和密碼時，輸入**User1**和其密碼，並按**確定**。  
  
5.  在 [歡迎] 對話方塊中的網域上按一下**確定**。  
  
6.  當提示您必須重新啟動電腦時，按一下 [確定]  。  
  
7.  按一下 [系統內容]  對話方塊中的 [關閉]  。  
  
8.  當提示您重新啟動電腦時，請按一下 [立即重新啟動]  。  
  
9. 重新啟動電腦之後，請輸入**User1**和密碼，選取 在 CORP**登入：** 下拉式清單中，按一下 **確定**。  
  
## <a name="BKMK_Firewall"></a>停用 RSA 上的 Windows 防火牆  
  
1.  按一下 **開始**，按一下**控制台**，按一下 **系統及安全性**，然後按一下**Windows 防火牆**。  
  
2.  按一下 **開啟或關閉 Windows 防火牆**。  
  
3.  **關閉 Windows 防火牆**的所有設定。  
  
4.  按一下 **確定**並關閉 Windows 防火牆。  
  
## <a name="install"></a>RSA 伺服器上安裝 RSA 驗證管理員  
  
1.  如果安全性警告訊息出現在任何時間，在此過程中，按一下**執行**以繼續。  
  
2.  開啟 C:\RSA 安裝資料夾，然後按兩下**autorun.exe**。  
  
3.  按一下 [**立即安裝**，按一下**下一步]** ，選取 [top 選項美洲地區，然後按一下**下一步]** 。  
  
4.  選取 [**我接受授權合約的條款**，然後按一下**下一步]** 。  
  
5.  選取 [**主要執行個體**，然後按一下**下一步]** 。  
  
6.  在 [**目錄名稱：** 欄位中輸入**C:\RSA**，然後按一下**下一步]** 。  
  
7.  確認伺服器名稱 (RSA.corp.contoso.com) 和 IP 位址是否正確，然後按一下**下一步**。  
  
8.  瀏覽 C:\RSA Installation\License 和權杖，然後按一下**下一步**。  
  
9. 在 [**確認授權檔案**頁面上，按一下**下一步]** 。  
  
10. 在 **使用者識別碼**欄位中輸入**系統管理員**，然後在**密碼**和**確認密碼**欄位中輸入強式密碼。 按一下 [下一步]  。  
  
11. 在記錄檔選取畫面上，接受預設值，並按一下**下一步**。  
  
12. 在 摘要 畫面中，按一下 **安裝**。  
  
13. 安裝完成後，按一下**完成**。  
  
## <a name="confiauthmgr"></a>設定 RSA 驗證管理員  
  
1.  如果 RSA Security 主控台不會自動開啟，然後在 RSA 電腦桌面上按兩下 「 RSA Security 主控台 」。  
  
2.  如果安全性憑證警告會出現安全性警示，按一下**繼續瀏覽此網站**或按**是**可以繼續，將此網站新增至信任的網站，如果要求。  
  
3.  在 **使用者識別碼**欄位中輸入**系統管理員**然後按一下**確定**。  
  
4.  在 **密碼**欄位中輸入系統管理員帳戶的密碼，然後按一下**登入**。  
  
5.  插入語彙基元資訊。  
  
    1.  在  **RSA Security Console**按一下 **驗證**，按一下  **SecurID 權杖**。  
  
    2.  按一下 **匯入語彙基元工作**，然後按一下**加入新**。  
  
    3.  在 **匯入選項**區段中，按一下**瀏覽**。 瀏覽並選取權杖 XML 檔案在 C:\RSA Installation\License 和語彙基元資料夾，然後按一下**開啟**。  
  
    4.  按一下 **提交作業**在頁面底部。  
  
6.  建立新使用者的 OTP。  
  
    1.  在**RSA Security 主控台**按一下**識別**索引標籤上，按一下 **使用者**，然後按一下**加入新**。  
  
    2.  在**Last Name:** 區段型別**使用者**，然後在**使用者 ID:** 區段型別**User1** （使用者識別碼必須是所使用的 AD 使用者名稱相同這個實驗室 （英文）。  在 **密碼：** 並**確認密碼：** 區段輸入強式密碼。 清除 **[需要使用者在下次登入時變更密碼]** 核取方塊，然後按一下**儲存**。  
  
7.  將 User1 指派給其中一個匯入的語彙基元。  
  
    1.  在 **使用者**頁面上，按一下**User1**然後按一下**SecurID 權杖**。  
  
    2.  按一下  **SecurID 權杖**然後按一下**權杖指派**。  
  
    3.  底下**序號**標題按一下第一個列出的數字，然後按一下**指派**。  
  
    4.  按一下 指派的權杖，然後按一下**編輯**。 在  **SecurID PIN 管理**區段**使用者驗證需求**，選取**不需要 PIN (僅 tokencode)** 。  
  
    5.  按一下 **儲存並發佈語彙基元**。  
  
    6.  在上**散發軟體語彙基元**頁面**基本概念**區段中，按一下**問題語彙基元檔案 (SDTID)** 。  
  
    7.  上**散發軟體語彙基元**頁面**語彙基元檔案選項**區段中，清除**啟用複製保護**核取方塊。 按一下 [**沒有密碼**並**下一步]** 。  
  
    8.  上**散發軟體語彙基元**頁面**Download File**區段中，按一下**立即下載**。 按一下 [儲存]  。 瀏覽至 C:\RSA 安裝，然後按一下**儲存**並**關閉**。  
  
    9. 最小化**RSA Security 主控台**供後續使用。  
  
8.  設定為 RADIUS 伺服器的驗證管理員。  
  
    1.  RSA 電腦桌面按兩下 **「 RSA 安全性 Operations 主控台 」** 。  
  
    2.  如果安全性憑證警告會出現安全性警示，按一下**繼續瀏覽此網站**或按**是**可以繼續，將此網站新增至信任的網站，如果要求。  
  
    3.  輸入使用者識別碼和密碼，然後按一下**登入**。  
  
    4.  按一下 **半徑-的部署組態伺服器設定**。  
  
    5.  在上**所需的其他認證**頁面上輸入系統管理員使用者識別碼和密碼，然後按一下**確定**。  
  
    6.  在上**設定 RADIUS 伺服器**頁面上，輸入系統管理員使用者所使用的相同密碼**祕密**並**主要密碼**。 輸入系統管理員使用者識別碼和密碼，然後按一下 **設定**。  
  
    7.  確認訊息 **'已成功設定的 RADIUS 伺服器'** 隨即出現。 按一下 [完成]  。 關閉**RSA Operations 主控台**。  
  
    8.  切換回 **「 RSA Security 主控台 」** 。  
  
    9. 在  **RADIUS**索引標籤處按一下**RADIUS 伺服器**。 確認已列出該 rsa.corp.contoso.com。  
  
9. RSA 伺服器設定為 RSA 驗證用戶端。  
  
    1.  在  **RADIUS**索引標籤上，按一下**RADIUS 用戶端**並**加入新**。  
  
    2.  按一下  **ANY 的 RADIUS 用戶端**核取方塊。  
  
    3.  輸入您選擇的強式密碼**共用祕密**欄位。 稍後設定 EDGE1 otp 時，您會使用這個相同的密碼。  
  
    4.  離開**IP 位址**欄位保留空白，而**讓 / 模型**項目標示為**標準 RADIUS**。  
  
    5.  按一下 **沒有 RSA 的代理程式儲存**。  
  
10. 建立設定 EDGE1 為 RSA Authentication Agent 所需的檔案。  
  
    1.  在 **存取**索引標籤上，反白顯示**驗證代理程式**，然後按一下**加入新**。  
  
    2.  型別**EDGE1**中**Hostname**欄位，然後按一下 **解決 IP**。  
  
    3.  請注意，EDGE1 的 IP 位址現在顯示在**IP 位址**欄位。 按一下 [儲存]  。  
  
11. 產生 EDGE1 伺服器 (AM_Config.zip) 的組態檔。  
  
    1.  在上**存取權**索引標籤上，反白顯示**驗證代理程式**，然後按一下**產生組態檔**。  
  
    2.  在 **產生組態檔**頁面上，按一下**產生組態檔**，然後按一下**立即下載**。  
  
    3.  按一下 **儲存**，瀏覽至 C:\RSA 安裝，然後按一下 **儲存**。  
  
    4.  按一下 **關閉**上**下載完整**對話方塊。  
  
12. 產生節點祕密檔案 EDGE1 伺服器 (EDGE1_NodeSecret.zip)。  
  
    1.  在 **存取**索引標籤上，反白顯示**驗證代理程式**，然後按一下**管理現有**。  
  
    2.  按一下目前已設定的節點 EDGE1，然後按一下**管理節點祕密**。  
  
    3.  請檢查**建立新的隨機節點祕密，並匯出至檔案的節點祕密**核取方塊。  
  
    4.  輸入中的系統管理員使用者所使用的相同密碼**加密密碼**並**確認加密密碼**欄位，然後按一下 **儲存**。  
  
    5.  在 **產生的節點祕密檔案**頁面上，按一下**立即下載**。  
  
    6.  在 **檔案下載**對話方塊中，按一下**儲存**中，瀏覽至 C:\RSA 安裝，然後按一下**儲存**。 按一下 **關閉**上**下載完整**對話方塊。  
  
    7.  從 RSA 驗證管理員媒體複製 \auth_mgr\windows-x86_64\am\rsa-ace_nsload\win32-5.0-x86\agent_nsload.exe C:\RSA 安裝。  
  
## <a name="BKMK_DAProbeUser"></a>建立 DAProbeUser  
  
1.  在**RSA Security 主控台**按一下**識別**索引標籤上，按一下 **使用者**，然後按一下**加入新**。  
  
2.  在**Last Name:** 區段型別**探查**，然後在**使用者 ID:** 區段型別**DAProbeUser**。 在 **密碼：** 並**確認密碼：** 區段輸入強式密碼。 清除 **[需要使用者在下次登入時變更密碼]** 核取方塊，然後按一下**儲存**。  
  
## <a name="InstToken"></a>在 CLIENT1 上安裝 RSA SecurID 軟體權杖  
您可以使用此程序，在 CLIENT1 上安裝 SecurID 軟體權杖。  
  
#### <a name="install-securid-software-token"></a>安裝 SecurID 軟體權杖  
  
1.  在 CLIENT1 電腦上，建立 C:\RSA 檔案的資料夾。 將檔案複製 Software_Tokens.zip C:\RSA 安裝 RSA 電腦上 C:\RSA 檔案。 展開 User1_000031701832.SDTID C:\RSA 檔案在 CLIENT1 上的檔案。  
  
2.  存取 RSA SecurID 軟體語彙基元的媒體來源，然後按兩下在 RSASECURIDTOKEN410 **SecurID SoftwareToken 用戶端應用程式**開始 RSA SecurID 安裝資料夾。 如果**開啟檔案-安全性警告**就會出現訊息，然後按一下**執行**。  
  
3.  在 [ **RSA SecurID 軟體權杖-InstallShield 精靈**對話方塊中，按一下**下一步]** 兩次。  
  
4.  接受授權合約，然後按一下 [**下一步]** 。  
  
5.  在上**安裝類型**對話方塊中，選取**典型**，按一下**下一步**，然後按一下**安裝**。  
  
6.  如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**是**]。  
  
7.  選取 **啟動 RSA SecurID 軟體語彙基元**核取方塊，然後按一下**完成**。  
  
8.  按一下 **從檔案匯入**。  
  
9. 按一下 **瀏覽**，選取 C:\RSA Files\User1_000031701832.SDTID，然後按一下**開啟**。  
  
10. 按兩次 **[確定]** 。  
  
## <a name="configAuthAgt"></a>將 EDGE1 設定為 RSA Authentication Agent  
您可以使用此程序來設定執行 RSA authentication EDGE1。  
  
#### <a name="configure-the-rsa-authentication-agent"></a>設定 RSA Authentication Agent  
  
1. 在 [EDGE1 上開啟 Windows 檔案總管] 並建立 C:\RSA 檔案的資料夾。 瀏覽至 RSA ACE 安裝媒體。  
  
2. 複製檔案 agent_nsload.exe，AM_Config.zip，並從 RSA 媒體的 EDGE1_NodeSecret.zip C:\RSA 檔案。  
  
3. 擷取到下列位置的兩個 zip 檔案的內容：  
  
   1.  C:\Windows\system32\  
  
   2.  C:\Windows\SysWOW64\  
  
4. 複製 agent_nsload.exe 至 C:\Windows\SysWOW64\\。  
  
5. 開啟提升權限的命令提示字元並瀏覽至 C:\Windows\SysWOW64。  
  
6. 型別**agent_nsload.exe-f nodesecret.rec-p <password>** 其中<password>是您在初始的 RSA 設定期間所建立的強式密碼。 按 Enter 鍵。  
  
7. 將 C:\Windows\SysWOW64\securid 複製到 C:\Windows\System32 中。  
  
## <a name="configOTP"></a>若要支援 OTP 驗證設定 EDGE1  
使用此程序來設定 DirectAccess 的 OTP，並確認組態。  
  
#### <a name="configure-otp-for-directaccess"></a>設定 DirectAccess OTP  
  
1.  在 EDGE1，開啟 伺服器管理員，然後按一下**遠端存取**的左窗格中。  
  
2.  以滑鼠右鍵按一下**EDGE1**伺服器 窗格中，然後選取**遠端存取管理**。  
  
3.  按一下 **組態**。  
  
4.  在  **DirectAccess 安裝** 視窗底下**步驟 2-遠端存取伺服器**，按一下 **編輯**。  
  
5.  按一下 [**下一步]** 三次，然後在**驗證**區段中，選取**雙因素驗證**並**使用 OTP**，並確定**使用電腦憑證**已核取。 確認根 CA 已設定為**CN = corp-APP1-CA**。 按一下 [下一步]  。  
  
6.  在  **OTP 的 RADIUS 伺服器**區段中，按兩下空白**伺服器名稱**欄位。  
  
7.  在 **新增 RADIUS 伺服器**對話方塊中，輸入**RSA**中**伺服器名稱**欄位。 按一下 **變更**旁**共用祕密**欄位，並輸入中的 RSA 伺服器上設定 RADIUS 用戶端時所使用的相同密碼**新祕密**並**確認新的祕密**欄位。 按一下 [ **[確定]** 兩次，然後按一下**下一步]** 。  
  
    > [!NOTE]  
    > 如果 RADIUS 伺服器位於不同的遠端存取伺服器，網域則有**伺服器名稱**欄位必須指定 RADIUS 伺服器的 FQDN。  
  
8.  在  **OTP CA 伺服器**區段選取 APP1.corp.contoso.com，然後按一下 **新增**。 按一下 [下一步]  。  
  
9. 在  **OTP 憑證範本**頁面上，按一下**瀏覽**以選取 使用 OTP 驗證，並在簽發憑證的註冊憑證範本**憑證範本** 對話方塊中選取**DAOTPLogon**。 按一下 [確定]  。 按一下 **瀏覽**以選取用來註冊登 OTP 憑證註冊要求，然後在遠端存取伺服器所使用之憑證的憑證範本**憑證範本**對話方塊選取  **DAOTPRA**。 按一下 [確定]  。 按一下 [下一步]  。  
  
10. 在上**遠端存取伺服器安裝**頁面上，按一下**完成**，然後按一下**完成**上**DirectAccess 專家精靈**。  
  
11. 在 **遠端存取權檢閱**對話方塊中，按一下**套用**，等候 DirectAccess 原則被更新，然後按一下**關閉**。  
  
12. 上**開始**畫面上，輸入**powershell.exe**，以滑鼠右鍵按一下**powershell**，按一下 **進階**，然後按一下**身分執行系統管理員**。 如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**是**]。  
  
13. 在 Windows PowerShell 視窗中，輸入**gpupdate /force**按 ENTER 鍵。  
  
14. 關閉並重新開啟 「 遠端存取管理主控台，確認所有的 OTP 設定正確無誤。  
  


