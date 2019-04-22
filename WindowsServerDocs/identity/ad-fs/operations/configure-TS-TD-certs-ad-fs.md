---
title: 取得和設定 AD FS 權杖簽署和權杖解密憑證
description: 本文件說明如何取得和設定的 TS 和 TD 適用於 AD FS 憑證
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/23/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d16a886c9fb8f88748ffe732a75f0fd6a32c3702
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820209"
---
# <a name="obtain-and-configure-ts-and-td-certificates-for-ad-fs"></a>取得和設定 AD FS TS 和 TD 憑證

本主題描述工作和程序，您可以執行以確保您的 AD FS 權杖簽署和權杖解密憑證是最新狀態。

權杖簽署憑證是標準 X509 憑證，用來安全地簽署同盟伺服器簽發的所有權杖。 權杖解密憑證是標準 X509 憑證，用來解密任何傳入的權杖。 它們也會發佈在同盟中繼資料。

如需詳細資訊，請參閱[憑證需求](../design/ad-fs-requirements.md#BKMK_1)

## <a name="determine-whether-ad-fs-renews-the-certificates-automatically"></a>判斷是否 AD FS 可自動更新憑證
根據預設，AD FS 權杖簽署和權杖解密憑證自動產生，在初始設定時和當憑證接近到期日設定。

您可以執行下列 Windows PowerShell 命令： `Get-AdfsProperties`。
  
  ![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts1.png)
  
AutoCertificateRollover 屬性描述是否設定 AD FS 權杖簽署和權杖解密憑證會自動更新。

如果 AutoCertificateRollover 設為 TRUE 時，將更新 AD FS 憑證，並自動設定 AD FS 中。 一旦設定了新的憑證，以避免服務中斷，您必須確定 （由您的 AD FS 伺服器陣列中的宣告提供者信任或信賴憑證者信任） 的每個同盟夥伴，會更新這個新的憑證。
    
如果 AD FS 不是設定為更新權杖簽署和權杖解密憑證的自動 （如果 AutoCertificateRollover 設為 False），AD FS 將不會自動產生或開始使用新的權杖簽署或權杖解密憑證。 您必須手動執行這些工作。
    
如果已使用 AD FS 權杖簽署和權杖解密憑證會自動更新 （AutoCertificateRollover 設為 TRUE），您可以判斷更新的時機：

CertificateGenerationThreshold 描述憑證的不晚於日期之前的幾天會產生新的憑證。

CertificatePromotionThreshold 決定多少天之後產生新的憑證，將會升級為主要憑證 （亦即，AD FS 將開始使用它來簽署它所發出的權杖，並將從身分識別提供者的權杖）。

![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts2.png)
  
如果已使用 AD FS 權杖簽署和權杖解密憑證會自動更新 （AutoCertificateRollover 設為 TRUE），您可以判斷更新的時機：

 - **CertificateGenerationThreshold**描述憑證的不晚於日期之前的幾天會產生新的憑證。
 - **CertificatePromotionThreshold**決定多少天之後產生新的憑證，將會升級為主要憑證 （亦即，AD FS，將開始使用它來簽署它所發出的權杖，並將權杖解密從身分識別提供者）。

## <a name="determine-when-the-current-certificates-expire"></a>判斷目前的憑證已過期時
識別主要權杖簽署和權杖解密憑證，並判斷目前的憑證已過期時，您可以使用下列程序。

您可以執行下列 Windows PowerShell 命令： `Get-AdfsCertificate –CertificateType token-signing` (或`Get-AdfsCertificate –CertificateType token-decrypting `)。 或者，您可以檢查目前的憑證，在 MMC 中：服務-> 憑證。

![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts3.png)

憑證**IsPrimary**值設定為 **，則為 True**是 AD FS 目前所使用的憑證。

所顯示的日期**不晚於**所必須設定新的主要權杖簽署或解密憑證的日期。

若要確保服務連續性，新的權杖簽署和權杖解密憑證，在此訂閱到期前的，必須使用 （由您的 AD FS 伺服器陣列中的宣告提供者信任或信賴憑證者信任） 的所有同盟夥伴。 我們建議您開始此程序至少 60 天事先規劃。

## <a name="generating-a-new-self-signed-certificate-manually-prior-to-the-end-of-the-grace-period"></a>產生新的自我簽署的憑證，在寬限期結束之前手動
若要產生新的自我簽署的憑證，以手動方式在寬限期結束之前，您可以使用下列步驟。

1. 請確定您已登入主要 AD FS 伺服器。
2. 開啟 Windows PowerShell 並執行下列命令： `Add-PSSnapin "microsoft.adfs.powershell"`
3. （選擇性） 您可以檢查 AD FS 中目前的簽署憑證。 若要這樣做，請執行下列命令： `Get-ADFSCertificate –CertificateType token-signing`。 查看命令輸出，以查看列出任何憑證的 「 不晚於 」 日期。
4. 若要產生新的憑證，請執行下列命令以更新 AD FS 伺服器上的憑證： `Update-ADFSCertificate –CertificateType token-signing`。
5. 再次執行下列命令，以驗證更新： `Get-ADFSCertificate –CertificateType token-signing`
6. 現在應該會列出兩個憑證，其中有**不晚**日期大約一年在未來，並為其**IsPrimary**值是**False**。

>[!IMPORTANT]
>若要避免服務中斷，更新的憑證資訊在 Azure AD 中如何執行這些步驟，以更新 Azure AD 的有效權杖簽署憑證。

## <a name="if-youre-not-using-self-signed-certificates"></a>如果您不使用自我簽署的憑證...
如果您不使用預設的自動產生、 自我簽署的權杖簽署和權杖解密憑證，您必須更新，並以手動方式設定這些憑證。

首先，您必須從您的憑證授權單位取得新的憑證和它匯入到本機電腦個人憑證存放區，每個同盟伺服器上。 如需相關指示，請參閱 <<c0> [ 匯入憑證](https://technet.microsoft.com/library/cc754489.aspx)文章。

然後，您必須設定此做為第二個 AD FS 權杖簽署憑證或解密憑證。 （您將它設定為次要憑證，以允許您的同盟夥伴足夠的時間來取用這個新的憑證，才能將它升級為主要憑證）。

### <a name="to-configure-a-new-certificate-as-a-secondary-certificate"></a>若要設定新的憑證做為次要憑證
1. 開啟 PowerShell 並執行下列命令： `Set-ADFSProperties -AutoCertificateRollover $false`
2. 一旦您已匯入憑證。 開啟**AD FS 管理**主控台。
3. 依序展開**服務**，然後選取**憑證**。
4. 在 [動作] 窗格中，按一下**新增權杖簽署憑證**。
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts4.png)</br>
5. 從顯示的憑證清單中選取新的憑證，然後按一下 [確定]。
6.  開啟 PowerShell 並執行下列命令： `Set-ADFSProperties -AutoCertificateRollover $true`

>[!WARNING]
>確認新的憑證具有與其相關聯的私密金鑰和 AD FS 服務帳戶會授與讀取權限的私密金鑰。 確認在每部同盟伺服器上。 若要這樣做，在 [憑證] 嵌入式管理單元中，以滑鼠右鍵按一下新的憑證，按一下 [所有工作]，然後都按一下管理私用金鑰。

一旦您已有足夠的時間，為您的同盟夥伴，以使用新的憑證 （將會提取您的同盟中繼資料或傳送您的新憑證的公開金鑰），您就必須升級主要憑證的次要憑證。

### <a name="to-promote-the-new-certificate-from-secondary-to-primary"></a>若要將新憑證從次要到主要升階

1. 開啟**AD FS 管理**主控台。
2. 依序展開**服務**，然後選取**憑證**。
3. 按一下次要權杖簽署憑證。
4. 在 **動作**窗格中，按一下**設為主要**。 在確認提示按一下 [是]。
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts5.png)</br>


