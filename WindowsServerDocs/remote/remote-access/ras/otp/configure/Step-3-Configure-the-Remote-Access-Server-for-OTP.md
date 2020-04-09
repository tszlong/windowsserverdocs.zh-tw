---
title: 步驟3設定 OTP 的遠端存取服務器
description: 本主題是在 Windows Server 2016 中使用 OTP 驗證部署遠端存取指南的一部分。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: df1e87f2-6a0f-433b-8e42-816ae75395f9
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 98de4a1e673065e0b759186a7a53ce45675302be
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858211"
---
# <a name="step-3-configure-the-remote-access-server-for-otp"></a>步驟3設定 OTP 的遠端存取服務器

>適用於：Windows Server (半年通道)、Windows Server 2016

當 RADIUS 伺服器已設定軟體發佈權杖之後，就會開啟通訊埠、建立共用密碼、對應至 Active Directory 的使用者帳戶已在 RADIUS 伺服器上建立，而遠端存取服務器則具有已設定為 RADIUS 驗證代理程式，則必須將遠端存取服務器設定為支援 OTP。  
  
|工作|描述|  
|----|--------|  
|[3.1 豁免使用者免于 OTP 驗證（選擇性）](#BKMK_Exempt)|如果使用 OTP 驗證來豁免 DirectAccess 的特定使用者，請遵循這些預備步驟。|  
|[3.2 設定遠端存取服務器以支援 OTP](#BKMK_Config)|在遠端存取服務器上，更新遠端存取設定以支援 OTP 雙因素驗證。|  
|[3.3 智慧卡以取得額外的授權](#BKMK_Smartcard)|有關使用智慧卡的其他資訊。|  
  
> [!NOTE]  
> 本主題包含可讓您用以自動化文中所述部分程序的範例 Windows PowerShell 指令程式。 如需詳細資訊，請參閱[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="31-exempt-users-from-otp-authentication-optional"></a><a name="BKMK_Exempt"></a>3.1 豁免使用者免于 OTP 驗證（選擇性）  
如果要豁免特定使用者的 OTP 驗證，則必須在遠端存取設定之前採取這些步驟：  
  
> [!NOTE]  
> 設定 OTP 豁免群組時，您必須等候網域之間的複寫完成。  
  
#### <a name="create-user-exemption-security-group"></a>建立使用者豁免安全性群組  
  
1.  在 Active Directory 中，為目的 OTP 豁免建立安全性群組。  
  
2.  將所有要豁免的使用者從 OTP 驗證新增到安全性群組。  
  
    > [!NOTE]  
    > 在 OTP 豁免安全性群組中，請務必只包含使用者帳戶，而不是電腦帳戶。  
  
## <a name="32-configure-the-remote-access-server-to-support-otp"></a><a name="BKMK_Config"></a>3.2 設定遠端存取服務器以支援 OTP  
若要設定遠端存取以搭配使用雙因素驗證和 OTP 與上一節的 RADIUS 伺服器和憑證部署，請使用下列步驟：  
  
#### <a name="configure-remote-access-for-otp"></a>設定 OTP 的遠端存取  
  
1.  開啟 [**遠端存取管理**]，**然後按一下 [** 設定]。  
  
2.  在 [ **DirectAccess 設定**] 視窗的 [**步驟 2-遠端存取服務器**] 底下，按一下 [**編輯**]。  
  
3.  按三次 [**下一步]** ，然後在 [**驗證**] 區段中選取 [**雙因素驗證**] 和 [**使用 OTP**]，並確定已核取 [**使用電腦憑證**]。  
  
    > [!NOTE]  
    > 在遠端存取服務器上啟用 OTP 之後，如果您取消選擇 [**使用 otp**] 來停用 otp，則會在伺服器上卸載 ISAPI 和 CGI 延伸模組。  
  
4.  如果需要 Windows 7 支援，請選取 [**啟用 windows 7 用戶端電腦透過 DirectAccess 進行連線]** 核取方塊。 注意：如規劃一節中所述，Windows 7 用戶端必須安裝 DCA 2.0，才能支援使用 OTP 的 DirectAccess。  
  
5.  按 [下一步]。  
  
6.  在 [ **OTP RADIUS 伺服器**] 區段中，按兩下 [空白**伺服器名稱**] 欄位。  
  
7.  在 [**新增 Radius 伺服器**] 對話方塊的 [**伺服器名稱**] 欄位中，輸入 radius 伺服器的名稱。 按一下 [**共用密碼**] 欄位旁的 [**變更**]，然後輸入您在 [**新密碼**] 和 [**確認新密碼**] 欄位中設定 RADIUS 伺服器時所使用的相同密碼。 按兩次 **[確定]** ，然後按 **[下一步]** 。  
  
    > [!NOTE]  
    > 如果 RADIUS 伺服器所在的網域與遠端存取服務器不同，則 [**伺服器名稱**] 欄位必須指定 RADIUS 伺服器的 FQDN。  
  
8.  在 [ **OTP CA 伺服器**] 區段中，選取要用於註冊 OTP 用戶端驗證憑證的 CA 伺服器，然後按一下 [**新增**]。 按 [下一步]。  
  
9. 在 [ **OTP 憑證範本**] 區段中，按一下 **[流覽]** 以選取憑證範本，用來註冊針對 OTP 驗證所簽發的憑證。  
  
    > [!NOTE]  
    > 公司 CA 所簽發之 OTP 憑證的憑證範本必須設定為不使用 [在發行的憑證中包含撤銷資訊] 選項。 如果在憑證範本建立期間選取此選項，OTP 用戶端電腦將無法正確登入。  
  
    按一下 **[流覽]** 以選取憑證範本，用來註冊遠端存取服務器用來簽署 OTP 憑證註冊要求的憑證。 按一下 [確定]。 按 [下一步]。  
  
10. 如果需要使用 OTP 從 DirectAccess 排除特定使用者，請在 [ **Otp 豁免**] 區段中，選取 [**不要求指定安全性群組中的使用者使用雙因素驗證來驗證**]。 按一下 [**安全性群組**]，然後選取針對 OTP 豁免所建立的安全性群組。  
  
11. 在 [**遠端存取服務器設定**] 頁面上，按一下 **[完成]** 。  
  
12. 在 [ **DirectAccess 設定**] 視窗的 [**步驟 3-基礎結構伺服器**] 底下，按一下 [**編輯**]。  
  
13. 按兩次 [**下一步]** ，然後在 [**管理**] 區段中按兩下 [**管理伺服器**] 欄位。  
  
14. 輸入設定為發行 OTP 憑證之 CA 伺服器的**電腦名稱稱**或**位址**，然後按一下 **[確定]** 。  
  
15. 在 [**遠端存取設定**] 視窗中，按一下 **[完成]** 。  
  
16. 按一下 [ **DirectAccess 專家嚮導**] 上的 **[完成]** 。  
  
17. 在 [**遠端存取審核**] 對話方塊上 **，按一下 [** 套用]，等待 DirectAccess 原則更新，然後按一下 [**關閉**]。  
  
18. 在 [**開始**] 畫面上，輸入**powershell**，以滑鼠右鍵按一下 [ **powershell**]，然後按一下 [ **Advanced**]，再按一下 [以**系統管理員身分執行**]。 如果出現 [使用者帳戶控制] 對話方塊，請確認其顯示的動作為您想要的動作，然後按一下 [是]。  
  
19. 在 Windows PowerShell 視窗中，輸入**gpupdate/force** ，然後按 enter。  
  
若要使用 PowerShell 命令來設定 OTP 的遠端存取：  
  
![Windows PowerShell](../../../../media/Step-3-Configure-the-Remote-Access-Server-for-OTP/PowerShellLogoSmall.gif)**Windows powershell 對等命令**  
  
下列 Windows PowerShell 指令程式會執行與前述程序相同的功能。 請逐行各輸入一個指令程式，儘管有些指令程式可能因為受制於內文格式而自動換行拆成好幾行。  
  
若要在目前使用電腦憑證驗證的部署上設定遠端存取以使用雙因素驗證：  
  
```  
Set-DAServer -UserAuthentication TwoFactor  
```  
  
若要將遠端存取設定為使用 OTP 驗證，請使用下列設定：  
  
-   名為 OTP.corp.contoso.com 的 OTP 伺服器。  
  
-   名為 APP1 的 CA 伺服器。 com\corp-APP1-CA1。  
  
-   名為 DAOTPLogon 的憑證範本，用來註冊針對 OTP 驗證所簽發的憑證。  
  
-   名為 DAOTPRA 的憑證範本，用來註冊遠端存取服務器用來簽署 OTP 憑證註冊要求的註冊授權單位憑證。  
  
```  
Enable-DAOtpAuthentication -CertificateTemplateName 'DAOTPLogon' -SigningCertificateTemplateName 'DAOTPRA' -CAServer @('APP1.corp.contoso.com\corp-APP1-CA1') -RadiusServer OTP.corp.contoso.com -SharedSecret Abcd123$  
```  
  
執行 PowerShell 命令之後，請完成前一個程式中的步驟12-19，將遠端存取服務器設定為支援 OTP。  
  
> [!NOTE]  
> 在新增進入點之前，請務必先確認您已在遠端存取服務器上套用 OTP 設定。  
  
## <a name="33-smart-cards-for-additional-authorization"></a><a name="BKMK_Smartcard"></a>3.3 智慧卡以取得額外的授權  
在 [遠端存取設定向導] 的步驟 2 [驗證] 頁面上，您可以要求使用智慧卡來存取內部網路。 選取此選項時，[遠端存取設定] 嚮導會設定 DirectAccess 伺服器上內部網路通道的 IPsec 連線安全性規則，以要求使用智慧卡的通道模式授權。 通道模式授權可讓您指定只有授權的電腦或使用者可以建立輸入通道。  
  
若要使用智慧卡搭配 IPsec 通道模式授權進行內部網路通道，您必須使用智慧卡基礎結構來部署公開金鑰基礎結構（PKI）。  
  
因為您的 DirectAccess 用戶端會使用智慧卡來存取內部網路，所以您也可以使用 Windows Server 2008 R2 的驗證機制保證（這是一項功能）來控制對資源（例如，檔案、資料夾和印表機）的存取，這是根據使用智慧卡型憑證登入的使用者。 驗證機制保證需要 Windows Server 2008 R2 的網域功能等級。  
  
### <a name="allowing-access-for-users-with-unusable-smart-cards"></a>允許具有無法使用智慧卡的使用者存取  
若要允許有智慧卡無法使用的使用者暫時存取，請執行下列動作：  
  
1.  建立 Active Directory 安全性群組，以包含暫時無法使用其智慧卡的使用者帳戶。  
  
2.  針對 DirectAccess 伺服器群組原則物件，請設定 IPsec 通道授權的全域 IPsec 設定，並將 Active Directory 安全性群組新增至授權使用者的清單。  
  
若要將存取權授與無法使用智慧卡的使用者，請暫時將他們的使用者帳戶新增至 Active Directory 安全性群組。 當智慧卡可供使用時，從群組中移除使用者帳戶。  
  
### <a name="under-the-covers-smart-card-authorization"></a>在幕後：智慧卡授權  
智慧卡授權的運作方式是在 DirectAccess 伺服器的內部網路通道連線安全性規則上，針對特定的 Kerberos 安全識別碼（SID）啟用通道模式授權。 對於智慧卡授權而言，這是已知的 SID （S-1-5-65-1），它會對應至智慧卡登入。 此 SID 會出現在 DirectAccess 用戶端的 Kerberos 權杖中，在全域 IPsec 通道模式授權設定中設定時，稱為「此組織憑證」。  
  
當您在 DirectAccess 設定向導的步驟2中啟用智慧卡授權時，DirectAccess 安裝程式會為 DirectAccess 伺服器群組原則物件的此 SID 設定全域 IPsec 通道模式授權設定。 若要在 DirectAccess 伺服器的 [具有 Advanced Security 的 Windows 防火牆] 嵌入式管理單元中查看這項設定群組原則物件，請執行下列動作：  
  
1.  以滑鼠右鍵按一下 [具有 Advanced Security 的 Windows 防火牆]，然後按一下 [內容]。  
  
2.  在 [IPsec 設定] 索引標籤的 [IPsec 通道授權] 中，按一下 [自訂]。  
  
3.  按一下 [使用者] 索引標籤。您應該會看到 [NT AUTHORITY\This 組織憑證] 為授權使用者。  
  


