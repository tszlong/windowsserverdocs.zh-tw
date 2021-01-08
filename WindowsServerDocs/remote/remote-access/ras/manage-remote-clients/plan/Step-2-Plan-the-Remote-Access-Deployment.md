---
title: 步驟2規劃遠端存取部署
description: 瞭解如何規劃 Remote Access Setup Wizard 將使用的設定。
manager: brianlic
ms.topic: article
ms.assetid: cc9f02b9-8ddd-4cae-b397-a832996144dd
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 3e84e2e7df85a0eea08c3b6bd8cff6ca81be863a
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98039908"
---
# <a name="step-2-plan-the-remote-access-deployment"></a>步驟2規劃遠端存取部署

>適用於：Windows Server (半年度管道)、Windows Server 2016

在您規劃要用來設定單一遠端存取服務器的基礎結構，以便遠端系統管理 DirectAccess 用戶端之後，您就可以開始規劃 Remote Access Setup Wizard 將使用的設定。

> [!NOTE]
> 繼續進行這些工作之前，請參閱 [步驟1：規劃遠端存取基礎結構](Step-1-Plan-the-Remote-Access-Infrastructure.md)。

|工作|描述|
|----|--------|
|[規劃用戶端部署策略](#plan-a-client-deployment-strategy)|決定哪些受管理的電腦會設定為 DirectAccess 用戶端。|
|[規劃遠端存取服務器部署策略](#plan-a-remote-access-server-deployment-strategy)|規劃如何部署遠端存取伺服器。|
|[規劃基礎結構伺服器的設定](#plan-the-infrastructure-servers-configurations)|規劃遠端存取部署中的基礎結構伺服器，包括 DirectAccess 網路位置伺服器、DNS 伺服器和 DirectAccess 管理伺服器。|

## <a name="plan-a-client-deployment-strategy"></a>規劃用戶端部署策略
規劃用戶端部署時，需要做三個決定：

1.  DirectAccess 只適用于行動電腦，或指定安全性群組中的每一部電腦？

    當您在 DirectAccess 用戶端安裝 Wizard 中設定 DirectAccess 用戶端時，您可以選擇只允許指定安全性群組中的行動電腦使用 DirectAccess 來連線至伺服器。 如果您限制只有攜帶型電腦才能存取，「遠端存取」就會自動設定 WMI 篩選器，以確保 DirectAccess 用戶端 GPO 只會套用到指定之安全性群組中的攜帶型電腦。 遠端存取系統管理員需要建立或修改群組原則 WMI 篩選器的權限，才能啟用這個設定。

2.  哪些安全性群組將包含 DirectAccess 用戶端電腦？

    DirectAccess 設定包含在 DirectAccess 用戶端群組原則物件 (GPO) 中。 這個 GPO 會套用到屬於您在「DirectAccess 用戶端安裝精靈」中指定之安全性群組的電腦。 您可以指定包含在任何支援之網域中的安全性群組。

    在設定遠端存取之前，您必須先建立安全性群組。 完成遠端存取部署之後，您可以將電腦新增至安全性群組。 但是，如果您新增的用戶端電腦位於與安全性群組不同的網域中，則用戶端 GPO 不會套用至這些用戶端。 例如，如果您在網域 A 中為 DirectAccess 用戶端建立 SG1，並在稍後將用戶端從網域 B 新增至此群組，用戶端 GPO 將不會套用至網域 B 中的用戶端。

    若要避免此問題，請為包含用戶端電腦的每個網域建立新的用戶端安全性群組。 或者，如果您不想要建立新的安全性群組，請使用新網域的新 GPO 名稱來執行 **add-daclient** Windows PowerShell Cmdlet。

3.  您將為 DirectAccess 網路連接助理設定哪些設定？

    DirectAccess 網路連線能力輔助程式會在用戶端電腦上執行，並提供與終端使用者的 DirectAccess 連線相關的其他資訊。 在「DirectAccess 用戶端安裝精靈」中，您可以設定下列各項：

    -   **連線能力檢查器**

        系統會建立一個用戶端用來驗證內部網路連線能力的預設 Web 探查。 預設名稱為 `https://directaccess-WebProbeHost.<domain_name>`。 應該以手動方式在 DNS 中登錄這個名稱。 您可以建立其他透過 HTTP 或 PING 使用其他網址的連線能力檢查器。 每個連線能力檢查器都必須有一個 DNS 項目。

    -   **技術支援中心電子郵件地址**

        如果終端使用者遇到 DirectAccess 連線問題，則可以將包含診斷資訊的電子郵件傳送給遠端存取系統管理員，讓他們可以針對問題進行疑難排解。

    -   **DirectAccess 連線名稱**

        您可以指定 DirectAccess 連線名稱，以協助終端使用者識別其電腦上的 DirectAccess 連線。

    -   **允許 DirectAccess 用戶端使用本機名稱解析**

        用戶端需要在本機解析名稱的方法。 如果您允許 DirectAccess 用戶端使用本機名稱解析，使用者便可以使用本機 DNS 伺服器來解析名稱。 當終端使用者選擇使用本機 DNS 伺服器進行名稱解析時，DirectAccess 不會將單一標籤名稱的解析要求傳送至內部公司 DNS 伺服器。 它使用本機名稱解析，而不是使用連結-本機多播名稱解析 (LLMNR) 和 NetBios over TCP/IP 通訊協定) 進行 (。

## <a name="plan-a-remote-access-server-deployment-strategy"></a>規劃遠端存取服務器部署策略
規劃部署遠端存取服務器時所需進行的決策包括：

-   **網路拓撲**

    部署遠端存取服務器時，有兩種可用的拓撲：

    -   **兩張介面卡**：有兩張網路介面卡，遠端存取可以設定為直接連線到網際網路的一張網路介面卡，以及另一張連線到內部網路的網路介面卡。 或者，伺服器會安裝在邊緣裝置後方，例如防火牆或路由器。 在此設定中，一個網路介面卡連接到周邊網路，另一個則連接到內部網路。

    -   **單一網路介面卡**：在此設定中，遠端存取服務器會安裝在邊緣裝置後方，例如防火牆或路由器。 網路介面卡會連線到內部網路。

-   **網路介面卡**

    [遠端存取服務器安裝程式] 會自動偵測在遠端存取服務器上設定的網路介面卡。 您必須確定已選取正確的介面卡。

-   **IP-HTTPS 憑證**

    「遠端存取伺服器安裝精靈」會自動偵測適合 IP-HTTPS 連線的憑證。 您所選憑證的主體名稱必須與 ConnectTo 位址相符。 如果您使用的是自我簽署的憑證，您可以選擇使用遠端存取服務器自動建立的憑證。

-   **IPv6 首碼**

    如果「遠端存取伺服器安裝精靈」偵測到網路介面卡上已部署 IPv6，它就會自動填入內部網路的 IPv6 首碼、要指派給 DirectAccess 用戶端電腦的 IPv6 首碼，以及要指派給 VPN 用戶端電腦的 IPv6 首碼。 如果自動產生的首碼不適用您的原生 IPv6 或 ISATAP 基礎結構，您必須手動變更它們。

-   **驗證**

    您可以選擇下列其中一種方法，向遠端存取服務器驗證 DirectAccess 用戶端：

    -   **使用者驗證**：您可以讓使用者使用 Active Directory 認證或雙因素驗證來進行驗證。

    -   **電腦驗證**：您可以將電腦驗證設定為使用憑證。 或遠端存取服務器可以作為 Kerberos 驗證的 proxy，而不需要憑證。

    -   **Windows 7 用戶端** 根據預設，執行 Windows 7 的用戶端電腦無法連接到執行 Windows Server 2012 的遠端存取部署。 如果您的組織中有執行 Windows 7 的用戶端，而該用戶端需要從遠端存取內部資源，您可以允許它們連線。 任何您想要允許存取內部資源的用戶端電腦都必須是您在「DirectAccess 用戶端安裝精靈」中指定之安全性群組的成員。

        > [!NOTE]
        > 若要讓執行 Windows 7 的用戶端使用 DirectAccess 進行連線，您必須使用電腦憑證驗證。

-   **VPN 設定**

    在設定遠端存取之前，請先決定是否要提供遠端用戶端的 VPN 存取權。 如果您的組織中有不支援 DirectAccess 連線的用戶端電腦，您應該提供 VPN 存取 (例如，這些電腦未受管理，或它們執行的是不支援 DirectAccess 的作業系統) 。 遠端存取服務器設定向導可讓您設定如何使用 DHCP 或從靜態位址集區指派 IP 位址 () 以及如何使用 Active Directory 或 RADIUS 伺服器)  (驗證 VPN 用戶端的方式。

## <a name="plan-the-infrastructure-servers-configurations"></a>規劃基礎結構伺服器的設定
遠端存取需要三種類型的基礎結構伺服器：

-   **網路位置伺服器**

-   **DNS 伺服器**

-   **管理伺服器**

## <a name="additional-references"></a>其他參考資料

-   [步驟 1：規劃遠端存取基礎架構](Step-1-Plan-the-Remote-Access-Infrastructure.md)



