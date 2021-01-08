---
title: 系統管理工具和登入類型參考 - Windows Server
description: 識別與不同系統管理工具相關聯之認證暴露的風險
ms.topic: reference
ms.date: 12/15/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: 9b3457cf489e11ce7ede4e0965ec8555102754ec
ms.sourcegitcommit: 4bbb284c909b91cc02b5988b28056f976bc2ca6a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/18/2020
ms.locfileid: "97676074"
---
# <a name="administrative-tools-and-logon-types"></a>系統管理工具和登入類型

此為提供的參考資訊，可協助識別與使用不同系統管理工具進行遠端管理相關聯的認證暴露風險。

在遠端系統管理案例中，認證一律會公開於來源電腦上，因此，一律建議針對機密或高度影響的帳戶使用值得信任的特殊權限存取工作站 (PAW)。 是否會向目標 (遠端) 電腦上潛在的竊取動作公開認證，主要取決於連線方法所使用的 Windows 登入類型。

下表包含最常見系統管理工具和連線方法的指引：

|連線方式|登入類型|目的地上可重複使用的認證|評價|
|-----------|-------|--------------------|------|
|在主控台登入|Interactive (互動式)|v|包含硬體遠端存取 / 熄燈介面卡和網路 KVM。|
|RUNAS|Interactive (互動式)|v||
|RUNAS /NETWORK|NewCredentials|v|複製目前的 LSA 工作階段以進行本機存取，但連線到網路資源時，請使用新的認證。|
|遠端桌面 (成功)|RemoteInteractive|v|如果已將遠端桌面用戶端設為共用本機裝置和資源，則這些也可能會遭到破解。|
|遠端桌面 (失敗 - 登入類型已遭拒)|RemoteInteractive|-|根據預設，如果 RDP 登入失敗，則只會短暫儲存認證。 若電腦已遭破解，可能就不是這樣了。|
|Net use * \\\SERVER|網路|-||
|Net use * \\\SERVER /u:user|網路|-||
|MMC 嵌入式管理單元到遠端電腦|網路|-|範例：電腦管理、事件檢視器、裝置管理員、服務|
|PowerShell WinRM|網路|-|範例：Enter-PSSession 伺服器|
|PowerShell WinRM with CredSSP|NetworkClearText|v|New-PSSession 伺服器<br />-Authentication Credssp<br />-Credential cred|
|PsExec，而不需明確的認證|網路|-|範例：PsExec \\\server cmd|
|PsExec，以及明確的認證|網路 + 互動式|v|PsExec \\\server -u user -p pwd cmd<br />建立多個登入工作階段。|
|遠端登錄|網路|-||
|遠端桌面閘道|網路|-|向遠端桌面閘道進行驗證。|
|排定的工作|Batch|v|密碼也會當成 LSA 密碼儲存於磁碟上。|
|以服務形式執行工具|Service|v|密碼也會當成 LSA 密碼儲存於磁碟上。|
|弱點掃描器|網路|-|大部分的掃描器預設都會使用網路登入，但有些廠商可能會實作非網路登入，因而引進了更多認證遭竊的風險。|

針對 Web 驗證，使用下表中的參考：

|連線方式|登入類型|目的地上可重複使用的認證|評價|
|-----------|-------|--------------------|------|
|IIS「基本驗證」|NetworkCleartext<br />(IIS 6.0+)<p>Interactive (互動式)<br />(IIS 6.0 之前)|v||
|IIS「整合式 Windows 驗證」|網路|-|NTLM 和 Kerberos 提供者。|

欄位定義：

- **登入類型** - 可識別連線所起始的登入類型。
- **目的地上可重複使用的認證** - 指出下列認證類型將儲存於本機登入指定帳戶之目的地電腦上的 LSASS 程序中：
   - LM 與 NT 雜湊
   - Kerberos TGT
   - 純文字密碼 (如果適用)。

此表格中的符號定義如下：

- (-) 表示不會公開認證時。
- (+) 表示會公開認證時。

對於不在此表格中的管理應用程式，您可以從稽核登入事件的登入類型欄位判斷登入類型。 如需詳細資訊，請參閱[稽核登入事件](/previous-versions/windows/it-pro/windows-server-2003/cc787567(v=ws.10))。

在 Windows 架構的電腦中，所有的驗證都會當成數個登入類型之一來處理，而不論所使用的驗證通訊協定或驗證器為何。 此表格包含最常見的登入類型及其相對於認證竊取的屬性：

|登入類型|#|接受的驗證器|LSA 工作階段中可重複使用的認證|範例|
|-------|---|--------------|--------------------|------|
|互動式 (也稱為本機登入)|2|密碼、智慧卡、<br />其他|是|主控台登入；<br />RUNAS；<br />硬體遠端控制解決方案 (例如，網路 KVM 或遠端存取 / 伺服器中的熄燈介面卡)<br />IIS 基本驗證 (IIS 6.0 之前)|
|網路|3|密碼、<br />NT 雜湊、<br />Kerberos 票證|否 (但如果已啟用委派，則會有 Kerberos 票證存在)|NET USE；<br />RPC 呼叫；<br />遠端登錄；<br />IIS 整合式 Windows 驗證；<br />SQL Windows 驗證；|
|Batch|4|密碼 (儲存為 LSA 密碼)|是|排定的工作|
|Service|5|密碼 (儲存為 LSA 密碼)|是|Windows 服務|
|NetworkCleartext|8|密碼|是|IIS 基本驗證 (IIS 6.0 及更新版本)；<br />含 CredSSP 的 Windows PowerShell|
|NewCredentials|9|密碼|是|RUNAS /NETWORK|
|RemoteInteractive|10|密碼、智慧卡、<br />其他|是|遠端桌面 (先前稱為「終端機服務」)|

欄位定義：

- **登入類型** - 是所要求的登入類型。
- **#** - 是登入類型的數值識別碼，其報告於安全性事件記錄檔的稽核事件中。
- **接受的驗證器** - 表示哪些類型的驗證器能夠起始此類型的登入。
- **LSA 工作階段中可重複使用的認證** - 指出登入類型是否會產生 LSA 工作階段來保存認證，例如，純文字密碼、NT 雜湊或 Kerberos 票證，其可用來向其他網站資源進行驗證。
- **範例** - 列出使用該登入類型的常見案例清單。

> [!NOTE]
> 如需登入類型的詳細資訊，請參閱 [SECURITY_LOGON_TYPE 列舉](/windows/win32/api/ntsecapi/ne-ntsecapi-security_logon_type)。

**後續步驟**

[AD DS 設計與規劃](../ad-ds/plan/AD-DS-Design-and-Planning.md)
