---
title: 疑難排解驗證問題
description: 本主題是在 Windows Server 2016 中使用 OTP 驗證部署「遠端存取」指南的一部分。
manager: brianlic
ms.topic: article
ms.assetid: 71307757-f8f4-4f82-b8b3-ffd4fd8c5d6d
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 9717b4c12579564d2c3e567fdce17ac105804217
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947234"
---
# <a name="troubleshooting-authentication-issues"></a>疑難排解驗證問題

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題包含在嘗試使用 OTP 驗證連接到 DirectAccess 時，與使用者可能遇到之問題相關的疑難排解資訊。 DirectAccerss OTP 相關事件會記錄在用戶端電腦上的 [ **應用程式及服務記錄檔/Microsoft/Windows/OtpCredentialProvider**] 下事件檢視器中。 針對 DirectAccess OTP 的問題進行疑難排解時，請確定已啟用此記錄。

## <a name="failed-to-access-the-ca-that-issues-otp-certificates"></a>無法存取發行 OTP 憑證的 CA
**案例**。 使用者無法使用 OTP 進行驗證，錯誤為：「驗證因內部錯誤而失敗」

 (用戶端事件記錄檔) **收到錯誤**。 <username>CA 伺服器 <上的 OTP 憑證註冊失敗 CA_name>、要求失敗、失敗的可能原因：無法解析 ca 伺服器名稱、無法透過第一個 DirectAccess 通道存取 ca 伺服器，或無法建立 ca 伺服器的連線。

**原因**

使用者提供了有效的單次密碼，而 DirectAccess 伺服器已簽署憑證要求;不過，用戶端電腦無法連絡發出 OTP 憑證的 CA 來完成註冊程式。

**解決方案**

在 DirectAccess 伺服器上，執行下列 Windows PowerShell 命令：

1.  取得已設定的 OTP 發行 Ca 的清單，並檢查 ' CAServer ' 的值： `Get-DAOtpAuthentication`

2.  請確定 Ca 已設定為管理伺服器： `Get-DAMgmtServer -Type All`

3.  確定用戶端電腦已建立基礎結構通道：在 [具有 Advanced Security 的 Windows 防火牆] 主控台中，依序展開 [ **監視/安全性關聯**] 和 [ **主要模式]**，並確定您的 DirectAccess 設定有正確的遠端位址來顯示 IPsec 安全性關聯。

## <a name="directaccess-server-connectivity-issues"></a>DirectAccess 伺服器連線能力問題
**案例**。 使用者無法使用 OTP 進行驗證，錯誤為：「驗證因內部錯誤而失敗」

 (用戶端事件記錄檔) **收到錯誤**

下列其中一個錯誤：

-   使用基底路徑 <OTP_authentication_path> 和埠 <OTP_authentication_port>，無法建立遠端存取服務器 <DirectAccess_server_hostname> 的連接。 錯誤碼： <internal_error_code>。

-   您無法使用基底路徑 <OTP_authentication_path> 和埠 <OTP_authentication_port>，將使用者認證傳送至遠端存取服務器 <DirectAccess_server_hostname>。 錯誤碼： <internal_error_code>。

-   使用基底路徑 <OTP_authentication_path> 和埠 <OTP_authentication_port>，未從遠端存取服務器收到回應 <DirectAccess_server_hostname>。 錯誤碼： <internal_error_code>。

**原因**

用戶端電腦無法透過網際網路存取 DirectAccess 伺服器，因為網路問題或 DirectAccess 伺服器上的 IIS 伺服器設定錯誤。

**解決方案**

請確定用戶端電腦上的網際網路連線正常運作，並確定 DirectAccess 服務正在執行，且可透過網際網路存取。

## <a name="failed-to-enroll-for-the-directaccess-otp-logon-certificate"></a>無法註冊 DirectAccess OTP 登入憑證
**案例**。 使用者無法使用 OTP 進行驗證，錯誤為：「驗證因內部錯誤而失敗」

 (用戶端事件記錄檔) **收到錯誤**。 從 CA <CA_name> 的憑證註冊失敗。 OTP 簽署憑證未如預期般簽署要求，或使用者沒有註冊的許可權。

