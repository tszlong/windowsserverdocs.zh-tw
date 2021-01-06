---
title: 步驟3設定 OTP 的遠端存取服務器
description: 本主題是在 Windows Server 2016 中使用 OTP 驗證部署「遠端存取」指南的一部分。
manager: brianlic
ms.topic: article
ms.assetid: df1e87f2-6a0f-433b-8e42-816ae75395f9
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: c5b4cde212da9c585c4d9afdb068147f406beb51
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946454"
---
# <a name="step-3-configure-the-remote-access-server-for-otp"></a>步驟3設定 OTP 的遠端存取服務器

>適用於：Windows Server (半年度管道)、Windows Server 2016

使用軟體發佈權杖設定 RADIUS 伺服器之後，就會開啟通訊埠、建立共用密碼、已在 RADIUS 伺服器上建立對應至 Active Directory 的使用者帳戶，而且遠端存取服務器已設定為 RADIUS 驗證代理程式，因此必須設定遠端存取服務器來支援 OTP。

|Task|描述|
|----|--------|
|[3.1 從 OTP 驗證豁免使用者 (選擇性) ](#BKMK_Exempt)|如果將使用 OTP 驗證從 DirectAccess 豁免特定使用者，請遵循這些初步步驟。|
|[3.2 設定遠端存取服務器支援 OTP](#BKMK_Config)|在遠端存取服務器上，更新遠端存取設定以支援 OTP 雙因素驗證。|
|[3.3 智慧卡以取得額外授權](#BKMK_Smartcard)|使用智慧卡的其他相關資訊。|

> [!NOTE]
> 本主題包含可讓您用來將部分所述的程序自動化的 Windows PowerShell Cmdlet 範例。 如需詳細資訊，請參閱[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693).

## <a name="31-exempt-users-from-otp-authentication-optional"></a><a name="BKMK_Exempt"></a>3.1 從 OTP 驗證豁免使用者 (選擇性) 
如果要豁免特定使用者的 OTP 驗證，則必須在遠端存取設定之前採取這些步驟：

> [!NOTE]
> 設定 OTP 豁免群組時，您必須等待網域之間的複寫完成。

#### <a name="create-user-exemption-security-group"></a>建立使用者豁免安全性群組

1.  在 Active Directory 中為目的 OTP 豁免建立安全性群組。

2.  新增要從 OTP 驗證豁免至安全性群組的所有使用者。

    > [!NOTE]
    > 請務必只在 [OTP 豁免] 安全性群組中包含使用者帳戶，而不包含電腦帳戶。

## <a name="32-configure-the-remote-access-server-to-support-otp"></a><a name="BKMK_Config"></a>3.2 設定遠端存取服務器支援 OTP
若要設定遠端存取以使用雙因素驗證和 OTP 搭配 RADIUS 伺服器和先前章節的憑證部署，請使用下列步驟：

#### <a name="configure-remote-access-for-otp"></a>設定 OTP 的遠端存取

1.  開啟 [ **遠端存取管理** ]， **然後按一下 [** 設定]。

2.  在 [ **DirectAccess 設定** ] 視窗的 [ **步驟 2-遠端存取服務器**] 下，按一下 [ **編輯**]。

3.  按三次 [ **下一步]** ，然後在 [ **驗證** ] 區段中同時選取 **兩個因素驗證** 和 [ **使用 OTP**]，並確定已核取 [ **使用電腦憑證** ]。

    > [!NOTE]
    > 在遠端存取服務器上啟用 OTP 之後，如果您藉由取消選擇 [ **使用 otp**] 來停用 otp，則會在伺服器上卸載 ISAPI 和 CGI 延伸模組。

4.  如果需要 Windows 7 支援，請選取 [ **啟用 windows 7 用戶端電腦透過 DirectAccess 進行連線]** 核取方塊。 注意：如規劃一節所述，Windows 7 用戶端必須安裝 DCA 2.0，才能支援使用 OTP 的 DirectAccess。

5.  按一下 [下一步] 。

6.  在 [ **OTP RADIUS 伺服器** ] 區段中，按兩下 [空白 **伺服器名稱** ] 欄位。

7.  在 [ **新增 Radius 伺服器** ] 對話方塊的 [ **伺服器名稱** ] 欄位中，輸入 radius 伺服器的名稱。 按一下 [**共用密碼**] 欄位旁的 [**變更**]，然後輸入您在 **新密碼** 中設定 RADIUS 伺服器時所使用的相同密碼，然後 **確認新的秘密** 欄位。 按兩次 **[確定]** ，然後按 **[下一步**]。

    > [!NOTE]
    > 如果 RADIUS 伺服器位於與遠端存取服務器不同的網域中，則 [ **伺服器名稱** ] 欄位必須指定 RADIUS 伺服器的 FQDN。

8.  在 [ **OTP CA 伺服器** ] 區段中，選取要用於註冊 OTP 用戶端驗證憑證的 CA 伺服器，然後按一下 [ **新增**]。 按一下 [下一步] 。

9. 在 [ **OTP 憑證範本** ] 區段中，按一下 **[流覽]** ，選取用來註冊 OTP 驗證所發行憑證的憑證範本。

    > [!NOTE]
    > 公司 CA 所發行之 OTP 憑證的憑證範本必須設定為不使用 [在發行的憑證中包含撤銷資訊] 選項。 如果在建立憑證範本時選取此選項，OTP 用戶端電腦將無法正確登入。

    按一下 **[流覽]** ，選取用來註冊遠端存取服務器用來簽署 OTP 憑證註冊要求之憑證的憑證範本。 按一下 [確定]。 按 [下一步] 。

10. 如果需要使用 OTP 從 DirectAccess 排除特定使用者，請在 [ **Otp 豁免** ] 區段中，選取 [ **不需要指定安全性群組中的使用者使用雙因素驗證來進行驗證**]。 按一下 [ **安全性群組** ]，然後選取為 OTP 豁免建立的安全性群組。

11. 在 [ **遠端存取服務器設定** ] 頁面上，按一下 **[完成]**。

12. 在 [ **DirectAccess 設定** ] 視窗的 [ **步驟 3-基礎結構伺服器**] 下，按一下 [ **編輯**]。

13. 按兩次 [ **下一步]** ，然後在 [ **管理** ] 區段中，按兩下 [ **管理伺服器** ] 欄位。

14. 輸入設定為發行 OTP 憑證之 CA 伺服器的 **電腦名稱稱** 或 **位址** ，然後按一下 **[確定]**。

15. 在 [ **遠端存取設定** ] 視窗中，按一下 **[完成]**。

16. 按一下 [ **DirectAccess 專家 Wizard]** 上的 **[完成]** 。

17. 在 [ **遠端存取審核** ] 對話方塊中 **，按一下**[套用]，等候 DirectAccess 原則更新，然後按一下 [ **關閉**]。

18. 在 [ **開始** ] 畫面上，輸入 **powershell.exe**，以滑鼠右鍵按一下 **powershell**，按一下 [ **Advanced**]，然後按一下 [以 **系統管理員身分執行**]。 如果出現 [**使用者帳戶控制**] 對話方塊，請確認它所顯示的動作就是您所需的動作，然後按一下 [**是**]。

19. 在 [Windows PowerShell] 視窗中，輸入 **gpupdate/force** ，然後按 enter 鍵。

使用 PowerShell 命令設定 OTP 的遠端存取：

![Windows PowerShell](../../../../media/Step-3-Configure-the-Remote-Access-Server-for-OTP/PowerShellLogoSmall.gif)**Windows PowerShell 對等命令**

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

若要將遠端存取設定為在目前使用電腦憑證驗證的部署上使用雙因素驗證：

```
Set-DAServer -UserAuthentication TwoFactor
```

若要使用下列設定來設定遠端存取以使用 OTP 驗證：

-   名為 OTP.corp.contoso.com 的 OTP 伺服器。

-   名為 APP1 的 CA 伺服器。 com\corp-APP1-CA1。

-   名為 DAOTPLogon 的憑證範本，用於註冊 OTP 驗證所發行的憑證。

-   名為 DAOTPRA 的憑證範本，用來註冊遠端存取服務器用來簽署 OTP 憑證註冊要求的註冊授權單位憑證。

```
Enable-DAOtpAuthentication -CertificateTemplateName 'DAOTPLogon' -SigningCertificateTemplateName 'DAOTPRA' -CAServer @('APP1.corp.contoso.com\corp-APP1-CA1') -RadiusServer OTP.corp.contoso.com -SharedSecret Abcd123$
```

執行 PowerShell 命令之後，請完成上一個程式中的步驟12-19，將遠端存取服務器設定為支援 OTP。

> [!NOTE]
> 在新增進入點之前，請務必先確認您已在遠端存取服務器上套用 OTP 設定。

## <a name="33-smart-cards-for-additional-authorization"></a><a name="BKMK_Smartcard"></a>3.3 智慧卡以取得額外授權
在 [遠端存取設定] 嚮導的步驟2的 [驗證] 頁面上，您可以要求使用智慧卡來存取內部網路。 選取此選項時，[遠端存取安裝精靈] 會針對 DirectAccess 伺服器上的內部網路通道設定 IPsec 連線安全性規則，以要求使用智慧卡進行通道模式授權。 通道模式授權可讓您指定只有經過授權的電腦或使用者可以建立輸入通道。

若要將智慧卡與 IPsec 通道模式授權用於內部網路通道，您必須使用智慧卡基礎結構 (PKI) 部署公開金鑰基礎結構。

因為您的 DirectAccess 用戶端使用智慧卡來存取內部網路，所以您也可以使用 Windows Server 2008 R2 的「驗證機制保證」（Windows Server R2 的功能）來控制對資源的存取（例如檔案、資料夾和印表機），並根據使用者是否以智慧卡憑證登入。 驗證機制保證需要 Windows Server 2008 R2 的網域功能等級。

### <a name="allowing-access-for-users-with-unusable-smart-cards"></a>允許具有無法使用之智慧卡的使用者存取
若要允許有智慧卡無法使用的使用者暫時存取，請執行下列動作：

1.  建立 Active Directory 安全性群組，以包含暫時無法使用其智慧卡之使用者的使用者帳戶。

2.  針對 DirectAccess 伺服器群組原則物件，設定 IPsec 通道授權的全域 IPsec 設定，並將 Active Directory 安全性群組新增至授權使用者清單。

若要將存取權授與無法使用其智慧卡的使用者，請暫時將他們的使用者帳戶新增至 Active Directory 安全性群組。 當智慧卡可供使用時，請從群組中移除使用者帳戶。

### <a name="under-the-covers-smart-card-authorization"></a>涵蓋範圍：智慧卡授權
智慧卡授權的運作方式，是在 DirectAccess 伺服器的內部網路通道連線安全性規則上啟用通道模式授權， (SID) 的特定 Kerberos 安全性識別元。 若為智慧卡授權，這是已知的 SID (S-1-5-65-1) ，其對應至以智慧卡為基礎的登入。 此 SID 存在於 DirectAccess 用戶端的 Kerberos 權杖中，在全域 IPsec 通道模式授權設定中設定時，即稱為「此組織憑證」。

當您在 DirectAccess 安裝程式的步驟2中啟用智慧卡授權時，DirectAccess 安裝程式會為 DirectAccess 伺服器群組原則物件的此 SID 設定全域 IPsec 通道模式授權設定。 若要在 DirectAccess 伺服器群組原則物件的 [具有 Advanced Security 的 Windows 防火牆] 嵌入式管理單元中查看此設定，請執行下列動作：

1.  以滑鼠右鍵按一下 [具有 Advanced Security 的 Windows 防火牆]，然後按一下 [內容]。

2.  在 [IPsec 設定] 索引標籤的 [IPsec 通道授權] 中，按一下 [自訂]。

3.  按一下 [使用者] 索引標籤。您應該會看到「NT AUTHORITY\This 組織憑證」以授權使用者的形式出現。



