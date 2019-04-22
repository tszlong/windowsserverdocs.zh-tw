---
title: 防止 Kerberos 變更使用 RC4 祕密金鑰的密碼
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: de207d55-aa3d-4c16-bd3b-496db43663a4
manager: alanth
author: justinha
ms.technology: security-crdential-protection-and-management
ms.date: 11/09/2016
ms.openlocfilehash: 3cfa4ae2442f0cd1e3e4110a355da675fa2d61db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816269"
---
# <a name="preventing-kerberos-change-password-that-uses-rc4-secret-keys"></a>防止 Kerberos 變更使用 RC4 祕密金鑰的密碼

>適用於：Windows Server （半年通道）、 Windows Server 2016、 Windows Server 2008 R2 和 Windows Server 2008

本主題適用於 IT 專業人員說明一些可能會導致惡意使用者的使用者帳戶控制的 Kerberos 通訊協定的限制。 沒有 Kerberos 網路驗證服務 (V5) 標準 (RFC 4120)，這是眾所皆知產業，藉此讓攻擊者可以驗證使用者身分或變更該使用者的密碼，如果攻擊者知道使用者的祕密金鑰的限制。

每個 RFC 4757 Kerberos 密碼變更交換期間，會驗證使用者的密碼衍生 Kerberos 祕密金鑰 （RC4 和進階加密標準 [AES] 依預設） 的擁有權。 使用者的純文字密碼永遠不會提供以金鑰發佈中心 (KDC)，並根據預設，Active Directory 網域控制站未擁有一份純文字密碼的帳戶。 如果網域控制站不支援 Kerberos 加密類型，該祕密金鑰不能變更密碼。 

在本主題開頭的 [套用到] 清單中指定的 Windows 作業系統，有三種方式可以封鎖透過 Kerberos 與 RC4 密鑰變更密碼的能力：

- 設定使用者時需要互動式登入帳戶加入 智慧卡上的 帳戶 選項。 這會限制使用者只能登入有效的智慧卡，讓 RC4 驗證服務要求 （AS 所） 會遭到拒絕。 若要設定帳戶的帳戶選項，以滑鼠右鍵按一下該帳戶中，按一下 [內容]，然後按一下 [帳戶] 索引標籤。 

- 停用所有的網域控制站上的 RC4 支援 kerberos。 這需要最少的 Windows Server 2008 網域功能等級和所有 Kerberos 用戶端、 應用程式伺服器，以及與網域的信任關係必須都支援 AES 的環境。 Windows Server 2008 和 Windows Vista 中引進到 aes 的支援。

    [!NOTE]
    沒有停用 RC4，這可能會導致系統重新啟動的已知的問題。 請參閱下列 hotfix:
    - [Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/3038261)
    - [Windows Server 2012](https://support.microsoft.com/en-us/kb/3086213)
    - 任何 hotfix 不可供舊版的 Windows Server

- 部署網域集，為 Windows Server 2012 R2 網域功能等級或更高版本，並將使用者設定為 Protected Users 安全性群組的成員。 因為這項功能會中斷不只是 RC4 中 Kerberos 通訊協定的使用量，請參閱下列資源[另請參閱](#see-also)一節。

## <a name="see-also"></a>另請參閱

- 如需如何預防 Windows Server 2012 R2 網域中的 RC4 加密類型的使用方式的資訊，請參閱[Protected Users 安全性群組](/../credentials-protection-and-management/protected-users-security-group.md)，並[How to Configure Protected Accounts](/../credentials-protection-and-management/how-to-configure-protected-accounts.md)。

- 如需有關 RFC 4120 和 RFC 4757 的說明，請參閱[IETF 文件](http://tools.ietf.org/html/)。
