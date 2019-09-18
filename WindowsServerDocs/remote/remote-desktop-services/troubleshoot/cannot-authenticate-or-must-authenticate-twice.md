---
title: 使用者無法驗證，或必須驗證兩次
description: 針對啟動遠端桌面連線時，使用者無法驗證或必須驗證兩次的問題進行疑難排解。
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: ''
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 8af2b1a171a77def2bbb74cc7301e0562cb5b92c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870603"
---
# <a name="user-cant-authenticate-or-must-authenticate-twice"></a>使用者無法驗證，或必須驗證兩次

本文將說明數個影響使用者驗證的問題。

## <a name="access-denied-restricted-type-of-logon"></a>拒絕存取，限制的登入類型

在此情況下，嘗試連線到 Windows 10 或 Windows Server 2016 電腦的 Windows 10 使用者存取遭拒，並出現下列訊息：

> 遠端桌面連線：  
> 系統管理員已限制您可以使用的登入類型 (網路或互動式)。 如需協助，請連絡系統管理員或技術支援。

當網路層級驗證 (NLA) 為 RDP 連線所需，且使用者不是**遠端桌面使用者**群組的成員，就會發生這個問題。 如果**遠端桌面使用者**群組未指派給**從網路存取此電腦**使用者權限，也會發生這個問題。

