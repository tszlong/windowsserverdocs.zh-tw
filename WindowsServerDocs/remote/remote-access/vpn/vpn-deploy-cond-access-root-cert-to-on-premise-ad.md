---
description: 深入瞭解：步驟7.4。 將條件式存取根憑證部署到內部部署 AD
title: 將條件式存取根憑證部署至內部部署 AD
ms.topic: article
ms.date: 06/28/2019
ms.author: v-tea
author: Teresa-MOTIV
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: a317dfeedbd7d53578ea7568574ee6c8e31c1190
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97039406"
---
# <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-ad"></a>步驟 7.4. 將條件式存取根憑證部署到內部部署 AD

>適用于： Windows Server (半年通道) 、Windows Server 2016、Windows Server 2012 R2、Windows 10

在此步驟中，您會將條件式存取根憑證部署為您內部部署 AD 的 VPN 驗證的受信任根憑證。

- [**上一步：** 步驟7.3。設定條件式存取原則](vpn-config-conditional-access-policy.md)
- [**下一步：** 步驟7.5。建立 OMA-URI 型 >vpnv2 設定檔以 Windows 10 裝置](vpn-create-oma-dm-based-vpnv2-profiles.md)

1. 在 [ **VPN 連線能力** ] 頁面上，選取 [ **下載憑證**]。

   >[!NOTE]
   >[ **下載 base64 憑證** ] 選項適用于一些需要 base64 憑證進行部署的設定。

2. 以企業系統管理員許可權登入已加入網域的電腦，然後從系統管理員命令提示字元執行下列命令，以將雲端根憑證 () 新增至 *Enterprise NTauth* store：

   >[!NOTE]
   >針對 VPN 伺服器未加入 Active Directory 網域的環境，必須手動將雲端根憑證新增至 [ _信任的根憑證授權_ 單位] 存放區。

   | 命令 | 描述 |
   | --- | --- |
   | `certutil -dspublish -f VpnCert.cer RootCA` | 在 **CN = AIA** 和 **CN = 憑證授權單位** 單位容器底下建立兩個 **microsoft VPN 根 ca gen 1** 容器，並將每個根憑證發佈為 **Microsoft Vpn 根 ca gen 1** 容器之 _cACertificate_ 屬性的值。 |
   | `certutil -dspublish -f VpnCert.cer NTAuthCA` | 在 **cn = AIA** 和 **CN = 憑證授權單位** 單位容器底下建立一個 **cn = NTAuthCertificates** 容器，並將每個根憑證發佈為 **cn = NTAuthCertificates** 容器之 _cACertificate_ 屬性的值。 |
   | `gpupdate /force` | 加速將根憑證新增至 Windows 伺服器和用戶端電腦。 |

3. 確認根憑證存在於企業 NTauth 存放區中，並顯示為受信任：
   1. 使用已安裝 **憑證授權單位單位管理工具** 的企業系統管理員許可權登入伺服器。

   >[!NOTE]
   >根據預設， **憑證授權單位單位管理工具** 會安裝在憑證授權單位單位伺服器上。 您可以在伺服器管理員的 **角色管理工具** 中，將它們安裝在其他成員伺服器上。

   1. 在 VPN 伺服器上的 [開始] 功能表中，輸入 **pkiview** ，以開啟 [企業 PKI] 對話方塊。
   1. 在 [[開始] 功能表] 中，輸入 **pkiview** 以開啟 [企業 PKI] 對話方塊。
   1. 在 [ **企業 PKI** ] 上按一下滑鼠右鍵，然後選取 [ **管理 AD 容器**]。
   1. 確認每個 Microsoft VPN 根 CA gen 1 憑證都存在於：
      - NTAuthCertificates
      - AIA 容器
      - 憑證授權單位單位容器

## <a name="next-steps"></a>後續步驟

[步驟7.5。建立 OMA-URI 型 >vpnv2 設定檔以 Windows 10 裝置](vpn-create-oma-dm-based-vpnv2-profiles.md)：在此步驟中，您可以使用 Intune 建立以 oma-uri 為基礎的 >vpnv2 設定檔，以部署 VPN 裝置設定原則。 如果您想要使用 Microsoft Endpoint Configuration Manager 或 PowerShell 腳本來建立 >vpnv2 設定檔，請參閱 [>VPNV2 CSP 設定](/windows/client-management/mdm/vpnv2-csp) 以取得詳細資料。
