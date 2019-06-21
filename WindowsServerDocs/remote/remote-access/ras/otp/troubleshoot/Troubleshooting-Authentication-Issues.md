---
title: 疑難排解驗證問題
description: 本主題是 Windows Server 2016 中的 OTP 驗證部署遠端存取快速入門的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 71307757-f8f4-4f82-b8b3-ffd4fd8c5d6d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 08dd6822cc30135506d82041cfbeab0bc1a058ab
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282360"
---
# <a name="troubleshooting-authentication-issues"></a>疑難排解驗證問題

>適用於：Windows Server （半年通道），Windows Server 2016

本主題包含當您嘗試連接到使用 OTP 驗證的 DirectAccess 時，使用者可能有問題的相關問題的疑難排解資訊。 DirectAccerss OTP 的相關事件會記錄在事件檢視器用戶端電腦上**應用程式和服務記錄檔/Microsoft/Windows/OtpCredentialProvider**。 請確定，利用 DirectAccess OTP 的問題進行疑難排解時，會啟用此記錄檔。  
  
## <a name="failed-to-access-the-ca-that-issues-otp-certificates"></a>無法存取發行 OTP 憑證的 CA  
**案例**。 使用者會失敗並出現錯誤使用 OTP 進行驗證：「 驗證失敗因為發生內部錯誤 」  
  
**收到的錯誤**（用戶端事件記錄檔）。 使用者的 OTP 憑證註冊<username>CA 伺服器 < CA_name > 上失敗，要求失敗，可能失敗的原因：無法解析的憑證授權單位伺服器名稱、 CA 伺服器無法存取透過第一個 DirectAccess 通道或無法建立 CA 伺服器的連接。  
  
**原因**  
  
使用者提供有效的單次密碼和 DirectAccess 伺服器簽署憑證要求;不過，用戶端電腦無法連絡 CA 簽發 OTP 的憑證，以完成註冊程序。  
  
**解決方法**  
  
在 DirectAccess 伺服器上，執行下列 Windows PowerShell 命令：  
  
1.  取得發行 Ca 的設定 OTP 的清單，並檢查 'CAServer' 的值： `Get-DAOtpAuthentication`  
  
2.  請確定 Ca 會設定為管理伺服器： `Get-DAMgmtServer -Type All`  
  
3.  請確定用戶端電腦已建立基礎結構通道：在 Windows 防火牆與進階安全性的主控台中，依序展開**監視/安全性關聯**，按一下**主要模式**，並確定使用正確的遠端，出現的 IPsec 安全性關聯DirectAccess 設定的位址。  
  
## <a name="directaccess-server-connectivity-issues"></a>DirectAccess 伺服器連線問題  
**案例**。 使用者會失敗並出現錯誤使用 OTP 進行驗證：「 驗證失敗因為發生內部錯誤 」  
  
**收到的錯誤**（用戶端事件記錄檔）  
  
其中一個下列的錯誤：  
  
-   不使用基底路徑 < OTP_authentication_path > 和 < OTP_authentication_port > 的連接埠的遠端存取伺服器 < DirectAccess_server_hostname > 建立連線。 錯誤碼： < internal_error_code >。  
  
-   使用者認證無法傳送到遠端存取伺服器 < DirectAccess_server_hostname > 使用基底路徑 < OTP_authentication_path > 和 < OTP_authentication_port > 的連接埠。 錯誤碼： < internal_error_code >。  
  
-   回應不被來自使用基底路徑 < OTP_authentication_path > 和 < OTP_authentication_port > 的連接埠的遠端存取伺服器 < DirectAccess_server_hostname >。 錯誤碼： < internal_error_code >。  
  
**原因**  
  
用戶端電腦無法存取網際網路，因為可能是網路問題，或到 DirectAccess 伺服器上設定不正確的 IIS 伺服器上的 DirectAccess 伺服器。  
  
**解決方法**  
  
請確定，就會在用戶端電腦上的網際網路連線運作，，並確定 DirectAccess 服務透過網際網路，正在執行且可存取。  
  
## <a name="failed-to-enroll-for-the-directaccess-otp-logon-certificate"></a>無法註冊 DirectAccess OTP 登入憑證  
**案例**。 使用者會失敗並出現錯誤使用 OTP 進行驗證：「 驗證失敗因為發生內部錯誤 」  
  
