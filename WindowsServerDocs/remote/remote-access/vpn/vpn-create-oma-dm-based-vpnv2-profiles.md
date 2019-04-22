---
title: 建立 OMA-DM 型 VPNv2 設定檔至 Windows 10 裝置
description: '您可以使用其中兩個方法來建立 OMA DM 基礎 VPNv2 設定檔。 '
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
documentationcenter: ''
ms.assetid: ''
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 1ce20d09c304b26e3708429cc45da06d020e5809
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816479"
---
# <a name="step-75-create-oma-dm-based-vpnv2-profiles-to-windows-10-devices"></a>步驟 7.5. 建立 OMA DM 基礎 VPNv2 到 Windows 10 裝置的設定檔

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows 10

&#171;  [**前一個：** 步驟 7.4.將條件式存取的根憑證部署至內部部署 AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)<br>
&#187;[ **下一步:** 了解 VPN works 的條件式存取](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

在此步驟中，您可以建立 OMA DM 基礎 VPNv2 使用 Intune 來部署 VPN 裝置組態原則的設定檔。 如果您想要使用 SCCM 或 PowerShell 指令碼來建立 VPNv2 設定檔，請參閱 < [VPNv2 CSP 設定](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp)如需詳細資訊。 

## <a name="managed-deployment-using-intune"></a>使用 Intune 受管理的部署

本章節所討論的所有項目是使用條件式存取的 VPN 所需的最小值。 它未涵蓋分割通道，使用 WIP，建立自訂 Intune 裝置組態設定檔以取得 AutoVPN 運作或 SSO。 整合您在稍早建立的 VPN 設定檔中的下列設定[步驟 5。設定 Windows 10 用戶端一律開啟 VPN 連線](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)。  在此範例中，我們會將其整合[藉由使用 Intune 設定 VPN 用戶端](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune)原則。 

**必要條件：**<p>
Windows 10 用戶端電腦已設定使用 Intune 的 VPN 連接。   


**程序：**

1. 在 Azure 入口網站中，按一下**Intune** > **裝置組態** > **設定檔**選取您稍早在中建立的VPN設定檔[使用 Intune 設定 VPN 用戶端](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune)。
    
2. 在原則編輯器 中，選取**屬性** > **設定** > **基底 VPN**。 擴充現有**EAP Xml**包含篩選器，讓 VPN 用戶端邏輯，它需要從使用者的憑證存放區，而不是讓它可以讓它使用第一個擷取 AAD 條件式存取憑證探索到的憑證。

    >[!NOTE]
    >如果沒有這麼做，VPN 用戶端可以擷取從內部憑證授權單位發出的使用者憑證導致失敗的 VPN 連線。

    ![Intune 入口網站](../../media/Always-On-Vpn/intune-eap-xml.png)

3. 找出區段的結尾 **\</AcceptServerName >\</EapType >** 並插入下列字串以提供 VPN 用戶端選取 AAD 條件式邏輯這兩個值之間存取憑證：

    ```XML
    <TLSExtensions xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2"><FilteringInfo xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV3"><EKUMapping><EKUMap><EKUName>AAD Conditional Access</EKUName><EKUOID>1.3.6.1.4.1.311.87</EKUOID></EKUMap></EKUMapping><ClientAuthEKUList Enabled="true"><EKUMapInList><EKUName>AAD Conditional Access</EKUName></EKUMapInList></ClientAuthEKUList></FilteringInfo></TLSExtensions>
    ```

4. 選取 **條件式存取**刀鋒視窗，然後切換**此 VPN 連線的條件式存取**來**已啟用**。<p>啟用此設定變更 **\<DeviceCompliance >\<已啟用 >，則為 true\<啟用 >** VPNv2 設定檔 XML 中設定。

    ![條件式存取的一律開啟 VPN-屬性](../../media/Always-On-Vpn/vpn-conditional-access-azure-ad.png)

6. 按一下 [確定] 。

6. 選取 **指派**，在 包含 下按一下 **選取要包含的群組**。

7. 選取  **VPN 使用者**會接收這個原則並按一下 群組**儲存**。

    ![自動 VPN 使用者指派的上限](../../media/Always-On-Vpn/cap-for-auto-vpn-users-assignments.png)

## <a name="force-mdm-policy-sync-on-the-client"></a>強制在用戶端上的 MDM 原則同步
如果 VPN 設定檔不會顯示在用戶端裝置上，設定下\\網路和網際網路\\VPN，您可以強制 MDM 原則進行同步處理。

1. 成員的身分登入已加入網域的用戶端電腦**VPN 使用者**群組。

2. 在 [開始] 功能表中，輸入**帳戶**，然後按 Enter。

3.  在左側的導覽窗格中，按一下**存取工作或學校**。

5.  在存取公司或學校中，按一下**連接到 < \domain > MDM**然後按一下**資訊**。

6.  按一下 **同步處理**，並確認出現 設定 下的 VPN 設定檔\\網路和網際網路\\VPN。


## <a name="next-step"></a>後續步驟
完成設定 VPN 設定檔，以使用 Azure AD 條件式存取。 

|如果您想要...  |然後請參閱...  |
|---------|---------|
|深入了解使用 Vpn 的條件式存取運作  |[VPN 和條件式存取](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access):此頁面會提供與 Vpn 搭配運作的條件式存取的詳細資訊。      |
|深入了解的進階 VPN 功能  |[進階 VPN 功能](always-on-vpn/deploy/always-on-vpn-adv-options.md#advanced-vpn-features):此頁面會提供指引，了解如何啟用 VPN 流量篩選器、 如何設定自動 VPN 連線使用應用程式-觸發程序，以及如何設定 NPS 只允許來自用戶端使用 Azure AD 所簽發的憑證的 VPN 連線。        |


---

## <a name="related-topics"></a>相關主題
- [VPNv2 CSP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp):本主題為您提供的 VPNv2 CSP 概觀。 VPNv2 組態服務提供者可讓行動裝置管理 (MDM) 伺服器，來設定裝置的 VPN 設定檔。

- [設定 Windows 10 用戶端一律開啟 VPN 連線](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections):本主題提供資訊的 ProfileXML 選項和結構描述，以及如何建立 ProfileXML VPN。 設定好的伺服器基礎結構之後，您必須設定 Windows 10 用戶端電腦通訊的 VPN 連線與該基礎結構。 

- [使用 Intune 設定 VPN 用戶端](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections#configure-the-vpn-client-by-using-intune):本主題提供如何部署 Windows 10 的遠端存取一律在 VPN 設定檔資訊。 Intune 現在會使用 Azure AD 群組。 如果 Azure AD Connect 同步處理至 Azure AD 中，從內部部署 VPN 使用者群組，則不需要設定 VPN 用戶端使用 Intune。

---
