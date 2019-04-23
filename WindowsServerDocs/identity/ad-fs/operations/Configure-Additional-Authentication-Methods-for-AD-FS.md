---
ms.assetid: ddfbbda3-30ca-4537-af12-667efc6f63ff
title: Configure Additional Authentication Methods for AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/04/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 73de8908677b3f74651b10c29ef2abe62e484694
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868409"
---
# <a name="configure-additional-authentication-methods-for-ad-fs"></a>Configure Additional Authentication Methods for AD FS

>適用於：Windows Server 2016, Windows Server 2012 R2

若要啟用多因素驗證 (MFA)，您必須選取至少一個其他驗證方法。 根據預設，在 Windows Server 2012 r2 中的 Active Directory Federation Services (AD FS) 您可以選取憑證驗證 （亦即，智慧卡為基礎的驗證） 作為其他驗證方法。

> [!NOTE]
> 如果您選取憑證驗證，請確定已經安全佈建智慧卡憑證，且具有 PIN 要求。

您知道 Microsoft Azure 在雲端中提供類似的功能嗎？ 深入了解 [Microsoft Azure 身分識別解決方案](http://aka.ms/m2w274)。<br /><br />在 Microsoft Azure 中建立混合式身分識別解決方案：<br /> - [深入了解 Azure Multi-factor Authentication。](http://aka.ms/ey6o9r)<br /> - [管理單一樹系混合式環境，使用雲端驗證的身分識別。](http://aka.ms/g1jat8)<br /> - [透過機密應用程式的其他多因素驗證管理風險。](http://aka.ms/kt1bbm)

## <a name="microsoft-and-third-party-additional-authentication-methods"></a>Microsoft 和協力廠商其他驗證方法
您也可以設定及啟用 Microsoft 和 Windows Server 2012 R2 中的 AD FS 中的第三方驗證方法。 一旦安裝並註冊與 AD FS，您可以強制執行 MFA 作為全域或每一位信賴的驗證原則的一部分。

以下按字母排序列出的 Microsoft 和協力廠商提供者，目前有 MFA 供應項目可用於 Windows Server 2012 R2 中的 AD FS。

|提供者|供應項目|更多資訊的連結|
|-|-|-| 
|aPersona|aPersona Microsoft ADFS sso 的自適性 Multi-factor Authentication|[aPersona ASM ADFS 配接器](https://www.apersona.com/adfs)|
|Duo 安全性|適用於 AD FS duo MFA 配接器|[適用於 AD FS duo 驗證](https://duo.com/docs/adfs)|
|Gemalto|Gemalto Identity & Security Services|[http://www.gemalto.com/identity](http://www.gemalto.com/identity)|
|inWebo Technologies|inWebo Enterprise Authentication service|[inWebo 企業驗證](http://www.inwebo.com)|
|Login People|適用於 AD FS 2012 R2 的 Login People MFA API 連接器 (Public Beta)|[https://www.loginpeople.com](https://www.loginpeople.com)|
|Microsoft Corp.|Microsoft Azure MFA|[逐步解說指南：透過其他多因素驗證管理機密應用程式的風險](https://technet.microsoft.com/library/dn280946.aspx) (請參閱步驟 3)|
Mideye | Adfs 的 mideye 驗證提供者 | [使用 Microsoft Active Directory Federation Service 的 mideye 雙因素驗證](https://www.mideye.com/support/administrators/documentation/integration/microsoft-adfs/)|
|Okta | Active Directory Federation services Okta MFA | [Okta MFA Active Directory federation Services (ADFS)](https://help.okta.com/en/prod/Content/Topics/integrations/adfs-okta-int.htm)|
|一個身分識別| Starling 2FA AD FS|[Starling 2FA AD FS 配接器](https://www.oneidentity.com/products/starling-two-factor-authentication/)|
|一個身分識別| Defender AD FS|[Defender AD FS 配接器](https://www.oneidentity.com/products/defender/)|
|Ping 身分識別|適用於 AD FS PingID MFA 配接器|[適用於 AD FS PingID MFA 配接器](https://documentation.pingidentity.com/pingid/pingidAdminGuide/index.shtml#pid_c_PingIDforADFSSSO.html)|
|RSA, The Security Division of EMC|RSA SecurID Authentication Agent for Microsoft Active Directory Federation Services|[RSA SecurID 驗證代理程式，Microsoft Active Directory Federation services](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)|
|SafeNet，Inc.|SafeNet Authentication Service (SAS) Agent for AD FS|[SafeNet Authentication Service:AD FS Agent Configuration Guide](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)|
|SecureMFA|SecureMFA OTP 提供者| [ADFS 多因素驗證提供者](https://www.securemfa.com/)|
|Swisscom|Mobile ID Authentication Service and Signature Services|[行動裝置的識別碼驗證服務](http://swisscom.ch/mid)|
|Symantec|Symantec Validation and ID Protection Service (VIP)|[Symantec 驗證和識別碼保護服務 (VIP)](http://www.symantec.com/vip-authentication-service)|
|Trusona|基本 (無密碼的 MFA) 和高階主管 （基本 + 身分識別校訂）| [Trusona 多重要素驗證](https://www.trusona.com/solution-overview/)|


## <a name="custom-authentication-method-for-ad-fs-in-windows-server-2012-r2"></a>Windows Server 2012 R2 中 AD FS 的自訂驗證方法
我們現在會提供相關指示，以便針對 Windows Server 2012 R2 中的 AD FS 建立自己的自訂驗證方法。 如需詳細資訊，請參閱 [針對 Windows Server 2012 R2 中的 AD FS 建立自訂驗證方法](https://go.microsoft.com/fwlink/?LinkID=511980)。

## <a name="see-also"></a>另請參閱
[透過機密應用程式的其他多因素驗證管理風險](Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)


