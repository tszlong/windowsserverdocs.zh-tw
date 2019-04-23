---
title: Always On VPN 增強功能
description: 一律開啟 」 VPN 會透過 Windows VPN 解決方案的過去有許多優點。 在整合、 安全性、 連線、 網路控制和相容性的主要改善項目會將一律開啟 」 VPN 配合 Microsoft 的雲端優先、 行動優先的願景。
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 11/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: d7844d93ac898316580123c82e08bb6f02d51a2b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836519"
---
# <a name="always-on-vpn-enhancements"></a>Always On VPN 增強功能

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows 10

&#171;[**前一個：** 了解的一律開啟 」 VPN 功能](../vpn-map-da.md)<br>
&#187;[**下一步:** 深入了解一律開啟 」 VPN 技術](always-on-vpn-technology-overview.md)

一律開啟 」 VPN 會透過 Windows VPN 解決方案的過去有許多優點。 下列的主要改善項目會將一律開啟 」 VPN 配合 Microsoft 的雲端優先、 行動優先的願景：

- **平台整合：** 一律開啟 」 VPN 已改進的 Windows 作業系統和協力廠商解決方案，以提供無數的進階的連線案例穩固的平台的整合。

- **安全性：** 一律開啟 」 VPN 有新的進階安全性功能，可限制類型的流量，哪些應用程式可以使用 VPN 連線，以及您可以使用的驗證方法來起始連線。 連線時作用中大部分，它是情況的特別重要的安全連線。 如需詳細資訊，請參閱 < [VPN 驗證選項](https://docs.microsoft.com/en-us/windows/security/identity-protection/vpn/vpn-authentication)。

- **VPN 連線能力：** 一律開啟 」 VPN，不論裝置通道會提供自動觸發程序功能。 之前一律開啟 」 VPN，觸發自動透過使用者或裝置驗證連線能力不是可能的。  

- **網路控制：** 一律開啟 」 VPN 可讓系統管理員指定更細微的層級的路由原則，甚至還有個別的應用程式 — 這是適合的營運 (LOB) 應用程式需要特殊的遠端存取。  一律開啟 」 VPN 也是完全相容，這兩個網際網路通訊協定第 4 版 (IPv4) 和第 6 版 (IPv6)。 與 DirectAccess，沒有任何特定的相依性在 IPv6 上。

  >[!Note]
  >開始之前，請確定 VPN 伺服器上啟用 IPv6。 否則，無法建立連線，並會顯示錯誤訊息。
  
- **組態和相容性：** 一律開啟 」 VPN 可部署及管理數種方式，提供一律開啟 」 VPN 其他 VPN 用戶端軟體的幾項優點。

## <a name="platform-integration"></a>平台整合

Microsoft 已導入，或改善下列的整合功能，在 一律開啟 」 VPN:

| 重要的改進 | 描述 |
| ---- | ---- |
| **[Windows 資訊保護 (WIP)](https://docs.microsoft.com/windows/threat-protection/windows-information-protection/protect-enterprise-data-using-wip)** | 使用 WIP 的整合可讓以判斷是否會允許流量，經過 VPN 的網路原則強制執行。 如果使用者設定檔為作用中和 WIP 原則會套用，一律開啟 」 VPN 會自動觸發連線。 此外，當您使用 WIP 時，沒有 AppTriggerList 和 TrafficFilterList 規則分別指定 VPN 設定檔中 （除非您想要更進階的組態） 需要因為 WIP 原則和應用程式清單會自動生效。 |
| **[Windows hello 企業版](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-overview)** | 一律開啟 」 VPN 原本就支援 Windows Hello for Business （以憑證為基礎的驗證模式） 提供無縫單一登入體驗，這兩個登入電腦和 VPN 連線。 因此，沒有第二項驗證 （使用者認證） 所需的 VPN 連線，讓您可以使用 Always On 連線 Windows Hello for Business 驗證。 |
| **[Microsoft Azure 條件式存取](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls)** | 一律開啟 」 VPN 用戶端可以整合的 Azure 條件式存取平台，可強制執行多重要素驗證 (MFA)、 裝置合規性或兩者的組合。 當符合條件式存取原則時，Azure Active Directory (Azure AD) 會發出短期 （根據預設，60 分鐘） 的 IP 安全性 (IPsec) 驗證憑證，然後可以用來驗證 VPN 閘道。 裝置合規性會使用 System Center Configuration Manager/Intune 合規性原則，其中可以包括裝置健康情況證明狀態連接合規性檢查的一部分。 |
| **Azure MFA** | 當搭配遠端驗證撥號使用者服務 (RADIUS) 服務和網路原則伺服器 (NPS) 擴充功能，適用於 Azure MFA，VPN 驗證可以使用強式的 MFA。 |
| **協力廠商 VPN 外掛程式** | 使用通用 Windows 平台 (UWP)，第三方 VPN 提供者可以建立單一應用程式的完整範圍的 windows 10 裝置。 UWP 提供保證的核心 API 層裝置上，刪除的複雜性和通常與撰寫核心層級驅動程式相關的問題。 目前，Windows 10 UWP VPN 外掛程式存在[Pulse Secure](https://www.microsoft.com/p/pulse-secure/9nblggh3b0bp)， [F5 存取](https://www.microsoft.com/p/f5-access/9wzdncrdsfn0)，[檢查 Point Capsule VPN](https://www.microsoft.com/p/check-point-capsule-vpn/9wzdncrdjxtj)， [FortiClient](https://www.microsoft.com/p/forticlient/9wzdncrdh6mc)， [SonicWall Mobile Connect](https://www.microsoft.com/p/sonicwall-mobile-connect/9wzdncrdsfkz)，並[GlobalProtect](https://www.microsoft.com/p/globalprotect/9nblggh6bzl3); 當然，其他項目會出現在未來。 |
---

## <a name="security"></a>安全性

在安全性中主要的增強功能是在下列區域：

| 重要的改進 | 描述 |
| ---- | ---- |
| **流量篩選器** | 透過流量篩選器，您可以指定用戶端原則，以決定允許哪些流量進入公司網路。 如此一來，系統管理員可以套用應用程式或流量限制在 VPN 介面中，限制使用特定的來源、 目的地連接埠和 IP 位址。 有兩種類型的篩選規則：<ul><li>**應用程式為基礎的規則。** 應用程式為基礎的防火牆規則根據指定的應用程式的清單，以便允許來自這些應用程式的流量會透過 VPN 介面。</li><li>**資料傳輸為基礎的規則。** 資料傳輸為基礎的防火牆規則是以網路需求，例如連接埠、 位址和通訊協定為基礎。 允許這些規則僅針對符合特定條件的流量會透過 VPN 介面的使用。<p><p>_**請注意。**_<br>這些規則僅適用於輸出流量從裝置。 使用流量篩選區塊的流量從公司網路用戶端。 </li></ul> |
| **Per-App VPN** | 個別 App VPN 就像是有的應用程式為基礎的流量篩選條件，但它進一步獲得應用程式觸發程序結合的應用程式為基礎的流量篩選器，使 VPN 連線能力受限於特定的應用程式，而不是 VPN 用戶端上的所有應用程式。 應用程式啟動時，此功能會自動起始。 |
| **支援自訂 IPsec 加密演算法** | 一律開啟 」 VPN 支援 RSA 和橢圓曲線密碼編譯為基礎的自訂密碼編譯演算法符合嚴格的政府機構或組織的安全性原則的使用。 |
| **原生的可延伸驗證通訊協定 (EAP) 支援** | 一律開啟 」 VPN 原本就支援 EAP，可讓您使用多樣的 Microsoft 和協力廠商 EAP 類型的驗證工作流程。 EAP 提供下列驗證類型為基礎的安全驗證：<ul><li>使用者名稱和密碼</li><li>智慧卡 （實體和虛擬）</li><li>使用者憑證</li><li>Windows Hello 企業版</li><li>透過 EAP RADIUS 整合 MFA 支援</li></ul>應用程式廠商會控制第三方 UWP VPN 外掛程式的驗證方法，雖然它們有可用的選項，包括自訂的認證類型，以及 OTP 支援的陣列。 |
---

## <a name="vpn-connectivity"></a>VPN 連線

以下是一律開啟 」 VPN 連線能力的主要增強功能：

| 重要的改進 | 描述 |
| ---- | ---- |
| **Always On** | 永遠開啟 」 是一種 Windows 10 功能，讓作用中的 VPN 設定檔，自動連線並維持連線根據觸發程序 — 也就是使用者登入、 網路狀態變更或作用中裝置螢幕。 Always On 也會整合到連線的待命體驗盡可能延長電池壽命。 |
| **應用程式觸發** | 您可以設定自動連線的一組指定的應用程式啟動的 Windows 10 VPN 設定檔。 您可以設定桌面和 UWP 應用程式，以觸發 VPN 連線。 |
| **名稱為基礎的觸發** | 使用 永遠開啟 」 VPN，您可以定義規則，使特定的網域名稱查詢觸發 VPN 連線。 Windows 10 現在支援已加入網域的和已加入網域的機器名稱架構型觸發 （先前只有已加入網域的機器所支援）。 |
| **受信任的網路偵測** | 一律開啟 」 VPN 包含此功能，以確保如果使用者已連線到信任的網路公司界限內，則不會觸發 VPN 連線能力。 您可以使用任何先前所述，提供無縫式的 「 只連接時所需 」 使用者經驗的觸發方法結合這項功能。 |
| **[裝置通道](../vpn-device-tunnel-config.md)** | 一律開啟 」 VPN 可讓您能夠建立專用的 VPN 設定檔針對裝置或電腦。 不同於_使用者通道_，這只會連接使用者登入裝置或電腦之後,_裝置通道_可讓使用者登入之前連接 VPN。 裝置通道與使用者通道獨立運作，使用自己的 VPN 設定檔，可以連線在此同時，也可以視需要使用不同的驗證方法和其他 VPN 組態設定。 |
---

## <a name="networking"></a>網路功能

以下是一些一律開啟 」 VPN 的網路增強功能：

| 重要的改進 | 描述 |
| ---- | ---- |
| **IPv4 和 IPv6 的雙重堆疊支援** | 一律開啟 」 VPN 原生支援使用 IPv4 和 IPv6 的雙重堆疊方法中。 它並沒有特定相依性一個通訊協定透過另一個，以達到最大的 IPv4/IPv6 應用程式相容性，結合網路需求適用於未來的 IPv6 支援。<p>**_請注意。_** 開始之前，請確定 VPN 伺服器上啟用 IPv6。 否則，無法建立連線，並會顯示錯誤訊息。|
| **應用程式特定的路由原則** | 除了定義全域 VPN 連線的路由原則網際網路和內部網路流量分隔，就能夠新增路由原則，控制使用的分割通道或強制通道設定，針對每個應用程式。 此選項可讓您更細微的控制哪些應用程式允許透過 VPN 通道的資源進行互動。 |
| **排除路由** | 一律開啟 」 VPN 支援讓您指定具體控制路由行為來定義哪些流量應該周遊僅 VPN，並不會經過實體網路介面的排除路由。<p><p>_**附註。**_<br>-排除路由目前可用的用戶端，相同的子網路內的流量，例如 LinkLocal。<br>-排除路由只運作的分割通道設定中。 |
---

## <a name="configuration-and-compatibility"></a>組態和相容性 

以下是一些一律開啟 」 VPN 的組態和相容性增強功能：

| 重要的改進 | 描述 |
| ---- | ---- |
| **第三方 VPN 閘道相容性** | 一律開啟 」 VPN 用戶端不需要使用 Microsoft 為基礎的 VPN 閘道運作。 透過 IKEv2 通訊協定的支援，用戶端可加速互通性與支援此業界標準的通道類型的第三方 VPN 閘道。 您也可以使用 UWP VPN 外掛程式結合自訂的通道型別，而不會犧牲一律開啟 」 VPN 平台功能和優點，來達到互通性與第三方 VPN 閘道。<p><p>_**請注意。**_<br>請洽詢您的閘道或協力廠商後端設備廠商組態和相容性與一律開啟 」 VPN 裝置通道使用 ikev2 做。 |
| **業界標準 IKEv2 VPN 通訊協定支援** | 一律開啟 」 VPN 用戶端支援 IKEv2，現今的其中一個最常使用業界標準的通道通訊協定。 此相容性最大化與第三方 VPN 閘道的互通性。 |
| **平台支援** | AAlways 上 VPN 支援已加入網域的、 非網域聯結 （工作群組） 或 Azure AD – 已加入的裝置，才能進行這兩個企業及攜帶您自己的裝置 (BYOD) 案例。 此外，一律開啟 」 VPN 是適用於所有 Windows 版本。 |
| **不同的管理和部署機制** | 您可以使用許多管理和部署機制來管理 VPN 設定 (稱為_VPN 設定檔_)，包括 Windows PowerShell、 System Center Configuration Manager、 Intune （或協力廠商行動裝置管理 [MDM] 工具）與 Windows 設定設計工具。 這些選項來簡化一律開啟 」 VPN 的設定，無論您使用用戶端管理工具。 |
| **標準化的 VPN 設定檔定義** | 一律開啟 」 VPN 支援使用標準的 XML 設定檔 (ProfileXML)，提供大部分的管理和部署工具組使用的標準設定範本格式。 |
---


## <a name="next-steps"></a>後續步驟

- [深入了解一些進階的一律開啟 」 VPN 功能](deploy/always-on-vpn-adv-options.md)

- [深入了解一律開啟 」 VPN 技術](always-on-vpn-technology-overview.md)

- [開始規劃您的一律開啟 」 VPN 部署](deploy/always-on-vpn-deploy-deployment.md)



---
