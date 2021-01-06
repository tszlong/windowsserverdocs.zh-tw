---
title: 步驟2規劃 DirectAccess 部署
description: 瞭解如何規劃 Enable DirectAccesss Wizard 的設定。
manager: brianlic
ms.topic: article
ms.assetid: 72b5b2af-6925-41e0-a3f9-b8809ed711d1
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 696747b64ee0c3b5412f1397daa3a1acbffd043d
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947034"
---
# <a name="step-2-plan-the-directaccess-deployment"></a>步驟2規劃 DirectAccess 部署

>適用於：Windows Server (半年度管道)、Windows Server 2016

規劃遠端存取基礎結構之後，啟用 DirectAccess 的下一步是規劃「啟用 DirectAccesss 精靈」的設定。

|Task|描述|
|----|--------|
|規劃用戶端部署|規劃如何讓用戶端電腦使用 DirectAccess 連線。 決定哪些受管理的電腦會設定為 DirectAccess 用戶端。|
|規劃遠端存取伺服器部署|規劃如何部署遠端存取伺服器。|

## <a name="planning-for-client-deployment"></a><a name="bkmk_2_1_client"></a>規劃用戶端部署
規劃用戶端部署時要進行兩個決策：

-   DirectAccess 將僅供攜帶型電腦使用，或是可供任何電腦使用？

    當您在「啟用 DirectAccess 精靈」中設定 DirectAccess 用戶端時，您可以選擇只允許指定的安全性群組中的攜帶型電腦使用 DirectAccess 連線。 如果您限制只有攜帶型電腦才能存取，「遠端存取」就會自動設定 WMI 篩選器，以確保 DirectAccess 用戶端 GPO 只會套用到指定之安全性群組中的攜帶型電腦。 遠端存取系統管理員需要建立或修改群組原則 WMI 篩選器的權限，才能啟用這個設定。

-   哪些安全性群組將包含 DirectAccess 用戶端電腦？

    DirectAccess 設定包含在 DirectAccess 用戶端 GPO 中。 GPO 會套用到您在「啟用 DirectAccess 精靈」指定之安全性群組中的電腦。 您可以指定在任何支援的網域中所包含的安全性群組。 設定遠端存取之前，必須先建立安全性群組。 您可以在完成遠端存取部署之後，將電腦新增到安全性群組，但請注意，如果您新增位於不同網域中的用戶端電腦到安全性群組，用戶端 GPO 將不會套用到這些用戶端。 例如，如果您在網域 A 中建立 SG1 做為 DirectAccess 用戶端，之後新增網域 B 的用戶端到此群組，用戶端 GPO 將不會套用到網域 B 中的用戶端。若要避免這個問題，請為每個包含用戶端電腦的網域建立新的用戶端安全性群組。 或者，如果您不想要建立新的安全性群組，請執行 Add-DAClient Cmdlet，加上新網域的新 GPO 名稱。

## <a name="planning-for-remote-access-server-deployment"></a><a name="bkmk_2_2_server"></a>規劃遠端存取伺服器部署
規劃部署遠端存取伺服器時要進行許多決策：

-   **網路拓撲**-部署遠端存取服務器時有兩種可用的拓撲：

    -   **兩張介面卡**-有兩張網路介面卡，遠端存取可以設定為直接連線到網際網路的一張網路介面卡，另一個則連接到內部網路。 或者，伺服器安裝在邊緣裝置 (例如防火牆或路由器) 後面。 在此設定中，一張網路介面卡連線到周邊網路，另一張連線到內部網路。

    -   **單一網路介面卡**-在此設定中，遠端存取服務器會安裝在邊緣裝置後方，例如防火牆或路由器。 網路介面卡會連線到內部網路。

-   **網路介面卡**-[啟用 DirectAccess] 嚮導會根據 VPN 所使用的介面，自動偵測在遠端存取服務器上設定的網路介面卡。 您必須確定已選取正確的介面卡。

-   **ConnectTo 位址**-用戶端電腦使用 ConnectTo 位址來連線到遠端存取服務器。 您選擇的位址必須符合您部署 IP-HTTPS 連線的 IP-HTTPS 憑證主體名稱，而且必須能在公用 DNS 中使用。 請參閱規劃 IP-HTTPS 的網站憑證。

-   **Ip-HTTPs 憑證**-如果已設定 sstp VPN，[啟用 DirectAccess] 嚮導會挑選 SSTP 針對 ip-HTTPs 所使用的憑證。 如果未設定 SSTP VPN，精靈會嘗試查看是否已設定 IP-HTTPS 的憑證。 如果沒有，它會自動為 IP-HTTPS 佈建自我簽署的憑證。精靈會自動啟用 Kerberos 驗證。 精靈也會啟用 NAT64 和 DNS64，以便轉換 IPv4 環境使用的通訊協定。

-   **Ipv6 首碼**-如果 wizard 偵測到網路介面卡已部署 ipv6，它會自動為內部網路建立 ipv6 首碼、將 ipv6 首碼指派給 DirectAccess 用戶端電腦，以及將 ipv6 首碼指派給 VPN 用戶端電腦。 如果自動產生的首碼不適用您的原生 IPv6 或 ISATAP 基礎結構，您必須手動變更它們。 請參閱1.1 規劃網路和伺服器拓撲和設定。

-   **Windows 7 客戶** 端-根據預設，windows 7 用戶端電腦無法連線至 windows Server 2012 遠端存取部署。 如果您的組織中有需要遠端存取內部資源的 Windows 7 用戶端電腦，您可以允許它們連線。 您想要允許存取內部資源的任何用戶端電腦都必須是您在「啟用 DirectAccess 精靈」中指定之安全性群組的成員。

    > [!NOTE]
    > 若要允許 Windows 7 用戶端電腦使用 DirectAccess 進行連線，您必須使用電腦憑證驗證。

-   **驗證**-「啟用 DirectAccess wizard」會使用 Active Directory 來驗證使用者認證。 若要部署雙因素驗證，請參閱[部署 OTP 驗證的遠端存取](../../ras/otp/Deploy-RA-OTP.md)。