**收到的錯誤**（用戶端事件記錄檔）。 從 CA < CA_name > 的憑證註冊失敗。 要求未如預期般 OTP 簽署憑證所簽署，或使用者沒有註冊權限。  
  
**原因**  
  
使用者所提供的單次密碼不正確，但是拒絕讓它發出 OTP 登入憑證的發行憑證授權單位 (CA)。 憑證要求可能不會正確地簽署與正確的 EKU （OTP 登錄授權單位的應用程式原則），或使用者沒有 DA OTP 範本的 「 註冊 」 權限。  
  
**解決方法**  
  
請確定 DirectAccess OTP 使用者有權限，才能註冊 DirectAccess OTP 登入憑證，而且 DA OTP 登錄授權單位簽署範本中包含適當 「 應用程式原則 」。 也請確定該遠端存取伺服器上的 DirectAccess 登錄授權單位憑證有效。 請參閱 3.2 規劃 OTP 憑證範本和 3.3 計劃的登錄授權單位憑證。  
  
## <a name="missing-or-invalid-computer-account-certificate"></a>遺漏或無效的電腦帳戶憑證  
**案例**。 使用者會失敗並出現錯誤使用 OTP 進行驗證：「 驗證失敗因為發生內部錯誤 」  
  
**收到的錯誤**（用戶端事件記錄檔）。  無法完成 OTP 驗證，因為本機電腦憑證存放區中找不到所需的 OTP 的電腦憑證。  
  
**原因**  
  
DirectAccess OTP 驗證需要用戶端電腦憑證建立 SSL 連線與 DirectAccess 伺服器;不過，用戶端電腦憑證找不到或不是有效的比方說，如果憑證已過期。  
  
**解決方法**  
  
請確定電腦憑證是否存在，而且有效：  
  
1.  用戶端電腦上，在 MMC 憑證主控台中，本機電腦帳戶，開啟**Personal/Certificates**。  
  
2.  請確定已發出的憑證符合電腦名稱，然後連按兩下憑證。  
  
3.  在 **憑證**對話方塊的 **憑證路徑**索引標籤之下**憑證狀態**，請確定它說 「 這個憑證沒有問題 」。  
  
如果找不到有效的憑證，刪除無效的憑證 （若有的話） 並重新註冊電腦憑證： 執行`gpupdate /Force`從提升權限的命令提示字元，或重新啟動用戶端電腦。  
  
## <a name="missing-ca-that-issues-otp-certificates"></a>遺漏發出 OTP 憑證的 CA  
**案例**。 使用者會失敗並出現錯誤使用 OTP 進行驗證：「 驗證失敗因為發生內部錯誤 」  
  
**收到的錯誤**（用戶端事件記錄檔）。 無法完成 OTP 驗證，因為 DA 伺服器未傳回發行 CA 的位址。  
  
**原因**  
  
沒有發行設定，OTP 憑證的 Ca，或者所有設定的 Ca 發出 OTP 憑證都沒有回應。  
  
**解決方法**  
  
1.  若要取得 OTP 憑證 （憑證授權單位名稱示 CAServer） 發行的 Ca 清單中使用下列命令： `Get-DAOtpAuthentication`。  
  
2.  如果沒有 Ca 設定：  
  
    1.  使用任一個命令`Set-DAOtpAuthentication`或遠端存取管理主控台，以設定發出 DirectAccess OTP 登入憑證的 Ca。  
  
    2.  套用新的組態，並強制執行重新整理 DirectAccess GPO 設定的用戶端`gpupdate /Force`從提升權限的命令提示字元，或重新啟動用戶端電腦。  
  
3.  如果有設定的 Ca，請確定它們線上註冊要求的回應。  
  
## <a name="misconfigured-directaccess-server-address"></a>設定錯誤的 DirectAccess 伺服器位址  
**案例**。 使用者會失敗並出現錯誤使用 OTP 進行驗證：「 驗證失敗因為發生內部錯誤 」  
  
**收到的錯誤**（用戶端事件記錄檔）。 OTP 驗證無法完成，如預期般運作。 無法判斷的名稱或的遠端存取伺服器的位址。  錯誤碼： < error_code >。 伺服器系統管理員應該驗證 DirectAccess 設定。  
  
**原因**  
  
未正確設定 DirectAccess 伺服器的位址。  
  
**解決方法**  
  
設定 DirectAccess 伺服器位址中使用的核取`Get-DirectAccess`和修正的位址，如果設定不正確。  
  
