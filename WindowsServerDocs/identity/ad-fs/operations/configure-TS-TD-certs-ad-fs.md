---
title: "取得並設定 AD FS 權杖登入和權杖解密憑證"
description: "本文件告訴您如何取得和設定的 TS 和 t D AD FS 的憑證"
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/23/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0d7f8ba4efb74a377f9f9d748949a934398ebefc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="obtain-and-configure-ts-and-td-certificates-for-ad-fs"></a>取得並設定 AD FS TS 和 t d 憑證

本主題描述工作和程序可以確保您 AD FS 權杖登入和權杖解密憑證最新狀態。

權杖簽署的憑證會標準 X509 安全地登入聯盟伺服器問題的所有權杖所使用的憑證。 權杖解密憑證的標準 X509 用來解密任何連入權杖的憑證。 它們也被發行聯盟中繼資料中。

如需詳細資訊請查看[認證需求](../design/ad-fs-requirements.md#BKMK_1)

## <a name="determine-whether-ad-fs-renews-the-certificates-automatically"></a>判斷 AD FS 是否會自動續約憑證
根據預設，AD FS 設定為產生權杖登入和權杖解密憑證會自動在初始設定時間和時憑證即將他們的到期日期。

您可以執行下列 Windows PowerShell 命令：`Get-AdfsProperties`。
  
  ![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts1.png)
  
[AutoCertificateRollover 屬性描述 AD FS 是否已設定為權杖登入和權杖解密憑證會自動續約。

如果 AutoCertificateRollover 設定為 TRUE，將續約 AD FS 憑證，並自動設定 AD FS。 一旦設定新的憑證，以避免服務中斷，您必須確定（由 AD FS 陣列中信賴廠商信任或宣告提供者信任）每個聯盟協力廠商更新的這個新的憑證。
    
如果 AD FS 不是設定為約權杖登入並權杖解密憑證會自動（如果 AutoCertificateRollover 設定為），AD FS 不會自動將產生或開始使用新權杖登入或預付碼解密的憑證。 您必須手動執行這些工作。
    
如果 AD FS 設定為權杖登入和權杖解密憑證會自動續約（AutoCertificateRollover 設定為 TRUE，）更新時，您可以判斷：

CertificateGenerationThreshold 描述幾天未之後日期憑證的之前將產生新的憑證。

CertificatePromotionThreshold 判斷幾天後新的憑證也它將會升級為主要憑證（亦即，AD FS 將會開始使用它來登入該問題的權杖和解密權杖身分提供者）。

![Get-ADFSProperties](media/configure-TS-TD-certs-ad-fs/ts2.png)
  
如果 AD FS 設定為權杖登入和權杖解密憑證會自動續約（AutoCertificateRollover 設定為 TRUE，）更新時，您可以判斷：

 - **CertificateGenerationThreshold**描述幾天未之後日期憑證的之前將產生新的憑證。
 - **CertificatePromotionThreshold**判斷幾天後新的憑證也它將會升級為主要憑證（亦即，AD FS 將會開始使用它來登入該問題的權杖和解密權杖的身分提供者）。

## <a name="determine-when-the-current-certificates-expire"></a>判斷最憑證何時到期
找出主要 token 登入和權杖解密憑證，並判斷目前憑證的到期時，您可以使用下列程序。

您可以執行下列 Windows PowerShell 命令：`Get-AdfsCertificate –CertificateType token-signing` (或`Get-AdfsCertificate –CertificateType token-decrypting `)。 您可以檢查 MMC 中的目前憑證或者：服務]-> [的憑證。

![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts3.png)

憑證的**IsPrimary**值設定為**，則為 True**是目前正在使用 AD FS 使用的憑證。

顯示日期**未之後**是新的主要 token 登入或解密憑證必須設定的日期。

若要確保服務連續性，所有聯盟協力廠商（由 AD FS 陣列中信賴廠商信任或宣告提供者信任）必須都使用的新權杖登入和權杖解密此到期日之前的憑證。 我們建議您開始至少 60 天前規劃此程序。

## <a name="generating-a-new-self-signed-certificate-manually-prior-to-the-end-of-the-grace-period"></a>產生手動之前優惠期間結束新自動簽署的憑證
您可以使用下列步驟來產生手動之前優惠期間結束新自動簽署的憑證。

1. 請確定您的登入主要 AD FS 伺服器。
2. 打開 Windows PowerShell，執行下列命令： `Add-PSSnapin "microsoft.adfs.powershell"`
3. 或者，您可以檢查 AD FS 中目前專屬的簽署憑證。 若要這樣做，請執行下列命令：`Get-ADFSCertificate –CertificateType token-signing`。 若要查看的任何列出的憑證不之後日期命令輸出查看。
4. 若要產生新的憑證，執行下列命令來更新和更新 AD FS 伺服器上的憑證：`Update-ADFSCertificate –CertificateType token-signing`。
5. 執行下列命令，再試一次確認更新： `Get-ADFSCertificate –CertificateType token-signing`
6. 現在應該會列出兩個憑證，其中有**未之後**日期的大約一年未來和的**IsPrimary**價值，是**\ [false\]**。

>[!IMPORTANT]
>為避免服務中斷，請更新 Azure AD 憑證的詳細資訊，如何步驟執行來更新 Azure AD 的有效權杖簽署的憑證。

## <a name="if-youre-not-using-self-signed-certificates"></a>如果您無法使用自動簽署的憑證...
如果您未使用的預設會自動產生、自我權杖登入及權杖解密憑證，您必須更新，並手動設定這些憑證。

首先，您必須從您的憑證授權單位取得新的憑證，並匯入到本機個人憑證存放區每個聯盟伺服器上。 適用於的指示，請查看[憑證匯入](https://technet.microsoft.com/library/cc754489.aspx)文章。

然後，您必須設定此作為次要 AD FS 權杖簽署的憑證或解密憑證。 （您設定為次要憑證以允許聯盟協力廠商不足，無法使用此新的憑證之前您將它升級為主要的憑證的時間）。

### <a name="to-configure-a-new-certificate-as-a-secondary-certificate"></a>若要設定新的憑證為次要憑證
1. 打開 PowerShell，並執行下列動作： `Set-ADFSProperties -AutoCertificateRollover $false`
2. 一次憑證已匯入。 開放**AD FS 管理**主機。
3. 展開**服務**，然後選取 [**的憑證**。
4. 在 [動作] 窗格中，按一下 [**的憑證加入權杖登入**。
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts4.png)</br>
5. 從清單中顯示的憑證，選取新的憑證，然後按一下 [確定]。
6.  打開 PowerShell，並執行下列動作： `Set-ADFSProperties -AutoCertificateRollover $true`

>[!WARNING]
>確認新的憑證已私密金鑰關聯，並 AD FS 服務 account 授與朗讀私密金鑰權限。 請確認此每個聯盟伺服器上。 若要這樣做，您可以在憑證嵌入式管理單元，以滑鼠右鍵按一下新的憑證，按一下 [所有工作，然後都按一下私密金鑰管理。

一旦您已允許不足，無法使用您新的憑證（他們拉聯盟中繼資料或傳送的新憑證）聯盟協力廠商的時間，您必須將升級次要憑證主要憑證。

### <a name="to-promote-the-new-certificate-from-secondary-to-primary"></a>若要將新的憑證升級次要改為主要的

1. 開放**AD FS 管理**主機。
2. 展開**服務**，然後選取 [**的憑證**。
3. 按一下 [登入憑證次要預付碼。
4. 在**動作**窗格中，按**設定為主要**。 確認的提示時，請按一下 [是]。
![Get-ADFSCertificate](media/configure-TS-TD-certs-ad-fs/ts5.png)</br>


## <a name="updating-federation-partners"></a>更新聯盟合作夥伴

### <a name="partners-who-can-consume-federation-metadata"></a>合作夥伴可以使用聯盟中繼資料
如果您有續約，並設定的新權杖登入或權杖解密憑證，請務必的所有聯盟協力廠商 (資源組織或 account 組織合作夥伴在您 AD FS 由信賴廠商信任和宣告提供者信任）細小的金屬新的憑證。

### <a name="partners-who-can-not-consume-federation-metadata"></a>合作夥伴使用者無法使用聯盟中繼資料
如果聯盟協力廠商無法使用聯盟中繼資料，您必須以手動方式傳送給他們的新權杖簽署 / 權杖解密憑證。 將您新的憑證公開金鑰傳送 (.cer 檔案或如果您想要包含整個鏈結.p7b) 資源組織或 account 組織合作夥伴的所有 (在您 AD FS 由信賴廠商信任並宣告提供者信任）。 必須實作變更其一邊信任的新的憑證合作夥伴。

### <a name="promote-to-primary-if-autocertificaterollover-is-false"></a>促銷主要（如果 AutoCertificateRollover 是 False）
如果**AutoCertificateRollover**設為**\ [false\]**、AD FS 不會自動創造或 [開始] 畫面使用全新權杖登入或權杖解密的憑證。 您必須手動執行這些工作。
使用新的第二個憑證聯盟協力廠商提供允許不足一段時間之後, 促銷此次要憑證主要（MMC 嵌入式管理單元，按一下 [次要權杖簽署憑證動作] 窗格中按一下 [**設定為主要**。)

## <a name="updating-azure-ad"></a>Azure AD 的更新
AD FS 藉由驗證使用者透過他們的現有 AD DS 認證提供至 Microsoft 雲端服務，例如 Office 365 的單一登入的存取。  如需使用憑證的詳細資訊請查看[更新聯盟憑證 Office 365，以及 Azure AD 的](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-o365-certs)。