**原因**

使用者所提供的單次密碼正確無誤，但頒發證書授權單位 (CA) 拒絕發行 OTP 登入憑證。 憑證要求可能未正確地使用正確的 EKU (OTP 登錄授權單位應用程式原則) 來簽署，或使用者沒有 DA OTP 範本的「註冊」許可權。

**解決方案**

請確定 DirectAccess OTP 使用者具有註冊 DirectAccess OTP 登入憑證的許可權，而且 DA OTP 登錄授權單位簽署範本中包含適當的「應用程式原則」。 也請確定遠端存取服務器上的 DirectAccess 登錄授權單位憑證有效。 請參閱3.2 規劃 OTP 憑證範本和3.3 規劃註冊授權單位憑證。

## <a name="missing-or-invalid-computer-account-certificate"></a>電腦帳戶憑證遺失或無效
**案例**。 使用者無法使用 OTP 進行驗證，錯誤為：「驗證因內部錯誤而失敗」

 (用戶端事件記錄檔) **收到錯誤**。  無法完成 OTP 驗證，因為在本機電腦憑證存放區中找不到 OTP 所需的電腦憑證。

**原因**

DirectAccess OTP 驗證需要用戶端電腦憑證，才能建立與 DirectAccess 伺服器的 SSL 連線;不過，找不到或不正確用戶端電腦憑證，例如憑證已過期。

**解決方案**

請確定電腦憑證存在且有效：

1.  在用戶端電腦上的 [MMC 憑證] 主控台中，針對本機電腦帳戶開啟 [ **個人/憑證**]。

2.  請確定已發出符合電腦名稱稱的憑證，然後按兩下該憑證。

3.  在 [ **憑證] 對話方塊的** [ **憑證路徑** ] 索引標籤上，于 [ **憑證狀態**] 底下，確定其顯示「此憑證正常」。

如果找不到有效的憑證，請刪除不正確憑證 (如果) 存在，請從提高許可權的命令提示字元執行， `gpupdate /Force` 或重新開機用戶端電腦，以重新註冊電腦憑證。

## <a name="missing-ca-that-issues-otp-certificates"></a>缺少頒發 OTP 憑證的 CA
**案例**。 使用者無法使用 OTP 進行驗證，錯誤為：「驗證因內部錯誤而失敗」

 (用戶端事件記錄檔) **收到錯誤**。 無法完成 OTP 驗證，因為 DA 伺服器未傳回發行 CA 的位址。

**原因**

沒有任何 Ca 會發行已設定的 OTP 憑證，或發行 OTP 憑證的所有已設定 Ca 都沒有回應。

**解決方案**

1.  使用下列命令來取得頒發 OTP 憑證 (CA 名稱會顯示在 CAServer) ：中的 Ca 清單 `Get-DAOtpAuthentication` 。

2.  如果未設定任何 Ca：

    1.  使用命令 `Set-DAOtpAuthentication` 或 [遠端存取管理] 主控台來設定發出 DIRECTACCESS OTP 登入憑證的 ca。

    2.  套用新的設定，並強制用戶端 `gpupdate /Force` 從提高許可權的命令提示字元執行，或重新開機用戶端電腦，以重新整理 DIRECTACCESS GPO 設定。

3.  如果有已設定的 Ca，請確定其在線上並回應註冊要求。

## <a name="misconfigured-directaccess-server-address"></a>DirectAccess 伺服器位址設定錯誤
**案例**。 使用者無法使用 OTP 進行驗證，錯誤為：「驗證因內部錯誤而失敗」

 (用戶端事件記錄檔) **收到錯誤**。 OTP 驗證無法如預期般完成。 無法判斷遠端存取服務器的名稱或位址。  錯誤碼： <error_code>。 DirectAccess 設定應由伺服器管理員進行驗證。

**原因**

DirectAccess 伺服器的位址未正確設定。

**解決方案**

使用來檢查已設定的 DirectAccess 伺服器位址 `Get-DirectAccess` ，如果位址設定錯誤，請加以更正。

