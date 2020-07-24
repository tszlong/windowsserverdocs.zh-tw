---
ms.assetid: cb834273-828a-4141-9387-37dd8270e932
title: Winlogon 自動重新開機登入（ARSO）
description: Windows 自動重新開機登入如何讓您的使用者更具生產力。
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.reviewer: cahick
ms.date: 08/20/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 3ad6658c504cc90eedef2c1cb6688c6f12233b3c
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86959870"
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>Winlogon 自動重新開機登入（ARSO）

在 Windows Update 期間，必須進行使用者特定進程，才能完成更新。 這些進程需要使用者登入其裝置。 在起始更新後的第一次登入時，使用者必須等到這些使用者特定的進程完成後，才能開始使用其裝置。

## <a name="how-does-it-work"></a>如何運作？

當 Windows Update 起始自動重新開機時，ARSO 會將目前登入之使用者的衍生認證解壓縮，並將其保存在磁片中，並為使用者設定自動登入。 Windows Update 以 TCB 許可權執行為系統時，將會起始 RPC 呼叫來進行此動作。

在最後 Windows Update 重新開機之後，使用者會自動透過自動登入機制登入，而使用者的會話會以保存的秘密解除凍結。 此外，裝置會被鎖定，以保護使用者的會話。 鎖定會透過 Winlogon 起始，而認證管理則是由本地安全機構（LSA）來完成。 成功 ARSO 設定和登入時，會立即從磁片刪除已儲存的認證。

藉由在主控台上自動登入和鎖定使用者，Windows Update 可以在使用者回到裝置之前，先完成使用者特定的處理常式。 如此一來，使用者就可以立即開始使用其裝置。

ARSO 會以不同方式來處理非受控和受管理的裝置。 若為未受管理的裝置，則會使用裝置加密，但不需要使用者取得 ARSO。 針對受管理的裝置，ARSO 設定需要 TPM 2.0、SecureBoot 和 BitLocker。 IT 系統管理員可以透過群組原則覆寫這項需求。 受管理裝置的 ARSO 目前僅適用于已加入 Azure Active Directory 的裝置。

|   | Windows Update| shutdown-g-t 0  | 使用者起始的重新開機 | 具有 SHUTDOWN_ARSO/EWX_ARSO 旗標的 Api |
| --- | :---: | :---: | :---: | :---: |
| 受控裝置 | :heavy_check_mark:  | :heavy_check_mark: |   | :heavy_check_mark: |
| 未受管理的裝置 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |

> [!NOTE]
> 在 Windows Update 引發重新開機之後，最後一個互動式使用者會自動登入，而且會話會被鎖定。 這讓使用者的鎖定畫面應用程式即使在 Windows Update 重新開機，仍可執行。

![設定頁面](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/gtr-adds-lockscreenapp.png)

## <a name="policy-1"></a>原則 #1

### <a name="sign-in-and-lock-last-interactive-user-automatically-after-a-restart"></a>重新開機後自動登入並鎖定上一個互動式使用者

在 Windows 10 中，已停用伺服器 Sku 的 ARSO，並退出宣告用戶端 Sku。

**群組原則位置：** 電腦設定 > 系統管理範本 > windows 元件 > Windows 登入選項

**Intune 原則：**

- 平台：Windows 10 及以上版本
- 配置檔案類型：系統管理範本
- 路徑： \Windows \Windows 登入選項

**支援于：** 至少 Windows 10 版本1903

**描述:**

此原則設定可控制裝置是否會在系統重新開機之後，或關閉後冷開機後，自動登入和鎖定最後一個互動式使用者。

只有在上次互動式使用者未在重新開機或關機之前登出時，才會發生這種情況。

如果裝置已加入 Active Directory 或 Azure Active Directory，則此原則僅適用于 Windows Update 重新開機。 否則，這會同時適用于 Windows Update 重新開機和使用者起始的重新開機和關機。

如果您未設定此原則設定，則預設會啟用。 當原則啟用時，使用者會自動登入，而且會話會自動鎖定，並會在裝置開機之後，使用為該使用者設定的所有鎖定畫面應用程式來進行。

啟用此原則之後，您可以透過 ConfigAutomaticRestartSignOn 原則設定其設定，這會設定在重新開機或冷開機後自動登入和鎖定最後一個互動式使用者的模式。

如果您停用此原則設定，則裝置不會設定自動登入。 在系統重新開機之後，使用者的鎖定畫面應用程式不會重新開機。

**登錄編輯程式：**

| 值名稱 | 類型 | 資料 |
| --- | --- | --- |
| DisableAutomaticRestartSignOn | DWORD | 0（啟用 ARSO） |
|   |   | 1（停用 ARSO） |

**原則登錄位置：** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**類型：** 值

![winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/gtr-adds-signinpolicy.png)

## <a name="policy-2"></a>原則 #2

### <a name="configure-the-mode-of-automatically-signing-in-and-locking-last-interactive-user-after-a-restart-or-cold-boot"></a>設定在重新開機或冷開機之後自動登入和鎖定上一個互動式使用者的模式

**群組原則位置：** 電腦設定 > 系統管理範本 > windows 元件 > Windows 登入選項

**Intune 原則：**

