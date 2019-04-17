---
title: "Windows 驗證中使用群組原則設定"
description: "Windows Server 安全性"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="group-policy-settings-used-in-windows-authentication"></a>Windows 驗證中使用群組原則設定

>適用於：Windows Server（以每年次管道）、Windows Server 2016

這適用於 IT 專業人員的參考主題描述使用和驗證程序群組原則設定的影響。

您可以管理 Windows 作業系統中的驗證使用者、 電腦及服務帳號加入群組，然後將驗證原則套用到群組。 這些原則定義本機安全性原則和系統管理範本，也就是群組原則設定。 這兩個組可以設定，並散發您的組織使用群組原則。

> [!NOTE]
> 在 Windows Server 2012 R2 推出的功能可讓您設定驗證原則的目標服務或帳號保護的應用程式通常稱為 [驗證筒倉，使用。 為執行此動作 Active Directory 中相關資訊，請查看[設定保護帳號如何](how-to-configure-protected-accounts.md)。

例如，您可以套用下列原則給群組，根據其組織中的功能：

-   在本機或網域登入

-   在網路上登入

-   重設帳號

-   建立帳號

下表列出相關驗證原則群組，並提供連結到文件可以協助您設定的原則。

|群組原則|位置|描述|
|--------|------|--------|
|**密碼原則**|本機電腦電腦原則 \ \windows 安全性設定帳戶原則|密碼原則影響的特性和密碼的行為。 網域帳號或本機帳號所使用的密碼的原則。 它們判斷設定密碼，例如執法和期間。<br /><br />適用於特定的設定有關的資訊，請查看[密碼原則](https://technet.microsoft.com/itpro/windows/keep-secure/password-policy)。|
|**Account 鎖定原則**|本機電腦電腦原則 \ \windows 安全性設定帳戶原則|Account 鎖定原則選項來停用設定的嘗試登入失敗的次數後帳號。 使用這些選項，可協助您偵測及封鎖破壞密碼。<br /><br />適用於 account 鎖定原則選項的相關資訊，請查看[Account 鎖定原則](https://technet.microsoft.com/itpro/windows/keep-secure/account-lockout-policy)。|
|**Kerberos 原則**|本機電腦電腦原則 \ \windows 安全性設定帳戶原則|Kerberos 相關設定包括票證期間與執法規則。 Kerberos 原則不適用於本機 account 資料庫，因為 Kerberos 驗證通訊協定不用來驗證本機帳號。 因此，您可以設定 Kerberos 原則設定只能使用群組原則物件 (GPO)，它會影響網域登入預設的網域。<br /><br />為網域控制站 Kerberos 原則選項的相關資訊，請查看[Kerberos 原則](https://technet.microsoft.com/itpro/windows/keep-secure/kerberos-policy)。|
|**稽核原則**|本機電腦電腦原則 \ \windows 安全性設定本機稽核原則|稽核原則可讓您控制，並了解存取物件，例如的檔案和資料夾，和管理使用者和群組帳號使用者登入及登出。 稽核原則可以指定類事件您要稽核、 的大小和行為安全性登入設定，並判斷您要的物件的監視存取，而您想要監視類型的存取權限。<br /><br />|
|**使用者權限指派**|本機電腦電腦原則 \ \windows 安全性設定本機原則權限指派|根據安全性所屬的群組的使用者，例如系統管理員、 進階的使用者或使用者通常指派使用者權利。 在這個分類的原則設定通常用於授與或拒絕根據存取和安全性群組成員資格的方法是電腦的存取權限。|
|**安全性選項**|本機電腦電腦原則 \ \windows 安全性設定本機原則安全性選項|相關驗證原則包括：<br /><br />-裝置<br />網域控制站<br />網域成員<br />互動式登入<br />Microsoft 網路伺服器<br />網路存取權<br />網路安全性<br />修復主機<br />關機<br /><br />|
|**認證委派**|電腦設定 Templates\System\Credentials 委派|認證委派是機制，可讓本機認證尤其是最成員伺服器以及網域中的網域控制站其他系統上使用。 這些設定適用於應用程式使用 Credential 安全性支援提供者 (Cred SSP)。 遠端桌面連接是一個範例。|
|**\ [KDC**|電腦設定 Templates\System\KDC|這些原則設定影響金鑰 Distribution 中心 (KDC)，其為網域控制站的服務，會如何處理 Kerberos 驗證要求。|
|**Kerberos**|電腦設定 Templates\System\Kerberos|這些原則設定影響 Kerberos 處理支援宣告、 Kerberos 保護 \、 複合驗證、 辨識 proxy 伺服器，以及其他設定的設定方式。|
|**登入**|電腦設定登入|這些原則設定來控制為何系統顯示登入的使用者體驗。|
|**網路登入**|電腦設定 Templates\System\Net 登入|這些原則設定來控制系統會如何處理網路登入要求包括網域控制站定位器的運作方式。<br /><br />如需有關如何網域控制站定位器納入複寫處理程序，請查看[之間網站來了解複製](https://technet.microsoft.com/library/cc771251.aspx)。|
|**生物**|電腦設定 \ 系統管理 Components\Biometrics|這些原則設定通常允許或拒絕生物使用的驗證方法。<br /><br />適用於 Windows 的生物實作的相關資訊，查看 Windows 生物特徵辨識 Framework 概觀。|
|**Credential 使用者介面**|電腦設定 \ 系統管理 Components\Credential 使用者介面|這些原則設定來控制認證如何管理在項目。|
|**密碼同步**|電腦設定 \ 系統管理 Components\Password 同步處理|這些原則設定判斷系統管理的密碼，Windows 與 unix 作業系統之間同步處理的方式。<br /><br />如需詳細資訊，請查看[密碼同步](https://technet.microsoft.com/library/cc732609.aspx)。|
|**智慧卡**|電腦設定 \ 系統管理 Components\Smart 卡|這些原則設定來控制系統管理智慧卡登入方式。<br /><br />|
|**Windows 登入選項**|電腦設定 \ 系統管理替登入選項|這些原則設定來控制時，如何登入機會可供使用。|
|**Ctrl + Alt + Del 選項**|電腦設定 \ 系統管理 Components\Ctrl + Alt + Del 選項|這些原則設定影響的外觀及功能 UI （安全桌面版） 登入，例如工作管理員和鍵盤鎖定電腦的協助工具功能。|
|**登入**|電腦設定 \ 系統管理 Components\Logon|這些原則設定判斷或使用者登入時可以執行哪些處理程序。|

## <a name="see-also"></a>也了
[Windows 驗證技術概觀](windows-authentication-technical-overview.md)


