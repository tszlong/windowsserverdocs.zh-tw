---
title: 將條件式存取根憑證部署至內部部署 AD
description: ''
services: active-directory
ms.prod: windows-server
ms.technology: networking-ras
ms.workload: identity
ms.topic: article
ms.date: 06/28/2019
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 67d361db7a2dd3f2879e8beb924075dae68d52a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404313"
---
# <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-ad"></a>步驟 7.4. 將條件式存取根憑證部署到內部部署 AD

>適用于： Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows 10

在此步驟中，您會將條件式存取根憑證部署為受信任的根憑證，以進行 VPN 驗證至您的內部部署 AD。

- [**上一步：** 步驟7.3。設定條件式存取原則](vpn-config-conditional-access-policy.md)
- [**下一步：** 步驟7.5。建立以 OMA DM 為基礎的 VPNv2 設定檔到 Windows 10 裝置](vpn-create-oma-dm-based-vpnv2-profiles.md)

1. 在 [ **VPN 連線能力**] 頁面上，選取 [**下載憑證**]。

   >[!NOTE]
   >[**下載 base64 憑證**] 選項適用于需要用來部署 base64 憑證的某些設定。

2. 使用企業系統管理員許可權登入已加入網域的電腦，然後從系統管理員命令提示字元執行下列命令，以將雲端根憑證新增到*Enterprise NTauth*存放區：

   >[!NOTE]
   >若為 VPN 伺服器未加入 Active Directory 網域的環境，則必須手動將雲端根憑證新增至「信任的_根憑證授權_單位」存放區。

   | 命令 | 描述 |
   | --- | --- |
   | `certutil -dspublish -f VpnCert.cer RootCA` | 會在**CN = AIA**和**Cn = 憑證授權單位**單位容器底下建立兩個**microsoft VPN 根 ca gen 1**容器，並將每個根憑證發佈為兩個**microsoft VPN 根 CA gen 1**容器的_cACertificate_屬性值。 |
   | `certutil -dspublish -f VpnCert.cer NTAuthCA` | 在**cn = AIA**和**CN = 憑證授權單位**單位容器底下建立一個**cn = NTAuthCertificates**容器，並將每個根憑證發佈為**cn = NTAuthCertificates**容器之_cACertificate_屬性的值。 |
   | `gpupdate /force` | 加速將根憑證新增至 Windows 伺服器和用戶端電腦。 |

3. 確認根憑證存在於 Enterprise NTauth store 中，並顯示為 [受信任]：
   1. 使用已安裝**憑證授權單位單位管理工具**的企業系統管理員許可權登入伺服器。

   >[!NOTE]
   >根據預設，**憑證授權單位單位管理工具**會安裝憑證授權單位單位伺服器。 在伺服器管理員中，您可以將它們安裝在其他成員伺服器上做為**角色管理工具**的一部分。

   1. 在 VPN 伺服器上的 [開始] 功能表中，輸入**pkiview**以開啟 [企業 PKI] 對話方塊。
   1. 在 [開始] 功能表中，輸入**pkiview**以開啟 [企業 PKI] 對話方塊。
   1. 在 [**企業 PKI** ] 上按一下滑鼠右鍵，然後選取 [**管理 AD 容器**]。
   1. 確認每個 Microsoft VPN 根 CA gen 1 憑證都出現在底下：
      - NTAuthCertificates
      - AIA 容器
      - 憑證授權單位單位容器

## <a name="next-steps"></a>後續步驟

[步驟7.5。建立以 OMA-URI 為基礎的 VPNv2 設定檔到 Windows 10 裝置](vpn-create-oma-dm-based-vpnv2-profiles.md)：在此步驟中，您可以使用 Intune 建立 oma-uri 型的 VPNv2 設定檔，以部署 VPN 裝置設定原則。 如果您想要 SCCM 或 PowerShell 腳本來建立 VPNv2 設定檔，請參閱[VPNV2 CSP 設定](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp)以取得更多詳細資料。
