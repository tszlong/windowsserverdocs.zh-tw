---
title: "防止 Kerberos 變更密碼使用 RC4 秘密鍵"
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: de207d55-aa3d-4c16-bd3b-496db43663a4
manager: alanth
author: justinha
ms.technology: security-crdential-protection-and-management
ms.date: 11/09/2016
ms.openlocfilehash: 3cfa4ae2442f0cd1e3e4110a355da675fa2d61db
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="preventing-kerberos-change-password-that-uses-rc4-secret-keys"></a>使用 RC4 秘密金鑰阻止 Kerberos 變更密碼

>適用於：Windows Server（以每年次通道）、Windows Server 2016、Windows Server 2008 R2 和 Windows Server 2008

本主題適用於 IT 專業人員解釋惡意使用者帳號控制可能會導致 Kerberos 通訊協定一些限制。 還有 Kerberos 網路驗證服務 (V5) 標準 (RFC 4120)，這是已知業界攻擊可以驗證使用者或變更密碼使用者的攻擊者知道使用者的金鑰，在限制。

在每個 RFC 4757 Kerberos 密碼變更交換驗證擁有的使用者的密碼衍生 Kerberos 密碼金鑰（RC4 和進階加密標準 [好一段] 的預設值）。 使用者純文字密碼不提供以金鑰 Distribution 中心 (KDC)，並預設 Active Directory 網域控制站不擁有一份適用於帳號純文字密碼。 如果不支援的網域控制站 Kerberos 加密類型，該私密金鑰無法用來變更密碼。 

在本主題的開頭套用至清單中指定的 Windows 作業系統，有三種方式可以封鎖變更密碼 Kerberos 使用 RC4 秘密按鍵的功能：

- 設定使用者 account 包含 account 選項智慧卡，才互動式登入。 此限制使用者只登使用有效的智慧卡，會拒絕 RC4 驗證服務要求（需求為）。 若要設定 account 選項，在帳號上, 按一下滑鼠右鍵按一下屬性，帳號，並按一下 Account] 索引標籤。 

- 停用所有網域控制站 Kerberos RC4 支援。 這需要的 Windows Server 2008 網域功能層級和所有 Kerberos 戶端、應用程式的伺服器和的網域信任關係必須都支援好一段環境。 Windows Server 2008 和 Windows Vista 中引進好一段的支援。

    [!NOTE]
    還有停用，可能會導致電腦重新開機 RC4 的已知的問題。 請查看下列 hotfix:
    - [Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/3038261)
    - [Windows Server 2012](https://support.microsoft.com/en-us/kb/3086213)
    - 不 hotfix 適用於舊版的 Windows Server

- 部署網域設定 Windows Server 2012 R2 網域功能等級或更高版本，並設定的使用者的受保護的使用者安全性群組成員。 由於這項功能會中斷不只是 RC4 Kerberos 通訊協定的使用量，查看資源在下列[也看到](#see-also)一節。

## <a name="see-also"></a>也了

- 若要防止 Windows Server 2012 R2 網域中的 RC4 加密類型的使用方式的相關資訊，會看到[受保護的使用者安全性群組](/../credentials-protection-and-management/protected-users-security-group.md)，和[設定保護帳號如何](/../credentials-protection-and-management/how-to-configure-protected-accounts.md)。

- 如有關 RFC 4120 與 RFC 4757 解釋，[IETF 文件](http://tools.ietf.org/html/)。
