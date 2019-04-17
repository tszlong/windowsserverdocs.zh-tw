---
title: 使用 Azure AD 的 VPN 連線性條件式存取
description: 在這個選用的步驟中，您可以微調如何已獲授權的 VPN 使用者存取您使用 Azure Active Directory (Azure AD) 的條件式存取的資源。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 07/13/2018
ms.reviewer: deverette
ms.openlocfilehash: c9104a5d9ae3069e753b8b771270502c4264db96
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067118"
---
# 步驟 7. （選擇性）使用 Azure AD 的 VPN 連線的條件式存取

& #171; [**先前：** 步驟 6。一律在 VPN 連線設定 Windows 10 用戶端](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)<br>
& #187;[**下一步：** 7.1 步驟。設定 EAP-TLS 忽略檢查憑證撤銷清單 (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)

在這個選用的步驟中，您可以微調 VPN 使用者如何存取您使用[Azure Active Directory (Azure AD) 的條件式存取](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)的資源。 使用 Azure AD 的虛擬私人網路 (VPN) 連線的條件式存取，您可以協助保護 VPN 連線。 「條件式存取」是原則型評估引擎，可讓您為任何 Azure Active Directory (Azure AD) 已連線的應用程式建立存取規則。 

## 必要條件

您已熟悉下列主題：
- [Azure Active Directory 中的條件式存取](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
- [VPN 和條件式存取](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

若要設定的 VPN 連線的 Azure Active Directory 條件式存取，您需要有下列設定：
- [伺服器基礎結構](always-on-vpn/deploy/vpn-deploy-server-infrastructure.md)
- [一律開啟 VPN，適用於遠端存取伺服器](always-on-vpn/deploy/vpn-deploy-ras.md)
- [網路原則伺服器](always-on-vpn/deploy/vpn-deploy-nps.md)
- [DNS 和防火牆設定](always-on-vpn/deploy/vpn-deploy-dns-firewall.md)
- [一律開啟的 VPN 連線的 Windows 10 用戶端](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)

## [步驟 7.1. 設定 EAP-TLS 以忽略憑證撤銷清單 (CRL) 檢查](vpn-config-eap-tls-to-ignore-crl-checking.md)

在此步驟中，您可以新增**IgnoreNoRevocationCheck** ，並將它設定為允許用戶端的驗證時的憑證不包括 CRL 發佈點。 根據預設，IgnoreNoRevocationCheck 設為 0 （已停用）。

EAP-TLS 用戶端無法連線，除非 NPS 伺服器完成撤銷檢查憑證鏈結 （包括根憑證）。 Azure AD 所簽發給使用者的雲端憑證沒有 CRL，因為它們是在一小時存留的短期憑證。 在 NPS 上的 EAP 必須忽略 CRL 缺少設定。 因為 EAP-TLS 驗證方法，此登錄值時，才需要**EAP\13**底下。 如果使用其他 EAP 驗證方法，然後登錄值應該新增下的那些也。 




## [步驟 7.2. 使用 Azure AD 建立 VPN 驗證的根憑證](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

在此步驟中，您可以設定 VPN 驗證的根的憑證與 Azure AD，會自動租用戶中建立的 VPN 伺服器雲端應用程式。  

若要設定的 VPN 連線的條件式存取，您需要：
1. 建立 VPN 憑證 （您可以建立多個憑證） 在 Azure 入口網站。
2. 下載 VPN 憑證。
3. 將憑證部署到您的 VPN 伺服器。

## [步驟 7.3. 設定條件式存取原則](vpn-config-conditional-access-policy.md)

在此步驟中，您可以設定 VPN 連線的條件式存取的原則。 

若要設定的條件式存取原則，您需要：
1. 建立會指派給 VPN 使用者的條件式存取原則。
2. 將雲端應用程式設定為**VPN 伺服器**。
3. 將授權 （存取控制）**需要多重要素驗證**。  您可以視需要使用其他控制項。

## [步驟 7.4. 將條件式存取根憑證部署到內部部署 AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

在此步驟中，您將部署 VPN 驗證的受信任的根憑證到您內部部署 AD。

若要部署受信任的根憑證，您需要：
1. 新增已下載的憑證做為*受信任的根 CA VPN 驗證*。
2. 將根憑證匯入的 VPN 伺服器和 VPN 用戶端。
3. 確認憑證存在，並顯示為受信任。


## [步驟 7.5. 建立 OMA-DM 型 VPNv2 設定檔至 Windows 10 裝置](vpn-create-oma-dm-based-vpnv2-profiles.md)

在此步驟中，您可以建立 OMA DM 基礎 VPNv2 設定檔使用 Intune 部署 VPN 裝置設定原則。 如果您想要使用 SCCM 或 PowerShell 指令碼來建立 VPNv2 設定檔，請參閱[VPNv2 CSP 設定](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp)，如需詳細資訊。 


## 後續步驟
[步驟 7.1。設定要忽略檢查憑證撤銷清單 (CRL) EAP-TLS](vpn-config-eap-tls-to-ignore-crl-checking.md)： 在此步驟中，您必須新增**IgnoreNoRevocationCheck** ，並將它設定為允許用戶端的驗證時的憑證不包括 CRL 發佈點。 根據預設，IgnoreNoRevocationCheck 設為 0 （已停用）。

---

## 相關主題
- [設定 VPNv2 設定檔](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)： 將 VPN 用戶端現在是可以與雲端型條件式存取平台，以提供遠端用戶端的裝置相容性選項整合。 在此步驟中，您可以設定使用的 VPNv2 設定檔**\ < DeviceCompliance > \ < Enabled > true\ < / 啟用 >**。 
 
- [增強使用自動的 VPN 設定檔的 Windows 10 中的遠端存取](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile)： 了解 Microsoft 如何實作的 VPN 連線的條件式存取。 VPN 設定檔包含裝置需要連線到公司網路，包括支援驗證方法和裝置應該連線到 VPN 伺服器的所有資訊。 在 Windows 10 年度更新版包括條件式存取和單一登入，變更它讓我們來建立我們 Always-On VPN 連線設定檔。 我們建立的連線設定檔已加入網域和 Microsoft Intune – 受管理的裝置使用 System Center Configuration Manager 主控台。 

- [條件式存取 Azure Active Directory 中](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)： 安全性是使用雲端的組織的首要考量。 雲端安全性的重點是身分識別與存取時，它可以管理您的雲端資源。 在行動優先，雲端優先的世界中，使用者可以存取您的組織從任何地方使用各種不同的裝置和應用程式的資源。 由於這，只將焦點放在誰可以存取資源不足不再。 若要主要安全性和生產力之間的平衡，IT 專業人員也需要分解成存取控制決策如何存取資源。

- [VPN 和條件式存取](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)： 將 VPN 用戶端現在是可以與雲端型條件式存取平台，以提供遠端用戶端的裝置相容性選項整合。 「條件式存取」是原則型評估引擎，可讓您為任何 Azure Active Directory (Azure AD) 已連線的應用程式建立存取規則。 

---