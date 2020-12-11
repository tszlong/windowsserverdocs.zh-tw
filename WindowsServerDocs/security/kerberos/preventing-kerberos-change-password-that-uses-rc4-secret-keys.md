---
description: 深入瞭解：防止使用 RC4 秘密金鑰的 Kerberos 變更密碼
title: 防止使用 RC4 秘密金鑰的 Kerberos 變更密碼
ms.topic: article
ms.assetid: de207d55-aa3d-4c16-bd3b-496db43663a4
author: justinha
ms.author: Justinha
ms.date: 11/09/2016
ms.openlocfilehash: cad6948346c7d4209e9b476882a9c6649f4d8990
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97046126"
---
# <a name="preventing-kerberos-change-password-that-uses-rc4-secret-keys"></a>防止 Kerberos 變更使用 RC4 祕密金鑰的密碼

>適用于： Windows Server (半年通道) 、Windows Server 2016、Windows Server 2008 R2 和 Windows Server 2008

本主題適用于 IT 專業人員，說明 Kerberos 通訊協定的一些限制，這些限制可能會導致惡意使用者接管使用者帳戶的控制。 Kerberos 網路驗證服務有一項限制， (V5) 標準 (RFC 4120) （在產業內已知），攻擊者可以用使用者身分進行驗證，或在攻擊者知道使用者的秘密金鑰時變更該使用者的密碼。

依預設，擁有使用者的密碼衍生 Kerberos 秘密金鑰 (RC4 和進階加密標準 [AES]) 會在每個 RFC 4757 的 Kerberos 密碼變更交換期間進行驗證。 使用者的純文字密碼永遠不會提供給金鑰發佈中心 (KDC) ，Active Directory 網域控制站預設不會有帳戶的純文字密碼複本。 如果網域控制站不支援 Kerberos 加密類型，則無法使用該秘密金鑰來變更密碼。

在本主題開頭的適用物件清單中指定的 Windows 作業系統中，有三種方式可以封鎖使用 Kerberos 和 RC4 秘密金鑰來變更密碼的能力：

- 設定使用者帳戶以包含互動式登入需要智慧卡的帳戶選項。 這會限制使用者只能使用有效的智慧卡登入，因此會拒絕 RC4 驗證服務要求 (先決條件) 。 若要在帳戶上設定帳戶選項，請在帳戶上按一下滑鼠右鍵，按一下 [屬性]，然後按一下 [帳戶] 索引標籤。

- 在所有網域控制站上停用 Kerberos 的 RC4 支援。 這至少需要 Windows Server 2008 網域功能等級，以及與網域之間的所有 Kerberos 用戶端、應用程式伺服器和信任關係都必須支援 AES 的環境。 AES 的支援是在 Windows Server 2008 和 Windows Vista 中引進。

    [!NOTE]
    停用 RC4 可能會導致系統重新開機的已知問題。 請參閱下列修補程式：
    - [Windows Server 2012 R2](https://support.microsoft.com/kb/3038261)
    - [Windows Server 2012](https://support.microsoft.com/kb/3086213)
    - 舊版 Windows Server 沒有任何可用的修正程式

- 將網域設定為 Windows Server 2012 R2 網域功能等級或更高版本，並將使用者設定為 [Protected Users] 安全性群組的成員。 因為這項功能不只會干擾 Kerberos 通訊協定中的 RC4 使用方式 [，請參閱下列另](#see-also) 一節中的資源。

## <a name="see-also"></a>另請參閱

- 如需有關如何避免在 Windows Server 2012 R2 網域中使用 RC4 加密類型的詳細資訊，請參閱 [Protected Users 安全性群組](/../credentials-protection-and-management/protected-users-security-group.md)，以及 [如何設定受保護的帳戶](/../credentials-protection-and-management/how-to-configure-protected-accounts.md)。

- 如需 RFC 4120 和 RFC 4757 的說明，請參閱 [IETF 檔](http://tools.ietf.org/html/)。
