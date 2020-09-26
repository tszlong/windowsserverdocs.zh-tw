---
title: schtasks 變更
description: 「Schtasks 變更」命令的參考文章，可排程要定期執行或在特定時間執行的命令和程式、加入和移除排程中的工作、啟動和停止隨選工作，以及顯示和變更排定的工作。
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 09/16/2020
ms.openlocfilehash: 06cbceaaa0e428743a725127d9495759dd0097e1
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91390720"
---
# <a name="schtasks-change"></a>schtasks 變更

變更工作的下列一或多個屬性：

- 工作執行的程式 (**/tr**) 

- 用來執行工作的使用者帳戶 (的 **/ru**) 

- 使用者帳戶的密碼 (**/rp**) 

- 將僅限互動式屬性新增至工作 (**/it**) 

## <a name="required-permissions"></a>所需的權限

- 若要排程、查看及變更本機電腦上的所有工作，您必須是 Administrators 群組的成員。

- 若要排程、查看及變更遠端電腦上的所有工作，您必須是遠端電腦上 Administrators 群組的成員，或者您必須使用 **/u** 參數提供遠端電腦的系統管理員認證。

- 如果本機和遠端電腦位於相同網域中，或本機電腦位於遠端電腦網域信任的網域中，您可以在 **/create**或 **/change**操作中使用 **/u**參數。 否則，遠端電腦無法驗證指定的使用者帳戶，也無法確認該帳戶是否為系統管理員群組的成員。

- 您打算執行的工作必須具有適當的許可權;這些許可權因工作而異。 根據預設，工作會以本機電腦目前使用者的許可權執行，或以 **/u** 參數所指定之使用者的許可權執行（如果包含的話）。 o 執行具有不同使用者帳戶許可權或系統許可權的工作，請使用 **/ru** 參數。

## <a name="syntax"></a>語法

