---
title: Always On VPN 增強功能
description: Always On VPN 過去的 Windows VPN 解決方案有許多好處。 重要的改進功能中整合、 安全性、 連線、 網路功能的控制項及相容性符合 Microsoft 的雲端優先、 行動優先願景 Always On VPN。
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 11/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: d7844d93ac898316580123c82e08bb6f02d51a2b
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067138"
---
# Always On VPN 增強功能

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows 10

& #171;[**前一個：** 了解有關 Always On VPN 功能](../vpn-map-da.md)<br>
& #187;[**下一步：** 了解有關 Always On VPN 技術](always-on-vpn-technology-overview.md)

Always On VPN 過去的 Windows VPN 解決方案有許多好處。 下列重要的改進功能與 Microsoft 的雲端優先、 行動優先願景一致 Always On VPN:

- **平台整合：** Always On VPN 有與 Windows 作業系統和協力廠商解決方案，以提供等無以計數的進階的連線案例健全的平台改善的整合。

- **安全性：** Always On VPN 有新的進階安全性功能，以限制的流量，哪些應用程式可以使用 VPN 連線，類型，您可以使用的驗證方法來起始連線。 連線時 active 大部分時候，它是特別重要到安全的連線。 如需詳細資訊，請參閱[VPN 驗證選項](https://docs.microsoft.com/en-us/windows/security/identity-protection/vpn/vpn-authentication)。

- **VPN 連線：** Always On VPN，使用或不使用裝置通道提供自動觸發程序的能力。 Always On VPN 之前, 不可能觸發使用者或裝置驗證透過自動連線的能力。  

- **網路功能控制項：** Always On VPN 能讓系統管理員指定更細微層級的路由原則 — 甚至到個別的應用程式 — 也就是完美的特定業務 (LOB) 應用程式可能需要特殊的遠端存取。  Always On VPN 也是使用這兩個網際網路通訊協定第 4 版 (IPv4) 完全相容和第 6 版 (IPv6)。 不同於 DirectAccess，沒有任何特定相依性 IPv6 上。

  >[!Note]
  >在您開始之前，請確定在 VPN 伺服器上啟用 IPv6。 否則，無法建立的連線，並顯示錯誤訊息。
  
- **組態和相容性：** Always On VPN 可以進行部署和管理數種方式，讓其他 VPN 用戶端軟體的數個優於 Always On VPN。

## 平台整合

Microsoft 已導入，或改善 Always On VPN 中的下列整合功能：

| 索引鍵的改進 | 描述 |
| ---- | ---- |
| **[Windows 資訊保護 (WIP)](https://docs.microsoft.com/windows/threat-protection/windows-information-protection/protect-enterprise-data-using-wip)** | 與 WIP 整合可讓網路原則強制執行，以判斷是否允許流量通過 VPN。 如果是有效的使用者設定檔，並會套用 WIP 原則，Always On VPN 自動觸發連線。 此外，當您使用 WIP，則不需要指定 AppTriggerList 和 TrafficFilterList 規則分別在 VPN 設定檔中 （除非您想要更多進階的設定） 因為 WIP 原則和應用程式清單會自動生效。 |
| **[Windows Hello 企業版](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-overview)** | Always On VPN 原生支援 Windows Hello 企業版 （在憑證式驗證模式） 為這兩個登入到電腦並連線至 VPN，提供順暢單一登入體驗。 因此，沒有任何次要驗證 （使用者認證） 所需的 VPN 連線，這樣就可以使用一律開啟連線搭配 Windows Hello 企業版驗證。 |
| **[Microsoft Azure 條件式存取](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls)** | Always On VPN 用戶端可以與強制執行多重要素驗證 (MFA)、 裝置合規性或兩者的組合 Azure 的條件式存取平台整合。 當使用條件式存取原則符合規範時，Azure Active Directory (Azure AD) 問題然後可以用來驗證 VPN 閘道的短期 （根據預設，60 分鐘的時間） IP 安全性 (IPsec) 驗證憑證。 裝置相容性會使用 System Center Configuration Manager/Intune 合規性原則可以包含裝置健康情況證明狀態，做為連接相容性檢查的一部分。 |
| **Azure MFA** | 當使用遠端驗證撥入使用者服務 (RADIUS) 服務和網路原則伺服器 (NPS) 延伸模組結合多個 Azure MFA，VPN 驗證可以使用強式 MFA。 |
| **協力廠商 VPN 外掛程式** | 使用通用 Windows 平台 (UWP)，協力廠商 VPN 提供者可以建立單一應用程式的完整範圍的 Windows 10 的裝置。 UWP 提供保證的核心 API 層跨裝置，這可消除的複雜性和問題通常與寫入核心層級驅動程式關聯。 目前，Windows 10 UWP VPN 外掛程式的方式存在的[Pulse Secure](https://www.microsoft.com/p/pulse-secure/9nblggh3b0bp)、 [F5 Access](https://www.microsoft.com/p/f5-access/9wzdncrdsfn0)、 [Check Point Capsule VPN\](https://www.microsoft.com/p/check-point-capsule-vpn/9wzdncrdjxtj)、 [FortiClient](https://www.microsoft.com/p/forticlient/9wzdncrdh6mc)、 [SonicWall Mobile Connect](https://www.microsoft.com/p/sonicwall-mobile-connect/9wzdncrdsfkz)及[GlobalProtect](https://www.microsoft.com/p/globalprotect/9nblggh6bzl3);毫無疑問，其他則會出現在未來。 |
---

## 安全性

安全性改良的主要是在下列區域中：

| 索引鍵的改進 | 描述 |
| ---- | ---- |
| **流量篩選器** | 透過流量篩選器，您可以指定決定允許哪些流量進入公司網路的用戶端原則。 如此一來，系統管理員可以限制應用程式或流量上套用 VPN 介面，限制其使用特定的來源、 目的地連接埠，以及 IP 位址。 可用的兩種類型的篩選規則如下：<ul><li>**以 App 為基礎的規則。** 應用程式為基礎的防火牆規則根據指定的應用程式的清單，以便允許來自這些應用程式的流量通過 VPN 介面。</li><li>**以流量為基礎的規則。** 以流量為基礎的防火牆規則以網路需求，例如連接埠、 位址和通訊協定為基礎。 允許這些規則只針對符合這些特定條件的流量通過 VPN 介面的使用。<p><p>_**注意。**_<br>這些規則僅適用於輸出流量從裝置。 使用的流量篩選器區塊輸入的流量從公司網路用戶端。 </li></ul> |
| **每個應用程式 VPN** | 每個應用程式 VPN 在像是為應用程式為基礎的流量篩選器，但它遠移至應用程式觸發程序結合為應用程式為基礎的流量篩選器，以便 VPN 連線會被限制為特定的應用程式，而不是在 VPN 用戶端上的所有應用程式。 在 app 啟動時，自動起始功能。 |
| **自訂 IPsec 密碼編譯演算法的支援** | Always On VPN 支援使用 RSA 與橢圓曲線密碼編譯型自訂密碼編譯演算法符合嚴格的政府或組織的安全性原則。 |
| **原生可延伸驗證通訊協定 (EAP) 支援** | Always On VPN 原生支援 EAP，可讓您使用不同組 Microsoft 和協力廠商 EAP 類型做為驗證工作流程的一部分。 EAP 提供安全的驗證，根據下列驗證類型：<ul><li>使用者名稱和密碼</li><li>智慧卡 （實體和虛擬）</li><li>使用者憑證</li><li>Windows Hello 企業版</li><li>透過 EAP 半徑整合的 MFA 支援</li></ul>應用程式廠商會控制第三方 UWP VPN 外掛程式的驗證方法，雖然它們有可用的選項，包括自訂認證類型和 OTP 支援的陣列。 |
---

## VPN 連線

以下是 Always On VPN 連線中的主要改良功能：

| 索引鍵的改進 | 描述 |
| ---- | ---- |
| **一律開啟** | 一律開啟 」 是 Windows 10 功能，可讓作用中的 VPN 設定檔，以自動連線並保持連線根據觸發程序 — 也就是使用者登入、 網路狀態變更或裝置的螢幕，作用中。 一律開啟已也整合到已連接的待命體驗，以延長電池壽命。 |
| **應用程式觸發** | 您可以在 Windows 10 上的一組指定的應用程式啟動時自動連線來設定 VPN 設定檔。 您可以設定桌面和 UWP 應用程式觸發 VPN 連線。 |
| **名稱型觸發** | 透過永遠開啟的 VPN，您可以定義規則，讓特定網域名稱查詢觸發 VPN 連線。 Windows 10 現已支援用於已加入網域及未加入網域的電腦名稱型觸發 （以前，只有未加入網域的電腦已支援）。 |
| **受信任的網路偵測** | Always On VPN 包含這項功能，以確保使用者連線至受信任的網路在公司界限內，才不會觸發 VPN 連線。 您可以使用任何先前所述，以提供順暢的 「 僅限連線時所需 「 使用者體驗的觸發方法結合這項功能。 |
| **[裝置通道](../vpn-device-tunnel-config.md)** | Always On VPN 可讓您能夠建立專用的 VPN 設定檔的裝置或電腦。 不同於_使用者通道_，這只連接之後有使用者登入的裝置或電腦時，_裝置通道_可讓 VPN 建立使用者登入前的連線能力。 裝置通道和使用者通道都獨立運作以其 VPN 設定檔、 可在此同時，連線和可以適當地使用不同的驗證方法及其他 VPN 組態設定。 |
---

## 網路

以下是一些改良 Always On VPN 的網路功能：

| 索引鍵的改進 | 描述 |
| ---- | ---- |
| **IPv4 與 IPv6 的雙堆疊支援** | Always On VPN 原生支援使用 IPv4 和 IPv6 雙重堆疊方法中。 它並沒有特定相依性一個通訊協定透過其他，也能結合支援未來的 IPv6 網路功能需求的最大 IPv4/IPv6 應用程式相容性。<p>**_注意。_** 在您開始之前，請確定在 VPN 伺服器上啟用 IPv6。 否則，無法建立的連線，並顯示錯誤訊息。|
| **應用程式特定的路由原則** | 除了定義的全域 VPN 連線的路由原則網際網路及內部網路流量的區隔，它可能會新增控制分割通道的使用，或強制通道設定每個應用程式為基礎的路由原則。 此選項可讓您更細微的控制哪些應用程式可透過 VPN 通道哪些資源進行互動。 |
| **排除路由** | Always On VPN 可讓您指定特別控制路由行為來定義哪些流量應該周遊 VPN 只和未通過實體網路介面的排除路由。<p><p>_**附註。**_<br>-排除路由目前適用於做為用戶端，相同的子網路內的流量，例如 LinkLocal。<br>-排除路由只能在分割通道設定中使用。 |
---

## 設定和相容性 

以下是一些 Always On VPN 改良的組態與相容性：

| 索引鍵的改進 | 描述 |
| ---- | ---- |
| **協力廠商 VPN 閘道相容性** | Always On VPN 用戶端不需要 Microsoft 型 VPN 閘道操作的使用。 透過 IKEv2 通訊協定的支援，用戶端，協助與支援此業界標準通道類型的協力廠商 VPN 閘道的互通性。 您也可以使用 UWP VPN 外掛程式結合自訂通道類型，而不犧牲 Always On VPN 平台功能與優點，來達成與協力廠商 VPN 閘道的互通性。<p><p>_**注意。**_<br>請洽詢您的閘道或第三方後端應用裝置廠商上設定與相容性 Always On VPN 和使用 IKEv2 裝置通道。 |
| **業界標準 IKEv2 VPN 通訊協定支援** | Always On VPN 用戶端支援 IKEv2、 現今的其中一個最廣泛使用業界標準的通道通訊協定。 這個相容性最大化與協力廠商 VPN 閘道的互通性。 |
| **平台支援** | 已加入網域的 AAlways On VPN 支援、 未加入網域的 （工作群組） 或加入 Azure AD – 裝置允許適用於這兩個企業和攜帶您自己的裝置 (BYOD) 案例。 此外，Always On VPN 是適用於所有 Windows 版本。 |
| **各種不同的管理和部署機制** | 您可以使用許多管理和部署機制來管理 VPN 設定值 （稱為 「 _VPN 設定檔_），包括 Windows PowerShell，System Center Configuration Manager、 Intune （或第三方行動裝置管理 [MDM] 工具），以及 Windows設定設計工具。 這些選項來簡化 Always On VPN 的設定，無論您使用的用戶端管理工具。 |
| **標準化的 VPN 設定檔定義** | Always On VPN 支援設定，使用標準的 XML 設定檔 (ProfileXML)，提供大部分的管理和部署工具組會使用標準設定範本格式。 |
---


## 後續步驟

- [深入了解的一些進階 Always On VPN 功能](deploy/always-on-vpn-adv-options.md)

- [深入了解 Always On VPN 技術](always-on-vpn-technology-overview.md)

- [開始規劃您的 Always On VPN 部署](deploy/always-on-vpn-deploy-deployment.md)



---
