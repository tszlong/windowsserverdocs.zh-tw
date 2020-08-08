---
title: Always On VPN 增強功能
description: Always On VPN 在過去的 Windows VPN 解決方案方面有許多好處。 整合、安全性、連線能力、網路控制及相容性的主要改進 Always On VPN 與 Microsoft 的雲端優先、行動優先願景。
ms.topic: article
ms.author: v-tea
author: Teresa-MOTIV
ms.date: 11/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: db2d33f2452d7fddffef5458d0e80954b3603a48
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87958236"
---
# <a name="always-on-vpn-enhancements"></a>Always On VPN 增強功能

>適用于： Windows Server (半年通道) 、Windows Server 2016、Windows 10

- [**上一步：** 瞭解 Always On 的 VPN 功能](../vpn-map-da.md)
- [**下一步：** 瞭解 Always On VPN 技術](always-on-vpn-technology-overview.md)

Always On VPN 在過去的 Windows VPN 解決方案方面有許多好處。 下列重要的改良功能，與 Microsoft 的雲端優先、行動優先願景 Always On 的 VPN 一致：

- **平臺整合：** Always On VPN 已改善與 Windows 作業系統和協力廠商解決方案的整合，為無數的先進連線案例提供強大的平臺。

- **安全性：** Always On VPN 具有全新、先進的安全性功能，可限制流量類型、哪些應用程式可以使用 VPN 連線，以及您可以使用哪些驗證方法來起始連線。 當連接在大多數時間都處於作用中狀態時，請務必保護連接的安全。 如需詳細資訊，請參閱[VPN 驗證選項](/windows/security/identity-protection/vpn/vpn-authentication)。

- **VPN 連線能力：** Always On VPN，不論裝置通道是否提供自動觸發程式功能。 在 Always On VPN 之前，無法透過使用者或裝置驗證觸發自動連線的功能。

- **網路控制：** Always On VPN 可讓系統管理員以更細微的層級（甚至是個別的應用程式）指定路由原則，這對企業營運 (LOB) 需要特殊遠端存取的應用程式而言是最理想的選擇。  Always On VPN 也會與網際網路通訊協定第4版 (IPv4) 和第6版 (IPv6) 完全相容。 與 DirectAccess 不同的是，IPv6 沒有特定的相依性。

  >[!NOTE]
  >開始之前，請務必在 VPN 伺服器上啟用 IPv6。 否則，就無法建立連接，並顯示錯誤訊息。

- 設定**和相容性：** 您可以透過數種方式來部署和管理 Always On VPN，讓 Always On VPN 與其他 VPN 用戶端軟體相比有數個優點。

## <a name="platform-integration"></a>平臺整合

Microsoft 已在 Always On VPN 中引進或改良了下列整合功能：


