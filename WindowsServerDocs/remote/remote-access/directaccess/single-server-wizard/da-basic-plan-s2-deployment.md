---
title: 步驟2規劃基本 DirectAccess 部署
description: 瞭解如何規劃消費者入門 Wizard 的設定。
manager: brianlic
ms.topic: article
ms.assetid: 7ddcb162-dd92-406c-acab-d3de7239c644
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 3f4d41feb2a41226676c12de4cd0e72966ae7077
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948244"
---
# <a name="step-2-plan-the-basic-directaccess-deployment"></a>步驟2規劃基本 DirectAccess 部署

>適用於：Windows Server (半年度管道)、Windows Server 2016

規劃 DirectAccess 基礎結構之後，在具有基本設定的單一伺服器上部署 DirectAccess 的下一步是規劃消費者入門 Wizard 的設定。

|Task|描述|
|----|--------|
|規劃用戶端部署|根據預設，消費者入門 Wizard 會藉由將 WMI 篩選器套用至用戶端設定 GPO，將 DirectAccess 部署到網域中的所有膝上型電腦和筆記本電腦|
|規劃 DirectAccess 伺服器部署|規劃如何部署 DirectAccess 伺服器。|

## <a name="planning-for-client-deployment"></a><a name="bkmk_2_1_client"></a>規劃用戶端部署
規劃用戶端部署時要進行兩個決策：

1.  DirectAccess 將僅供攜帶型電腦使用，或是可供任何電腦使用？

    當您在消費者入門 Wizard 中設定 DirectAccess 用戶端時，您可以選擇只允許指定安全性群組中的行動電腦使用 DirectAccess 進行連接。 如果您限制對行動電腦的存取，DirectAccess 會自動設定 WMI 篩選器，以確保 DirectAccess 用戶端 GPO 只會套用到指定安全性群組中的行動電腦。 DirectAccess 系統管理員需要建立或修改群組原則 WMI 篩選器的許可權，才能啟用此設定。

2.  哪些安全性群組將包含 DirectAccess 用戶端電腦？

    DirectAccess 設定包含在 DirectAccess 用戶端 GPO 中。 GPO 會套用至您在消費者入門 wizard 中指定的安全性群組所屬的電腦。 您可以指定在任何支援的網域中所包含的安全性群組。 設定 DirectAccess 之前，應該先建立安全性群組。 完成 DirectAccess 部署之後，您可以將電腦新增至安全性群組，但請注意，如果您將位於不同網域中的用戶端電腦新增至安全性群組，用戶端 GPO 將不會套用至這些用戶端。 例如，如果您在網域 A 中建立 SG1 做為 DirectAccess 用戶端，之後新增網域 B 的用戶端到此群組，用戶端 GPO 將不會套用到網域 B 中的用戶端。若要避免這個問題，請為每個包含用戶端電腦的網域建立新的用戶端安全性群組。 或者，如果您不想要建立新的安全性群組，請執行 Add-DAClient Cmdlet，加上新網域的新 GPO 名稱。

## <a name="planning-for-directaccess-server-deployment"></a><a name="bkmk_2_2_server"></a>規劃 DirectAccess 伺服器部署
規劃部署 DirectAccess 伺服器時，有許多決策需要進行：

-   **網路拓撲** -部署 DirectAccess 伺服器時有兩種可用的拓撲：

    -   **兩張介面卡** -有兩張網路介面卡，DirectAccess 可以設定一張網路介面卡直接連線到網際網路，另一個則連接到內部網路。 或者，伺服器安裝在邊緣裝置 (例如防火牆或路由器) 後面。 在此設定中，一張網路介面卡連線到周邊網路，另一張連線到內部網路。

    -   **單一網路介面卡** -在這項設定中，DirectAccess 伺服器會安裝在邊緣裝置（例如防火牆或路由器）後方。 網路介面卡會連線到內部網路。

-   **網路介面卡** -directaccess wizard 會自動偵測在 DirectAccess 伺服器上設定的網路介面卡。 您可以確認已從 [ **審核** ] 頁面中選取正確的介面卡。

-   **Ip-HTTPs 憑證** -因為在此部署中不需要 PKI，所以如果沒有任何憑證) ，則嚮導會自動為 Ip-HTTPs 和網路位置 (伺服器布建自我簽署憑證，並自動啟用 Kerberos proxy。 此 wizard 也會在僅限 IPv4 的環境中啟用通訊協定轉譯的 NAT64 和 DNS64。 精靈套用設定成功完成之後，按一下 [關閉]。

-   **Windows 7 客戶** 端-您無法從消費者入門 Wizard 啟用 windows 7 用戶端的支援。 您可以從 [Advanced Setup Wizard] 來啟用此功能。 如需詳細資訊，請參閱 [使用 Advanced Settings 部署單一 DirectAccess 伺服器](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。

-   **VPN** 設定-設定 DirectAccess 之前，請先決定是否要提供遠端用戶端的 VPN 存取權。 如果您的組織中有不支援 DirectAccess 連線的用戶端電腦，您應該提供 VPN 存取 (因為它們是未受管理或執行不支援 DirectAccess 的作業系統) 。 消費者入門 Wizard 會使用 DHCP 設定 VPN IP 位址指派，並將 VPN 用戶端設定為使用 Active Directory 進行驗證。

-   **強制通道** -如果您打算使用強制通道，或未來可能會新增，您應該使用 [使用 [Advanced Settings 部署單一 DirectAccess 伺服器](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) ] 部署兩個通道設定。 基於安全性考慮，不支援單一通道設定中的強制通道。

## <a name="previous-step"></a><a name="BKMK_Links"></a>上一個步驟

-   [步驟 1：規劃基本 DirectAccess 基礎結構](da-basic-plan-s1-infrastructure.md)