請確定用戶端電腦上部署最新的設定，則將它執行`gpupdate /force`從提升權限的命令提示字元或重新啟動用戶端電腦。  
  
## <a name="failed-to-generate-the-otp-logon-certificate-request"></a>無法產生 OTP 登入的憑證要求  
**案例**。 使用者會失敗並出現錯誤使用 OTP 進行驗證：「 驗證失敗因為發生內部錯誤 」  
  
**收到的錯誤**（用戶端事件記錄檔）。 無法初始化 OTP 驗證的憑證要求。 無法產生的私用金鑰，或使用者<username>無法存取網域控制站上的憑證範本 < OTP_template_name >。  
  
**原因**  
  
有兩個可能的原因造成此錯誤：  
  
-   使用者沒有讀取 OTP 登入範本的權限。  
  
-   使用者的電腦無法存取網域控制站，因為網路問題。  
  
**解決方法**  
  
-   檢閱 OTP 登入的範本上設定的權限，並確定，針對 DirectAccess OTP 佈建的所有使用者具有 「 都讀取 」 權限。  
  
-   請確定網域控制站設定為管理伺服器和用戶端電腦可以透過基礎結構通道連線到網域控制站。 請參閱 3.2 規劃 OTP 憑證範本。  
  
## <a name="no-connection-to-the-domain-controller"></a>網域控制站沒有連線  
**案例**。 使用者會失敗並出現錯誤使用 OTP 進行驗證：「 驗證失敗因為發生內部錯誤 」  
  
**收到的錯誤**（用戶端事件記錄檔）。 無法建立與網域控制站，以進行 OTP 驗證的連線。 錯誤碼： < error_code >。  
  
**原因**  
  
有兩個可能的原因造成此錯誤：  
  
-   使用者的電腦有沒有網路連線。  
  
-   透過基礎結構通道，網域控制站無法存取。  
  
**解決方法**  
  
-   請確定網域控制站設定為管理伺服器，從 PowerShell 提示字元執行下列命令： `Get-DAMgmtServer -Type All`。  
  
-   請確定用戶端電腦可以透過基礎結構通道連線到網域控制站。  
  
## <a name="otp-provider-requires-challengeresponse"></a>OTP 提供者需要挑戰/回應  
**案例**。 使用者會失敗並出現錯誤使用 OTP 進行驗證：「 驗證失敗因為發生內部錯誤 」  
  
**收到的錯誤**（用戶端事件記錄檔）。 使用者的遠端存取伺服器 (< DirectAccess_server_name >) 使用的 OTP 驗證 (<username>) 需要使用者的挑戰。  
  
**原因**  
  
使用的 OTP 提供者會要求使用者提供額外的認證，不支援 Windows Server 2012 DirectAccess OTP 的 RADIUS 挑戰/回應交換的形式。  
  
**解決方法**  
  
設定 OTP 提供者不需要在任何案例的挑戰/回應。  
  
## <a name="incorrect-otp-logon-template-used"></a>使用不正確的 OTP 登入範本  
**案例**。 使用者會失敗並出現錯誤使用 OTP 進行驗證：「 驗證失敗因為發生內部錯誤 」  
  
**收到的錯誤**（用戶端事件記錄檔）。 哪位使用者的 CA 範本<username>要求憑證未設定為發出 OTP 憑證。  
  
**原因**  
  
DirectAccess OTP 登入範本已被取代，並在用戶端電腦嘗試使用較舊的範本進行驗證。  
  
**解決方法**  
  
請確定用戶端電腦使用最新的 OTP 設定，藉由執行下列其中一項：  
  
-   從提升權限的命令提示字元執行下列命令來強制群組原則更新： `gpupdate /Force`。  
  
-   重新啟動用戶端電腦。  
  
## <a name="missing-otp-signing-certificate"></a>遺漏簽署憑證的 OTP  
**案例**。 使用者會失敗並出現錯誤使用 OTP 進行驗證：「 驗證失敗因為發生內部錯誤 」  
  
**收到的錯誤**（用戶端事件記錄檔）。 找不到 OTP 簽章憑證。 無法簽署 OTP 憑證註冊要求。  
  
**原因**  
  
遠端存取伺服器找不到簽章憑證的 DirectAccess OTP因此，無法簽署使用者憑證要求，由遠端存取伺服器。 沒有任何簽署的憑證，或簽署的憑證已過期，已不會更新。  
  
