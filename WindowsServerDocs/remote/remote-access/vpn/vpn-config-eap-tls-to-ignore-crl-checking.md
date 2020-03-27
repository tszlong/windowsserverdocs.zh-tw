---
title: 設定 EAP-TLS 以忽略憑證撤銷清單 (CRL) 檢查
description: 除非 NPS 伺服器已完成用戶端憑證鏈（包含根憑證）的撤銷檢查，並確認已撤銷憑證，否則 EAP-TLS 用戶端無法連線。
services: active-directory
ms.prod: windows-server
ms.technology: networking-ras
documentationcenter: ''
ms.assetid: ''
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2018
ms.author: lizross
author: eross-msft
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: c58386467d632d77fdc96bf3bc18b5a85d1ecdaf
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80307699"
---
# <a name="step-71-configure-eap-tls-to-ignore-certificate-revocation-list-crl-checking"></a>步驟 7.1. 設定 EAP-TLS 以忽略憑證撤銷清單 (CRL) 檢查

>適用于： Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows 10

- [**上一步：** 步驟7：選擇性使用 Azure AD 進行 VPN 連線的條件式存取](ad-ca-vpn-connectivity-windows10.md)
- [**下一步：** 步驟7.2。使用 Azure AD 建立根憑證以進行 VPN 驗證](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

>[!IMPORTANT]
>若無法執行此登錄變更，將會導致使用雲端憑證與 PEAP 的 IKEv2 連線失敗，但使用從內部部署 CA 發出的用戶端驗證憑證的 IKEv2 連線仍會繼續作用。

在此步驟中，您可以新增**IgnoreNoRevocationCheck**並將它設定為允許用戶端在憑證不包含 CRL 發佈點時進行驗證。 根據預設，IgnoreNoRevocationCheck 會設定為0（已停用）。

>[!NOTE]
>如果 Windows 路由及遠端存取服務器（RRAS）使用 NPS 來 proxy 對第二個 NPS 的 RADIUS 呼叫，則您必須在兩部伺服器上設定**IgnoreNoRevocationCheck = 1** 。

除非 NPS 伺服器已完成憑證鏈（包含根憑證）的撤銷檢查，否則 EAP-TLS 用戶端無法連接。 由 Azure AD 發行給使用者的雲端憑證不具有 CRL，因為其存留期為一小時的短期憑證。 NPS 上的 EAP 必須設定為忽略不存在 CRL 的情況。 根據預設，IgnoreNoRevocationCheck 會設定為0（已停用）。 新增 IgnoreNoRevocationCheck 並將它設定為1，以便在憑證不包含 CRL 發佈點時，允許用戶端進行驗證。 

由於驗證方法是 EAP-TLS，因此只有在 EAP\13. 下才需要此登錄值 如果使用其他 EAP 驗證方法，則也應該在這些情況下新增登錄值。 

**步**

1. 在 NPS 伺服器上開啟**regedit.exe。**

2. 流覽至**HKEY_LOCAL_MACHINE \system\currentcontrolset\services\rasman\ppp\eap\13**。

3. 選取 **編輯 > 新增**，然後選取**DWORD （32-位） 值**並輸入**IgnoreNoRevocationCheck**。

4. 按兩下 [ **IgnoreNoRevocationCheck** ]，並將值資料設定為**1**。

5. 選取 **[確定]** ，然後重新開機伺服器。 重新開機 RRAS 和 NPS 服務並不足夠。

如需詳細資訊，請參閱[如何啟用或停用用戶端上的憑證撤銷檢查（CRL）](https://technet.microsoft.com/library/bb680540.aspx)。


|登錄路徑  |EAP 延伸模組  |
|---------|---------|
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13     |EAP-TLS         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\25     |PEAP         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\26     |EAP-MSCHAP v2         |

## <a name="next-steps"></a>後續步驟

[步驟7.2。使用 Azure AD 建立根憑證以進行 VPN 驗證](vpn-create-root-cert-for-vpn-auth-azure-ad.md)：在此步驟中，您會設定使用 AZURE AD vpn 驗證的條件式存取根憑證，這會自動在租使用者中建立 Vpn 伺服器雲端應用程式。
