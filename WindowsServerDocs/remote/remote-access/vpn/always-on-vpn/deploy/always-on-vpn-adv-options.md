---
title: Always On VPN 進階功能
description: 超過此部署中所提供的部署案例，您可以新增以提升安全性和可用性，您的 VPN 連線的其他進階的 VPN 功能。
ms.assetid: 51a1ee61-3ffe-4f65-b8de-ff21903e1e74
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 7534f631cf0ac3f8230ea12e790dcd946da0ffbd
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749513"
---
# <a name="advanced-features-of-always-on-vpn"></a>一律開啟 」 VPN 的進階的功能

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows 10

- [**前一個：** 深入了解一律開啟 」 VPN 技術](../always-on-vpn-technology-overview.md)
- [**下一步:** 開始規劃一律開啟 」 VPN 部署](always-on-vpn-deploy-planning.md)

超過所提供的部署案例，您可以新增以提升安全性和可用性，您的 VPN 連線的其他進階的 VPN 功能。 比方說，這類元件可協助確保連線的用戶端之前允許的連線狀況良好。

## <a name="high-availability"></a>高可用性

以下是高可用性的其他選項。

|選項  |描述  |
|---------|---------|
|伺服器恢復與負載平衡     |在需要高可用性或支援大量要求的環境中，您可以增加效能和恢復功能的遠端存取使用正在執行網路原則伺服器 (NPS)，並啟用遠端多部伺服器之間的負載平衡存取伺服器叢集。<p>相關文件：<ul><li>[NPS Proxy 伺服器負載平衡](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md)</li><li>[在叢集中部署遠端存取](https://docs.microsoft.com/windows-server/remote/remote-access/ras/cluster/deploy-remote-access-in-cluster)</li></ul>        |
|地理站台復原能力     |以 IP 為主的地理位置，您可以使用全域流量管理員使用 Windows Server 2016 中的 DNS。 更強大的地理負載平衡，您可以使用全域伺服器負載平衡解決方案，Microsoft Azure 流量管理員 」 等。<p>相關文件：<ul><li>[流量管理員概觀](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)</li><li>[Microsoft Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager)</li></ul>         |

## <a name="advanced-authentication"></a>進階的驗證

以下是驗證的其他選項。

|選項  |描述  |
|---------|---------|
|Windows Hello 企業版     |在 Windows 10 中，Windows Hello 企業版會使用強式雙因素驗證取代電腦與行動裝置上的密碼。 此驗證是由新類型的繫結至裝置，並使用生物識別的使用者認證或個人識別碼 (PIN) 所組成。<p>Windows 10 VPN 用戶端可與 Windows hello 企業版相容。 在使用者登入的筆勢之後，VPN 連線會使用 Windows Hello 進行憑證型驗證的商業憑證。<p>相關文件：<ul><li>[Windows hello 企業版](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification)</li><li>技術性案例研究：[啟用遠端存取以 Windows Hello for Business in Windows 10](https://msdn.microsoft.com/library/mt728163.aspx)</li></ul>         |
|Azure 多重要素驗證 (MFA)     |Azure MFA 具有雲端和內部部署版本，您可以整合 Windows VPN 驗證機制。<p>如需有關這項機制的運作方式的詳細資訊，請參閱 <<c0> [ 將 RADIUS 驗證與 Azure Multi-factor Authentication Server](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius)。         |

## <a name="advanced-vpn-features"></a>進階的 VPN 功能

以下是進階功能的其他選項。

|選項  |描述  |
|---------|---------|
|流量篩選     |如果您需要強制執行用戶端可以存取哪些應用程式 VPN，您可以啟用 VPN 流量篩選器。<p>如需詳細資訊，請參閱 < [VPN 的安全性功能](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features)。         |
|應用程式觸發型 VPN     |您可以設定特定的應用程式或類型的應用程式啟動時自動連線的 VPN 設定檔。<p>如需有關這個主題以及其他的觸發選項的詳細資訊，請參閱[VPN 自動觸發型的設定檔選項](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile)。         |
|VPN 的條件式存取   |條件式存取與裝置的合規性可能需要受管理的裝置，以符合標準，才能連線至 VPN。 VPN 的條件式存取的進階功能的其中一個可讓您限制為只有那些 VPN 連線用戶端驗證憑證包含 'AAD 條件式存取的 OID 的 ' 1.3.6.1.4.1.311.87' 的位置。<p>若要限制的 VPN 連線，您需要：<ol><li>在 NPS 伺服器上，開啟**網路原則伺服器**嵌入式管理單元。</li><li>依序展開**原則** > **網路原則**。</li><li>以滑鼠右鍵按一下**虛擬私人網路 (VPN) 連線**網路原則，然後選取**屬性**。</li><li>選取 [**設定**] 索引標籤。</li><li>選取 **特定廠商**，然後選取**新增**。</li><li>選取 **允許憑證 OID**選項，然後選取**新增**。</li><li>貼上 AAD 條件式存取的 OID **1.3.6.1.4.1.311.87**作為屬性值，然後選取**確定**兩次。</li><li>選取 **關閉**，然後**套用**。<p>現在當 VPN 用戶端會嘗試使用短期雲端憑證以外的任何憑證進行連線，連接將會失敗。</li></ol>如需有關條件式存取的詳細資訊，請參閱 < [VPN 和條件式存取](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)。   |

## <a name="additional-protection"></a>額外的保護

### <a name="trusted-platform-module-tpm-key-attestation"></a>可信賴平台模組 (TPM) 金鑰證明

使用 TPM 證明金鑰的使用者憑證提供更高的安全性保證，由非匯出性、 防止攻擊，和隔離的 TPM 所提供的索引鍵。

如需有關在 Windows 10 中的 TPM 金鑰證明的詳細資訊，請參閱 < [TPM 金鑰證明](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation)。

## <a name="next-step"></a>後續步驟

[開始規劃一律開啟 」 VPN 部署](always-on-vpn-deploy-planning.md):在您打算使用做為 VPN 伺服器的電腦上安裝遠端存取伺服器角色之前，請執行下列工作。 之後適當的規劃中，您可以部署一律開啟 」 VPN，並選擇性地設定 VPN 連線使用 Azure AD 條件式存取。  

## <a name="related-topics"></a>相關主題
- [NPS Proxy 伺服器負載平衡](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md):遠端驗證撥號使用者服務 (RADIUS) 用戶端，也就是網路存取伺服器，例如虛擬私人網路 (VPN) 伺服器以及無線存取點，會建立連線要求，並將它們傳送至 NPS 類的 RADIUS 伺服器。 在某些情況下，NPS 伺服器可能會收到太多的連線要求一次，導致效能降低或多載。

- [流量管理員概觀](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview):本主題提供 Azure 流量管理員概觀，可讓您控制的服務端點的使用者流量分配。 流量管理員會使用網域名稱系統 (DNS) 用戶端將要求導向到最適當的端點，根據流量路由方法和端點的健全狀況。 

- [Windows hello 企業版](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification):本主題提供必要的元件，例如僅雲端的部署和混合式部署。  本主題也會列出有關 Windows hello 企業版常見問題。

- [技術性案例研究：啟用以 Windows Hello 的遠端存取的 Windows 10 中的商務](https://msdn.microsoft.com/library/mt728163.aspx):在本技術案例研究您了解 Microsoft 如何實作使用 Windows Hello for Business 的遠端存取。  Windows hello 企業版是私密金鑰/公開金鑰或憑證式驗證之組織和取用者的方法超越密碼。 這種形式的驗證依賴金鑰組認證，可取代密碼且能抵抗漏洞、 竊取的行為及網路釣魚。 

- [將 RADIUS 驗證與 Azure Multi-factor Authentication Server 整合](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius):本主題會逐步引導您加入和設定 Azure Multi-factor Authentication server 的 RADIUS 用戶端驗證。 RADIUS 是接受驗證要求以及處理這些要求的標準通訊協定。 Azure Multi-factor Authentication Server 可做為 RADIUS 伺服器。 

- [VPN 的安全性功能](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features):本主題會提供您 VPN 安全性指導方針的鎖定 VPN、 與 VPN 和流量篩選器的 Windows 資訊保護 (WIP) 整合。 

- [VPN 的自動觸發型的設定檔選項](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile):本主題會提供 VPN 自動觸發型的設定檔選項，例如應用程式觸發程序中，名稱為基礎的觸發程序，以及 Always On。

- [VPN 和條件式存取](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access):本主題提供雲端型條件式存取平台提供遠端用戶端的裝置合規性選項的概觀。 「條件式存取」是原則型評估引擎，可讓您為任何 Azure Active Directory (Azure AD) 已連線的應用程式建立存取規則。 

- [TPM 金鑰證明](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation):本主題將提供部署 TPM 金鑰證明的受信任的平台模組 (TPM) 和步驟的概觀。 您也可以找到疑難排解的資訊和步驟來解決問題。