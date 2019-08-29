---
ms.assetid: eefcc989-8763-45ee-8a64-3a97b4397160
title: AD FS 操作
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 594e1605f44dad69ab7eee8b22e6a620ade02ad0
ms.sourcegitcommit: c307886e96622e9595700c94128103b84f5722ce
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2019
ms.locfileid: "70108736"
---
# <a name="ad-fs-operations"></a>AD FS 操作



本檔包含 AD FS 的所有檔作業的清單。 

## <a name="service-configuration"></a>服務設定
- [更新 AD FS 和 WAP 2016 中的 SSL 憑證](../ad-fs/operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)
- [AD FS 快速還原工具](../ad-fs/operations/AD-FS-Rapid-Restore-Tool.md)
- [在 AD FS 中設定憑證驗證的替代主機名稱系結](../ad-fs/operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)
- [新增屬性存放區](../ad-fs/operations/Add-an-Attribute-Store.md)
- [使用 AD FS 2019 自訂 HTTP 安全性回應標頭](../ad-fs/operations/customize-http-security-headers-ad-fs.md)
- [將 AD FS Powershell Cmdlet 存取權委派給非系統管理員使用者](../ad-fs/operations/delegate-ad-fs-pshell-access.md)
- [微調 SQL 並解決延遲](../ad-fs/operations/adfs-sql-latency.md) 


## <a name="authentication-configuration"></a>驗證設定
### <a name="strong-authentication-mfa--password-less"></a>增強式驗證 (MFA) & 不限密碼
- [在 AD FS 中將外部驗證提供者設定為主要 (2019 或更新版本)](../ad-fs/operations/Additional-Authentication-Methods-AD-FS.md)
- [設定 AD FS (2016 或更新版本) 和 Azure MFA](../ad-fs/operations/Configure-AD-FS-2016-and-Azure-MFA.md)
- [設定 AD FS 的其他驗證方法](../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)

### <a name="lockout-protection"></a>鎖定保護
- [設定 AD FS 外部網路軟鎖定保護](../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md)
- [設定 AD FS 外部網路智慧鎖定保護](../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md)
- [設定 AD FS 外部網路禁用 IP](../ad-fs/operations/Configure-AD-FS-Banned-IP.md)

### <a name="policy-configuration"></a>原則設定
- [使用驗證原則](../ad-fs/operations/Configure-Authentication-Policies.md)
- [設定替代登入識別碼](../ad-fs/operations/Configuring-Alternate-Login-ID.md)
- [設定 Azure AD 提示登入行為以搭配 AD FS 原則使用](../ad-fs/operations/AD-FS-Prompt-Login.md)

### <a name="kerberos--certificate-authentication"></a>Kerberos & 憑證驗證
- [在 AD FS 中啟用 AD DS 宣告 & kerberos 複合驗證](../ad-fs/operations/AD-FS-Compound-Authentication-and-AD-DS-claims.md)
- [設定使用者憑證驗證的 AD FS](../ad-fs/operations/Configure-User-Certificate-Authentication.md)
- [在 AD FS 中設定憑證驗證的替代主機名稱系結](../ad-fs/operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)


### <a name="device"></a>裝置
- [AD FS 中的裝置驗證控制項](../ad-fs/operations/device-authentication-controls-in-AD-FS.md) 


## <a name="authorization-configuration"></a>授權設定
- [在 AD FS 中設定存取控制原則](../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)
- [設定裝置型條件式存取內部部署](../ad-fs/operations/Configure-Device-based-Conditional-Access-on-Premises.md)

## <a name="rpt--cpt-configuration"></a>RPT & CPT 設定
- [設定 AD FS 驗證 LDAP 目錄中儲存的使用者](../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md)
- [設定宣告規則](../ad-fs/operations/Configure-Claim-Rules.md) 
- [建立宣告提供者信任](../ad-fs/operations/Create-a-Claims-Provider-Trust.md) 
- [建立一個非宣告感知信賴憑證者信任](../ad-fs/operations/Create-a-Non-Claims-Aware-Relying-Party-Trust.md)
- [建立信賴憑證者信任](../ad-fs/operations/Create-a-Relying-Party-Trust.md)
- [設定 AD FS 以使用匯總的同盟提供者 (例如 InCommon)](../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md)

## <a name="sign-in-experience-configuration"></a>登入體驗設定
- [設定 AD FS 2016 單一登入設定](../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)
- [設定 AD FS 編頁登入](../ad-fs/operations/AD-FS-paginated-sign-in.md)
- [設定 AD FS 使用者登入自訂](../ad-fs/operations/AD-FS-user-sign-in-customization.md)
- [設定 AD FS 傳送密碼到期宣告](../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md)
- [為不支援 WIA 的裝置設定內部網路表單型驗證](../ad-fs/operations/Configure-intranet-forms-based-authentication-for-devices-that-do-not-support-WIA.md)

## <a name="other"></a>其他
- [從任何裝置加入工作地點網路，並在公司的各個應用程式提供 SSO 和無縫式的次要因素驗證](../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
- [透過其他多重要素驗證管理機密應用程式的風險](../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
- [使用條件式存取控制管理風險](../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)
- [設定 AD FS 實驗室環境](../ad-fs/operations/Set-up-an-AD-FS-lab-environment.md)
- [逐步解說指南：透過其他多因素驗證管理機密應用程式的風險](../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
- [逐步解說指南：使用條件式存取控制管理風險](../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)
- [逐步解說：將 Windows 裝置加入工作地點網路](../ad-fs/operations/Walkthrough--Workplace-Join-with-a-Windows-Device.md)
- [逐步解說：將 iOS 裝置加入工作地點網路](../ad-fs/operations/Walkthrough--Workplace-Join-with-an-iOS-Device.md)

  


