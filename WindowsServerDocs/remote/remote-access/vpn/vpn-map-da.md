---
title: Always On VPN 功能
description: 本主題中，您會了解的 Always On VPN 的功能。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.localizationpriority: medium
ms.date: 11/05/2018
ms.assetid: 8fe1c810-4599-4493-b4b8-73fa9aa18535
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 68d8561eb55b844a80c8b6a38d1255ad44457af6
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066852"
---
# Always On VPN 特性與功能

>適用於： Windows Server \(Semi-Annual Channel\)，Windows Server 2016、 Windows 10

& #171; [**先前：** 一律開啟 VPN 部署適用於 Windows Server 和 Windows 10](always-on-vpn/deploy/always-on-vpn-deploy.md)<br>
& #187;[**下一步：** 深入了解一律開啟的 VPN 增強功能](../vpn/always-on-vpn/always-on-vpn-enhancements.md)

本主題中，您會了解特色和功能的 Always On VPN。  下表不是完整清單，不過，它沒有包含一些最常見的特色和遠端存取解決方案中使用的功能。 

>[!TIP]
>如果您目前使用 DirectAccess，我們建議您調查 Always On VPN 功能，請仔細以判斷它可解決您的遠端存取需求，之後再移轉的所有形成 DirectAccess 來 Always On VPN。  

