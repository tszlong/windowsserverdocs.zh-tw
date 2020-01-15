---
title: 防止使用 RC4 秘密金鑰的 Kerberos 變更密碼
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: de207d55-aa3d-4c16-bd3b-496db43663a4
manager: alanth
author: justinha
ms.technology: security-crdential-protection-and-management
ms.date: 11/09/2016
ms.openlocfilehash: 21c2d3d79653bd02fea9d2ac0d09bd18690a388f
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949739"
---
# <a name="preventing-kerberos-change-password-that-uses-rc4-secret-keys"></a>防止 Kerberos 變更使用 RC4 祕密金鑰的密碼

>適用于： Windows Server （半年通道）、Windows Server 2016、Windows Server 2008 R2 和 Windows Server 2008

本主題適用于 IT 專業人員，說明 Kerberos 通訊協定中的一些限制，可能會導致惡意使用者控制使用者的帳戶。 Kerberos 網路驗證服務（V5）標準（RFC 4120）有一項限制，這在業界中是知名的，因此攻擊者可以使用者身分驗證，或在攻擊者知道使用者的秘密金鑰時變更該使用者的密碼。

根據預設，使用者的密碼衍生 Kerberos 秘密金鑰（RC4 和進階加密標準 [AES]）會在每個 RFC 4757 的 Kerberos 密碼變更交換期間進行驗證。 使用者的純文字密碼永遠不會提供給金鑰發佈中心（KDC），而且根據預設，Active Directory 網域控制站不會有帳戶的純文字密碼複本。 如果網域控制站不支援 Kerberos 加密類型，則不能使用該秘密金鑰來變更密碼。 

在本主題開頭的適用物件清單中指定的 Windows 作業系統中，有三種方式可以使用 Kerberos 搭配 RC4 秘密金鑰來封鎖變更密碼的功能：

- 將使用者帳戶設定為包含帳戶選項 [互動式登入需要智慧卡]。 這會限制使用者只能使用有效的智慧卡登入，因此會拒絕 RC4 驗證服務要求（先決條件）。 若要設定帳戶的帳戶選項，請在帳戶上按一下滑鼠右鍵，按一下 [屬性]，然後按一下 [帳戶] 索引標籤。 

- 停用所有網域控制站上 Kerberos 的 RC4 支援。 這需要至少 Windows Server 2008 網域功能等級，以及與網域之間的所有 Kerberos 用戶端、應用程式伺服器和信任關係都必須支援 AES 的環境。 對 AES 的支援是在 Windows Server 2008 和 Windows Vista 中引進。

    [!NOTE]
    停用 RC4 可能會造成系統重新開機的已知問題。 請參閱下列修補程式：
    - [Windows Server 2012 R2](https://support.microsoft.com/kb/3038261)
    - [Windows Server 2012](https://support.microsoft.com/kb/3086213)
    - 舊版 Windows Server 沒有任何可用的修補程式

- 部署設定為 Windows Server 2012 R2 網域功能等級或更高層級的網域，並將使用者設定為 Protected Users 安全性群組的成員。 因為這項功能只會干擾 Kerberos 通訊協定中的 RC4 使用方式，請參閱下列另[請參閱](#see-also)一節中的資源。

## <a name="see-also"></a>請參閱

- 如需如何防止在 Windows Server 2012 R2 網域中使用 RC4 加密類型的相關資訊，請參閱[Protected Users 安全性群組](/../credentials-protection-and-management/protected-users-security-group.md)，以及[如何設定受保護的帳戶](/../credentials-protection-and-management/how-to-configure-protected-accounts.md)。

- 如需 RFC 4120 和 RFC 4757 的說明，請參閱[IETF 檔](http://tools.ietf.org/html/)。
