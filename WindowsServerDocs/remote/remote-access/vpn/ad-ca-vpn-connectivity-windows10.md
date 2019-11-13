---
title: 使用 Azure AD 的 VPN 連線性條件式存取
description: 在此選擇性步驟中，您可以使用 Azure Active Directory （Azure AD）條件式存取，微調授權的 VPN 使用者如何存取您的資源。
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 06/28/2019
ms.reviewer: deverette
ms.openlocfilehash: be50c8eaf789b6f0737cbe07cf10d041d25e74f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388197"
---
# <a name="step-7-optional-conditional-access-for-vpn-connectivity-using-azure-ad"></a>步驟 7. 選擇性使用 Azure AD 進行 VPN 連線的條件式存取

- [**上一步：** 步驟6。設定 Windows 10 用戶端 Always On VPN 連線](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [**下一步：** 步驟7.1。設定 EAP-TLS 以忽略憑證撤銷清單（CRL）檢查](vpn-config-eap-tls-to-ignore-crl-checking.md)

在此選擇性步驟中，您可以使用[Azure Active Directory （Azure AD）條件式存取](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)，微調 VPN 使用者存取資源的方式。 透過 Azure AD 虛擬私人網路（VPN）連線的條件式存取，您可以協助保護 VPN 連接。 「條件式存取」是原則型評估引擎，可讓您為任何 Azure Active Directory (Azure AD) 已連線的應用程式建立存取規則。

## <a name="prerequisites"></a>必要條件

您已熟悉下列主題：

- [Azure Active Directory 中的條件式存取](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
- [VPN 和條件式存取](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

若要設定 VPN 連線 Azure Active Directory 的條件式存取，您必須設定下列各項：

- [伺服器基礎結構](always-on-vpn/deploy/vpn-deploy-server-infrastructure.md)
- [Always On VPN 的遠端存取服務器](always-on-vpn/deploy/vpn-deploy-ras.md)
- [網路原則伺服器](always-on-vpn/deploy/vpn-deploy-nps.md)
- [DNS 和防火牆設定](always-on-vpn/deploy/vpn-deploy-dns-firewall.md)
- [Windows 10 用戶端 Always On VPN 連線](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)

## <a name="step-71-configure-eap-tls-to-ignore-certificate-revocation-list-crl-checkingvpn-config-eap-tls-to-ignore-crl-checkingmd"></a>[步驟7.1。設定 EAP-TLS 以忽略憑證撤銷清單（CRL）檢查](vpn-config-eap-tls-to-ignore-crl-checking.md)

在此步驟中，您可以新增**IgnoreNoRevocationCheck**並將它設定為允許用戶端在憑證不包含 CRL 發佈點時進行驗證。 根據預設，IgnoreNoRevocationCheck 會設定為0（已停用）。

除非 NPS 伺服器已完成憑證鏈（包含根憑證）的撤銷檢查，否則 EAP-TLS 用戶端無法連接。 由 Azure AD 發行給使用者的雲端憑證不具有 CRL，因為其存留期為一小時的短期憑證。 NPS 上的 EAP 必須設定為忽略不存在 CRL 的情況。 由於驗證方法是 EAP-TLS，因此只有在**EAP\13**底下才需要此登錄值。 如果使用其他 EAP 驗證方法，則也應該在這些情況下新增登錄值。

## <a name="step-72-create-root-certificates-for-vpn-authentication-with-azure-advpn-create-root-cert-for-vpn-auth-azure-admd"></a>[步驟7.2。使用 Azure AD 建立根憑證以進行 VPN 驗證](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

在此步驟中，您會使用 Azure AD 來設定 VPN 驗證的根憑證，這會自動在租使用者中建立 VPN 伺服器雲端應用程式。  

若要設定 VPN 連線能力的條件式存取，您需要：

1. 在 Azure 入口網站中建立 VPN 憑證。
2. 下載 VPN 憑證。
3. 將憑證部署至您的 VPN 伺服器。

> [!IMPORTANT]
> 一旦在 Azure 入口網站中建立 VPN 憑證，Azure AD 就會立即開始使用它來發出短期憑證給 VPN 用戶端。 請務必立即將 VPN 憑證部署到 VPN 伺服器，以避免任何與 VPN 用戶端認證驗證有關的問題。

## <a name="step-73-configure-the-conditional-access-policyvpn-config-conditional-access-policymd"></a>[步驟7.3。設定條件式存取原則](vpn-config-conditional-access-policy.md)

在此步驟中，您會設定 VPN 連線能力的條件式存取原則。

若要設定條件式存取原則，您需要：

1. 建立指派給 VPN 使用者的條件式存取原則。
2. 將雲端應用程式設定為**VPN 伺服器**。
3. 將授與（存取控制）設定為**需要多重要素驗證**。  您可以視需要使用其他控制項。

## <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-advpn-deploy-cond-access-root-cert-to-on-premise-admd"></a>[步驟7.4。將條件式存取根憑證部署到內部部署 AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

在此步驟中，您會將受信任的根憑證用於 VPN 驗證，部署至您的內部部署 AD。

若要部署受信任的根憑證，您需要：

1. 新增下載的憑證作為*VPN 驗證的根信任 CA*。
2. 將根憑證匯入 VPN 伺服器和 VPN 用戶端。
3. 確認憑證存在並顯示為受信任。

## <a name="step-75-create-oma-dm-based-vpnv2-profiles-to-windows-10-devicesvpn-create-oma-dm-based-vpnv2-profilesmd"></a>[步驟7.5。建立以 OMA DM 為基礎的 VPNv2 設定檔到 Windows 10 裝置](vpn-create-oma-dm-based-vpnv2-profiles.md)

在此步驟中，您可以使用 Intune 建立 OMA DM 型 VPNv2 設定檔，以部署 VPN 裝置設定原則。 如果您想要使用 SCCM 或 PowerShell 腳本來建立 VPNv2 設定檔，請參閱[VPNV2 CSP 設定](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp)以取得更多詳細資料。

## <a name="next-steps"></a>後續步驟

[步驟7.1。設定 EAP-TLS 以忽略憑證撤銷清單（CRL）檢查](vpn-config-eap-tls-to-ignore-crl-checking.md)：在此步驟中，您必須新增**IgnoreNoRevocationCheck**並將它設定為允許在憑證不包含 CRL 發佈點時驗證用戶端。 根據預設，IgnoreNoRevocationCheck 會設定為0（已停用）。

## <a name="related-topics"></a>相關主題

- [設定 VPNv2 設定檔](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)： VPN 用戶端現在可以與雲端型條件式存取平臺整合，以提供遠端用戶端的裝置相容性選項。 在此步驟中，您會設定具有 **\<DeviceCompliance > \<啟用 > true\</Enabled >** 的 VPNv2 設定檔。

- [使用自動 VPN 設定檔強化 Windows 10 中的遠端存取](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile)：瞭解 Microsoft 如何實行 VPN 連線的條件式存取。 VPN 設定檔包含裝置連線到公司網路所需的所有資訊，包括支援的驗證方法，以及裝置應連接的 VPN 伺服器。 Windows 10 年度更新版中的變更（包括條件式存取和單一登入）讓我們能夠建立我們的 Always On VPN 連線設定檔。 我們已使用 System Center Configuration Manager 主控台為已加入網域和 Microsoft Intune 管理的裝置建立連線設定檔。

- [Azure Active Directory 中的條件式存取](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)：安全性對於使用雲端的組織而言是最重要的考慮。 雲端安全性的重要層面是管理雲端資源時的身分識別和存取權。 在行動優先、雲端優先的世界中，使用者可以從任何地方使用各種裝置和應用程式來存取您組織的資源。 因此，只著重于誰可以存取資源並不足夠。 為了讓安全性與生產力之間的平衡，IT 專業人員也必須考慮如何將資源存取為存取控制決策。

- [Vpn 和條件式存取](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)： vpn 用戶端現在可以與雲端型條件式存取平臺整合，以提供遠端用戶端的裝置相容性選項。 「條件式存取」是原則型評估引擎，可讓您為任何 Azure Active Directory (Azure AD) 已連線的應用程式建立存取規則。