- 平台：Windows 10 及以上版本
- 配置檔案類型：系統管理範本
- 路徑： \Windows \Windows 登入選項

**支援于：** 至少 Windows 10 版本1903

**描述:**

此原則設定可控制在重新開機或冷開機之後，自動重新開機和登入和鎖定發生的設定。 如果您在「重新開機後自動登入和鎖定最後一個互動式使用者」原則中選擇 [停用]，則不會進行自動登入，而且不需要設定此原則。

如果您啟用此原則設定，您可以選擇下列兩個選項的其中一個：

1. 「如果 BitLocker 已開啟且未暫止」，指定只有在 BitLocker 處於作用中狀態且在重新開機或關機期間未暫停時，才會自動登入和鎖定。 如果 BitLocker 在更新期間未開啟或暫止，則可以在裝置的硬碟上存取個人資料。 BitLocker 擱置會暫時移除系統元件和資料的保護，但在某些情況下可能會需要成功更新開機關鍵元件。
   - 在下列情況中，BitLocker 會在更新期間暫停：
      - 裝置沒有 TPM 2.0 和 PCR7，或
      - 裝置不會使用僅限 TPM 的保護裝置
2. 「永遠啟用」指定即使在重新開機或關閉期間 BitLocker 已關閉或暫停，仍會進行自動登入。 若未啟用 BitLocker，就可以在硬碟上存取個人資料。 只有在您確信設定的裝置位於安全的實體位置時，自動重新開機和登入才應該在此條件下執行。

如果您停用或未設定此設定，自動登入將會預設為「如果 BitLocker 已開啟且未擱置，則會啟用」行為。

**登錄編輯程式**

| 值名稱 | 類型 | 資料 |
| --- | --- | --- |
| AutomaticRestartSignOnConfig | DWORD | 0（啟用 ARSO （如果安全）） |
|   |   | 1（啟用一律為 ARSO） |

**原則登錄位置：** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**類型：** 值

![winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/arso-policy-setting.png)

## <a name="troubleshooting"></a>疑難排解

當 WinLogon 自動鎖定時，WinLogon 的狀態追蹤將會儲存在 WinLogon 事件記錄檔中。

記錄自動登入設定嘗試的狀態

- 如果成功
   - 將其記錄為
- 如果失敗：
   - 記錄失敗的內容
- 當 BitLocker 的狀態變更時：
   - 將會記錄移除認證
   - 這些會儲存在 LSA 操作記錄中。

### <a name="reasons-why-autologon-might-fail"></a>自動登登可能失敗的原因

在幾種情況下，使用者無法自動登入。  本節的目的是要用來捕捉可能發生這種情況的已知案例。

### <a name="user-must-change-password-at-next-login"></a>使用者必須在下次登入時變更密碼

使用者登入可以在下次登入時需要密碼變更時進入封鎖狀態。  在大部分情況下，您可以在重新開機之前偵測到此情況，但不是全部（例如，在關機和下一次登入時都可以達到密碼到期。

### <a name="user-account-disabled"></a>使用者帳戶已停用

即使停用了現有的使用者會話，也可以加以維護。  在大部分情況下，可在本機偵測到已停用帳戶的重新開機，視 gp 而定，網域帳戶可能不適用（某些網域快取的登入案例即使在 DC 上已停用帳戶也能正常執行）。

### <a name="logon-hours-and-parental-controls"></a>登入時數和家長監護

[登入時數] 和 [家長監護] 可以禁止建立新的使用者會話。  如果在此視窗期間發生重新開機，則不允許使用者登入。  還有其他原則會導致鎖定或登出做為合規性動作。 系統會記錄自動登入設定嘗試的狀態。

## <a name="security-details"></a>安全性詳細資料

### <a name="credentials-stored"></a>儲存的認證

|   | 密碼雜湊 | 認證金鑰 | 票證授權票證 | 主要重新整理權杖 |
| --- | :---: | :---: | :---: | :---: |
| 本機帳戶 | :heavy_check_mark: | :heavy_check_mark: |   |   |
| MSA 帳戶 | :heavy_check_mark: | :heavy_check_mark: |   |   |
| Azure AD 加入的帳戶 | :heavy_check_mark: | :heavy_check_mark: | ： heavy_check_mark：（如果是混合式） | :heavy_check_mark: |
| 已加入網域的帳戶 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | ： heavy_check_mark：（如果是混合式） |

### <a name="credential-guard-interaction"></a>Credential Guard 互動

如果裝置已啟用 Credential Guard，則會使用目前開機會話特定的金鑰來加密使用者的衍生密碼。 因此，在啟用 Credential Guard 的裝置上目前不支援 ARSO。

## <a name="additional-resources"></a>其他資源

自動登入功能是已在 Windows 中針對數個版本而呈現的功能。 這是 Windows 的已記載功能，甚至有 Windows [HTTP：/technet. .com/sysinternals/bb963905](/sysinternals/downloads/autologon)的自動登出等工具。 它允許裝置的單一使用者自動登入，而不需要輸入認證。 認證會以加密的 LSA 秘密設定並儲存在登錄中。 這可能會對許多子案例造成問題，其中的帳戶鎖定可能會在床時間和喚醒之間發生，特別是在這段期間通常是在維護期間。
