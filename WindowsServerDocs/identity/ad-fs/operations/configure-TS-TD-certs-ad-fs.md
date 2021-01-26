---
title: 取得和設定 AD FS 的權杖簽署和權杖解密憑證
description: 本檔說明如何取得及設定 AD FS 的 TS 和 TD 憑證
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/23/2017
ms.topic: article
ms.openlocfilehash: 9676962e2c0917f950ab81cd0577cd41fea51710
ms.sourcegitcommit: 6717decb5839aa340c81811d6fde020aabaddb3b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/26/2021
ms.locfileid: "98781858"
---
# <a name="obtain-and-configure-ts-and-td-certificates-for-ad-fs"></a>取得和設定 AD FS 的 TS 和 TD 憑證

本主題說明您可以執行的工作和程式，以確保 AD FS 的權杖簽署和權杖解密憑證都是最新的。

權杖簽署憑證是標準 X509 憑證，用來安全地簽署同盟伺服器簽發的所有權杖。 權杖解密憑證是標準 X509 憑證，用來解密任何傳入權杖。 它們也會在同盟中繼資料中發佈。

如需其他資訊，請參閱 [憑證需求](../design/ad-fs-requirements.md#BKMK_1)

## <a name="determine-whether-ad-fs-renews-the-certificates-automatically"></a>判斷 AD FS 是否自動更新憑證
依預設，會將 AD FS 設定為在初始設定時和當憑證接近其到期日時自動產生權杖簽署和權杖解密憑證。

您可以執行下列 Windows PowerShell 命令： `Get-AdfsProperties` 。

  ![PowerShell 視窗的螢幕擷取畫面，其中顯示 [自動憑證變換] 和 [憑證產生臨界值] 屬性已呼叫 Get-ADFSProperties 命令的結果。](media/configure-TS-TD-certs-ad-fs/ts1.png)

AutoCertificateRollover 屬性描述 AD FS 是否設定為自動更新權杖簽署和權杖解密憑證。

如果 AutoCertificateRollover 設定為 TRUE，就會自動更新 AD FS 的憑證，並在 AD FS 中進行設定。 設定新的憑證之後，為了避免中斷，您必須確定每個同盟夥伴 (由信賴憑證者信任或宣告提供者信任在 AD FS 伺服器陣列中表示，) 會以這個新的憑證更新。

如果 AD FS 未設定為自動更新權杖簽署和權杖解密憑證 (如果 AutoCertificateRollover 設為 False) ，AD FS 將不會自動產生或開始使用新的權杖簽署或權杖解密憑證。 您將必須手動執行這些工作。

如果 AD FS 已設定為自動更新權杖簽署和權杖解密憑證 (AutoCertificateRollover 設為 TRUE) ，您可以判斷何時會更新這些憑證：

CertificateGenerationThreshold 描述憑證在一天之後，將產生新憑證的天數。

CertificatePromotionThreshold 會判斷產生新憑證的天數，以將其提升為主要憑證 (換句話說，AD FS 將會開始使用它來簽署權杖，並從身分識別提供者) 中解密權杖。

![PowerShell 視窗的螢幕擷取畫面，其中顯示 [憑證產生閾值] 和 [憑證升級閾值] 屬性已被呼叫的 Get-ADFSProperties 命令結果。](media/configure-TS-TD-certs-ad-fs/ts2.png)

如果 AD FS 已設定為自動更新權杖簽署和權杖解密憑證 (AutoCertificateRollover 設為 TRUE) ，您可以判斷何時會更新這些憑證：

 - **CertificateGenerationThreshold** 描述憑證在一天之後，將產生新憑證的天數。
 - **CertificatePromotionThreshold** 會判斷產生新憑證的天數，以將其提升為主要憑證 (換句話說，AD FS 將會開始使用它來簽署權杖，並從身分識別提供者) 中解密權杖。

## <a name="determine-when-the-current-certificates-expire"></a>判斷目前憑證到期的時間
您可以使用下列程式來識別主要權杖簽署和權杖解密憑證，並判斷目前憑證到期的時間。

您可以執行下列 Windows PowerShell 命令：  `Get-AdfsCertificate –CertificateType token-signing` (或  `Get-AdfsCertificate –CertificateType token-decrypting `) 。 或者，您可以在 MMC： Service->憑證中檢查目前的憑證。

![PowerShell 視窗的螢幕擷取畫面，其中顯示 [CertificateType 權杖簽署] 命令的 Get-AdfsCertificate 結果，其中的 [不是] 和 [已呼叫的主要屬性]。](media/configure-TS-TD-certs-ad-fs/ts3.png)

**IsPrimary** 值設定為 **True** 的憑證是 AD FS 目前使用的憑證。

[ **不晚** ] 所顯示的日期是必須設定新主要權杖簽署或解密憑證的日期。

為確保服務持續性，所有同盟夥伴 (由信賴憑證者信任或宣告提供者信任在您的 AD FS 伺服器陣列中表示) 必須在到期前取用新的權杖簽署和權杖解密憑證。 建議您提前至少60天開始規劃此程式。

## <a name="generating-a-new-self-signed-certificate-manually-prior-to-the-end-of-the-grace-period"></a>在寬限期結束之前手動產生新的自我簽署憑證
您可以使用下列步驟，在寬限期結束之前手動產生新的自我簽署憑證。

1. 確定您已登入主要 AD FS 伺服器。
2. 開啟 Windows PowerShell，然後執行下列命令： `Add-PSSnapin "microsoft.adfs.powershell"`
3. （選擇性）您可以在 AD FS 中檢查目前的簽署憑證。 若要這樣做，請執行以下命令：`Get-ADFSCertificate –CertificateType token-signing`。 查看命令輸出，以查看所列任何憑證的非之後日期。
4. 若要產生新憑證，請執行下列命令來更新 AD FS 伺服器上的憑證並更新： `Update-ADFSCertificate –CertificateType token-signing` 。
5. 再次執行下列命令以驗證更新：`Get-ADFSCertificate –CertificateType token-signing`
6. 現在應該會列出兩個憑證，其中一個憑證的 **到期日** 大約是未來的一年，而 **IsPrimary** 的值為 **False**。

>[!IMPORTANT]
>若要避免服務中斷，請執行如何使用有效的權杖簽署憑證來更新 Azure AD 中的步驟，以更新 Azure AD 上的憑證資訊。

## <a name="if-youre-not-using-self-signed-certificates"></a>如果您不是使用自我簽署的憑證 .。。
如果您未使用預設自動產生的自我簽署權杖簽署和權杖解密憑證，您必須手動更新並設定這些憑證。

首先，您必須從您的憑證授權單位單位取得新憑證，並將它匯入每部同盟伺服器上的本機電腦個人憑證存儲中。 如需相關指示，請參閱匯 [入憑證](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754489(v=ws.11)) 文章。

然後您必須將此憑證設定為次要 AD FS 權杖簽署或解密憑證。  (您將它設定為次要憑證，以在您將其升級至主要憑證) 之前，讓同盟合作夥伴有足夠的時間取用這個新的憑證。

### <a name="to-configure-a-new-certificate-as-a-secondary-certificate"></a>將新憑證設定為次要憑證
1. 開啟 PowerShell，然後執行下列動作： `Set-ADFSProperties -AutoCertificateRollover $false`
2. 在匯入憑證後， 開啟 **AD FS 管理** 主控台。
3. 展開 [服務] 並選取 [憑證]。
4. 在 [動作] 窗格中，按一下 [ **新增 Token-Signing 憑證**]。
    ![[D F S] 對話方塊的螢幕擷取畫面，其中顯示已呼叫的 [新增權杖簽署憑證] 選項。](media/configure-TS-TD-certs-ad-fs/ts4.png)</br>
5. 在顯示的憑證清單中選取新憑證，然後按一下 [確定]。
6.  開啟 PowerShell，然後執行下列動作： `Set-ADFSProperties -AutoCertificateRollover $true`

>[!WARNING]
>請確定新的憑證有相關聯的私密金鑰，而且 AD FS 服務帳戶獲得私密金鑰的讀取權限。 在每部同盟伺服器上確認此項。 若要這樣做，請在 [憑證] 嵌入式管理單元中的新憑證上按一下滑鼠右鍵，接著再依序按一下 [所有工作] 和 [管理私密金鑰]。

當您允許您的同盟夥伴使用足夠的時間來取用新的憑證 (可以提取您的同盟中繼資料，或將新憑證的公開金鑰傳送) ，您必須將次要憑證升級為主要憑證。

### <a name="to-promote-the-new-certificate-from-secondary-to-primary"></a>將新憑證從次要提升為主要

1. 開啟 **AD FS 管理** 主控台。
2. 展開 [服務] 並選取 [憑證]。
3. 按一下次要權杖簽署憑證。
4. 按一下 [動作] 窗格中的 [設為主要]。 在確認提示中按一下 [是]。
    ![在 [動作] 窗格中顯示 [設為主要] 選項的 D F S 對話方塊螢幕擷取畫面。](media/configure-TS-TD-certs-ad-fs/ts5.png)</br>


## <a name="updating-federation-partners"></a>正在更新同盟夥伴

### <a name="partners-who-can-consume-federation-metadata"></a>可以取用同盟中繼資料的合作夥伴
如果您已更新並設定新的權杖簽署或權杖解密憑證，您必須確定您的所有同盟夥伴 (資源組織或帳戶組織夥伴 AD FS，且由信賴憑證者信任和宣告提供者信任) 已挑選新的憑證。

### <a name="partners-who-can-not-consume-federation-metadata"></a>無法使用同盟中繼資料的合作夥伴
如果您的同盟夥伴無法取用同盟中繼資料，您必須手動傳送新權杖簽署/權杖解密憑證的公開金鑰給他們。 如果您想要將整個鏈) 傳送給所有資源組織或帳戶組織) AD FS (夥伴，請將您的新憑證公開金鑰 ( .cer 檔案或....。 讓合作夥伴在其端執行變更，以信任新的憑證。

### <a name="promote-to-primary-if-autocertificaterollover-is-false"></a>如果 AutoCertificateRollover 為 False，則升級為主要 () 
如果 **AutoCertificateRollover** 設定為 **False**，AD FS 將不會自動產生或開始使用新的權杖簽署或權杖解密憑證。 您將必須手動執行這些工作。
在允許足夠的時間讓所有同盟夥伴取用新的次要憑證之後，請將此次要憑證升級至 MMC 嵌入式管理單元中的主要 (，按一下次要權杖簽署憑證，然後按一下 [動作] 窗格中的 [ **設為主要**]。 ) 

## <a name="updating-azure-ad"></a>更新 Azure AD
AD FS 藉由使用現有的 AD DS 認證來驗證使用者，提供對 Microsoft 雲端服務（例如 Office 365）的單一登入存取。  如需使用憑證的詳細資訊，請參閱 [更新 Office 365 的同盟憑證及 Azure AD](/azure/active-directory/connect/active-directory-aadconnect-o365-certs)。
