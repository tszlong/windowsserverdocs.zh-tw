---
title: 步驟 3 的 OTP 設定遠端存取伺服器
description: 本主題是 Windows Server 2016 中的 OTP 驗證部署遠端存取快速入門的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: df1e87f2-6a0f-433b-8e42-816ae75395f9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5e192c062e8ecd18128109321058ddc58b6b1e4a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839839"
---
# <a name="step-3-configure-the-remote-access-server-for-otp"></a>步驟 3 的 OTP 設定遠端存取伺服器

>適用於：Windows Server （半年通道），Windows Server 2016

使用軟體發佈權杖設定 RADIUS 伺服器之後, 通訊連接埠已開啟、 已建立的共用的密碼、 在 RADIUS 伺服器上，建立對應至 Active Directory 的使用者帳戶和遠端存取伺服器具有已設定為 RADIUS 驗證代理程式，則遠端存取伺服器必須設定為支援 OTP。  
  
|工作|描述|  
|----|--------|  
|[3.1 OTP 驗證 （選擇性） 讓使用者得以豁免](#BKMK_Exempt)|如果特定使用者無須從 DirectAccess 使用 OTP 驗證，請遵循這些預備步驟。|  
|[3.2 設定遠端存取伺服器來支援 OTP](#BKMK_Config)|在 遠端存取伺服器會更新遠端存取設定，以支援 OTP 雙因素驗證。|  
|[3.3 智慧卡進行額外的授權](#BKMK_Smartcard)|其他的智慧卡使用相關資訊。|  
  
> [!NOTE]  
> 本主題包含可讓您用來將部分所述的程序自動化的 Windows PowerShell Cmdlet 範例。 如需詳細資訊，請參閱[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_Exempt"></a>3.1 OTP 驗證 （選擇性） 讓使用者得以豁免  
如果特定使用者豁免的 OTP 驗證，這些步驟必須採取的遠端存取組態前：  
  
> [!NOTE]  
> 您必須等到完成時設定 OTP 豁免群組的網域之間的複寫。  
  
#### <a name="create-user-exemption-security-group"></a>建立使用者豁免安全性群組  
  
1.  建立 Active Directory 中的目的 OTP 豁免安全性群組。  
  
2.  新增至要免除的安全性群組的 OTP 驗證的所有使用者。  
  
    > [!NOTE]  
    > 請務必只包含使用者帳戶和不是電腦帳戶，在 OTP 豁免安全性群組。  
  
## <a name="BKMK_Config"></a>3.2 設定遠端存取伺服器來支援 OTP  
若要設定使用雙因素驗證的遠端存取，以及 OTP 的 RADIUS 伺服器與上一節中的憑證部署，使用下列步驟：  
  
#### <a name="configure-remote-access-for-otp"></a>設定 OTP 的遠端存取  
  
1.  開啟**遠端存取管理**然後按一下**組態**。  
  
2.  在  **DirectAccess 安裝** 視窗底下**步驟 2-遠端存取伺服器**，按一下 **編輯**。  
  
3.  按一下 [**下一步]** 三次，然後在**驗證**區段中，選取這兩**雙因素驗證**並**使用 OTP**，並確保可**使用電腦憑證**已核取。  
  
    > [!NOTE]  
    > OTP 已啟用的遠端存取伺服器上，如果您取消選取 停用 OTP 之後**使用 OTP**，將會解除安裝伺服器上的 ISAPI 及 CGI 延伸。  
  
4.  如果需要 Windows 7 的支援，請選取**啟用 Windows 7 用戶端電腦透過 DirectAccess 進行連線**核取方塊。 注意：規劃一節所述，Windows 7 用戶端必須安裝支援 DirectAccess 與 OTP 的 DCA 2.0。  
  
5.  按一下 [下一步] 。  
  
6.  在  **OTP 的 RADIUS 伺服器**區段中，按兩下空白**伺服器名稱**欄位。  
  
7.  在 [**新增 RADIUS 伺服器**] 對話方塊中，輸入 RADIUS 伺服器的名稱**伺服器名稱**欄位。 按一下 **變更**旁**共用祕密**欄位，並輸入您在設定中的 RADIUS 伺服器時使用的相同密碼**新祕密**並**確認新的祕密**欄位。 按一下 [ **[確定]** 兩次，然後按一下**下一步]**。  
  
    > [!NOTE]  
    > 如果 RADIUS 伺服器位於不同的遠端存取伺服器，網域則有**伺服器名稱**欄位必須指定 RADIUS 伺服器的 FQDN。  
  
8.  在  **OTP CA 伺服器**區段中，選取可用於註冊 OTP 用戶端驗證憑證，然後按一下 CA 伺服器**新增**。 按一下 [下一步] 。  
  
9. 在  **OTP 憑證範本**區段中，按一下**瀏覽**選取用於所發出的 OTP 驗證的憑證註冊的憑證範本。  
  
    > [!NOTE]  
    > 沒有 「 不包含撤銷資訊中發行的憑證 」 選項時，必須設定公司的 CA 所發行的 OTP 憑證的憑證範本。 如果憑證範本建立期間選取此選項，將無法登入正確 OTP 用戶端電腦。  
  
    按一下 **瀏覽**選取用來註冊憑證遠端存取伺服器用來登入 OTP 憑證註冊要求的憑證範本。 按一下 [確定] 。 按一下 [下一步] 。  
  
10. 如果免除從 DirectAccess OTP 與特定使用者為必要項，然後在**OTP 豁免**區段中，選取**不需要使用雙因素驗證來驗證指定的安全性群組中的使用者**. 按一下 **安全性群組**並選取已建立的 OTP 豁免安全性群組。  
  
11. 在 **遠端存取伺服器安裝**頁面上，按一下**完成**。  
  
12. 在  **DirectAccess 安裝** 視窗底下**步驟 3-基礎結構伺服器**，按一下 **編輯**。  
  
13. 按一下 [**下一步]** 兩次，然後在**管理**一節按兩下**管理伺服器**欄位。  
  
14. 輸入**電腦名稱**或是**位址**CA 伺服器設定為發出 OTP 的憑證，然後按一下 **確定**。  
  
15. 在 **遠端存取安裝**視窗中按一下**完成**。  
  
16. 按一下 **完成**上**DirectAccess 專家精靈**。  
  
17. 在 **遠端存取權檢閱**對話方塊中，按一下**套用**，等候 DirectAccess 原則被更新，然後按一下**關閉**。  
  
18. 上**開始**畫面上，輸入**powershell.exe**，以滑鼠右鍵按一下**powershell**，按一下 **進階**，然後按一下**身分執行系統管理員**。 如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**是**]。  
  
19. 在 Windows PowerShell 視窗中，輸入**gpupdate /force**按 ENTER 鍵。  
  
若要使用 PowerShell 命令的 otp 設定遠端存取：  
  
![Windows PowerShell](../../../../media/Step-3-Configure-the-Remote-Access-Server-for-OTP/PowerShellLogoSmall.gif)**Windows PowerShell 對等的命令**  
  
下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。  
  
若要設定在目前使用電腦憑證驗證的部署上使用雙因素驗證的遠端存取：  
  
```  
Set-DAServer -UserAuthentication TwoFactor  
```  
  
若要設定遠端存取使用 OTP 驗證，使用下列設定：  
  
-   名為 OTP.corp.contoso.com OTP 伺服器。  
  
-   名為 APP1.corp.contoso.com\corp-APP1-CA1 CA 伺服器。  
  
-   名為 DAOTPLogon 用於所發出的 OTP 驗證的憑證，來註冊憑證範本。  
  
-   名為 DAOTPRA 的憑證範本用來註冊登錄授權單位憑證遠端存取伺服器用來登入 OTP 憑證註冊要求。  
  
```  
Enable-DAOtpAuthentication -CertificateTemplateName 'DAOTPLogon' -SigningCertificateTemplateName 'DAOTPRA' -CAServer @('APP1.corp.contoso.com\corp-APP1-CA1') -RadiusServer OTP.corp.contoso.com -SharedSecret Abcd123$  
```  
  
之後執行 PowerShell 命令完成步驟 12 月 19 日從先前的程序來設定遠端存取伺服器來支援 OTP。  
  
> [!NOTE]  
> 請務必確認您擁有的 OTP 設定套用遠端存取伺服器上新增的進入點之前。  
  
## <a name="BKMK_Smartcard"></a>3.3 智慧卡進行額外的授權  
在步驟 2 遠端存取安裝精靈中的 [驗證] 頁面中，您可以要求使用智慧卡存取內部網路。 選取此選項時，遠端存取安裝精靈會要求通道模式授權，智慧卡與 DirectAccess 伺服器上設定內部網路通道的 IPsec 連線安全性規則。 通道模式授權可讓您指定，只有獲得授權的電腦或使用者可以建立輸入的通道。  
  
若要使用智慧卡的內部網路通道的 IPsec 通道模式授權，您必須部署公開金鑰基礎結構 (PKI) 與智慧卡基礎結構。  
  
因為 DirectAccess 用戶端會使用智慧卡，以存取內部網路，您也可以使用驗證機制保證的 Windows Server 2008 R2，一項功能來控制存取資源，例如檔案、 資料夾和印表機，根據是否使用者登入的智慧卡為基礎的憑證。 驗證機制保證需要 Windows Server 2008 R2 網域功能等級。  
  
### <a name="allowing-access-for-users-with-unusable-smart-cards"></a>允許存取的使用者無法使用智慧卡  
若要允許與無法使用的智慧卡的使用者暫時存取權，請執行下列作業：  
  
1.  建立 Active Directory 安全性群組包含的使用者暫時無法使用其智慧卡的使用者帳戶。  
  
2.  DirectAccess 伺服器的 群組原則物件中，設定全域 IPsec 設定 IPsec 通道的授權並已獲授權的使用者清單中加入的 Active Directory 安全性群組。  
  
存取權授與使用者無法使用他們的智慧卡，暫時將他們的使用者帳戶新增至 Active Directory 安全性群組。 智慧卡可用時，請從群組移除使用者帳戶。  
  
### <a name="under-the-covers-smart-card-authorization"></a>實際上：智慧卡授權  
智慧卡授權的運作方式是啟用通道模式授權，在內部網路通道連線安全性規則上的 DirectAccess 伺服器的特定以 Kerberos 為基礎的安全性識別碼 (SID)。 智慧卡的授權，這是已知 SID (S-1-5-65-1)，這會對應到智慧卡為基礎的登入。 這個 SID 存在於 DirectAccess 用戶端的 Kerberos 權杖，而指 「 此組織憑證 」 中全域 IPsec 設定時為通道模式授權設定。  
  
當您啟用 DirectAccess 安裝精靈的步驟 2 中的智慧卡授權時，DirectAccess 安裝精靈會使用這個 SID 的 DirectAccess 伺服器的群組原則物件設定將全域 IPsec 通道模式授權設定。 若要檢視此設定具有進階安全性嵌入式管理單元的 DirectAccess 伺服器的群組原則物件的 Windows 防火牆 中，執行下列作業：  
  
1.  以滑鼠右鍵按一下 具有進階安全性的 Windows 防火牆，然後按一下 屬性。  
  
2.  在 IPsec 設定 索引標籤中，在 IPsec 通道授權中，按一下 自訂。  
  
3.  按一下 [使用者] 索引標籤。您應該為授權使用者會看到 「 NT AUTHORITY\This 組織憑證 」。  
  


