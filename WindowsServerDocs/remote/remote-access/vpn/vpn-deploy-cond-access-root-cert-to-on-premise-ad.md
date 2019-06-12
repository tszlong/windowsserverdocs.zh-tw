---
title: 將條件式存取根憑證部署至內部部署 AD
description: ''
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
documentationcenter: ''
ms.assetid: ''
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 4aaad98cd04c9b07bdea848294e10d9bcb602064
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749543"
---
# <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-ad"></a>步驟 7.4. 將條件式存取的根憑證部署至內部部署 AD

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows 10

在此步驟中，您將部署條件式存取的根憑證做為 VPN 驗證的受信任的根憑證到您內部部署 AD。

- [**前一個：** 步驟 7.3.設定條件式存取原則](vpn-config-conditional-access-policy.md)
- [**下一步:** 步驟 7.5.建立 OMA-DM 型 VPNv2 設定檔至 Windows 10 裝置](vpn-create-oma-dm-based-vpnv2-profiles.md)

1. 在  **VPN 連線能力**頁面上，選取**下載憑證**。 
   
    ![下載憑證進行條件式存取](../../media/Always-On-Vpn/06.png)

    >[!NOTE]
    >**下載 base64 憑證**選項只適用於某些需要 base64 憑證來進行部署的組態。 

2. 登入以企業系統管理員權限和執行這些命令從系統管理員命令提示字元來新增雲端根憑證到已加入網域的電腦*Enterprise NTauth*儲存：

    >[!NOTE]
    >對於未加入 Active Directory 網域加入 VPN 伺服器的環境，雲端的根憑證必須新增至_受信任的根憑證授權單位_以手動方式儲存。

    |命令  |描述  |  
    |---------|-------------| 
    |`certutil -dspublish -f VpnCert.cer RootCA`     |會建立兩個**Microsoft VPN 根 CA 層代 1**下容器**CN = AIA**並**CN = 憑證授權單位**容器，並將每個根憑證做為值發佈上_cACertificate_兩個屬性**Microsoft VPN 根 CA 層代 1**容器。|  
    |`certutil -dspublish -f VpnCert.cer NTAuthCA`   |會建立一個**CN = NTAuthCertificates**容器下的容器**CN = AIA**並**CN = 憑證授權單位**容器，並將每個根憑證做為值發佈上_cACertificate_屬性**CN = NTAuthCertificates**容器。 |  
    |`gpupdate /force`     |可以加速將根憑證新增至 Windows server 和用戶端電腦。  |

3.  確認存在於 Enterprise NTauth 存放區與顯示為受信任的根憑證：

    a.  登入擁有以企業系統管理員權限的伺服器**憑證授權單位管理工具**安裝。

    >[!NOTE]
    >依預設**憑證授權單位管理工具**是已安裝的憑證授權單位伺服器。 可將它們安裝在其他成員伺服器上的一部分**角色管理工具**在 [伺服器管理員] 中。

    b.  在 VPN 伺服器上，在 [開始] 功能表中，輸入**pkiview.msc**開啟 [企業 PKI] 對話方塊。

    c.  從 [開始] 功能表中，輸入**pkiview.msc**開啟 [企業 PKI] 對話方塊。

    d.  以滑鼠右鍵按一下**企業 PKI** ，然後選取**管理 AD 容器**。

    d.  請確認每個 Microsoft VPN 根 CA 層代 1 憑證下：
      - NTAuthCertificates
      - AIA 容器
      - 憑證授權單位容器

## <a name="next-steps"></a>後續步驟

[步驟 7.5.建立 OMA DM 基礎 VPNv2 設定檔到 Windows 10 裝置](vpn-create-oma-dm-based-vpnv2-profiles.md):在此步驟中，您可以建立 OMA DM 基礎 VPNv2 使用 Intune 來部署 VPN 裝置組態原則的設定檔。 如果您想要 SCCM 或 PowerShell 指令碼來建立 VPNv2 設定檔，請參閱[VPNv2 CSP 設定](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp)如需詳細資訊。
