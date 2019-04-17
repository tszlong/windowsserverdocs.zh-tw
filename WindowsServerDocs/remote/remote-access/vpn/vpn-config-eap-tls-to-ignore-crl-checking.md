---
title: 設定 EAP-TLS 以忽略憑證撤銷清單 (CRL) 檢查
description: EAP-TLS 用戶端無法連線，除非 NPS 伺服器完成撤銷檢查的用戶端憑證鏈結 （包括根憑證），並確認已撤銷的憑證。
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
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067154"
---
# 步驟 7.1. 設定 EAP-TLS 以忽略憑證撤銷清單 (CRL) 檢查

>適用於： Windows Server （半年度管道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10

& #171; [**先前：** 步驟 7。（選擇性）使用 Azure AD 的 VPN 連線的條件式存取](ad-ca-vpn-connectivity-windows10.md)<br>
& #187;[**下一步：** 7.2 步驟。使用 Azure AD 中建立 VPN 驗證的根憑證](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

>[!IMPORTANT]
>無法實作此登錄變更將導致 IKEv2 連線使用雲端憑證和 PEAP，失敗，但使用內部 ca 發行的用戶端驗證憑證的 IKEv2 連線會繼續運作。

在此步驟中，您可以新增**IgnoreNoRevocationCheck** ，並將它設定為允許用戶端的驗證時的憑證不包括 CRL 發佈點。 根據預設，IgnoreNoRevocationCheck 設為 0 （已停用）。

>[!NOTE]
>如果 Windows 路由及遠端存取伺服器 (RRAS) 會使用 NPS proxy 半徑會呼叫到第二個 NPS，則您必須設定**IgnoreNoRevocationCheck = 1**兩部伺服器上。

EAP-TLS 用戶端無法連線，除非 NPS 伺服器完成撤銷檢查憑證鏈結 （包括根憑證）。 Azure AD 所簽發給使用者的雲端憑證沒有 CRL，因為它們是在一小時存留的短期憑證。 在 NPS 上的 EAP 必須忽略 CRL 缺少設定。 根據預設，IgnoreNoRevocationCheck 設為 0 （已停用）。 新增 IgnoreNoRevocationCheck，並將其設為 1 可讓用戶端的驗證時的憑證不包括 CRL 發佈點。 

因為 EAP-TLS 驗證方法，此登錄值時，才需要 EAP\13 底下。 如果使用其他 EAP 驗證方法，然後登錄值應該新增下的那些也。 

**程序**

1. 開啟**regedit.exe** NPS 伺服器上。

2. 瀏覽至**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13**。

3. 按一下**編輯 > 新增**並選取 [ **DWORD （32 位元） 值**，並輸入**IgnoreNoRevocationCheck**。

4. 按兩下**IgnoreNoRevocationCheck** ，並將值資料設定為**1**。

5. 按一下 **[確定]** ，並重新啟動伺服器。 重新啟動 RRAS 及 NPS 服務不會不已足夠。

如需詳細資訊，請參閱[如何啟用或停用用戶端上的憑證撤銷檢查 (CRL)](https://technet.microsoft.com/library/bb680540.aspx)。


|登錄路徑  |EAP 擴充功能  |
|---------|---------|
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13     |EAP-TLS         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\25     |PEAP         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\26     |EAP MSCHAP v2         |

## 後續步驟

[步驟 7.2。使用 Azure AD 中建立 VPN 驗證的根憑證](vpn-create-root-cert-for-vpn-auth-azure-ad.md)： 在此步驟中，您可以設定的 VPN 驗證的條件式存取根憑證與 Azure AD，會自動租用戶中建立的 VPN 伺服器雲端應用程式。 

---