---
title: 建立 OMA-DM 型 VPNv2 設定檔至 Windows 10 裝置
description: '您可以使用兩種方法的其中一種來建立以 VPNv2 為基礎的 OMA-URI 設定檔。 '
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.date: 07/13/2018
ms.author: v-tea
author: Teresa-MOTIV
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 8829d6515c92751b85320a7c622a82b32ffb82ab
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80818871"
---
# <a name="step-75-create-oma-dm-based-vpnv2-profiles-to-windows-10-devices"></a>步驟 7.5. 建立以 OMA DM 為基礎的 VPNv2 設定檔到 Windows 10 裝置

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows 10

- [**上一步：** 步驟 7.4.將條件式存取根憑證部署到內部部署 AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)
- [**下一步：** 瞭解 VPN 的條件式存取如何運作](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

在此步驟中，您可以使用 Intune 建立 OMA DM 型 VPNv2 設定檔，以部署 VPN 裝置設定原則。 如果您想要使用 Microsoft Endpoint Configuration Manager 或 PowerShell 腳本來建立 VPNv2 設定檔，請參閱[VPNV2 CSP 設定](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp)以取得更多詳細資料。 

## <a name="managed-deployment-using-intune"></a>使用 Intune 的受控部署

本節討論的所有內容，都是讓 VPN 與條件式存取搭配使用的最低需求。 它不涵蓋分割通道、使用 WIP、建立自訂的 Intune 裝置設定設定檔，以取得 AutoVPN 運作或 SSO。 將下列設定整合到您稍早在 [步驟5所建立的 VPN 設定檔中。](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)設定 Windows 10 用戶端 Always On VPN 連線。  在此範例中，我們會將它們整合到[使用 Intune 原則設定 VPN 用戶端](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune)。 

**必要條件：**

Windows 10 用戶端電腦已使用 Intune 設定 VPN 連線。   


**步**

1. 在 Azure 入口網站中，選取  **Intune** > **裝置**設定 > [**設定檔**]，然後選取您稍早在[使用 Intune 設定 vpn 用戶端](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md#configure-the-vpn-client-by-using-intune)中建立的 vpn 設定檔。
    
2. 在原則編輯器中，選取 [**屬性**] > [**設定**] > [**基底 VPN**]。 擴充現有的**EAP Xml**以包含篩選準則，以提供 VPN 用戶端從使用者的憑證存放區抓取 AAD 條件式存取憑證所需的邏輯，而不是讓它能夠使用第一個探索到的憑證。

    >[!NOTE]
    >如果沒有這麼做，VPN 用戶端可以抓取從內部部署憑證授權單位單位發出的使用者憑證，導致 VPN 連線失敗。

    ![Intune 入口網站](../../media/Always-On-Vpn/intune-eap-xml.png)

3. 找出結尾為 **\</AcceptServerName >\</EapType >** 的區段，並在這兩個值之間插入下列字串，以提供 VPN 用戶端選取 AAD 條件式存取憑證的邏輯：

    ```XML
    <TLSExtensions xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2"><FilteringInfo xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV3"><EKUMapping><EKUMap><EKUName>AAD Conditional Access</EKUName><EKUOID>1.3.6.1.4.1.311.87</EKUOID></EKUMap></EKUMapping><ClientAuthEKUList Enabled="true"><EKUMapInList><EKUName>AAD Conditional Access</EKUName></EKUMapInList></ClientAuthEKUList></FilteringInfo></TLSExtensions>
    ```

4. 選取 [**條件式存取**] 分頁，並切換**此 VPN**連線的 [條件式存取] 以**啟用**。
   
   啟用此設定會變更 VPNv2 設定檔 XML 中的 **\<DeviceCompliance >\<已啟用 > true\</Enabled >** 設定。

    ![Always On VPN 的條件式存取-屬性](../../media/Always-On-Vpn/vpn-conditional-access-azure-ad.png)

5. 選取 **\[確定\]** 。

6. 選取 [**指派**]，在 [包含] 底下，選取 [**選取要包含的群組**]。

7. 選取接收此原則的**VPN 使用者**群組，然後選取 [**儲存**]。

    ![自動 VPN 使用者的上限-指派](../../media/Always-On-Vpn/cap-for-auto-vpn-users-assignments.png)

## <a name="force-mdm-policy-sync-on-the-client"></a>在用戶端上強制執行 MDM 原則同步處理

如果 VPN 設定檔未顯示在用戶端裝置上，您可以在 [設定] 底下的 [\\網路 & 網際網路\\VPN] 底下，強制 MDM 原則進行同步處理。

1. 以**VPN 使用者**群組的成員身分登入加入網域的用戶端電腦。

2. 在 [開始] 功能表上，輸入**account**，然後按 enter。

3. 在左側流覽窗格中，選取 [**存取公司或學校**]。

4. 在 [存取公司或學校] 底下，選取 [**連線到 < \domain > MDM**]，然後選取 [**資訊**]。

5. 選取 [**同步**處理]，並確認 vpn 設定檔出現在 [設定]\\[網路 & 網際網路\\VPN] 底下。


## <a name="next-steps"></a>接下來的步驟

您已完成設定 VPN 設定檔，以使用 Azure AD 條件式存取。 

|如果您要...  |然後查看 。  |
|---------|---------|
|深入瞭解條件式存取如何搭配 Vpn 運作  |[VPN 和條件式存取](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)：此頁面提供有關條件式存取如何搭配 Vpn 運作的詳細資訊。      |
|深入瞭解 advanced VPN 功能  |[ADVANCED VPN 功能](always-on-vpn/deploy/always-on-vpn-adv-options.md#advanced-vpn-features)：本頁面提供如何啟用 VPN 流量篩選器、如何使用應用程式觸發程式來設定自動 VPN 連線，以及如何設定 NPS 只允許使用 Azure AD 所發行之憑證的 VPN 連線的指引。        |


## <a name="related-topics"></a>相關主題

- [VPNV2 CSP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp)：本主題提供 VPNv2 CSP 的總覽。 「VPNv2 設定服務提供者」可讓行動裝置管理（MDM）伺服器設定裝置的 VPN 設定檔。

- [設定 Windows 10 用戶端 ALWAYS ON VPN](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections)連線：本主題提供 ProfileXML 選項和架構的相關資訊，以及如何建立 ProfileXML VPN。 設定伺服器基礎結構之後，您必須將 Windows 10 用戶端電腦設定為使用 VPN 連線與該基礎結構進行通訊。 

- [使用 Intune 設定 VPN 用戶端](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections#configure-the-vpn-client-by-using-intune)：本主題提供如何 Always On VPN 設定檔部署 Windows 10 遠端存取的相關資訊。 Intune 現在會使用 Azure AD 群組。 如果 Azure AD Connect 將 VPN 使用者群組從內部部署同步處理到 Azure AD，則不需要使用 Intune 設定 VPN 用戶端。