若要解決此問題，請執行下列其中一項作業：

  - [修改使用者的群組成員資格或使用者權限指派](#modify-the-users-group-membership-or-user-rights-assignment)。
  - 關閉 NLA (不建議)。
  - 使用 Windows 10 以外的遠端桌面用戶端。 例如，Windows 7 用戶端並沒有此問題。

### <a name="modify-the-users-group-membership-or-user-rights-assignment"></a>修改使用者的群組成員資格或使用者權限指派

如果此問題影響的是單一使用者，則此問題最簡單的解決方案是將使用者新增至**遠端桌面使用者**群組。

如果使用者已經是該群組的成員 (或如果多個群組成員有相同問題)，請檢查遠端 Windows 10 或 Windows Server 2016 電腦上的使用者權限設定。

1. 開啟群組原則物件編輯器 (GPE)，並連線到遠端電腦的本機原則。
2. 巡覽至**電腦設定\\Windows 設定\\安全性設定\\本機原則\\使用者權限指派**，以滑鼠右鍵按一下 [從網路存取此電腦]  ，然後選取 [內容]  。
3. 針對**遠端桌面使用者** (或父群組) 檢查使用者和群組的清單。
4. 如果清單未包含 [**遠端桌面使用者**] 或 [**所有人**] 之類的父群組，您就必須將其新增至清單。 如果您的部署中有一部以上的電腦，請使用群組原則物件。  
    例如，**從網路存取此電腦**的預設成員資格包含**所有人**。 如果您的部署使用群組原則物件來移除**所有人**，您可能需要更新群組原則物件來新增**遠端桌面使用者**，以便還原存取。

## <a name="access-denied-a-remote-call-to-the-sam-database-has-been-denied"></a>拒絕存取，SAM 資料庫的遠端呼叫已遭到拒絕

如果您的網域控制站執行 Windows Server 2016 或更新版本，且使用者嘗試使用自訂連線應用程式來連線，則很有可能發生此行為。 特別是，在 Active Directory 中存取使用者設定檔資訊的應用程式將被拒絕存取。

此行為起因於 Windows 的變更。 在 Windows Server 2012 R2 及較舊版本中，當使用者登入遠端桌面時，遠端連線管理員 (RCM) 會連絡網域控制站 (DC) 來查詢 Active Directory Domain Services (AD DS) 中使用者物件上遠端桌面的特定設定。 這項資訊會顯示在 [Active Directory 使用者和電腦] MMC 嵌入式管理單元中使用者物件屬性的 [遠端桌面服務設定檔] 索引標籤。

從 Windows Server 2016 開始，RCM 不會再查詢 AD DS 中的使用者物件。 如果因為您正在使用遠端桌面服務屬性，而需要 RCM 查詢 AD DS，則您必須手動啟用查詢。

> [!IMPORTANT]  
> 請仔細依循本節中的步驟。 如果您未正確修改登錄，可能會發生嚴重問題。 在修改之前，[備份登錄以供還原](https://support.microsoft.com/help/322756)，以免發生問題。

若要啟用 RD 工作階段主機伺服器上的舊版 RCM 行為，請設定下列登錄項目，然後重新啟動**遠端桌面服務**服務：  
  - **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Policies\\Microsoft\\Windows NT\\Terminal Services**
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<Winstation name\>\\**  
      - 名稱：**fQueryUserConfigFromDC**
      - 輸入：**Reg\_DWORD**
      - 值：**1** (十進位)

若要在 RD 工作階段主機伺服器以外的伺服器上啟用舊版 RCM 行為，請設定這些登錄項目以及下列額外登錄項目 (然後重新啟動服務)：
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**

有關此行為的詳細資訊，請參閱 KB 3200967 [在 Windows Server 中變更遠端連線管理員](https://support.microsoft.com/help/3200967/changes-to-remote-connection-manager-in-windows-server)。

## <a name="user-cant-sign-in-using-a-smart-card"></a>使用者無法使用智慧卡登入

本節將說明使用者無法使用智慧卡登入遠端桌面的三種常見案例。

### <a name="cant-sign-in-with-a-smart-card-in-a-branch-office-with-a-read-only-domain-controller-rodc"></a>無法使用智慧卡登入採用唯讀網域控制站 (RODC) 的分公司

此問題會發生在包含 RDSH 伺服器的部署中，該伺服器位於使用 RODC 的分公司網站。 RDSH 伺服器裝載在根網域中。 在分公司網站的使用者屬於子網域，且使用智慧卡進行驗證。 RODC 已設定快取使用者密碼 (RODC 屬於**允許的 RODC 密碼複寫群組**)。 當使用者嘗試登入 RDSH 伺服器上的工作階段，使用者會收到訊息如「登入嘗試無效。 因為使用者名稱或驗證資訊不正確。」

此問題起因於根 DC 和 RDOC 管理使用者認證加密的方式。 根 DC 會使用加密金鑰來加密認證，而 RODC 會提供解密金鑰給用戶端。 如果使用者收到「無效」錯誤，這表示兩個金鑰不相符。

若要解決此問題，請嘗試下列其中一個方式：

- 藉由關閉 RODC 上的密碼快取，或將可寫入的 DC 部署至分支網站，以變更您的 DC 拓撲。
- 將 RDSH 伺服器移至與使用者相同的子網域。
- 讓使用者不需透過智慧卡登入。

請注意，所有這些解決方案都勢必會影響效能或安全性層級。

### <a name="user-cant-sign-in-to-a-windows-server-2008-sp2-computer-using-a-smart-card"></a>使用者無法使用智慧卡登入 Windows Server 2008 SP2 電腦

當使用者登入透過 KB4093227 (2018.4B) 更新的 Windows Server 2008 SP2 電腦，就會發生此問題。 當使用者嘗試使用智慧卡登入，使用者存取遭拒且收到訊息如「找不到有效憑證。 請確認已正確插入智慧卡，且卡片緊密吻合插槽。」 在此同時，Windows Server 電腦會記錄應用程式事件「從插入的智慧卡擷取數位憑證時發生錯誤。 簽章無效。」

若要解決此問題，請使用 KB 4093227 的 2018.06 B 重新發行版本更新 Windows Server 電腦 ([在 Windows Server 2008 中 Windows 遠端桌面通訊協定 (RDP) 阻斷服務弱點的安全性更新說明：2018 年 4 月 10 日](https://support.microsoft.com/help/4093227/security-update-for-vulnerabilities-in-windows-server-2008))。

### <a name="cant-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs"></a>無法使用智慧卡保持登入，且「遠端桌面服務」服務停止回應

當使用者登入透過 KB 4056446 更新的 Windows 或 Windows Server 電腦，就會發生此問題。 一開始，使用者或許可以使用智慧卡登入系統，但接著會收到「SCARD\_E\_NO\_SERVICE」錯誤訊息。 遠端電腦可能沒有回應。

若要解決此問題，請重新啟動遠端電腦。

若要解決此問題，請使用適當的修正來更新遠端電腦：

  - Windows Server 2008 SP2：KB 4090928，[Windows 流失 lsm.exe 處理程序中的控點，且智慧卡應用程式可能會顯示「SCARD\_E\_NO\_SERVICE」錯誤](https://support.microsoft.com/help/4090928/scard-e-no-service-errors-when-windows-leaks-handles-in-the-lsm-exe)
  - Windows Server 2012 R2：KB 4103724，[2018 年 5 月 17 日—KB4103724 (每月彙總套件預覽)](https://support.microsoft.com/help/4103724/windows-81-update-kb4103724)
  - Windows Server 2016 與 Windows 10 版本 1607：KB 4103720，[2018 年 5 月 17 日—KB4103720 (作業系統組建 14393.2273)](https://support.microsoft.com/help/4103720/windows-10-update-kb4103720)

## <a name="if-the-remote-pc-is-locked-the-user-needs-to-enter-a-password-twice"></a>如果遠端電腦已鎖定，使用者必須輸入密碼兩次

當使用者在 RDP 連線不需要 NLA 的部署中嘗試連線到執行 Windows 10 版本 1709 的遠端桌面，就可能會發生此問題。 在這些條件下，如果遠端桌面已鎖定，使用者連線時必須輸入認證兩次。

若要解決此問題，請使用 KB 4343893，[2018 年 8 月 30 日—KB4343893 (作業系統組建 16299.637)](https://support.microsoft.com/help/4343893/windows-10-update-kb4343893) 來更新 Windows 10 版本 1709 電腦。

## <a name="user-cant-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages"></a>使用者無法登入且收到「驗證錯誤」和「CredSSP 加密預示修復」訊息

使用者使用任一版本的 Windows (從 Windows Vista SP2 和更新版本或 Windows Server 2008 SP2 和更新版本) 嘗試登入時，使用者存取遭拒且收到如下訊息：

```
An authentication error has occurred. The function requested is not supported.
...
This could be due to CredSSP encryption oracle remediation
...
```

「CredSSP 加密預示修復」是在 2018 年 3 月、4 月和 5 月發行的一組安全性更新。 CredSSP 是處理其他應用程式驗證要求的驗證提供者。 2018 年 3 月 13 日「3B」和後續更新處理了以下惡意探索問題，攻擊者可以轉送使用者認證以在目標系統上執行程式碼。

初始更新已為新的群組原則物件「加密預示修復」新增支援，其具有下列可能的設定：

  - 易受攻擊：使用 CredSSP 的用戶端應用程式可能回復為不安全的版本，但這種行為會使遠端桌面暴露於攻擊風險中。 使用 CredSSP 的服務會接受尚未更新的用戶端。
  - 風險緩解：使用 CredSSP 的用戶端應用程式無法回復為不安全的版本，但使用 CredSSP 的服務會接受尚未更新的用戶端。
  - 強制更新的用戶端：使用 CredSSP 的用戶端應用程式無法回復為不安全的版本，且使用 CredSSP 的服務將不接受未修補的用戶端。 
    > [!NOTE]
    > 這項設定不應進行部署，直到所有遠端主機都支援最新版本為止。

2018 年 5 月 8 日的更新將「加密預示修復」預設設定從「易受攻擊」變更為「風險緩解」。 套用此項變更之後，已更新的遠端桌面用戶端無法連線到未更新的伺服器 (或已更新但尚未重新啟動的伺服器)。 如需有關 CredSSP 更新的詳細資訊，請參閱 [KB 4093492](https://support.microsoft.com/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018)。

若要解決此問題，請更新所有系統並重新啟動。 如需更新的完整清單和弱點的詳細資訊，請參閱 [CVE-2018-0886 | CredSSP 遠端程式碼執行弱點](https://portal.msrc.microsoft.com/security-guidance/advisory/CVE-2018-0886)。

若要在更新完成之前處理此問題，請查看 KB 4093492 以取得允許的連線類型。 如果沒有可行的替代方案，您可以考慮下列方法之一：

- 針對受影響的用戶端電腦，將「加密預示修復」原則設回**易受攻擊**。
- 在**電腦設定\\系統管理範本\\Windows 元件\\遠端桌面服務\\遠端桌面工作階段主機\\安全性**群組原則資料夾中修改下列原則：  
  - **需要對遠端 (RDP) 連線使用特定的安全層**：設為 [已啟用]  並選取 [RDP]  。
  - **需要使用網路層級驗證對遠端連線進行使用者驗證**：設為 [已停用]  。
    > [!IMPORTANT]  
    > 變更這些群組原則會降低部署的安全性。 若有需要，我們建議您只是暫時地使用這些原則。

如需使用群組原則的詳細資訊，請參閱[修改封鎖 GPO](rdp-error-general-troubleshooting.md#modifying-a-blocking-gpo)。

## <a name="after-you-update-client-computers-some-users-need-to-sign-in-twice"></a>更新用戶端電腦之後，部分使用者需要登入兩次

當使用者使用執行 Windows 7 或 Windows 10 (版本 1709) 的電腦登入遠端桌面時，他們立即看到第二次的登入提示。 如果用戶端電腦具有下列更新，就會發生此問題：

  - Windows 7：KB 4103718，[2018 年 5 月 8 日—KB4103718 (每月彙總套件)](https://support.microsoft.com/help/4103718/windows-7-update-kb4103718)
  - Windows 10 1709：KB 4103727，[2018 年 5 月 8 日—KB4103727 (作業系統組建 16299.431)](https://support.microsoft.com/help/4103727/windows-10-update-kb4103727)

若要解決此問題，請確定使用者要連線到的電腦 (以及 RDSH 或 RDVI 伺服器) 已在 2018 年 6 月完整更新。 這包括下列更新︰

  - Windows Server 2016：KB 4284880，[2018 年 6 月 12 日—KB4284880 (作業系統組建 14393.2312)](https://support.microsoft.com/help/4284880/windows-10-update-kb4284880)
  - Windows Server 2012 R2：KB 4284815，[2018 年 6 月 12 日—KB4284815 (每月彙總套件)](https://support.microsoft.com/help/4284815/windows-81-update-kb4284815)
  - Windows Server 2012：KB 4284855，[2018 年 6 月 12 日—KB4284855 (每月彙總套件)](https://support.microsoft.com/help/4284855/windows-server-2012-update-kb4284855)
  - Windows Server 2008 R2：KB 4284826，[2018 年 6 月 12 日—KB4284826 (每月彙總套件)](https://support.microsoft.com/help/4284826/windows-7-update-kb4284826)
  - Windows Server 2008 SP2：KB4056564，[在 Windows Server 2008、Windows Embedded POSReady 2009 和 Windows Embedded Standard 2009 中 CredSSP 遠端程式碼執行弱點的安全性更新說明：2018 年 3 月 13 日](https://support.microsoft.com/help/4056564/security-update-for-vulnerabilities-in-windows-server-2008)

## <a name="users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers"></a>使用者在某部署上存取遭拒，該部署使用具多個 RD 連線代理人的遠端認證防護

如果系統正在使用 Windows Defender 遠端認證防護，此問題會發生在使用兩個或多個遠端桌面連線代理人的高可用性部署中。 使用者無法登入遠端桌面。

因為遠端認證防護使用 Kerberos 進行驗證，並限制 NTLM，因而發生此問題。 不過，在具負載平衡的高可用性設定中，RD 連線代理人無法支援 Kerberos 作業。

如果您需使用具負載平衡 RD 連線代理人的高可用性設定，您可以藉由停用遠端認證防護處理此問題。 有關如何管理 Windows Defender 遠端認證防護的詳細資訊，請參閱[使用 Windows Defender 遠端認證防護來保護遠端桌面認證](https://docs.microsoft.com/windows/security/identity-protection/remote-credential-guard#enable-windows-defender-remote-credential-guard)。