`gpupdate /force`從提升許可權的命令提示字元執行，或重新開機用戶端電腦，以確定已在用戶端電腦上部署最新的設定。

## <a name="failed-to-generate-the-otp-logon-certificate-request"></a>無法產生 OTP 登入憑證要求
**案例**。 使用者無法使用 OTP 進行驗證，錯誤為：「驗證因內部錯誤而失敗」

 (用戶端事件記錄檔) **收到錯誤**。 無法初始化 OTP 驗證的憑證要求。 可能無法產生私密金鑰，或使用者 <username> 無法存取網域控制站上的 <OTP_template_name> 的憑證範本。

**原因**

發生這個錯誤的可能原因有兩個：

-   使用者沒有讀取 OTP 登入範本的許可權。

-   因為發生網路問題，所以使用者的電腦無法存取網域控制站。

**解決方案**

-   請檢查 OTP 登入範本的許可權設定，並確定為 DirectAccess OTP 布建的所有使用者都具有「讀取」許可權。

-   請確定網域控制站已設定為管理伺服器，而且用戶端電腦可以透過基礎結構通道連線到網域控制站。 請參閱3.2 規劃 OTP 憑證範本。

## <a name="no-connection-to-the-domain-controller"></a>沒有連接到網域控制站
**案例**。 使用者無法使用 OTP 進行驗證，錯誤為：「驗證因內部錯誤而失敗」

 (用戶端事件記錄檔) **收到錯誤**。 無法建立與網域控制站的連線以進行 OTP 驗證。 錯誤碼： <error_code>。

**原因**

發生這個錯誤的可能原因有兩個：

-   使用者的電腦沒有網路連線能力。

-   無法透過基礎結構通道存取網域控制站。

**解決方案**

-   請從 PowerShell 提示字元執行下列命令，以確定網域控制站已設定為管理伺服器： `Get-DAMgmtServer -Type All` 。

-   請確定用戶端電腦可以透過基礎結構通道連線到網域控制站。

