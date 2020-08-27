---
ms.assetid: cb834273-828a-4141-9387-37dd8270e932
title: 'Winlogon 自動重新開機登入 (ARSO) '
description: Windows 自動重新開機登入如何讓您的使用者更具生產力。
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.reviewer: cahick
ms.date: 08/20/2019
ms.topic: article
ms.openlocfilehash: 3f2957d2290934505f67edbcb8a49733452939e2
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88939868"
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>Winlogon 自動重新開機登入 (ARSO) 

在 Windows Update 期間，需要有使用者特定的進程才能完成更新。 這些進程需要使用者登入其裝置。 在啟動更新後第一次登入時，使用者必須等到這些使用者特定的進程完成之後，才能開始使用其裝置。

## <a name="how-does-it-work"></a>如何運作？

當 Windows Update 起始自動重新開機時，ARSO 會將目前登入的使用者衍生認證解壓縮，並將其保存到磁片，並設定使用者的自動登入。 以 TCB 許可權執行為系統的 Windows Update 將會起始 RPC 呼叫來執行此動作。

最後 Windows Update 重新開機之後，使用者將會透過自動登入機制自動登入，而使用者的會話會以保存的密碼解除凍結。 此外，裝置會被鎖定，以保護使用者的會話。 鎖定將會透過 Winlogon 起始，而「本地安全機構」 (LSA) 來管理認證。 ARSO 設定和登入成功後，儲存的認證會立即從磁片中刪除。

藉由自動登入並鎖定主控台上的使用者，Windows Update 可以在使用者回到裝置之前完成使用者特定的處理常式。 如此一來，使用者就可以立即開始使用其裝置。

ARSO 會以不同的方式來處理非受控與受控裝置。 若是未受管理的裝置，則會使用裝置加密，但使用者不需要取得 ARSO。 針對受管理的裝置，需要 TPM 2.0、SecureBoot 和 BitLocker 才能進行 ARSO 設定。 IT 系統管理員可以透過群組原則覆寫這項需求。 受管理裝置的 ARSO 目前僅適用于已加入 Azure Active Directory 的裝置。

| Windows Update | 關機-g-t 0 | 使用者起始的重新開機 | 具有 SHUTDOWN_ARSO/EWX_ARSO 旗標的 Api |
|--|--|--|--|
| 受管理的裝置-是<p>非受控裝置-是 | 受管理的裝置-是<p>非受控裝置-是 | 受管理的裝置-否<p>非受控裝置-是 | 受管理的裝置-是<p>非受控裝置-是 |

> [!NOTE]
> 在 Windows Update 引發重新開機之後，最後一個互動式使用者會自動登入，並鎖定會話。 這可讓使用者的鎖定畫面應用程式即使在 Windows Update 重新開機的情況下仍能繼續執行。

![設定頁面](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/gtr-adds-lockscreenapp.png)

## <a name="policy-1"></a>原則 #1

### <a name="sign-in-and-lock-last-interactive-user-automatically-after-a-restart"></a>重新開機後自動登入和鎖定上次互動使用者

在 Windows 10 中，伺服器 Sku 的 ARSO 已停用，並退出宣告用戶端 Sku。

**群組原則位置：** 電腦設定 > 系統管理範本 > windows 元件 > Windows 登入選項

**Intune 原則：**

- 平台：Windows 10 及更新版本
- 配置檔案類型：系統管理範本
- 路徑： \Windows \Windows 登入選項

**支援：** 至少 Windows 10 版本1903

**描述：**

此原則設定可控制裝置是否會在系統重新開機之後，或在關機和冷開機之後自動登入並鎖定最後一個互動式使用者。

只有在上次互動式使用者未在重新開機或關機之前登出時，才會發生這種情況。

如果裝置已加入 Active Directory 或 Azure Active Directory，此原則只適用于 Windows Update 重新開機。 否則，這會同時適用于 Windows Update 重新開機和使用者起始的重新開機和關機。

如果您未設定此原則設定，預設會啟用它。 啟用此原則時，使用者會自動登入，並會在裝置開機之後，自動鎖定會話，並針對該使用者設定所有鎖定畫面應用程式。

啟用此原則之後，您可以透過 ConfigAutomaticRestartSignOn 原則設定其設定，這會設定在重新開機或冷開機之後自動登入和鎖定最後一個互動式使用者的模式。

如果您停用此原則設定，則裝置不會設定自動登入。 系統重新開機之後，不會重新開機使用者的鎖定畫面應用程式。

**登錄編輯程式：**

| 值名稱 | 類型 | 資料 |
| --- | --- | --- |
| DisableAutomaticRestartSignOn | DWORD | 0 (啟用 ARSO)  |
|   |   | 1 (停用 ARSO)  |

**原則登錄位置：** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**輸入：** Dword

![winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/gtr-adds-signinpolicy.png)

## <a name="policy-2"></a>原則 #2

### <a name="configure-the-mode-of-automatically-signing-in-and-locking-last-interactive-user-after-a-restart-or-cold-boot"></a>設定在重新開機或冷開機之後自動登入和鎖定最後一個互動式使用者的模式

**群組原則位置：** 電腦設定 > 系統管理範本 > windows 元件 > Windows 登入選項

**Intune 原則：**

- 平台：Windows 10 及更新版本
- 配置檔案類型：系統管理範本
- 路徑： \Windows \Windows 登入選項

