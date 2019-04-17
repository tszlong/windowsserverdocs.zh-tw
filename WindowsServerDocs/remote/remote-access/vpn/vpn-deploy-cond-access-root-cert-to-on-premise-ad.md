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
ms.openlocfilehash: 210540846f5d62dfc74a2e629a6b7675ccf9894d
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067122"
---
# 步驟 7.4. 將條件式存取根憑證部署到內部部署 AD

>適用於： Windows Server （半年度管道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10

在此步驟中，您將部署條件式存取根憑證做為受信任的根憑證的 VPN 驗證到您內部部署 AD。

& #171; [**先前：** 7.3 步驟。設定條件式存取原則](vpn-config-conditional-access-policy.md)<br>
& #187;[**下一步：** 7.5 步驟。建立 OMA DM 基礎 VPNv2 設定檔到 Windows 10 裝置](vpn-create-oma-dm-based-vpnv2-profiles.md)

1. **VPN 連線**在頁面上，按一下 [**下載憑證**。 
   
    ![下載憑證的條件式存取](../../media/Always-On-Vpn/06.png)

    >[!NOTE]
    >**下載 base64 憑證**選項只適用於需要 base64 憑證部署的一些設定。 

2. 登入企業系統管理員權限已加入網域的電腦，並從雲端根憑證新增到*企業 NTauth*存放區的系統管理員命令提示字元執行下列命令：

    >[!NOTE]
    >針對 VPN 伺服器未加入到 Active Directory 網域的環境，雲端的根憑證必須新增至_受信任的根憑證授權單位_存放區以手動方式。

    |命令  |描述  |  
    |---------|-------------| 
    |`certutil -dspublish -f VpnCert.cer RootCA`     |建立兩個**Microsoft VPN 根 CA 產生 1**容器底下**CN = AIA**和**CN = 憑證授權單位**容器，並做為值的這兩個**Microsoft VPN 根_Ca_屬性上發行的每個根憑證CA 產生 1**容器。|  
    |`certutil -dspublish -f VpnCert.cer NTAuthCA`   |會建立一個**CN = NTAuthCertificates**之下容器**CN = AIA**和**CN = 憑證授權單位**容器，並發行的每個根憑證做為上**CN 的_Ca_屬性值 =NTAuthCertificates**容器。 |  
    |`gpupdate /force`     |可以加速將根憑證新增至 Windows server 和用戶端電腦。  |

3.  確認存在在企業 NTauth 市集和顯示為受信任的根憑證：

    a.  登入已安裝**憑證授權單位管理工具**的伺服器與企業系統管理員權限。

    >[!NOTE]
    >根據預設**憑證授權單位管理工具**會安裝的憑證授權單位的伺服器。 他們可以安裝在其他成員伺服器上的**角色管理工具**伺服器管理員中的一部分。

    b。  在 VPN 伺服器上，在 [開始] 功能表中，輸入**pkiview.msc**即可開啟企業 PKI 對話方塊。

    c.  從 [開始] 功能表中，輸入**pkiview.msc**即可開啟企業 PKI 對話方塊。

    d.  **企業 PKI**上按一下滑鼠右鍵，然後選取 [**管理 AD 容器**。

    d.  確認每個 Microsoft VPN 根 CA 產生 1 憑證下方顯示：<ul><li>NTAuthCertificates</li><li>AIA 容器</li><li>憑證授權單位容器</li></ul>

    
## 後續步驟
[步驟 7.5。建立 OMA DM 基礎 VPNv2 設定檔到 Windows 10 裝置](vpn-create-oma-dm-based-vpnv2-profiles.md)： 在此步驟中，您可以建立 OMA DM 基礎 VPNv2 設定檔使用 Intune 部署 VPN 裝置設定原則。 如果您想要 SCCM 或 PowerShell 指令碼建立 VPNv2 設定檔時，請參閱[VPNv2 CSP 設定](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp)，如需詳細資訊。

---