## <a name="updating-federation-partners"></a>更新同盟夥伴

### <a name="partners-who-can-consume-federation-metadata"></a>合作夥伴可以使用同盟中繼資料
如果您有更新，並設定新的權杖簽署、 權杖解密憑證，您必須確定所有同盟合作夥伴 (資源組織或帳戶組織中，用您的 AD FS 中表示的信賴憑證者夥伴合作對象的信任和宣告提供者信任） 有挑選新的憑證。

### <a name="partners-who-can-not-consume-federation-metadata"></a>合作夥伴不可以使用同盟中繼資料
如果您的同盟夥伴無法使用您的同盟中繼資料，您必須以手動方式傳送其新權杖簽署 / 權杖解密憑證的公開金鑰。 將新憑證的公開金鑰 （.cer 檔案或如果您想要包含的整個鏈結的.p7b） 傳送至所有資源組織或帳戶 （由 AD fs 信賴憑證者信任和宣告提供者信任） 的組織夥伴。 已實作變更，在其端信任的新憑證的協力廠商。

### <a name="promote-to-primary-if-autocertificaterollover-is-false"></a>升階成主要 （如果 AutoCertificateRollover 為 False）
如果**AutoCertificateRollover**設為**False**，不會自動產生 AD FS 或開始使用新的權杖簽署或權杖解密憑證。 您必須手動執行這些工作。
所有您使用新的次要憑證的同盟夥伴，允許一段足夠的時間之後, 升階為主要 （在 MMC 嵌入式管理單元中，按一下次要權杖簽署憑證，並在 動作 窗格中，按一下 這個次要憑證**將設定為主要**。)

## <a name="updating-azure-ad"></a>更新 Azure AD
AD FS 驗證使用者透過其現有的 AD DS 認證提供 Office 365 等 Microsoft 雲端服務的單一登入存取。  如需有關使用憑證的詳細資訊，請參閱[更新 Office 365 和 Azure AD 的同盟憑證](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs)。