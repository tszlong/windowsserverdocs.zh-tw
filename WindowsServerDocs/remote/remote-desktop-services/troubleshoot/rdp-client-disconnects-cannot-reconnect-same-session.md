---
title: 遠端桌面用戶端連線中斷，且無法重新連線至同一工作階段
description: 針對遠端桌面用戶端連線中斷，且無法重新連線至同一工作階段的問題進行疑難排解。
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 2c54442a5ce1322f4157de5dbf82d1c662c6f91d
ms.sourcegitcommit: 2a15de216edde8b8e240a4aa679dc6d470e4159e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2020
ms.locfileid: "77465592"
---
# <a name="remote-desktop-client-disconnects-and-cant-reconnect-to-the-same-session"></a>遠端桌面用戶端連線中斷，且無法重新連線至同一工作階段

遠端桌面用戶端失去遠端桌面連線後，用戶端無法立即重新連線。 使用者收到下列其中一個錯誤訊息：

  - 由於安全性錯誤，用戶端無法連線到終端機伺服器。 請確定您已登入網路，然後再次嘗試連線。
  - 遠端桌面中斷連線。 由於安全性錯誤，用戶端無法連線到遠端電腦。 請確認您已登入網路，並再次嘗試連線。

當遠端桌面用戶端重新連線時，RDSH 伺服器會將用戶端重新連線至新的工作階段，而不是原本的工作階段。 不過，當您檢查 RDSH 伺服器時，其指出原本的工作階段仍在使用中，並未進入中斷連線的狀態。

若要處理此問題，您可以在**電腦設定\\系統管理範本\\Windows 元件\\遠端桌面服務\\遠端桌面工作階段主機\\連線**群組原則資料夾中，啟用**設定持續連線連線間隔**原則。 如果您啟用此原則，您必須輸入持續連線間隔。 持續連線間隔可決定伺服器檢查工作階段狀態的頻率 (以分鐘為單位)。

此問題也可以透過重新設定驗證和組態設定來修正。 您可以在伺服器層級上或使用群組原則物件 (GPO) 來重新設定這些設定。 以下是重新進行設定的方法：**電腦設定\\系統管理範本\\Windows 元件\\遠端桌面服務\\遠端桌面工作階段主機\\安全性**群組原則資料夾。

1. 在 RD 工作階段主機伺服器上，開啟 [遠端桌面工作階段主機設定]  。
2. 在 [連線]  底下，以滑鼠右鍵按一下連線名稱，然後選取 [內容]  。
3. 在連線的 [內容]  對話方塊中，[一般]  索引標籤上的 [安全性]  圖層中，選取安全性方法。
4. 移至 [加密層級]  ，然後選取您想要的層級。 您可以選取 [低]  、[用戶端相容]  、[高]  或 [FIPS 相容]  。

> [!NOTE]  
>  - 當用戶端與 RD 工作階段主機伺服器之間的通訊需要最高層級的加密時，請使用與 FIPS 相容的加密。
>  - 您在群組原則中設定的任何加密層級設定都會覆寫使用遠端桌面服務設定工具所設的設定。 此外，如果您啟用[系統密碼編譯：使用 FIPS 相容演算法以供加密、雜湊和簽署](https://docs.microsoft.com/windows/security/threat-protection/security-policy-settings/system-cryptography-use-fips-compliant-algorithms-for-encryption-hashing-and-signing)原則，此設定會覆寫**設定用戶端連線加密層級**原則。 系統密碼編譯原則位於**電腦設定\\Windows 設定\\安全性設定\\本機原則\\安全性選項**資料夾中。
>  - 當您變更加密層級時，新的加密層級會在下次使用者登入時生效。 如果您需要在單一伺服器上設定多重加密層級，請安裝多張網路介面卡並分別設定每張介面卡。
>  - 若要確認憑證是否具有對應的私密金鑰，請移至遠端桌面服務設定，以滑鼠右鍵按一下要檢視憑證的連線，選取 [一般]  ，然後選取 [編輯]  。 完成後，選取 [檢視憑證]  。 當您移至在 [一般]  索引標籤時，如果金鑰已存在，您應該會看到這句話：「您有此憑證相對應的私密金鑰」。 您也可以使用 [憑證] 嵌入式管理單元來檢視這項資訊。
>  - FIPS 相容加密 (**系統密碼編譯：使用 FIPS 相容演算法以供加密、雜湊和簽署**原則或在遠端桌面伺服器設定中的 **FIPS 相容**設定) 使用 Microsoft 密碼編譯模組，透過聯邦資訊處理標準 (FIPS) 140-1 加密演算法，可加密及解密在伺服器和用戶端之間傳送的資料。 如需詳細資訊，請參閱 [FIPS 140 驗證](https://docs.microsoft.com/windows/security/threat-protection/fips-140-validation)。
>  - **高**設定可使用強式 128 位元加密，將在伺服器和用戶端之間傳送的資料加密。
>  - **用戶端相容**設定會以用戶端所支援的最大機碼強度加密用戶端和伺服器之間傳送的資料。
>  - **低**設定可使用 56 位元加密，將用戶端傳送到伺服器的資料加密。
