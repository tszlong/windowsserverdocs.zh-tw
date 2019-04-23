---
title: 遠端存取一律開啟 」 VPN 移轉概觀
description: 一律開啟 」 VPN 可解決 Windows Vpn 和 DirectAccess，以及如何從 DirectAccess 移轉至一律開啟 」 VPN 之間的前一個間隔。
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 05/29/2018
ms.openlocfilehash: 402d8ff72fe869572c9e6129cdf1aa7e755c354a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845979"
---
# <a name="overview-of-the-directaccess-to-always-on-vpn-migration"></a>DirectAccess 移轉至 Always On VPN 概觀 

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows 10

&#187;[**下一步:** 規劃 DirectAccess，讓它一律開啟 」 VPN 移轉](da-always-on-migration-planning.md)

在舊版的 Windows VPN 架構中，平台限制並不容易以提供取代了 DirectAccess 之後，例如在使用者登入之前啟動的自動連線所需的重要功能。 不過，一律開啟 」 VPN，可能會有裝置減輕大部分的這些限制，或展開的 DirectAccess 功能的 VPN 功能。 一律開啟 」 VPN 可解決 Windows Vpn 和 DirectAccess 的上一個間隙。

DirectAccess – 以 – Alwayson VPN 移轉程序是由四個主要元件和高層級處理序所組成：


1.  **計劃一律開啟 」 VPN 移轉。** 計劃可協助識別使用者階段區隔，以及基礎結構和功能的目標用戶端。

    1.  [!INCLUDE [build-migration-rings-shortdesc-include](../includes/build-migration-rings-shortdesc-include.md)]

    2.  [!INCLUDE [review-feature-mapping-shortdesc-include](../includes/review-feature-mapping-shortdesc-include.md)] 

    3.  [!INCLUDE [review-the-enhancements-shortdesc-include](../includes/review-the-enhancements-shortdesc-include.md)] 

    4.  [!INCLUDE [review-the-technology-overview-shortdesc-include](../includes/review-the-technology-overview-shortdesc-include.md)]

2.  **部署的並排顯示 VPN 基礎結構。** 決定移轉階段，以及您想要在您的部署中包含的功能之後，您會部署與現有的 DirectAccess 基礎結構一律開啟 」 VPN 基礎結構。  

3.  **部署至用戶端的憑證及設定。**  將 VPN 基礎結構準備就緒後，您會建立，並將所需的憑證發佈到用戶端。 當用戶端已收到的憑證時，您會部署 VPN_Profile.ps1 組態指令碼。 或者，您可以使用 Intune 設定 VPN 用戶端。 使用 Microsoft System Center Configuration Manager 或 Microsoft Intune，以監視成功的 VPN 設定部署。

4.  **移除，並解除委任。** 每個人關閉 DirectAccess 移轉之後，正確解除委任的環境。

    1.  [!INCLUDE [remove-da-from-client-shortdesc-include](../includes/remove-da-from-client-shortdesc-include.md)]

    2.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]


## <a name="directaccess-deployment-scenario"></a>DirectAccess 部署案例

在此案例中，您使用簡單的 DirectAccess 部署案例做為起點本指南將介紹的移轉。 您不需要永遠開啟 」 VPN，以在移轉之前符合此部署案例，但對於許多組織而言，這個簡單的安裝程式可精確呈現其目前的 DirectAccess 部署。 下表提供此安裝程式的基本功能的清單。

許多的 DirectAccess 部署案例和選項存在，因此您的實作很可能是不同於此處所述。 如果是的話，請參閱[DirectAccess 與一律開啟 」 VPN 之間的功能對應](../vpn/vpn-map-da.md)判斷一律開啟 」 VPN 功能設定您目前的 addition 的對應，然後再將這些功能加入至您的設定。 此外，您可以參考[一律開啟 」 VPN 的增強功能](../vpn/always-on-vpn/always-on-vpn-enhancements.md)加入一律開啟 」 VPN 部署中的選項。

>[!NOTE] 
>針對已加入網域的裝置，有額外的考量，例如憑證註冊。 如需詳細資訊，請參閱 < [Always On VPN 部署的 Windows Server 和 Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md)。

### <a name="deployment-scenario-feature-list"></a>部署案例的功能清單

| DirectAccess 功能 | 典型的案例 |
|-----|----|
| 部署案例                   | 部署完整的 DirectAccess 用戶端存取和遠端管理                                               |
| 網路介面卡                      | 2                                                                                                              |
| 使用者驗證                   | Active Directory 認證                                                                                   |
| 使用電腦憑證             | 是                                                                                                            |
| 安全性群組                       | 是                                                                                                            |
| 單一 DirectAccess 伺服器            | 是                                                                                                            |
| 網路拓撲                      | 網路位址轉譯 (NAT) 在邊緣防火牆後面有兩個網路介面卡                            |
| 存取模式                           | 結束邊緣                                                                                                    |
| 通道                             | 分割通道                                                                                                   |
| 驗證                        | 使用電腦憑證，再加上 Kerberos (不 KerbProxy) 的標準公用基礎結構 (PKI) 驗證 |
| 通訊協定                             | 透過 HTTPS (IP-HTTPS) 的 IP                                                                                       |
| 網路位置伺服器 (NLS) 關閉方塊 | 是                                                                                                            |

## <a name="always-on-vpn-deployment-scenario"></a>Alwayson VPN 部署案例

在此案例中，您專注於簡單的 DirectAccess 環境移轉到簡單的一律開啟 」 VPN 環境，也就是 DirectAccess 的替代方案。 下表提供此簡單的解決方案中使用的功能。 如需詳細一律開啟 」 VPN 用戶端的其他增強功能相關資訊，請參閱[一律開啟 」 VPN 的增強功能](../vpn/always-on-vpn/always-on-vpn-enhancements.md)。

### <a name="always-on-vpn-features-used-in-the-simple-environment"></a>簡單的環境中所使用的一律開啟 」 VPN 功能

| VPN 功能 | 部署案例的組態 |
|-----|-----|
| 連線類型 | 原生的網際網路金鑰交換版本 2 (IKEv2) |
| 網路介面卡   | 2        |
| 使用者驗證  | Active Directory 認證            |
| 使用電腦憑證        | 是                          |
| 路由 | 分割通道 |
| 名稱解析 | 網域名稱資訊清單和網域名稱系統 (DNS) 尾碼 |
| 觸發 | Always on 且受信任網路偵測 |
| 驗證  | 受保護可延伸驗證通訊協定-傳輸層安全性 (PEAP-TLS) 與信賴平台模組 」 保護的使用者憑證 |

## <a name="next-step"></a>後續步驟

[規劃 DirectAccess，讓它一律開啟 」 VPN 移轉](da-always-on-migration-planning.md)。 移轉的主要目標是為了在整個過程在辦公室的遠端連線的使用者。

---