**解決方法**  
  
遠端存取伺服器上執行這些步驟。  
  
1.  執行 PowerShell cmdlet 來檢查已設定的 OTP 簽署憑證範本名稱`Get-DAOtpAuthentication`檢查值和`SigningCertificateTemplateName`。  
  
2.  請確定電腦上有此範本從註冊的有效憑證，請使用 憑證 MMC 嵌入式管理單元。  
  
3.  如果此憑證不存在，刪除過期的憑證 （如果有的話），並註冊新的憑證，在根據此範本。  
  
若要建立 OTP 簽署憑證範本會參閱 3.3 的計劃登錄授權單位憑證。  
  
## <a name="missing-or-incorrect-upndn-for-the-user"></a>遺失或不正確 UPN/DN 使用者  
**案例**。 使用者會失敗並出現錯誤使用 OTP 進行驗證：「 驗證失敗因為發生內部錯誤 」  
  
**收到的錯誤**（用戶端事件記錄檔）  
  
其中一個下列的錯誤：  
  
-   使用者<username>無法使用 OTP 驗證。 請確定 UPN 定義在 Active Directory 中的使用者名稱。 錯誤碼： < error_code >。  
  
-   使用者<username>無法使用 OTP 驗證。 請確定 DN 定義在 Active Directory 中的使用者名稱。 錯誤碼： < error_code >。  
  
**收到的錯誤**（伺服器事件記錄檔）  
  
使用者名稱<username>指定 OTP 驗證不存在。  
  
**原因**  
  
使用者沒有使用者主體名稱 (UPN) 或辨別名稱 (DN) 屬性設定正確的使用者帳戶中，這些屬性所需的 DirectAccess OTP 正常運作。  
  
**解決方法**  
  
使用網域控制站上的 [Active Directory 使用者和電腦] 主控台，來確認這兩個屬性已正確設定驗證的使用者。  
  
## <a name="otp-certificate-is-not-trusted-for-login"></a>OTP 憑證不受信任的登入  
**案例**。 使用者會失敗並出現錯誤使用 OTP 進行驗證：「 驗證失敗因為發生內部錯誤 」  
  
**原因**  
  
發出 OTP 憑證的 CA 不是 enterprise NTAuth 存放區中;因此，已註冊的憑證無法用於登入。 其中不會建立跨網域的信任 CA 的多網域和多樹系環境中，這可能會發生。  
  
**解決方法**  
  
請確定簽發 OTP 憑證的 CA 階層的根憑證已安裝的企業 NTAuth 憑證存放區的使用者嘗試進行驗證的網域中。  
  
## <a name="windows-could-not-verify-user-credentials"></a>Windows 無法驗證使用者認證  
**案例**。 使用者會失敗並出現錯誤使用 OTP 進行驗證：「 驗證失敗因為發生內部錯誤 」  
  
**收到的錯誤**（用戶端電腦）。 雖然 Windows 已驗證您的認證時發生錯誤。 再試一次，或您的系統管理員尋求協助。  
  
**原因**  
  
Kerberos 驗證通訊協定無法運作時的 DirectAccess OTP 登入憑證不包含 CRL。 DirectAccess OTP 登入憑證不含 CRL，因為其中一個：  
  
-   DirectAccess OTP 登入範本已設定的選項**簽發的憑證中，不包含撤銷資訊**。  
  
-   CA 是設定為不發佈 Crl。  
  
**解決方法**  
  
1.  若要確認此錯誤的原因，在遠端存取管理主控台中，在**步驟 2 遠端存取伺服器**，按一下**編輯**，然後在**遠端存取伺服器安裝**精靈中，按一下**OTP 憑證範本**。 記下，所發出的 OTP 驗證的憑證來註冊所用的憑證範本。 開啟憑證授權單位主控台中的，在左窗格中，按一下**憑證範本**，連按兩下，檢視 憑證範本內容的 OTP 登入憑證。  
  
    若要解決此問題，請設定 OTP 登入憑證的憑證，而且未選取**簽發的憑證中，不包含撤銷資訊** 核取方塊**Server**  索引標籤的範本內容對話方塊。  
  
2.  在 CA 伺服器上，開啟 [憑證授權單位] MMC，以滑鼠右鍵按一下發行 CA，按一下**屬性**。 在 [**延伸模組**] 索引標籤上，確定已正確設定 CRL 發佈。  
  


