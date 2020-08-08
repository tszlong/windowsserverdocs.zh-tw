---
title: Windows 驗證中使用的群組原則設定
description: Windows Server 安全性
ms.topic: article
ms.assetid: 9e237f89-45b1-4a4e-9b72-11dc7d6a470b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 912850e8203627f4cc7106731ac28cf9e914cc46
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87997639"
---
# <a name="group-policy-settings-used-in-windows-authentication"></a>Windows 驗證中使用的群組原則設定

>適用於：Windows Server (半年度管道)、Windows Server 2016

這個適用于 IT 專業人員的參考主題說明驗證程式中群組原則設定的使用與影響。

您可以藉由將使用者、電腦和服務帳戶新增至群組，然後將驗證原則套用至這些群組，來管理 Windows 作業系統中的驗證。 這些原則會定義為本機安全性原則和系統管理範本，也稱為群組原則設定。 您可以使用群組原則，在整個組織中設定和散發這兩個集合。

> [!NOTE]
> Windows Server 2012 R2 中引進的功能，可讓您使用受保護的帳戶，為目標服務或應用程式（通常稱為驗證定址接收器）設定驗證原則。 如需如何在 Active Directory 中執行此動作的詳細資訊，請參閱[如何設定受保護的帳戶](../../identity/ad-ds/manage/how-to-configure-protected-accounts.md)。

例如，您可以根據組織中的功能，將下列原則套用至群組：

-   本機登入或網域

-   透過網路登入

-   重設帳戶

-   建立帳戶

下表列出與驗證相關的原則群組，並提供可協助您設定這些原則的檔連結。

|原則群組|Location|描述|
|--------|------|--------|
|**密碼原則**|本機電腦原則配置 \Windows \Windows \ 原則|密碼原則會影響密碼的特性和行為。 密碼原則會用於網域帳戶或本機使用者帳戶。 它們會決定密碼的設定，例如強制和存留期。<p>如需特定設定的詳細資訊，請參閱[密碼原則](/windows/security/threat-protection/security-policy-settings/password-policy)。|
|**帳戶鎖定原則**|本機電腦原則配置 \Windows \Windows \ 原則|帳戶鎖定原則選項會在設定的登入嘗試失敗次數後停用帳戶。 使用這些選項可協助您偵測及封鎖中斷密碼的嘗試。<p>如需帳戶鎖定原則選項的詳細資訊，請參閱[帳戶鎖定原則](/windows/security/threat-protection/security-policy-settings/account-lockout-policy)。|
|**Kerberos 原則**|本機電腦原則配置 \Windows \Windows \ 原則|Kerberos 相關的設定包括票證存留期和強制執行規則。 Kerberos 原則不適用於本機帳戶資料庫，因為 Kerberos 驗證通訊協定不是用來驗證本機帳戶。 因此，Kerberos 原則設定只能透過預設網域群組原則物件 (GPO) 的方式來設定，而這會影響網域登入。<p>如需網域控制站之 Kerberos 原則選項的詳細資訊，請參閱[Kerberos 原則](/windows/security/threat-protection/security-policy-settings/kerberos-policy)。|
|**稽核原則**|本機電腦原則配置 \Windows \ 本機審核原則|稽核原則可讓您控制和瞭解物件（例如檔案和資料夾）的存取權，以及管理使用者和群組帳戶以及使用者登入和登出。 稽核原則可以指定您要審核的事件類別、設定安全性記錄檔的大小和行為，以及判斷您想要監視存取的物件，以及您要監視的存取類型。<p>|
|**使用者權限指派**|本機電腦原則的配置 \Windows 設置 \ 本機原則 \ 許可權指派|使用者權限通常是根據使用者所屬的安全性群組來指派，例如系統管理員、Power Users 或使用者。 此類別中的原則設定通常用來根據存取和安全性群組成員資格的方法，授與或拒絕存取電腦的許可權。|
|**安全性選項**|本機電腦原則配置 \Windows \ 原則 \ 安全性選項|與驗證相關的原則包括：<p>-裝置<br />-網域控制站<br />-網域成員<br />-互動式登入<br />-Microsoft network server<br />-網路存取<br />-網路安全性<br />-修復主控台<br />-關機<p>|
|**認證委派**|電腦建構管理的 Templates\System\Credentials 委派|認證的委派是一種機制，可讓您在其他系統上使用本機認證，尤其是網域內的成員伺服器和網域控制站。 這些設定會套用至使用認證安全性支援提供者 (憑證 SSP) 的應用程式。 遠端桌面連線是一個範例。|
|**KDC**|電腦管理的 Templates\System\KDC|這些原則設定會影響金鑰發佈中心 (KDC) （這是網域控制站上的服務）處理 Kerberos 驗證要求的方式。|
|**Kerberos**|電腦管理的 Templates\System\Kerberos|這些原則設定會影響 Kerberos 的設定方式，以處理對宣告、Kerberos 防護、複合驗證、識別 proxy 伺服器及其他設定的支援。|
|**登入**|電腦設定\系統管理範本\系統\登入|這些原則設定可控制系統如何呈現使用者的登入體驗。|
|**Net Logon**|電腦建構管理的 Templates\System\Net 登入|這些原則設定可控制系統處理網路登入要求的方式，包括網域控制站定位器的行為。<p>如需網域控制站定位程式如何融入複寫程式的詳細資訊，請參閱[瞭解網站間](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771251(v=ws.11))的複寫。|
|**生物特徵辨識**|電腦管理範本 \Windows Components\Biometrics|這些原則設定通常會允許或拒絕使用生物識別做為驗證方法。<p>如需有關生物識別的 Windows 實施的詳細資訊，請參閱 Windows 生物特徵辨識架構總覽。|
|**認證使用者介面**|電腦管理元件範本 Components\Credential 使用者介面|這些原則設定可控制在進入點時如何管理認證。|
|**密碼同步**|電腦管理元件範本 Components\Password 同步處理|這些原則設定會決定系統如何管理 Windows 與 UNIX 作業系統之間的密碼同步處理。<p>如需詳細資訊，請參閱[密碼同步](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732609(v=ws.11))處理。|
|**智慧卡**|電腦管理元件範本 Components\Smart 卡|這些原則設定會控制系統管理智慧卡登入的方式。<p>|
|**Windows 登入選項**|電腦建構管理範本 \windows 登入選項|這些原則設定可控制登入機會的時機與方式。|
|**Ctrl + Alt + Del 選項**|電腦管理元件範本 Components\Ctrl + Alt + Del 選項|這些原則設定會影響登入 UI 上功能的外觀和可存取性 (安全桌面) ，例如「工作管理員」和電腦的鍵盤鎖定。|
|**登入**|電腦管理範本 \Windows Components\Logon|這些原則設定會決定使用者登入時是否可以執行哪些處理常式。|

## <a name="additional-references"></a>其他參考資料
[Windows 驗證技術概觀](windows-authentication-technical-overview.md)