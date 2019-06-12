---
title: 使用 Azure AD 的 VPN 連線性條件式存取
description: 在此選擇性步驟中，您可以微調授權的 VPN 使用者如何存取您使用 Azure Active Directory (Azure AD) 條件式存取的資源。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 07/13/2018
ms.reviewer: deverette
ms.openlocfilehash: c87d0075696bf8ab5794667d42c40829c3eb61bd
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749532"
---
# <a name="step-7-optional-conditional-access-for-vpn-connectivity-using-azure-ad"></a>步驟 7. （選擇性）使用 Azure AD 的 VPN 連線能力的條件式存取

- [**前一個：** 步驟 6.設定 Windows 10 用戶端 Always On VPN 連線](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [**下一步:** 步驟 7.1.設定 EAP-TLS 以忽略憑證撤銷清單 (CRL) 檢查](vpn-config-eap-tls-to-ignore-crl-checking.md)

在此選擇性步驟中，您可以微調 VPN 使用者如何存取您使用的資源[Azure Active Directory (Azure AD) 條件式存取](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)。 使用 Azure AD 條件式存取虛擬私人網路 (VPN) 連線，您可以協助保護 VPN 連線。 「條件式存取」是原則型評估引擎，可讓您為任何 Azure Active Directory (Azure AD) 已連線的應用程式建立存取規則。 

## <a name="prerequisites"></a>必要條件

您熟悉下列主題：
- [Azure Active Directory 中的條件式存取](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
- [VPN 和條件式存取](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

若要設定 VPN 連線的 Azure Active Directory 條件式存取，您需要下列項目設定：
- [伺服器基礎結構](always-on-vpn/deploy/vpn-deploy-server-infrastructure.md)
- [Alwayson VPN 的遠端存取伺服器](always-on-vpn/deploy/vpn-deploy-ras.md)
- [網路原則伺服器](always-on-vpn/deploy/vpn-deploy-nps.md)
- [DNS 和防火牆設定](always-on-vpn/deploy/vpn-deploy-dns-firewall.md)
- [永遠開啟 VPN 連線的 Windows 10 用戶端](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)

## <a name="step-71-configure-eap-tls-to-ignore-certificate-revocation-list-crl-checkingvpn-config-eap-tls-to-ignore-crl-checkingmd"></a>[步驟 7.1.設定 EAP-TLS 以忽略憑證撤銷清單 (CRL) 檢查](vpn-config-eap-tls-to-ignore-crl-checking.md)

在此步驟中，您可以加入**IgnoreNoRevocationCheck**並將它設定為允許用戶端驗證憑證不含 CRL 發佈點時。 根據預設，IgnoreNoRevocationCheck 設為 0 （停用）。

EAP-TLS 用戶端無法連線，除非在 NPS 伺服器完成 （包括根憑證） 的憑證鏈結的撤銷檢查。 Azure AD 所簽發給使用者的雲端憑證沒有 CRL，因為它們是短期的憑證具有一小時的存留期。 在 NPS 上的 EAP 必須設定為忽略的 CRL 不存在。 驗證方法為 EAP-TLS，因為此登錄值時，才需要底下**EAP\13**。 如果使用其他 EAP 驗證方法，然後登錄值就應該將在這些以及。

## <a name="step-72-create-root-certificates-for-vpn-authentication-with-azure-advpn-create-root-cert-for-vpn-auth-azure-admd"></a>[ 7.2.使用 Azure AD 建立 VPN 驗證的根憑證](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

在此步驟中，您可以設定 VPN 驗證的根憑證與 Azure AD 中，會自動建立租用戶中的 VPN 伺服器的雲端應用程式。  

若要設定 VPN 連線能力的條件式存取，您需要：
1. （您可以建立多個憑證） 在 Azure 入口網站中建立 VPN 憑證。
2. 下載 VPN 憑證。
3. 將憑證部署到您的 VPN 伺服器。

## <a name="step-73-configure-the-conditional-access-policyvpn-config-conditional-access-policymd"></a>[步驟 7.3.設定條件式存取原則](vpn-config-conditional-access-policy.md)

在此步驟中，您可以設定 VPN 連線能力的條件式存取原則。

若要設定條件式存取原則，您需要：

1. 建立條件式存取原則指派給 VPN 使用者。
2. 若要設定雲端應用程式**VPN 伺服器**。
3. 授與 （存取控制） 設定為**需要多重要素驗證**。  您可以視需要使用其他控制項。

## <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-advpn-deploy-cond-access-root-cert-to-on-premise-admd"></a>[步驟 7.4.將條件式存取的根憑證部署至內部部署 AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

在此步驟中，您將部署 VPN 驗證的受信任的根憑證到您內部部署 AD。

若要部署信任的根憑證，您需要：
1. 將下載的憑證，作為*受信任的根 CA，VPN 驗證*。
2. 將根憑證匯入 VPN 伺服器和 VPN 用戶端。
3. 請確認憑證存在，而且顯示為受信任。

## <a name="step-75-create-oma-dm-based-vpnv2-profiles-to-windows-10-devicesvpn-create-oma-dm-based-vpnv2-profilesmd"></a>[步驟 7.5.建立 OMA-DM 型 VPNv2 設定檔至 Windows 10 裝置](vpn-create-oma-dm-based-vpnv2-profiles.md)

在此步驟中，您可以建立 OMA DM 基礎 VPNv2 使用 Intune 來部署 VPN 裝置組態原則的設定檔。 如果您想要使用 SCCM 或 PowerShell 指令碼來建立 VPNv2 設定檔，請參閱 < [VPNv2 CSP 設定](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp)如需詳細資訊。 

## <a name="next-steps"></a>後續步驟

[步驟 7.1.設定 EAP-TLS 要略過憑證撤銷清單 (CRL) 檢查](vpn-config-eap-tls-to-ignore-crl-checking.md):在此步驟中，您必須加入**IgnoreNoRevocationCheck**並將它設定為允許用戶端驗證憑證不含 CRL 發佈點時。 根據預設，IgnoreNoRevocationCheck 設為 0 （停用）。

## <a name="related-topics"></a>相關主題

- [設定 VPNv2 設定檔](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access):VPN 用戶端現在可以與雲端型「條件式存取平台」整合，來為遠端用戶端提供裝置合規性選項。 在此步驟中，您可以設定的 VPNv2 設定檔 **\<DeviceCompliance >\<已啟用 >，則為 true\<啟用 >** 。 

- [強化 Windows 10 中使用自動 VPN 設定檔的遠端存取](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile):了解 Microsoft 如何實作 VPN 連線能力的條件式存取。 VPN 設定檔包含裝置連接到公司網路，包括支援的驗證方法和裝置所應連線的 VPN 伺服器所需的所有資訊。 在 Windows 10 年度更新版，包括條件式存取和單一登入，變更會讓它可以讓我們建立我們 Always-On VPN 連線設定檔。 我們建立的連線設定檔已加入網域和使用 System Center Configuration Manager 主控台的 Microsoft Intune 管理裝置。

- [Azure Active Directory 中的條件式存取](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal):安全性是使用雲端的組織的首要考量。 雲端安全性的重要層面時，身分識別和存取管理雲端資源。 在行動優先、 雲端優先的世界裡，使用者可以存取您組織的資源，從任何地方使用各種不同的裝置和應用程式。 因此，只將焦點放在誰可以存取資源，已不再足夠。 為了掌控安全性與生產力之間的平衡，IT 專業人員也必須考量資源正在存取的方式進行存取控制決策的因素。

- [VPN 和條件式存取](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access):VPN 用戶端現在可以與雲端型「條件式存取平台」整合，來為遠端用戶端提供裝置合規性選項。 「條件式存取」是原則型評估引擎，可讓您為任何 Azure Active Directory (Azure AD) 已連線的應用程式建立存取規則。