| 功能區域 | Always On VPN  |
| ---- | ---- |
| 無縫、 透明連線至公司網路。 | 您可以設定 Always On VPN 以支援自動觸發根據應用程式啟動或命名空間解析的要求。<p><p>定義使用：<br>**VPNv2/ProfileName/AlwaysOn**<br>**VPNv2/ProfileName/AppTriggerList**<br>**VPNv2/ProfileName/來設定/AutoTrigger** |
| 使用專用的基礎結構通道來提供使用者未登入公司網路連線。 | 您可以在 VPN 設定檔中使用裝置通道功能來達成這項功能。<p><p>_**注意。**_<br>裝置通道只能在電腦憑證驗證搭配使用 IKEv2 加入網域的裝置上進行設定。<p><p>定義使用：<br>**VPNv2/ProfileName/DeviceTunnel** |
| 管理跨以允許遠端連線至用戶端，從公司網路上的管理系統使用。 | 您可以結合設定 VPN 連線的 VPN 設定檔中使用裝置通道功能為動態註冊已指派給 VPN 介面透過內部 DNS 服務 IP 位址來達成這項功能。<p><p>_**注意。**_<br>如果您開啟裝置通道設定檔中的流量篩選器時，裝置通道會拒絕輸入的流量 （從用戶端的公司網路）。<p><p>定義使用：<br>**VPNv2/ProfileName/DeviceTunnel**<br>**VPNv2/ProfileName/RegisterDNS** |
| 落回時防火牆或代理伺服器背後，都有用戶端。 | 您可以透過使用自動的通道/通訊協定類型，在 VPN 設定檔設定切換回 SSTP （從 IKEv2)。<p><p>_**注意。**_<br>使用者通道支援 SSTP 和 IKEv2，而裝置通道支援 IKEv2 只能在採用 SSTP 後援不支援。<p>定義使用：<br>**VPNv2/ProfileName/NativeProfile/NativeProtocolType** |
| 結束到邊緣存取模式的支援。 | Always On VPN 使用需要驗證與加密，直到到達 VPN 閘道的通道原則來提供連線至公司資源。 根據預設，通道工作階段終止於 VPN 閘道，這也可以當做 IKEv2 閘道，提供結束到邊緣安全性。 |
| 電腦憑證驗證的支援。 | IKEv2 通訊協定類型可 Always On VPN 平台的一部分進行 VPN 驗證專門支援的電腦或電腦憑證使用方式。<p><p>_**注意。**_<br>IKEv2 通道裝置的唯一支援的通訊協定，且沒有 SSTP 遞補用的支援選項。 <p>定義使用：<br>**VPNv2/ProfileName/NativeProfile/驗證/MachineMethod** |
| 您可以使用安全性群組來限制給特定的用戶端的遠端存取功能。 | 您可以設定 Always On VPN 使用半徑，包括使用安全性群組來控制 VPN 存取時，支援細微的授權。 |
| Edge 防火牆或 NAT 裝置的伺服器的支援。 | Always On VPN 可讓您使用像是 IKEv2 和 SSTP 完全支援的 NAT 裝置或 edge 防火牆後方的 VPN 閘道使用的通訊協定。<p><p>_**注意。**_<br>使用者通道支援 SSTP 和 IKEv2，而裝置通道支援 IKEv2 只能在採用 SSTP 後援不支援。 |
| 若要判斷當連接到公司網路的內部網路連線能力。 | 受信任的網路偵測提供偵測公司網路連線的功能，它根據連線特定 DNS 尾碼被指派給網路介面和網路設定檔的評定。<p><p>定義使用：<br>**VPNv2/ProfileName//trustednetworkdetection** |
| 使用網路存取保護 (NAP) 的相容性。 | Always On VPN 用戶端可以與 Azure MFA、 裝置合規性或將兩者結合強制執行的條件式存取整合。 當使用條件式存取原則符合規範時，Azure AD 會發行用戶端可以接著用來驗證 VPN 閘道 IPsec 短期 （根據預設，60 分鐘的時間） 驗證憑證。 裝置相容性會利用 System Center Configuration Manager/Intune 合規性原則可以包含裝置健康情況證明狀態。 在這個時候，Azure VPN 的條件式存取提供最接近取代現有的 NAP 解決方案，但沒有任何形式的補救服務或隔離網路功能。 如需詳細資訊，請參閱[VPN 和條件式存取](https://docs.microsoft.com/en-us/windows/security/identity-protection/vpn/vpn-conditional-access)。<p>定義使用：<br>**VPNv2/ProfileName/DeviceCompliance** |
| 定義使用者登入前存取哪些管理伺服器的能力。 | 您可以達到 Always On VPN 中的此功能結合流量篩選器來控制可透過存取哪些管理系統，公司網路上的 VPN 設定檔中使用裝置通道功能 （適用於版本 1709 – 適用於 IKEv2 只）裝置通道。<p><p>_**注意。**_<br>如果您開啟裝置通道設定檔中的流量篩選器時，裝置通道會拒絕輸入的流量 （從用戶端的公司網路）。<p>定義使用：<br>**VPNv2/ProfileName/DeviceTunnel**<br>**VPNv2/ProfileName/TrafficFilterList** |
---

## 其他功能

在本節中的每個項目是使用狀況或常使用的遠端存取功能的 Always On VPN 已經改良功能 — 透過擴充功能或消除先前的限制。


| 功能區域 | Always On VPN  |
| ---- | ---- |
| 加入網域的裝置與企業 Sku 需求。 | Always On VPN 支援已加入網域、 未加入網域的 （工作群組） 或加入 Azure AD – 裝置允許的企業與 BYOD 案例。 Always On VPN 是適用於所有 Windows 版本，並可供協力廠商透過支援 UWP VPN 外掛程式平台功能。<p><p>_**注意。**_<br>裝置通道只能在執行 Windows 10 企業版或教育版版本 1709年或更新版本的加入網域的裝置上進行設定。 沒有支援的裝置通道的第三方控制項。 |
| IPv4 與 IPv6 的支援。 | 透過永遠開啟的 VPN，使用者可以存取公司網路上的 IPv4 與 IPv6 資源。 Always On VPN 用戶端使用雙重堆疊的方法不會特別取決於 IPv6 或針對 VPN 閘道需要提供 NAT64 或 DNS64 翻譯服務。 |
| 雙因素或 OTP 驗證的支援。 |Always On VPN 平台原生支援 EAP，允許使用的各種 Microsoft 和協力廠商 EAP 類型做為驗證工作流程的一部分。 Always On VPN 專門支援智慧卡 （實體和虛擬） 和 Windows Hello 企業憑證來滿足雙因素驗證的需求。 此外，Always On VPN 支援 OTP 透過 MFA （不支援原生，才支援第三方外掛程式上） 透過 EAP 半徑整合。<p><p>定義使用：<br>**VPNv2/ProfileName/NativeProfile/驗證** |
| 支援多個網域和樹系。 | Always On VPN 平台上 Active Directory 網域服務 (AD DS) 的樹系或網域拓撲 （或相關功能的/結構描述層級） 有任何相依性，因為它不需要 VPN 用戶端函式來加入網域。 群組原則不因此來定義 VPN 設定檔設定，因為您不使用它在用戶端設定期間的相依性。 其中 Active Directory 授權整合是必要的您可以透過 EAP 驗證及授權程序的半徑達成這個目的。 |
| 支援這兩個分割並強制通道的網際網路/內部網路流量的區隔。 | 您可以設定 Always On VPN 支援這兩個強制通道 （預設作業模式） 和原生分割通道。 Always On VPN 提供額外的細微性，適用於特定應用程式的路由原則。<p><p>_**注意。**_<br>只有使用者通道的支援。<p><p>定義使用：<p> **VPNv2/ProfileName/NativeProfile/RoutingPolicyType**<br>**VPNv2/ProfileName/TrafficFilterList/應用程式/RoutingPolicyType** |
| 多個通訊協定支援。 | Always On VPN 可以設定為原生支援 SSTP，如果需要從 IKEv2 後援的 [安全通訊端層中。<p><p>_**注意。**_<br>使用者通道支援 SSTP 和 IKEv2，而裝置通道支援 IKEv2 只能在採用 SSTP 後援不支援。  |
| 若要提供公司的連線狀態資料連線小幫手。 | Always On VPN 使用原生網路連線能力小幫手 」 完全整合，並提供從檢視所有網路介面的連線狀態。 與 Windows 10 Creators Update （版本 1703年） 的到來、 VPN 連線狀態以及使用者通道的 VPN 連線控制都現在可透過網路飛出視窗 （適用於 Windows 內建 VPN 用戶端），以及。 |
| 使用簡短名稱、 完整的網域名稱 (FQDN)，以及 DNS 尾碼公司資源的名稱解析。 | Always On VPN 原生可以定義一或多個 DNS 尾碼 VPN 連線與 IP 位址指派程序，包括公司資源的簡短名稱、 Fqdn 或整個 DNS 命名空間的名稱解析的一部分。 Always On VPN 也支援使用的名稱解析原則表格，以提供特定命名空間的解析度資料粒度。<p><p>_**注意。**_<br>因為它們會干擾 shortname 解析度使用名稱解析原則表格時，請避免使用的全域尾碼。<p><p>定義使用：<br>**VPNv2/ProfileName/DnsSuffix**<br>**VPNv2/ProfileName/來設定** |
---



## 後續步驟

- [深入了解 Always On VPN 增強功能](always-on-vpn/always-on-vpn-enhancements.md)

- [深入了解的一些進階 Always On VPN 功能](always-on-vpn/deploy/always-on-vpn-adv-options.md)

- [深入了解 Always On VPN 技術](always-on-vpn/always-on-vpn-technology-overview.md)

- [開始規劃您的 Always On VPN 部署](always-on-vpn/deploy/always-on-vpn-deploy-deployment.md)

---