| 金鑰改進   |  描述  |
|----------------|---|
| **[Windows 資訊保護 (WIP)](/windows/threat-protection/windows-information-protection/protect-enterprise-data-using-wip)** | 與 WIP 的整合可讓網路原則強制執行，以判斷流量是否允許通過 VPN。 如果使用者設定檔為作用中，且已套用 WIP 原則，則會自動觸發 Always On VPN 以進行連線。 此外，當您使用 WIP 時，除非您想要更先進的設定) ，否則不需要分別在 VPN 設定檔 (中指定 AppTriggerList 和 TrafficFilterList 規則，因為 WIP 原則和應用程式清單會自動生效。 |
|**[Windows Hello 企業版](/windows/access-protection/hello-for-business/hello-overview)** |Always On VPN 原本就支援以憑證為基礎的驗證模式中的 Windows Hello 企業版 () 提供無縫單一登入體驗，可讓您登入電腦和 VPN 連線。 因此，VPN 連線不需要 (使用者認證) 進行次要驗證，因此可以使用與 Windows Hello 企業版驗證的 Always On 連線。 |
| **[Microsoft Azure 條件式存取](/azure/active-directory/active-directory-conditional-access-controls)**  |Always On VPN 用戶端可以與 Azure 條件式存取平臺整合，以強制執行多重要素驗證 (MFA) 、裝置合規性或兩者的組合。 與條件式存取原則相容時，Azure Active Directory (Azure AD) 預設會發出短期的 (60 分鐘) IP 安全性 (IPsec) 驗證憑證，以用來驗證 VPN 閘道。 裝置合規性會使用 Configuration Manager/Intune 合規性原則，其中可以包含裝置健康情況證明狀態作為連線合規性檢查的一部分。|
|  **Azure MFA** |當與遠端驗證撥入使用者服務結合時 (RADIUS) 服務和網路原則伺服器 (適用于 Azure MFA 的 NPS) 延伸模組，VPN 驗證可以使用強式 MFA。 | **協力廠商 VPN 外掛程式**  | 有了通用 Windows 平臺 (UWP) ，協力廠商 VPN 提供者可以為完整範圍的 Windows 10 裝置建立單一應用程式。 UWP 在各種裝置上提供保證的核心 API 層，以消除通常與寫入核心層級驅動程式相關的問題複雜度。 目前，Windows 10 UWP VPN 外掛程式存在於[脈衝安全](https://www.microsoft.com/p/pulse-secure/9nblggh3b0bp)、 [F5 存取](https://www.microsoft.com/p/f5-access/9wzdncrdsfn0)、 [Check Point 膠囊 VPN](https://www.microsoft.com/p/check-point-capsule-vpn/9wzdncrdjxtj)、 [FortiClient](https://www.microsoft.com/p/forticlient/9wzdncrdh6mc)、 [SonicWall Mobile Connect](https://www.microsoft.com/p/sonicwall-mobile-connect/9wzdncrdsfkz)和[GlobalProtect](https://www.microsoft.com/p/globalprotect/9nblggh6bzl3);不確定，未來將會出現其他的情況。 |

## <a name="security"></a>安全性

安全性的主要改良功能位於下欄區域：

| 金鑰改進 | 描述  |
|---|---|
| **流量篩選器** | 透過流量篩選器，您可以指定用戶端原則，以決定允許進入公司網路的流量。 如此一來，系統管理員可以在 VPN 介面上套用應用程式或流量限制，以限制其對特定來源、目的地埠和 IP 位址的使用。 有兩種類型的篩選規則可供使用：<ul><li>**以 App 為基礎的規則。** 以應用程式為基礎的防火牆規則是以指定的應用程式清單為基礎，只允許來自這些應用程式的流量通過 VPN 介面。</li><li>**以流量為基礎的規則。** 以流量為基礎的防火牆規則是以網路需求（例如埠、位址和通訊協定）為基礎。 這些規則僅適用于符合這些特定條件的流量，允許通過 VPN 介面。<p><p>***注意：***<br>這些規則僅適用于從裝置輸出的流量。 使用流量篩選器會封鎖從公司網路到用戶端的輸入流量。 </li></ul> |**個別應用程式 VPN**|個別應用程式 VPN 就像具有以應用程式為基礎的流量篩選，但它會更遠地結合應用程式觸發程式與以應用程式為基礎的流量篩選準則，因此 VPN 連線限制為特定的應用程式，而不是 VPN 用戶端上的所有應用程式。 此功能會在應用程式啟動時自動起始。|
|  **支援自訂的 IPsec 密碼編譯演算法**   |  Always On VPN 支援使用以 RSA 和橢圓曲線密碼編譯為基礎的自訂密碼編譯演算法，以符合嚴格的政府或組織安全性原則。|
| **原生可延伸驗證通訊協定 (EAP) 支援** |Always On VPN 原本就支援 EAP，這可讓您使用一組不同的 Microsoft 和協力廠商 EAP 類型做為驗證工作流程的一部分。 EAP 提供以下列驗證類型為基礎的安全驗證：<ul><li>使用者名稱和密碼</li><li>智慧卡 (實體和虛擬) </li><li>使用者憑證</li><li>Windows Hello 企業版</li><li>透過 EAP RADIUS 整合方式的 MFA 支援</li></ul>應用程式廠商會控制協力廠商 UWP VPN 外掛程式驗證方法，雖然它們有可用選項的陣列，包括自訂認證類型和 OTP 支援。|

## <a name="vpn-connectivity"></a>VPN 連線能力

以下是 Always On VPN 連線能力的主要改良功能：

|  金鑰改進  | 描述 |
|---|---|
|                    **Always On**                    |                                                                                          Always On 是一項 Windows 10 功能，可讓使用中的 VPN 設定檔自動連接，並根據觸發程式保持線上狀態，也就是使用者登入、網路狀態變更或裝置螢幕作用中。 Always On 也會整合到連線待命體驗中，以達到最高的電池壽命。                                                                                           |
|             **應用程式觸發**              |                                                                                                                                          您可以將 Windows 10 中的 VPN 設定檔設定為在一組指定的應用程式啟動時自動連接。 您可以設定桌面和 UWP 應用程式來觸發 VPN 連線。                                                                                                                                          |
|              **以名稱為基礎的觸發**              |                                                                                                            透過 Always On VPN，您可以定義規則，讓特定的功能變數名稱查詢觸發 VPN 連線。 Windows 10 現在支援以名稱為基礎的觸發程式，適用于已加入網域和未加入網域的電腦 (先前只支援) 的未聯結電腦。                                                                                                            |
|            **受信任的網路偵測**            |                                                                                    Always On VPN 包含這項功能，可確保當使用者連線至公司界限內的受信任網路時，不會觸發 VPN 連線能力。 您可以將此功能與先前所述的任何觸發方法結合，以提供順暢的「只在需要時連接」使用者體驗。                                                                                     |
| **[裝置通道](../vpn-device-tunnel-config.md)** | Always On VPN 可讓您為裝置或電腦建立專用的 VPN 設定檔。 不同*于使用者通道*，只有在使用者登入裝置或電腦之後才會連線，*裝置*通道可讓 VPN 在使用者登入前建立連線能力。 裝置通道和使用者通道都是獨立運作的 VPN 設定檔，可以同時連線，並且可以適當地使用不同的驗證方法和其他 VPN 設定。 |

## <a name="networking"></a>網路功能

以下是 Always On VPN 中的一些網路增強功能：


|              金鑰改進              |                                                                                                                                                                                                                描述                                                                                                                                                                                                                |
|-------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **IPv4 和 IPv6 的雙重堆疊支援**  | Always On VPN 原本就支援在雙重堆疊方法中使用 IPv4 和 IPv6。 它對另一個通訊協定沒有任何特定的相依性，它允許最大的 IPv4/IPv6 應用程式相容性，並支援未來的 IPv6 網路需求。<p>***注意：*** 開始之前，請務必在 VPN 伺服器上啟用 IPv6。 否則，就無法建立連接，並顯示錯誤訊息。 |
| **應用程式特定的路由原則** |                            除了定義網際網路和內部網路流量隔離的全域 VPN 連線路由原則之外，您也可以新增路由原則來控制分割通道的使用，或強制以每個應用程式為基礎的通道設定。 此選項可讓您更細微地控制哪些應用程式可以透過 VPN 通道與哪些資源互動。                             |
|           **排除路由**            |                 Always On VPN 支援指定排除路由的能力，以明確控制路由行為，以定義哪些流量應該只通過 VPN，而不是透過實體網路介面。<p><p>***注意：***<br>-排除路由目前適用于與用戶端相同的子網內的流量，例如 LinkLocal。<br>-排除路由只適用于分割通道設定。                  |

## <a name="configuration-and-compatibility"></a>設定和相容性

以下是 Always On VPN 中的一些設定和相容性改進：


|                 金鑰改進                  |                                                                                                                                                                                                                                                                                                                       描述                                                                                                                                                                                                                                                                                                                       |
|--------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    **協力廠商 VPN 閘道相容性**     | Always On 的 VPN 用戶端不需要使用以 Microsoft 為基礎的 VPN 閘道來操作。 透過 IKEv2 通訊協定的支援，用戶端可協助與支援此產業標準通道類型的協力廠商 VPN 閘道互通。 您也可以使用與自訂通道類型結合的 UWP VPN 外掛程式，達到與協力廠商 VPN 閘道的互通性，而不會犧牲 Always On 的 VPN 平臺功能和優點。<p><p>***注意：***<br>請洽詢您的閘道或協力廠商後端設備廠商，以使用 IKEv2 Always On VPN 和裝置通道的設定和相容性。 |
| **業界標準 IKEv2 VPN 通訊協定支援** |                                                                                                                                                                                                                              Always On 的 VPN 用戶端支援 IKEv2，這是現今最普遍使用的業界標準通道通訊協定之一。 這種相容性最大化與協力廠商 VPN 閘道的互通性。                                                                                                                                                                                                                               |
|               **平台支援**               |                                                                                                                                                                                                           Always On VPN 支援已加入網域、未加入網域的 (workgroup) ，或已加入 Azure AD 的裝置，以允許這兩個企業和攜帶您自己的裝置 (BYOD) 案例。 此外，所有 Windows 版本中都有提供 Always On VPN。                                                                                                                                                                                                           |
| **各種管理和部署機制** |                                                                                                                                 您可以使用許多管理和部署機制來管理 VPN 設定， (稱為*vpn 設定檔*) ，包括 Windows PowerShell、Microsoft 端點 Configuration Manager、Intune 或協力廠商行動裝置管理 (MDM) 工具和 Windows 設定設計工具。 這些選項可簡化 Always On VPN 的設定，而不論您使用的用戶端管理工具為何。                                                                                                                                 |
|     **標準化的 VPN 設定檔定義**      |                                                                                                                                                                                                                                  Always On VPN 支援使用標準的 XML 設定檔 (ProfileXML) 進行設定，提供大部分管理和部署工具組所使用的標準設定範本格式。                                                                                                                                                                                                                                   |

## <a name="next-steps"></a>後續步驟

- [瞭解一些 advanced Always On VPN 功能](deploy/always-on-vpn-adv-options.md)

- [深入瞭解 Always On VPN 技術](always-on-vpn-technology-overview.md)

- [開始規劃您的 Always On VPN 部署](deploy/always-on-vpn-deploy-deployment.md)
