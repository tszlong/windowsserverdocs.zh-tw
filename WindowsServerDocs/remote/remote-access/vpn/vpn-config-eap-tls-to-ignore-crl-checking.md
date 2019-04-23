---
title: 設定 EAP-TLS 以忽略憑證撤銷清單 (CRL) 檢查
description: EAP-TLS 用戶端無法連線，除非在 NPS 伺服器完成用戶端的憑證鏈結 （包括根憑證） 撤銷檢查，並確認憑證已遭撤銷。
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
ms.openlocfilehash: ac59c554c69a6138a106a648c3fab3ed4fe05b7b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836419"
---
# <a name="step-71-configure-eap-tls-to-ignore-certificate-revocation-list-crl-checking"></a>步驟 7.1. 設定 EAP-TLS 以忽略憑證撤銷清單 (CRL) 檢查

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows 10

&#171;  [**前一個：** 步驟 7.（選擇性）使用 Azure AD 的 VPN 連線能力的條件式存取](ad-ca-vpn-connectivity-windows10.md)<br>
&#187;[ **下一步:** 步驟 7.2.使用 Azure AD 中建立 VPN 驗證的根的憑證](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

>[!IMPORTANT]
>若要實作這項登錄變更的失敗會導致失敗，將雲端憑證使用 PEAP 的 IKEv2 連線，但使用來自內部 CA 簽發的用戶端驗證憑證的 IKEv2 連線會繼續運作。

在此步驟中，您可以加入**IgnoreNoRevocationCheck**並將它設定為允許用戶端驗證憑證不含 CRL 發佈點時。 根據預設，IgnoreNoRevocationCheck 設為 0 （停用）。

>[!NOTE]
>如果 Windows 路由及遠端存取伺服器 (RRAS) 會使用 NPS proxy RADIUS 呼叫第二個 NPS 中，則您必須將**IgnoreNoRevocationCheck = 1**兩部伺服器上。

EAP-TLS 用戶端無法連線，除非在 NPS 伺服器完成 （包括根憑證） 的憑證鏈結的撤銷檢查。 Azure AD 所簽發給使用者的雲端憑證沒有 CRL，因為它們是短期的憑證具有一小時的存留期。 在 NPS 上的 EAP 必須設定為忽略的 CRL 不存在。 根據預設，IgnoreNoRevocationCheck 設為 0 （停用）。 新增 IgnoreNoRevocationCheck，並將它設定為 1，以允許用戶端驗證憑證不含 CRL 發佈點時。 

因為驗證方法為 EAP-TLS，EAP\13 下才需要此登錄值。 如果使用其他 EAP 驗證方法，然後登錄值就應該將在這些以及。 

**程序**

1. 開啟**regedit.exe** NPS 伺服器上。

2. 瀏覽至**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13**。

3. 按一下 **編輯 > 新增**，然後選取**DWORD （32 位元） 值**然後輸入**IgnoreNoRevocationCheck**。

4. 按兩下**IgnoreNoRevocationCheck**並將值資料設定為**1**。

5. 按一下 **確定**並重新啟動伺服器。 重新啟動的 RRAS 和 NPS 服務不敷使用。

如需詳細資訊，請參閱 <<c0> [ 如何啟用或停用用戶端上的憑證撤銷檢查 (CRL)](https://technet.microsoft.com/library/bb680540.aspx)。


|登錄路徑  |EAP 延伸模組  |
|---------|---------|
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13     |EAP-TLS         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\25     |PEAP         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\26     |EAP-MSCHAP v2         |

## <a name="next-step"></a>後續步驟

[7.2 的步驟。使用 Azure AD 中建立根憑證以進行 VPN 驗證](vpn-create-root-cert-for-vpn-auth-azure-ad.md):在此步驟中，您可以設定 VPN 驗證的條件式存取的根憑證與 Azure AD 中，會自動建立租用戶中的 VPN 伺服器的雲端應用程式。 

---