```
schtasks /change /tn <Taskname> [/s <computer> [/u [<domain>\]<user> [/p <password>]]] [/ru <username>] [/rp <password>] [/tr <Taskrun>] [/st <Starttime>] [/ri <interval>] [{/et <Endtime> | /du <duration>} [/k]] [/sd <Startdate>] [/ed <Enddate>] [/{ENABLE | DISABLE}] [/it] [/z]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| /tn `<Taskname>` | 識別要變更的工作。 輸入工作名稱。 |
| /s `<computer>` | 指定遠端電腦的名稱或 IP 位址 (包含或不含反斜線) 。 預設是本機電腦。 |
| u `[<domain>]` | 使用指定之使用者帳戶的許可權來執行此命令。 根據預設，命令會以本機電腦目前使用者的許可權執行。 指定的使用者帳戶必須是遠端電腦上 Administrators 群組的成員。 只有當您使用 **/s**時， **/u**和 **/p**參數才有效。 |
| /p `<password>` | 指定 **/u** 參數中指定之使用者帳戶的密碼。 如果您在沒有 **/p**參數或 password 引數的情況下使用 **/u**參數，則 schtasks.exe 會提示您輸入密碼。 只有當您使用 **/s**時， **/u**和 **/p**參數才有效。 |
| /ru `<username>` | 變更排程工作必須執行的使用者名稱。 系統帳戶的有效值為 *""*、 *"NT AUTHORITY\SYSTEM"* 或 *"system"*。 |
| /rp `<password>` | 為現有的使用者帳戶或由 **/ru** 參數指定的使用者帳戶指定新密碼。 使用本機系統帳戶時，會忽略這個參數。 |
| /tr `<Taskrun>` | 變更工作執行的程式。 輸入可執行檔、指令檔或批次檔的完整路徑和檔案名。 如果您未加入路徑， **schtasks** 會假設檔案位於 `<systemroot>\System32` 目錄中。 指定的程式會取代工作所執行的原始程式。 |
| /st `<Starttime>` | 使用24小時制時間格式（HH： mm），指定工作的開始時間。 例如，14:30 的值相當於下午2:30 的12小時時間。 |
| /ri `<interval>` | 指定排程工作的重複間隔（以分鐘為單位）。 有效範圍是 1-599940 (599940 分鐘 = 9999 小時) 。 如果指定了 **/et** 或 **/du** 參數，預設值為 **10 分鐘**。 |
| /et `<Endtime>` | 使用24小時制時間格式（HH： mm）來指定工作的結束時間。 例如，14:30 的值相當於下午2:30 的12小時時間。 |
| /du `<duration>` | 值，指定執行工作的持續時間。 時間格式為 HH： mm (24 小時的時間) 。 例如，14:30 的值相當於下午2:30 的12小時時間。 |
| /k | 停止此工作在 **/et** 或 **/du**指定的時間執行的程式。 如果沒有 **/k**，在它到達 **/et** 或 **/du** 所指定的時間之後，仍不會重新開機程式，也不會在程式仍在執行時停止程式。 此參數是選擇性的，且僅適用于分鐘或小時的排程。 |
| /sd `<Startdate>` | 指定應執行工作的第一個日期。 日期格式為 MM/DD/YYYY。 |
| /ed `<Enddate>` | 指定執行工作的最後日期。 格式為 MM/DD/YYYY。 |
| /ENABLE | 指定要啟用排定的工作。 |
| /DISABLE | 指定要停用排程工作。 |
| /it | 指定只有當 [執行身分] 使用者 (執行工作的使用者帳戶) 登入電腦時，才執行排定的工作。 此參數不會影響以系統許可權執行的工作，或是已設定僅限互動式屬性的工作。 您無法使用變更命令從工作中移除僅限互動式的屬性。 依預設，[以使用者身份執行] 是排程工作時的目前本機電腦使用者，或是由 **/u** 參數指定的帳號（如果使用的話）。 但是，如果命令包含 **/ru** 參數，則 run as 使用者就是 **/ru** 參數所指定的帳號。 |
| /z | 指定在完成排程時刪除工作。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- **/Tn**和 **/s**參數會識別工作。 **/Tr**、 **/ru**和 **/rp**參數指定您可以變更的工作屬性。

- **/Ru**和 **/rp**參數指定工作執行時所用的許可權。 **/U**和 **/p**參數會指定用來變更工作的許可權。

- 若要變更遠端電腦上的工作，使用者必須使用遠端電腦上 Administrators 群組成員的帳戶登入本機電腦。

- 若要使用不同使用者的許可權來執行 **/change** 命令 (**/u**， **/p**) ，本機電腦必須位於與遠端電腦相同的網域中，或者必須位於遠端電腦網域信任的網域中。

- 系統帳戶沒有互動式登入許可權。 使用者不會看到並無法與互動，程式會以系統許可權執行。
若要使用 **/it** 屬性來識別工作，請使用詳細資訊查詢 (**/query/v**) 。 在使用 **/it**工作的詳細資訊查詢顯示中，[登入模式] 欄位的值為 [僅限互動]。

## <a name="examples"></a>範例

若要變更病毒檢查工作從 *VirusCheck.exe* 執行至 *VirusCheck2.exe*的程式，請輸入：

```
schtasks /change /tn Virus Check /tr C:\VirusCheck2.exe
```

此命令會使用 **/tn** 參數來識別工作，並使用 **/tr** 參數來指定工作的新程式。  (您無法變更工作名稱。 ) 

若要變更遠端電腦上 *RemindMe* 工作的使用者帳戶密碼 *>woodgrove-svr01*，請輸入：

```
schtasks /change /tn RemindMe /s Svr01 /rp p@ssWord3
```

每當使用者帳戶的密碼過期或變更時，就需要此程式。 如果儲存在工作中的密碼不再有效，則工作不會執行。 此命令會使用 **/tn** 參數來識別工作，並使用 **/s** 參數來指定遠端電腦。 它會使用 **/rp** 參數指定新的密碼 *p@ssWord3* 。

若要變更 ChkNews 工作（此工作會在每早上早上9:00 開始 Notepad.exe，以改為開始 Internet Explorer，請輸入：

```
schtasks /change /tn ChkNews /tr c:\program files\Internet Explorer\iexplore.exe /ru DomainX\Admin01
```

此命令會使用 **/tn** 參數來識別工作。 它會使用 **/tr** 參數來變更工作執行的程式，以及使用/ru 參數來變更工作執行時所使用的使用者帳戶。 不使用可提供使用者帳戶密碼的 **/ru** 和 **/rp** 參數。 您必須提供帳戶的密碼，但您可以使用 **/ru** 和 **/rp** 參數並以純文字輸入密碼，或等候 SchTasks.exe 提示您輸入密碼，然後在 [遮蔽文字] 中輸入密碼。

若要變更 SecurityScript 工作，使其以 System 帳戶的許可權執行，請輸入：

```
schtasks /change /tn SecurityScript /ru
```

此命令會使用 **/ru** 參數來表示系統帳戶。 由於以系統帳戶許可權執行的工作不需要密碼，因此 SchTasks.exe 不會提示您輸入密碼。

若要將僅限互動式屬性新增至 MyApp （現有的工作），請輸入：

```
schtasks /change /tn MyApp /it
```

這個屬性可確保只有當執行身分使用者（也就是工作執行所在的使用者帳戶）登入電腦時，才會執行此工作。 此命令會使用 **/tn** 參數來識別工作，並使用 **/it** 參數將僅限互動式屬性新增至工作。 因為工作已以我的使用者帳戶的許可權執行，所以您不需要變更工作的 **/ru** 參數。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [schtasks create 命令](schtasks-create.md)

- [schtasks delete 命令](schtasks-delete.md)

- [schtasks end 命令](schtasks-end.md)

- [schtasks 查詢命令](schtasks-query.md)

- [schtasks run 命令](schtasks-run.md)
