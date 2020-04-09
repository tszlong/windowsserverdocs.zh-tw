---
title: Windows Server 的最佳支援解決方案
description: 取得 Windows Server 問題解決方案的連結
ms.prod: windows-server
manager: alant
ms.technology: server-general
ms.date: 03/16/2018
ms.topic: article
author: kaushika-msft
ms.author: elizapo
ms.openlocfilehash: 6bd0d22c7df7344e6c4bfbf8360532ab0f36d117
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820751"
---
# <a name="top-support-solutions-for-windows-server-2016"></a>Windows Server 2016 的最佳支援解決方案

Microsoft 會定期發行 Windows Server 的更新及解決方案。 為了確保您的伺服器可以收到日後的更新 (包括安全性更新)，請務必持續更新這些伺服器。 如需已發行更新的完整清單，請查看 [Windows 10 及 Windows Server 2016 更新記錄](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history)。

這些是使用 Windows Server 2016 時所發生最常見問題的最佳 Microsoft 支援服務解決方案。 下列連結包括 KB 文件、更新及文件庫文章的連結。

>[!TIP]
> 尋找舊版 Windows Server 的相關資訊嗎？ 查看我們其他位於 docs.microsoft.com 的 [Windows Server 文件庫](/previous-versions/windows/)。 您也可以[搜尋這個網站](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions)以取得特定資訊。

## <a name="solutions-for-installing-or-upgrading-windows-server"></a>適用於安裝或升級 Windows Server 的解決方案

- [解決 Windows 10 升級錯誤： IT 專業人員的技術資訊](https://docs.microsoft.com/windows/deployment/upgrade/resolve-windows-10-upgrade-errors)
- [適用于 Windows 10 1607 版和 Windows Server 2016 的服務堆疊更新：2017年8月8日](https://support.microsoft.com/help/4035631)
- [升級至 Windows 10 1607 版和 Windows Server 2016 的相容性更新：2017年8月3日](https://support.microsoft.com/help/4033524)
- [以 Windows 為基礎的 Azure Vm 不支援就地系統升級](https://support.microsoft.com/help/4014997)
- [Windows Server 2016 的升級和轉換選項](../get-started/supported-upgrade-paths.md)
- [Windows Server 2016 的伺服器角色升級和遷移矩陣](../get-started/server-role-upgradeability-table.md)
- [Windows Server 安裝和升級](../get-started/installation-and-upgrade.md)
- [版本資訊： Windows Server 2016 中的重要問題](../get-started/windows-server-2016-ga-release-notes.md)
- [移至 Windows Server 2016 的建議](../get-started/recommendations-moving-to-server2016.md)

## <a name="solutions-for-volume-activation"></a>大量啟用解決方案
- [Windows Server 2016 啟用](../get-started/server-2016-activation.md)
- [審查並選取啟用方法](https://technet.microsoft.com/library/jj134256(ws.11).aspx)
- [大量啟用的啟用錯誤碼](https://technet.microsoft.com/library/dn502528.aspx)
- [如何針對金鑰管理服務（KMS）進行疑難排解](https://technet.microsoft.com/library/ee939272.aspx)
- [大量啟用疑難排解](https://technet.microsoft.com/library/ff793439.aspx)
- [啟用錯誤代碼](https://technet.microsoft.com/library/ff793399.aspx)
- [Windows 安裝可能會失敗，並出現錯誤：「輸入的產品金鑰不符合任何可供安裝的 Windows 映像。輸入不同的產品金鑰」](https://support.microsoft.com/help/2796988/windows-8-or-windows-server-2012-installation-may-fail-with-error-mess)

## <a name="solutions-related-to-dcpromo-and-installing-domain-controllers"></a>與 DCPromo 及安裝網域控制站相關的解決方案
- [Active Directory 和 Active Directory Domain Services 埠需求](https://technet.microsoft.com/library/dd772723(v=ws.10).aspx)
- [Active Directory 防火牆埠–讓我們試著簡化](http://blogs.msmvps.com/acefekay/2011/11/01/active-directory-firewall-ports-let-s-try-to-make-this-simple/)
- [Windows Server 2016 的 Exchange Server 支援](https://technet.microsoft.com/library/ff728623(v=exchg.150).aspx)
- [使用 Ntdsutil.exe 傳輸或抓取 FSMO 角色到網域控制站](https://support.microsoft.com/kb/255504)
- [疑難排解網域控制站部署](../identity/ad-ds/deploy/troubleshooting-domain-controller-deployment.md)
- [疑難排解 Active Directory 安裝精靈問題](https://msdn.microsoft.com/library/bb727058.aspx)
- [安裝和移除 AD DS 的已知問題](https://technet.microsoft.com/library/cc754463(v=ws.10).aspx)

## <a name="solutions-for-active-directory-federation-services-ad-fs"></a>Active Directory Federation Services (AD FS) 解決方案
- [如何設定以 Azure Active Directory 自動向已加入網域的 Windows 裝置註冊](/azure/active-directory/active-directory-conditional-access-automatic-device-registration-setup)
- [設定宣告的發行](/azure/active-directory/device-management-hybrid-azuread-joined-devices-setup#step-2-setup-issuance-of-claims)
- [設定 AD FS 驗證 LDAP 目錄中儲存的使用者](../identity/ad-fs/operations/configure-ad-fs-to-authenticate-users-stored-in-ldap-directories.md)
- [AD FS 支援憑證驗證的替代主機名稱繫結](../identity/ad-fs/operations/ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)
- [防範密碼攻擊](https://blogs.technet.microsoft.com/tspring/2017/01/20/federated-to-microsoft-cloud-and-account-lockouts/)
- [升級至使用 WID 資料庫的 Windows Server 2016 AD FS](../identity/ad-fs/deployment/upgrading-to-ad-fs-in-windows-server-2016.md)
- [Windows 10 登入-使用 AD FS 啟用裝置驗證](../identity/ad-fs/operations/configure-device-based-conditional-access-on-premises.md)
- [在 Windows Server 2016 的 AD FS 和 WAP 中管理 SSL 憑證](../identity/ad-fs/operations/manage-ssl-certificates-ad-fs-wap-2016.md)
- [Windows Server 2016 中的存取控制原則 AD FS](../identity/ad-fs/operations/access-control-policies-in-ad-fs.md)

## <a name="solutions-related-to-active-directory-replication"></a>與 Active Directory 複寫相關的解決方案

- [進行 Active Directory 複寫問題疑難排解](../identity/ad-ds/manage/troubleshoot/troubleshooting-active-directory-replication-problems.md)
- [從 Microsoft 下載中心下載 Active Directory 複寫狀態工具](https://www.microsoft.com/en-in/download/details.aspx?id=30005)
- [e2e：如何針對常見的 Active Directory 複寫錯誤進行疑難排解](https://support.microsoft.com/kb/3108513)
- [疑難排解 AD 複寫錯誤8606：提供了不足的屬性來建立物件](https://support.microsoft.com/kb/2028495)
- [Windows 2000 伺服器和 Windows Server 2003 中 Active Directory 的輸入複寫期間，發生事件識別碼2108和事件識別碼1084](https://support.microsoft.com/kb/837932)
- [疑難排解 AD 複寫錯誤8451：複寫作業發生資料庫錯誤](https://support.microsoft.com/kb/2645996)
- [疑難排解 AD 複寫錯誤1127：存取硬碟時，即使在重試之後磁片作業失敗](https://support.microsoft.com/kb/2025726)
- [清理伺服器中繼資料](https://technet.microsoft.com/library/cc816907.aspx)