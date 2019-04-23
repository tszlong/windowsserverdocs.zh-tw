---
title: Windows 驗證中使用的群組原則設定
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9e237f89-45b1-4a4e-9b72-11dc7d6a470b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 38df2033b57c0394b96f539f54efe6a3579500f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847929"
---
# <a name="group-policy-settings-used-in-windows-authentication"></a>Windows 驗證中使用的群組原則設定

>適用於：Windows Server （半年通道），Windows Server 2016

這個參考主題適合 IT 專業人員說明使用和驗證程序中的群組原則設定的影響。

將使用者、 電腦和服務帳戶新增至群組，然後將驗證原則套用至這些群組，您可以管理 Windows 作業系統中的驗證。 這些原則被定義為本機安全性原則，以及系統管理範本，也稱為群組原則設定。 這兩個集合可以設定並散佈到整個組織使用群組原則。

> [!NOTE]
> 在 Windows Server 2012 R2 中引進的功能可讓您設定驗證原則的目標服務或應用程式，通常使用稱為驗證定址接收器，受保護的帳戶。 如需如何執行這項操作 Active Directory 中的資訊，請參閱[How to Configure Protected Accounts](how-to-configure-protected-accounts.md)。

例如，您可以套用下列原則至群組，根據其組織中的函式：

-   在本機或網域登入

-   透過網路登入

-   重設帳戶

-   建立帳戶

下表列出與驗證相關的原則群組，並提供連結至文件，可協助您設定這些原則。

|原則群組|Location|描述|
|--------|------|--------|
|**密碼原則**|本機電腦原則 \ 電腦設定 \windows 設定 \ 安全性帳戶原則|密碼原則會影響的特性和行為的密碼。 網域帳戶或本機使用者帳戶所使用的密碼原則。 它們會判斷密碼，例如強制執行和存留期的設定。<br /><br />如需特定設定資訊，請參閱 <<c0> [ 密碼原則](https://technet.microsoft.com/itpro/windows/keep-secure/password-policy)。|
|**帳戶鎖定原則**|本機電腦原則 \ 電腦設定 \windows 設定 \ 安全性帳戶原則|設定的登入嘗試失敗的次數後，帳戶鎖定原則選項會停用帳戶。 使用這些選項，可協助您偵測及阻擋嘗試破解密碼。<br /><br />帳戶鎖定原則選項的相關資訊，請參閱[帳戶鎖定原則](https://technet.microsoft.com/itpro/windows/keep-secure/account-lockout-policy)。|
|**Kerberos 原則**|本機電腦原則 \ 電腦設定 \windows 設定 \ 安全性帳戶原則|Kerbero 相關的設定包括票證存留期和強制執行規則。 Kerberos 原則不適用於本機帳戶資料庫，因為 Kerberos 驗證通訊協定不用來驗證本機帳戶。 因此，Kerberos 原則設定可以設定只能透過預設網域群組原則物件 (GPO)，它會影響網域登入。<br /><br />網域控制站的 Kerberos 原則選項的相關資訊，請參閱[Kerberos 原則](https://technet.microsoft.com/itpro/windows/keep-secure/kerberos-policy)。|
|**稽核原則**|Local Computer Policy\Computer Configuration\Windows Settings\Security Settings\Local Policies\Audit Policy|稽核原則可讓您控制及了解物件，例如檔案和資料夾，以及管理使用者和群組帳戶的使用者登入和登出的存取權。 稽核原則可以指定您想要稽核、 設定的大小與行為的安全性記錄檔，並判斷哪些物件要監視的存取，以及您想要監視的存取類型的事件的類別。<br /><br />|
|**使用者權限指派**|本機電腦原則 \ 電腦設定 \windows 設定 \ 安全性設定 \ Settings\Local Policies\User Rights Assignment|通常會根據安全性使用者所屬的群組，例如 Administrators、 Power Users 或使用者指派的使用者權限。 此類別中的原則設定通常用於授與或拒絕存取的存取和安全性的群組成員資格的方法為基礎的電腦的權限。|
|**安全性選項**|本機電腦原則 \ 電腦設定 \windows 設定 \ 安全性設定 \ 本機原則 \ 安全性選項|與驗證相關的原則包括：<br /><br />裝置<br />網域控制站<br />網域成員<br />互動式登入<br />Microsoft 網路伺服器<br />-網路存取<br />網路安全性<br />-修復主控台<br />關機<br /><br />|
|**認證委派**|Computer Configuration\Administrative Templates\System\Credentials Delegation|認證的委派是一種機制，可讓其他系統，最值得注意的是成員伺服器和網域內的網域控制站上使用的本機認證。 這些設定套用至應用程式使用認證安全性支援提供者 (認證 SSP)。 例如遠端桌面連線。|
|**KDC**|Computer Configuration\Administrative Templates\System\KDC|這些原則設定會影響金鑰發佈中心 (KDC)，也就是網域控制站上的服務，如何處理 Kerberos 驗證要求。|
|**Kerberos**|Computer Configuration\Administrative Templates\System\Kerberos|這些原則設定會影響 Kerberos 的設定來處理支援宣告、 Kerberos 防護、 複合驗證、 識別 proxy 伺服器，以及其他設定的方式。|
|**登入**|電腦設定\系統管理範本\系統\登入|這些原則設定會控制如何系統會顯示使用者的登入體驗。|
|**Net Logon**|Computer Configuration\Administrative Templates\System\Net Logon|這些原則設定會控制系統的網路登入要求，包括網域控制站定位程式的運作方式的處理方式。<br /><br />如需有關如何將網域控制站定位程式放入複寫程序的詳細資訊，請參閱 <<c0> [ 了解站台之間的複寫](https://technet.microsoft.com/library/cc771251.aspx)。|
|**生物識別技術**|電腦設定 \ 系統管理範本 \windows 元件 \ Components\Biometrics|這些原則設定通常會允許或拒絕使用生物識別技術，做為驗證方法。<br /><br />如需 Windows 實作的生物識別技術資訊，請參閱 < Windows 生物特徵辨識架構概觀。|
|**認證的使用者介面**|電腦設定 \ 系統管理範本 \windows 元件 \ Components\Credential 使用者介面|這些原則設定會控制如何在項目管理認證。|
|**密碼同步處理**|電腦設定 \ 系統管理範本 \windows 元件 \ Components\Password 同步處理|這些原則設定會決定系統管理的 Windows 和 unix 作業系統之間的密碼同步處理的方式。<br /><br />如需詳細資訊，請參閱 <<c0> [ 密碼同步化](https://technet.microsoft.com/library/cc732609.aspx)。|
|**智慧卡**|Computer Configuration\Administrative Templates\Windows Components\Smart Card|這些原則設定會控制系統管理智慧卡登入的方式。<br /><br />|
|**Windows 登入選項**|電腦設定 \ 系統管理範本 \windows 元件 \windows 登入選項|這些原則設定會控制何時及如何登入的機會有。|
|**Ctrl + Alt + Del 選項**|電腦設定 \ 系統管理範本 \windows 元件 \ Components\Ctrl + Alt + Del 選項|這些原則設定會影響的外觀和功能登入 UI （安全桌面），例如 工作管理員和鍵盤鎖定之電腦的存取設定。|
|**登入**|Computer Configuration\Administrative Templates\Windows Components\Logon|這些原則設定會判斷如果或當使用者登入時，可以執行哪些處理程序。|

## <a name="see-also"></a>另請參閱
[Windows 驗證技術概觀](windows-authentication-technical-overview.md)