## <a name="otp-provider-requires-challengeresponse"></a>OTP 提供者需要挑戰/回應
**案例**。 使用者無法使用 OTP 進行驗證，錯誤為：「驗證因內部錯誤而失敗」

 (用戶端事件記錄檔) **收到錯誤**。 使用遠端存取服務器的 OTP 驗證 ( # B0 DirectAccess_server_name> 使用者 (的) <username>) 需要使用者提出挑戰。

**原因**

使用的 OTP 提供者要求使用者以 RADIUS 挑戰/回應交換的形式提供其他認證，Windows Server 2012 DirectAccess OTP 不支援此功能。

**解決方案**

在任何情況下，將 OTP 提供者設定為不需要挑戰/回應。

## <a name="incorrect-otp-logon-template-used"></a>使用了不正確的 OTP 登入範本
**案例**。 使用者無法使用 OTP 進行驗證，錯誤為：「驗證因內部錯誤而失敗」

 (用戶端事件記錄檔) **收到錯誤**。 使用者要求憑證的 CA 範本 <username> 未設定為發行 OTP 憑證。

**原因**

DirectAccess OTP 登入範本已被取代，而且用戶端電腦正嘗試使用較舊的範本進行驗證。

**解決方案**

藉由執行下列其中一項，確定用戶端電腦使用最新的 OTP 設定：

-   從提升許可權的命令提示字元執行下列命令，以強制群組原則更新： `gpupdate /Force` 。

-   重新開機用戶端電腦。

## <a name="missing-otp-signing-certificate"></a>遺漏 OTP 簽署憑證
**案例**。 使用者無法使用 OTP 進行驗證，錯誤為：「驗證因內部錯誤而失敗」

 (用戶端事件記錄檔) **收到錯誤**。 找不到 OTP 簽署憑證。 無法簽署 OTP 憑證註冊要求。

**原因**

在遠端存取服務器上找不到 DirectAccess OTP 簽署憑證;因此，遠端存取服務器無法簽署使用者憑證要求。 沒有簽署憑證，或簽署憑證已過期，但尚未更新。

**解決方案**

在遠端存取服務器上執行這些步驟。

1.  執行 PowerShell Cmdlet 以檢查已設定的 OTP 簽署憑證範本名稱 `Get-DAOtpAuthentication` ，並檢查的值 `SigningCertificateTemplateName` 。

2.  使用 [憑證] MMC 嵌入式管理單元，確定已從這個範本註冊的有效憑證存在於電腦上。

3.  如果沒有這類憑證存在，請刪除過期的憑證 (如果有的話) ，然後根據此範本註冊新的憑證。

若要建立 OTP 簽署憑證範本，請參閱3.3 規劃註冊授權單位憑證。

## <a name="missing-or-incorrect-upndn-for-the-user"></a>使用者的 UPN/DN 遺失或不正確
**案例**。 使用者無法使用 OTP 進行驗證，錯誤為：「驗證因內部錯誤而失敗」

 (用戶端事件記錄檔) **收到錯誤**

下列其中一個錯誤：

-   使用者 <username> 無法使用 OTP 進行驗證。 確定已在 Active Directory 中定義使用者名稱的 UPN。 錯誤碼： <error_code>。

-   使用者 <username> 無法使用 OTP 進行驗證。 確定已針對 Active Directory 中的使用者名稱定義 DN。 錯誤碼： <error_code>。

 (伺服器事件記錄檔) **收到錯誤**

<username>為 OTP 驗證指定的使用者名稱不存在。

**原因**

使用者未在使用者帳戶中正確設定使用者主體名稱 (UPN) 或辨別名稱 (DN) 屬性，則需要這些屬性才能正常運作 DirectAccess OTP。

**解決方案**

使用網域控制站上的 Active Directory 消費者和電腦主控台，確認已為驗證使用者正確設定這兩個屬性。

## <a name="otp-certificate-is-not-trusted-for-login"></a>OTP 憑證不受信任而無法登入
**案例**。 使用者無法使用 OTP 進行驗證，錯誤為：「驗證因內部錯誤而失敗」

**原因**

發行 OTP 憑證的 CA 不在 enterprise NTAuth store 中;因此，註冊的憑證無法用於登入。 這可能會發生在未建立跨網域 CA 信任的多網域和多樹系環境中。

**解決方案**

請確定頒發 OTP 憑證的 CA 階層根目錄的憑證已安裝在使用者嘗試驗證之網域的 enterprise NTAuth 憑證存放區中。

## <a name="windows-could-not-verify-user-credentials"></a>Windows 無法驗證使用者認證
**案例**。 使用者無法使用 OTP 進行驗證，錯誤為：「驗證因內部錯誤而失敗」

 (用戶端電腦) **收到錯誤**。 Windows 正在驗證您的認證時發生錯誤。 再試一次，或洽詢您的系統管理員以取得協助。

**原因**

當 DirectAccess OTP 登入憑證未包含 CRL 時，Kerberos 驗證通訊協定無法運作。 DirectAccess OTP 登入憑證不包含 CRL，因為下列其中一項：

-   DirectAccess OTP 登入範本已設定為 [ **未在發行的憑證中包含撤銷資訊**] 選項。

-   CA 設定為不發佈 Crl。

**解決方案**

1.  若要確認此錯誤的原因，請在 [遠端存取管理] 主控台的 [ **步驟2遠端存取服務器**] 中，按一下 [ **編輯**]，然後在 [ **遠端存取服務器安裝程式** ] 中，按一下 [ **OTP 憑證範本**]。 記下用來註冊 OTP 驗證所發行憑證的憑證範本。 開啟 [憑證授權單位單位] 主控台，按一下左窗格中的 [ **憑證範本**]，再按兩下 [OTP 登入憑證] 以查看憑證範本屬性。

    若要解決此問題，請設定 OTP 登入憑證的憑證，並不要在 [範本內容] 對話方塊的 [**伺服器**] 索引標籤上，選取 [不 **包含發行的憑證中的撤銷資訊**] 核取方塊。

2.  在 CA 伺服器上，開啟 [憑證授權單位單位] MMC，以滑鼠 **按右鍵發行** CA，然後按一下 [內容]。 在 [ **擴充** 功能] 索引標籤上，確認已正確設定 CRL 發佈。



