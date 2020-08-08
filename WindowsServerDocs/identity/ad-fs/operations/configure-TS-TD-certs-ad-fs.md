---
title: 取得及設定 AD FS 的權杖簽署和權杖解密憑證
description: 本檔說明如何取得及設定 AD FS 的 TS 和 TD 憑證
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/23/2017
ms.topic: article
ms.openlocfilehash: a96b256fbd2f1a5ce3db71bd11de8715eccf60e9
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87966905"
---
# <a name="obtain-and-configure-ts-and-td-certificates-for-ad-fs"></a>取得並設定 AD FS 的 TS 和 TD 憑證

本主題說明您可以執行的工作和程式，以確保您的 AD FS 權杖簽署和權杖解密憑證是最新的。

權杖簽署憑證是標準 X509 憑證，用來安全地簽署同盟伺服器簽發的所有權杖。 權杖解密憑證是標準 X509 憑證，用來解密任何傳入權杖。 它們也會在同盟中繼資料中發行。

如需其他資訊，請參閱[憑證需求](../design/ad-fs-requirements.md#BKMK_1)

## <a name="determine-whether-ad-fs-renews-the-certificates-automatically"></a>判斷 AD FS 是否自動更新憑證
依預設，會將 AD FS 設定為在初始設定時和當憑證接近其到期日時自動產生權杖簽署和權杖解密憑證。

您可以執行下列 Windows PowerShell 命令： `Get-AdfsProperties` 。

  ![Set-adfsproperties](media/configure-TS-TD-certs-ad-fs/ts1.png)

AutoCertificateRollover 屬性描述 AD FS 是否設定為自動更新權杖簽署和權杖解密憑證。

如果 AutoCertificateRollover 設定為 TRUE，則會自動更新並在 AD FS 中設定 AD FS 憑證。 一旦設定新憑證，為了避免中斷，您必須確定每個同盟夥伴 (由信賴憑證者信任或宣告提供者信任在 AD FS 伺服器陣列中表示，) 以這個新憑證更新。

如果 AD FS 未設定為自動更新權杖簽署和權杖解密憑證 (如果 AutoCertificateRollover 設定為 False) ，AD FS 將不會自動產生或開始使用新的權杖簽署或權杖解密憑證。 您將必須手動執行這些工作。

如果 AD FS 設定為自動更新權杖簽署和權杖解密憑證， (AutoCertificateRollover 設為 TRUE) 時，您可以判斷更新的時間：

CertificateGenerationThreshold 描述憑證在到期前的多少天，將會產生新的憑證。

CertificatePromotionThreshold 會決定產生新憑證之後，要將其升級為主要憑證的天數 (換言之，AD FS 會開始使用它來簽署權杖，並將權杖從識別提供者) 中解密。

![Set-adfsproperties](media/configure-TS-TD-certs-ad-fs/ts2.png)

如果 AD FS 設定為自動更新權杖簽署和權杖解密憑證， (AutoCertificateRollover 設為 TRUE) 時，您可以判斷更新的時間：

 - **CertificateGenerationThreshold**描述憑證在到期前的多少天，將會產生新的憑證。
 - **CertificatePromotionThreshold**會決定產生新憑證之後，要將其升級為主要憑證的天數 (換言之，AD FS 會開始使用它來簽署權杖，並將權杖從識別提供者) 中解密。

## <a name="determine-when-the-current-certificates-expire"></a>判斷目前的憑證何時過期
您可以使用下列程式來識別主要權杖簽署和權杖解密憑證，以及判斷目前的憑證何時到期。

您可以執行下列 Windows PowerShell 命令： `Get-AdfsCertificate –CertificateType token-signing` (或 `Get-AdfsCertificate –CertificateType token-decrypting `) 。 或者，您可以檢查 MMC：服務 >憑證中的目前憑證。

![Get-adfscertificate](media/configure-TS-TD-certs-ad-fs/ts3.png)

**IsPrimary**值設定為**True**的憑證就是 AD FS 目前使用的憑證。

[**不晚**于] 顯示的日期是必須設定新主要權杖簽署或解密憑證的日期。

為確保服務持續性，所有同盟夥伴 (由信賴憑證者信任或宣告提供者信任在您的 AD FS 伺服器陣列中表示，) 必須在此到期前使用新的權杖簽署和權杖解密憑證。 我們建議您事先開始規劃此程式至少60天。

## <a name="generating-a-new-self-signed-certificate-manually-prior-to-the-end-of-the-grace-period"></a>在寬限期結束之前手動產生新的自我簽署憑證
您可以使用下列步驟，在寬限期結束之前手動產生新的自我簽署憑證。

1. 請確定您已登入主要 AD FS 伺服器。
2. 開啟 Windows PowerShell 並執行下列命令：`Add-PSSnapin "microsoft.adfs.powershell"`
3. （選擇性）您可以在 AD FS 中檢查目前的簽署憑證。 若要這樣做，請執行以下命令：`Get-ADFSCertificate –CertificateType token-signing`。 查看命令輸出，以查看所列之任何憑證的「不在」之後的日期。
4. 若要產生新的憑證，請執行下列命令來更新並更新 AD FS 伺服器上的憑證： `Update-ADFSCertificate –CertificateType token-signing` 。
5. 再次執行下列命令以驗證更新：`Get-ADFSCertificate –CertificateType token-signing`
6. 現在應該會列出兩個憑證，其中一個在未來**不**會有約一年的日期，而**IsPrimary**值為**False**。

>[!IMPORTANT]
>若要避免服務中斷，請執行如何使用有效的權杖簽署憑證來更新 Azure AD 中的步驟，以更新 Azure AD 上的憑證資訊。

## <a name="if-youre-not-using-self-signed-certificates"></a>如果您不是使用自我簽署憑證 .。。
如果您未使用預設自動產生的自我簽署權杖簽署和權杖解密憑證，您必須手動更新並設定這些憑證。

首先，您必須從您的憑證授權單位單位取得新的憑證，並將它匯入每部同盟伺服器上的本機電腦個人憑證存儲。 如需指示，請參閱匯[入憑證](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754489(v=ws.11))一文。

接著，您必須將此憑證設定為次要 AD FS 權杖簽署或解密憑證。  (您將其設定為次要憑證，以允許您的同盟合作夥伴在將此新憑證升階為主要憑證) 之前，有足夠的時間取用此新憑證。

### <a name="to-configure-a-new-certificate-as-a-secondary-certificate"></a>將新憑證設定為次要憑證
1. 開啟 PowerShell 並執行下列動作：`Set-ADFSProperties -AutoCertificateRollover $false`
2. 在匯入憑證後， 開啟 [ **AD FS 管理**主控台]。
3. 展開 [服務]**** 並選取 [憑證]****。
4. 在 [動作] 窗格中，按一下 [**新增權杖簽署憑證**]。
![Get-adfscertificate](media/configure-TS-TD-certs-ad-fs/ts4.png)</br>
5. 在顯示的憑證清單中選取新憑證，然後按一下 [確定]。
6.  開啟 PowerShell 並執行下列動作：`Set-ADFSProperties -AutoCertificateRollover $true`

>[!WARNING]
>請確定新憑證具有相關聯的私密金鑰，而且 AD FS 服務帳戶已被授與私密金鑰的讀取權限。 請在每部同盟伺服器上確認此項。 若要這樣做，請在 [憑證] 嵌入式管理單元中的新憑證上按一下滑鼠右鍵，接著再依序按一下 [所有工作] 和 [管理私密金鑰]。

一旦您已有足夠的時間讓同盟夥伴取用新憑證 (他們會提取您的同盟中繼資料，或將新憑證) 的公開金鑰傳送給它們，您必須將次要憑證升級為主要憑證。

### <a name="to-promote-the-new-certificate-from-secondary-to-primary"></a>將新憑證從次要提升為主要

1. 開啟 [ **AD FS 管理**主控台]。
2. 展開 [服務]**** 並選取 [憑證]****。
3. 按一下次要權杖簽署憑證。
4. 按一下 [動作]**** 窗格中的 [設為主要]****。 在確認提示中按一下 [是]。
![Get-adfscertificate](media/configure-TS-TD-certs-ad-fs/ts5.png)</br>


## <a name="updating-federation-partners"></a>正在更新同盟夥伴

### <a name="partners-who-can-consume-federation-metadata"></a>可以使用同盟中繼資料的合作夥伴
如果您已更新並設定新的權杖簽署或權杖解密憑證，則必須確定您的所有同盟夥伴 (資源組織或帳戶組織合作夥伴，其由信賴憑證者信任和宣告提供者信任所 AD FS，) 已挑選新的憑證。

### <a name="partners-who-can-not-consume-federation-metadata"></a>無法使用同盟中繼資料的合作夥伴
如果您的同盟夥伴無法使用您的同盟中繼資料，您必須手動將新權杖簽署/權杖解密憑證的公開金鑰傳送給它們。 如果您想要將整個鏈) 包含在 AD FS 中，由信賴憑證者信任和宣告提供者信任) 所表示的所有資源組織或帳戶組織 (合作夥伴，請將您的新憑證公用金鑰傳送 ( .cer 檔案或.. p7b。 讓合作夥伴在其端執行變更，以信任新的憑證。

### <a name="promote-to-primary-if-autocertificaterollover-is-false"></a>如果 AutoCertificateRollover 為 False，則升階為主要 () 
如果**AutoCertificateRollover**設定為**False**，AD FS 將不會自動產生或開始使用新的權杖簽署或權杖解密憑證。 您將必須手動執行這些工作。
在有足夠的時間讓所有同盟夥伴取用新的次要憑證之後，請將此次要憑證升級至 MMC 嵌入式管理單元中的主要 (，按一下次要權杖簽署憑證，然後在 [動作] 窗格中，按一下 [**設為主要**]。 ) 

## <a name="updating-azure-ad"></a>正在更新 Azure AD
AD FS 透過其現有的 AD DS 認證來驗證使用者，以提供 Office 365 這類 Microsoft 雲端服務的單一登入存取。  如需使用憑證的詳細資訊[，請參閱更新 Office 365 和 Azure AD 的同盟憑證](/azure/active-directory/connect/active-directory-aadconnect-o365-certs)。
