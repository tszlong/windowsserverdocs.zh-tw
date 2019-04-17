---
title: Always On VPN 進階功能
description: 超出此部署中提供的部署案例，您可以新增其他進階的 VPN 功能來改善的安全性和您的 VPN 連線的可用性。
ms.assetid: 51a1ee61-3ffe-4f65-b8de-ff21903e1e74
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: a544ac3c1a121874170a2fc78a34bd401b8bebe1
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067185"
---
# Always On VPN 的進階的功能

>適用於： Windows Server （半年度管道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10

& #171; [**前一個：** 了解有關 Always On VPN 技術](../always-on-vpn-technology-overview.md)<br>
& #187;[**下一步：** 開始規劃 Always On VPN 部署](always-on-vpn-deploy-planning.md)

除了所提供的部署案例，您可以新增其他進階的 VPN 功能來改善的安全性和您的 VPN 連線的可用性。 例如，這類元件可協助確保連線的用戶端之後，才允許連線狀況良好。


## 高可用性

以下是針對高可用性的其他選項。


|選項  |描述  |
|---------|---------|
|伺服器的彈性和負載平衡     |在環境中需要高可用性或支援大量的要求，提高效能和遠端存取的可復原性使用多部伺服器，執行網路原則伺服器 (NPS) 並啟用遠端之間的平衡存取伺服器叢集。<p>相關文件：<ul><li>[NPS Proxy 伺服器負載平衡](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md)</li><li>[在叢集中部署遠端存取](https://docs.microsoft.com/windows-server/remote/remote-access/ras/cluster/deploy-remote-access-in-cluster)</li></ul>        |
|地理站台復原能力     |針對 IP 為基礎的地理位置，您可以使用全域流量管理員以 Windows Server 2016 中 DNS。 適用於更健全的地理負載平衡，您可以使用伺服器負載平衡全域解決方案，例如 Microsoft Azure 流量管理員。<p>相關文件：<ul><li>[流量管理員概觀](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)</li><li>[Microsoft Azure 流量管理員](https://azure.microsoft.com/services/traffic-manager)</li></ul>         |

---

## 進階的驗證

以下是其他選項進行驗證。


|選項  |描述  |
|---------|---------|
|Windows Hello 企業版     |在 Windows 10 中，Windows Hello 企業版會使用強式雙因素驗證取代電腦與行動裝置上的密碼。 此驗證包含新型的使用者認證會繫結到裝置，並使用生物特徵辨識或個人識別碼 (PIN)。<p>Windows 10 VPN 用戶端可與 Windows Hello 企業版相容。 當使用者登入手勢之後，VPN 連線會使用 Windows Hello 企業版憑證的憑證型驗證。<p>相關文件：<ul><li>[Windows Hello 企業版](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification)</li><li>技術案例研究：[啟用與 Windows Hello 在 Windows 10 企業版的遠端存取](https://msdn.microsoft.com/library/mt728163.aspx)</li></ul>         |
|Azure 多重要素驗證 (MFA)     |Azure MFA 有雲端和內部版本，您可以將 Windows VPN 驗證機制與整合。<p>如需有關此機制的運作方式的詳細資訊，請參閱[整合 RADIUS 驗證 Azure Multi-factor Authentication Server 使用](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius)。         |

---

## 進階的 VPN 功能

以下是進階功能的額外選項。


|選項  |描述  |
|---------|---------|
|流量篩選     |如果您需要強制執行哪些應用程式 VPN 用戶端可以存取，您可以啟用 VPN 流量篩選器。<p>如需詳細資訊，請參閱[VPN 安全性功能](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features)。         |
|應用程式觸發 VPN     |您可以設定特定應用程式或類型的應用程式啟動時自動連線 VPN 設定檔。<p>如需有關這和其他觸發選項的詳細資訊，請參閱[VPN 自動觸發的設定檔選項](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile)。         |
|VPN 的條件式存取   |條件式存取和裝置合規性可能會要求以符合標準，他們可以連線至 VPN 之前, 受管理的裝置。 針對 VPN 的條件式存取權的進階功能的其中一個可讓您限制僅適用於 VPN 連線的用戶端驗證憑證其中包含 'AAD 條件式存取 ' OID 1.3.6.1.4.1.311.87 的 ''。<p>若要限制的 VPN 連線，您需要：<ol><li>在 NPS 伺服器上，開啟**網路原則伺服器**嵌入式管理單元。</li><li>展開 [**原則]** > **網路原則**。</li><li>以滑鼠右鍵按一下**虛擬私人網路 (VPN) 連線**網路原則，並選取 [**屬性**]。</li><li>選取 [**設定**] 索引標籤。</li><li>選取**特定廠商**，然後按一下 [**新增**]。</li><li>選取 \ [**允許-憑證-OID**選項，然後按一下 [**新增]**。</li><li>貼上**1.3.6.1.4.1.311.87**做為屬性值，AAD 條件式存取 OID，然後按一下 **[確定]** 兩次。</li><li>按一下 [**關閉**] 和 [**套用**]。<p>現在當 VPN 用戶端嘗試連線使用雲端短期憑證以外的任何憑證，連線將會失敗。</li></ol>如需條件式存取的詳細資訊，請參閱[VPN 和條件式存取](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)。   |

---


## 額外的保護

**信賴平台模組 (TPM) 金鑰證明**<p>
使用 TPM 證明金鑰的使用者憑證提供較高的安全性保證，由非匯出性、 反鎚和隔離 TPM 所提供的金鑰的備份。

如需有關 Windows 10 中的 TPM 金鑰證明的詳細資訊，請參閱[TPM 金鑰證明](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation)。

## 後續步驟
[開始規劃 Always On VPN 部署](always-on-vpn-deploy-planning.md)： 您打算使用為 VPN 伺服器的電腦上安裝遠端存取伺服器角色之前，請執行下列工作。 適當計畫之後，您可以部署 Always On VPN，並選擇性設定的 VPN 連線使用 Azure AD 的條件式存取。  

---

## 相關主題
- [NPS Proxy 伺服器負載平衡](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md)： 遠端驗證撥入使用者服務 (RADIUS) 用戶端，這是網路存取伺服器，例如虛擬私人網路 (VPN) 伺服器及無線存取點，建立連線要求，並將它們傳送給半徑例如 NPS 伺服器。 在某些情況下，NPS 伺服器可能會收到太多的連線要求一次，導致效能降低或多載。

- [流量管理員的概觀](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)： 本主題概述的 Azure 流量管理員] 中，這可讓您控制使用者為服務端點的流量的分布。 流量管理員會使用網域名稱系統 (DNS) 直接到最適當的端點根據流量路由的方法和端點的健康情況的用戶端要求。 

- [Windows Hello 企業版](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification)： 本主題提供的先決條件，例如雲端只部署和混合式部署。  本主題也會列出關於 Windows Hello 企業版常見問題的集。

- [技術案例研究： 啟用遠端存取與 Windows Hello 在 Windows 10 企業版](https://msdn.microsoft.com/library/mt728163.aspx)： 在這個技術案例研究您了解 Microsoft 如何實作搭配 Windows Hello 企業版的遠端存取。  Windows Hello 企業版是私密/公開金鑰金鑰或用於組織和消費者不僅僅只密碼的憑證型驗證方法。 這種形式的驗證依賴金鑰組認證可以取代密碼，而且漏洞、 竊取的行為，以及網路釣魚竄改。 

- [使用 Azure Multi-factor Authentication Server 的整合 RADIUS 驗證](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius)： 本主題會逐步引導您完成新增和使用 Azure Multi-factor Authentication Server 設定 RADIUS 用戶端驗證。 RADIUS 是標準的通訊協定接受驗證要求，並處理這些要求。 Azure Multi-factor Authentication Server 可以做為 RADIUS 伺服器。 

- [VPN 安全性功能](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features)： 本主題提供您 VPN 安全性指導方針鎖定 VPN，透過 VPN，Windows 資訊保護 (WIP) 整合與流量篩選器。 

- [VPN 自動觸發的設定檔選項](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile)： 本主題提供您 VPN 自動觸發的設定檔選項，例如應用程式觸發程序、 名稱型觸發程序，以及一律開啟。

- [VPN 和條件式存取](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)： 本主題提供您的雲端型條件式存取平台，以提供遠端用戶端的裝置相容性選項概觀。 「條件式存取」是原則型評估引擎，可讓您為任何 Azure Active Directory (Azure AD) 已連線的應用程式建立存取規則。 

- [TPM 金鑰證明](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation)： 本主題提供您的信賴平台模組 (TPM) 的概觀，以及步驟來部署 TPM 金鑰證明。 您也可以找到疑難排解資訊及解決問題的步驟。 

---