**支援：** 至少 Windows 10 版本1903

**描述：**

此原則設定會控制在重新開機或冷開機之後，自動重新開機和登入和鎖定所使用的設定。 如果您在「重新開機後自動登入並鎖定上次互動使用者」原則中選擇「停用」，則不會進行自動登入，也不需要設定此原則。

如果您啟用此原則設定，您可以選擇下列兩個選項的其中一個：

1. 「如果 BitLocker 處於開啟狀態且未暫止，則會指定只有當 BitLocker 處於作用中且未在重新開機或關機期間暫停時，才會進行自動登入和鎖定。 如果在更新期間未開啟或暫停 BitLocker，就可以在裝置的硬碟上存取個人資料。 BitLocker 暫止會暫時移除系統元件和資料的保護，但在某些情況下可能需要用到成功更新開機關鍵元件。
   - 下列情況會在更新期間暫停 BitLocker：
      - 裝置沒有 TPM 2.0 和 PCR7，或
      - 裝置不會使用僅限 TPM 的保護裝置
2. 「永遠啟用」指定即使在重新開機或關機期間關閉或暫停 BitLocker，也會發生自動登入。 未啟用 BitLocker 時，可以在硬碟上存取個人資料。 如果您確信設定的裝置位於安全的實體位置，則應該只在此情況下執行自動重新開機和登入。

如果您停用或未設定此設定，自動登入將會預設為「如果 BitLocker 已開啟且未暫停」行為，則會預設為「已啟用」。

**登錄編輯程式**

| 值名稱 | 類型 | 資料 |
| --- | --- | --- |
| AutomaticRestartSignOnConfig | DWORD | 0 (如果安全) ，請啟用 ARSO |
|   |   | 1 (啟用 ARSO 一律)  |

**原則登錄位置：** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**輸入：** Dword

![winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/arso-policy-setting.png)

## <a name="troubleshooting"></a>疑難排解

當 WinLogon 自動鎖定時，WinLogon 的狀態追蹤將會儲存在 WinLogon 事件記錄檔中。

系統會記錄自動登入設定嘗試的狀態

- 如果成功
   - 將其記錄為這類
- 如果失敗：
   - 記錄失敗的原因
- 當 BitLocker 的狀態變更時：
   - 將會記錄移除認證
   - 這些會儲存在 LSA 操作記錄檔中。

### <a name="reasons-why-autologon-might-fail"></a>自動登入可能會失敗的原因

在幾種情況下，無法達到使用者自動登入。  本節的目的是要捕捉可能發生這種情況的已知案例。

### <a name="user-must-change-password-at-next-login"></a>使用者必須在下次登入時變更密碼

當需要在下次登入時進行密碼變更時，使用者登入可能會進入封鎖狀態。  在大部分情況下都可以偵測到這種情況，但並非所有 (例如，在關機和下一次登入時都可以達到密碼到期。

### <a name="user-account-disabled"></a>使用者帳戶已停用

即使已停用，也可以維護現有的使用者會話。  您可以在大部分情況下，事先在大部分的情況下于本機偵測已停用帳戶的重新開機，這可能不是網域帳戶 (某些網域快取的登入案例仍可運作，即使 DC) 上已停用帳戶也是如此。

### <a name="logon-hours-and-parental-controls"></a>登入時間和家長監護

登入時間和家長監護可以防止建立新的使用者會話。  如果在此視窗期間重新開機，則不允許使用者登入。  有其他原則會導致鎖定或登出為合規性動作。 系統會記錄自動登入設定嘗試的狀態。

## <a name="security-details"></a>安全性詳細資料

### <a name="credentials-stored"></a>儲存的認證

| 密碼雜湊 | 認證金鑰 | 票證授權票證 | 主要重新整理權杖 |
|--|--|--|--|
| 本機帳戶-是 | 本機帳戶-是 | 本機帳戶-否 | 本機帳戶-否 |
| MSA 帳戶-是 | MSA 帳戶-是 | MSA 帳戶-否 | MSA 帳戶-否 |
| Azure AD 加入的帳戶-是 | Azure AD 加入的帳戶-是 | Azure AD 加入的帳戶-是 (（如果混合式) ） | Azure AD 加入的帳戶-是 |
| 已加入網域的帳戶-是 | 已加入網域的帳戶-是 | 已加入網域的帳戶-是 | 加入網域的帳戶-是 (如果混合式)  |

### <a name="credential-guard-interaction"></a>Credential Guard 互動

如果裝置已啟用 Credential Guard，則會使用目前開機會話特有的金鑰來加密使用者的衍生秘密。 因此，在啟用 Credential Guard 的裝置上，目前不支援 ARSO。

## <a name="additional-resources"></a>其他資源

自動登入是 Windows 中已有數個版本的功能。 它是 Windows 的記載功能，甚至是 Windows [HTTP：/technet. sysinternals/bb963905](/sysinternals/downloads/autologon)的自動登入等工具。 它可讓裝置的單一使用者自動登入，而不需要輸入認證。 認證會以加密的 LSA 秘密的形式設定和儲存在登錄中。 這可能會對許多子系的情況造成問題，也就是在床時間與喚醒時可能發生帳戶鎖定的情況，特別是在這段期間內的維護期間很普遍。
