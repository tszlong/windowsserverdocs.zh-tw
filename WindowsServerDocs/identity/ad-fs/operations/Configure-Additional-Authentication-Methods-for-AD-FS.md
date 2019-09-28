---
ms.assetid: ddfbbda3-30ca-4537-af12-667efc6f63ff
title: 設定 AD FS 的其他驗證方法
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/26/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f78c60ccd65b4c9148d53d894c572a4402948806
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407684"
---
# <a name="configure-additional-authentication-methods-for-ad-fs"></a>設定 AD FS 的其他驗證方法

若要啟用多因素驗證 (MFA)，您必須選取至少一個其他驗證方法。 根據預設，在 Windows Server 2012 R2 的 Active Directory 同盟服務（AD FS）中，您可以選取憑證驗證（也就是以智慧卡為基礎的驗證）做為額外的驗證方法。

> [!NOTE]
> 如果您選取憑證驗證，請確定已經安全佈建智慧卡憑證，且具有 PIN 要求。

您知道 Microsoft Azure 在雲端中提供類似的功能嗎？ 深入了解 [Microsoft Azure 身分識別解決方案](http://aka.ms/m2w274)。<br /><br />在 Microsoft Azure 中建立混合式身分識別解決方案：<br /> - [瞭解 Azure 多重要素驗證。](http://aka.ms/ey6o9r)<br /> - [使用雲端驗證管理單一樹系混合式環境的](http://aka.ms/g1jat8)身分識別。<br /> -  透過[其他多因素驗證管理機密應用程式的風險。](http://aka.ms/kt1bbm)

## <a name="microsoft-and-third-party-additional-authentication-methods"></a>Microsoft 和協力廠商其他驗證方法
您也可以在 Windows Server 2012 R2 的 AD FS 中設定及啟用 Microsoft 和協力廠商驗證方法。 一旦安裝並向 AD FS 註冊後，您就可以在全域或每一方信賴憑證者驗證原則中強制執行 MFA。

以下按字母排序列出的 Microsoft 和協力廠商提供者，目前有 MFA 供應項目可用於 Windows Server 2012 R2 中的 AD FS。

|提供者|供應項目|更多資訊的連結|
|-|-|-| 
|aPersona|aPersona 適用于 Microsoft ADFS SSO 的彈性多重要素驗證|[aPersona ASM ADFS 介面卡](https://www.apersona.com/adfs)|
|雙核安全性|適用于 AD FS 的雙核 MFA 介面卡|[AD FS 的雙核驗證](https://duo.com/docs/adfs)|
|Futurae|AD FS 的 Futurae Authentication Suite|[Futurae 增強式驗證](https://futurae.com)|
|Gemalto|Gemalto Identity & Security Services|[http://www.gemalto.com/identity](http://www.gemalto.com/identity)|
|inWebo Technologies|inWebo Enterprise Authentication service|[inWebo 企業驗證](http://www.inwebo.com)|
|Login People|適用於 AD FS 2012 R2 的 Login People MFA API 連接器 (Public Beta)|[https://www.loginpeople.com](https://www.loginpeople.com)|
|Microsoft Corp.|Microsoft Azure MFA|[逐步解說指南：透過其他多因素驗證管理機密應用程式的風險](https://technet.microsoft.com/library/dn280946.aspx) (請參閱步驟 3)|
Mideye | ADFS 的 Mideye Authentication 提供者 | [使用 Microsoft Active Directory 同盟服務 Mideye 雙因素驗證](https://www.mideye.com/support/administrators/documentation/integration/microsoft-adfs/)|
|Okta | Active Directory 同盟服務的 Okta MFA | [Active Directory 同盟服務（ADFS）的 Okta MFA](https://help.okta.com/en/prod/Content/Topics/integrations/adfs-okta-int.htm)|
|一個身分識別| Starling 2FA AD FS|[Starling 2FA AD FS 介面卡](https://www.oneidentity.com/products/starling-two-factor-authentication/)|
|一個身分識別| Defender AD FS|[Defender AD FS 介面卡](https://www.oneidentity.com/products/defender/)|
|Ping 身分識別|PingID MFA Adapter for AD FS|[PingID MFA Adapter for AD FS](https://documentation.pingidentity.com/pingid/pingidAdminGuide/index.shtml#pid_c_PingIDforADFSSSO.html)|
|RSA, The Security Division of EMC|RSA SecurID Authentication Agent for Microsoft Active Directory Federation Services|[適用于 Microsoft Active Directory 同盟服務的 RSA SecurID 驗證代理程式](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)|
|SafeNet，Inc.|SafeNet Authentication Service (SAS) Agent for AD FS|[SafeNet Authentication Service:AD FS Agent Configuration Guide](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)|
|SecureMFA|SecureMFA OTP 提供者| [ADFS 多重要素驗證提供者](https://www.securemfa.com/)|
|Swisscom|Mobile ID Authentication Service and Signature Services|[Mobile ID 驗證服務](http://swisscom.ch/mid)|
|Symantec|Symantec Validation and ID Protection Service (VIP)|[Symantec 驗證與識別碼保護服務（VIP）](http://www.symantec.com/vip-authentication-service)|
|Trusona|基本（無密碼 MFA）和主管（重要 + 身分識別的校對）| [Trusona 多重要素驗證](https://www.trusona.com/solution-overview/)|


## <a name="custom-authentication-method-for-ad-fs-in-windows-server-2012-r2"></a>Windows Server 2012 R2 中 AD FS 的自訂驗證方法
我們現在會提供相關指示，以便針對 Windows Server 2012 R2 中的 AD FS 建立自己的自訂驗證方法。 如需詳細資訊，請參閱 [針對 Windows Server 2012 R2 中的 AD FS 建立自訂驗證方法](https://go.microsoft.com/fwlink/?LinkID=511980)。

## <a name="see-also"></a>另請參閱
[透過其他多重要素驗證管理機密應用程式的風險](Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)


