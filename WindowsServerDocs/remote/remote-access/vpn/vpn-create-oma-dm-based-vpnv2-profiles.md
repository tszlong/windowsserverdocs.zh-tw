---
title: 建立 OMA-DM 型 VPNv2 設定檔至 Windows 10 裝置
description: '您可以使用其中一個兩個方法來建立 OMA-DM 型 VPNv2 設定檔。 '
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
ms.sourcegitcommit: 4147e092d77d9458696e6686bccefe3788c90508
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/28/2019
ms.locfileid: "9031292"
---
# 步驟 7.5. 建立 OMA-DM 型 VPNv2 設定檔到 Windows 10 裝置

>適用於： Windows Server （半年度管道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10

& #171; [**先前：** 7.4 步驟。將條件式存取根憑證部署至內部部署 AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)<br>
& #187;[**下一步：** 了解的 VPN 的運作方式的條件式存取](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

在此步驟中，您可以建立 OMA-DM 型 VPNv2 設定檔使用 Intune 部署 VPN 裝置設定原則。 如果您想要使用 SCCM 或 PowerShell 指令碼來建立 VPNv2 設定檔，請參閱[VPNv2 CSP 設定](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp)，如需詳細資訊。 

## 使用 Intune 受管理的部署

在本節中所討論的所有項目是進行 VPN 使用條件式存取運作所需的最小值。 它並未涵蓋分割通道，使用 WIP，建立自訂 Intune 裝置設定的設定檔以取得 AutoVPN 運作或 SSO。 將下列設定整合到您在步驟 5[稍早建立的 VPN 設定檔中。設定 Windows 10 用戶端 Always On VPN 連線](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)。在此範例中，我們會將它們整合到[使用 Intune VPN 用戶端設定](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune)原則。 

**必要條件：**<p>
Windows 10 用戶端電腦已經使用 Intune VPN 連線設定。   


**程序：**

1. 在 Azure 入口網站中，按一下 [ **Intune** > **裝置設定** > 在[設定 VPN 用戶端使用 Intune](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune)建立**設定檔**，然後選取 VPN 設定檔。
    
2. 在原則編輯器中，選取 [**屬性** > **設定** > **基底 VPN**。 延伸現有**EAP Xml**包含可讓 VPN 用戶端的邏輯，它必須從使用者的憑證存放區，而不是將它有機會讓它使用的第一個憑證來抓取 AAD 條件式存取憑證的篩選器探索到。

    >[!NOTE]
    >沒有這麼做，VPN 用戶端可以擷取使用者將憑證發給從內部憑證授權單位，導致 VPN 連線失敗。

    ![Intune 入口網站](../../media/Always-On-Vpn/intune-eap-xml.png)

3. 找到區段**\</AcceptServerName>\</EapType>** 以結束並插入下列字串為 VPN 用戶端提供選取 AAD 條件式存取憑證的邏輯的這兩個值之間：

    ```XML
    <TLSExtensions xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2"><FilteringInfo xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV3"><EKUMapping><EKUMap><EKUName>AAD Conditional Access</EKUName><EKUOID>1.3.6.1.4.1.311.87</EKUOID></EKUMap></EKUMapping><ClientAuthEKUList Enabled="true"><EKUMapInList><EKUName>AAD Conditional Access</EKUName></EKUMapInList></ClientAuthEKUList></FilteringInfo></TLSExtensions>
    ```

4. 選取的**條件式存取**刀鋒視窗和 toogle**已啟用****此 VPN 連線的條件式存取**。<p>啟用此設定變更**\<DeviceCompliance>\<Enabled>true\</Enabled>** VPNv2 設定檔 XML 中設定。

    ![適用於 Always On VPN-屬性的條件式存取](../../media/Always-On-Vpn/vpn-conditional-access-azure-ad.png)

6. 按一下 **\[確定\]**。

6. 選取**指派**，包括底下，按一下 [**選取要包含的群組**。

7. 選取**VPN 使用者**會收到這項原則的群組，然後按一下 [**儲存**]。

    ![自動 VPN 使用者指派上限](../../media/Always-On-Vpn/cap-for-auto-vpn-users-assignments.png)

## 強制執行用戶端上的 MDM 原則同步
如果 VPN 設定檔不會顯示在用戶端裝置上下 Settings\\Network & Internet\\VPN，, 您可以強制同步的 MDM 原則。

1. **VPN 使用者**群組的成員登入加入網域的用戶端電腦。

2. 在 [開始] 功能表中，輸入**帳戶**，然後按 Enter。

3.  在左瀏覽窗格中，按一下 [**存取公司或學校資源**。

5.  在存取公司或學校下，按一下 [**已連線至 <\domain> MDM** ，然後按一下**資訊**。

6.  按一下 [**同步處理**，並確認 Settings\\Network & Internet\\VPN 下出現的 VPN 設定檔。


## 後續步驟
您完成設定來使用條件式存取的 Azure AD 的 VPN 設定檔。 

|如果您想要...  |然後查看...  |
|---------|---------|
|深入了解如何條件式存取運作 vpn  |[VPN 和條件式存取](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)： 本頁面提供更多有關如何條件式存取搭配 Vpn。      |
|深入了解進階 VPN 功能  |[進階 VPN 功能](always-on-vpn/deploy/always-on-vpn-adv-options.md#advanced-vpn-features)： 此頁面提供關於如何啟用 VPN 流量篩選器、 如何設定自動 VPN 連線使用的應用程式-觸發程序，以及如何設定 NPS 只允許來自用戶端使用 Azure 所發行的憑證的 VPN 連線的指導方針廣告。        |


---

## 相關主題
- [VPNv2 CSP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp)： 本主題提供您 VPNv2 CSP 的概觀。 VPNv2 設定服務提供者可讓行動裝置管理 (MDM) 伺服器來設定 VPN 設定檔的裝置。

- [設定 Windows 10 用戶端一律在 VPN 連線](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections)： 本主題提供有關的 ProfileXML 選項與結構描述，以及如何建立 ProfileXML VPN。 設定好伺服器基礎結構之後，您必須設定 VPN 連線使用該基礎結構與通訊的 windows 10 用戶端電腦。 

- [設定 VPN 用戶端使用 Intune](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections#configure-the-vpn-client-by-using-intune)： 本主題提供有關如何部署 Windows 10 遠端存取 Always On VPN 設定檔的資訊。 Intune 現在會使用 Azure AD 群組。 如果 Azure AD Connect 同步至 Azure AD，從內部部署 VPN 使用者群組，則不需要設定使用 Intune VPN 用戶端。

---
