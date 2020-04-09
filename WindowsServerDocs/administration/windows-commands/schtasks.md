---
title: schtasks
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2e713203-3dd8-491b-b9e1-9423618dc7e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d4c28072a8e4d01ea3a045314796bcda32c8a59
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835241"
---
# <a name="schtasks"></a>schtasks



排定在特定時間定期執行的命令和程式。 加入和移除排程中的工作、視需要啟動和停止工作，以及顯示和變更排程工作。

若要查看命令語法，請按一下下列其中一個命令：
-   [schtasks 建立](#BKMK_create)
-   [schtasks 變更](#BKMK_change)
-   [schtasks 執行](#BKMK_run)
-   [schtasks 結束](#BKMK_end)
-   [schtasks 刪除](#BKMK_delete)
-   [schtasks 查詢](#BKMK_query)

## <a name="remarks"></a>備註

- **Schtasks.exe**執行的作業與 [**控制台**] 中的 [**排程**工作] 相同。 您可以將這些工具一起和交替使用。
- **Schtasks**取代了舊版 Windows 中包含的工具 **。** 雖然**在 .exe**仍包含在 Windows Server 2003 系列中，但**schtasks**是建議的命令列工作排程工具。
- **Schtasks**命令中的參數可以依任何順序出現。 輸入**schtasks**而不使用任何參數，會執行查詢。
- **Schtasks**的許可權  
  -   您必須具有執行命令的許可權。 任何使用者都可以在本機電腦上排程工作，也可以查看和變更排程的工作。 Administrators 群組的成員可以排程、查看和變更本機電腦上的所有工作。
  -   若要在遠端電腦上排程、查看或變更工作，您必須是遠端電腦上 Administrators 群組的成員，或者必須使用 **/u**參數來提供遠端電腦之系統管理員的認證。
  -   只有當本機和遠端電腦位於相同網域，或本機電腦位於遠端電腦網域所信任的網域中時，您才可以在 **/create**或 **/change**作業中使用 **/u**參數。 否則，遠端電腦就無法驗證指定的使用者帳戶，也無法確認帳戶是否為系統管理員群組的成員。
  -   工作必須具有執行的許可權。 所需的許可權會因工作而異。 根據預設，工作會以本機電腦目前使用者的許可權執行，或使用 **/u**參數所指定之使用者的許可權（如果包含的話）。 若要執行具有不同使用者帳戶或具有系統許可權之許可權的工作，請使用 **/ru**參數。
- 若要確認排定的工作是否已執行，或找出排程工作未執行的原因，請參閱工作排程器服務交易記錄檔、 *SystemRoot*\SchedLgU.txt。 此記錄會記錄所有使用服務的工具所起始的執行，包括**排程**的工作和**schtasks.exe**。
- 在罕見的情況下，工作檔案會變成損毀。 損毀的工作不會執行。 當您嘗試對損毀的工作執行作業時， **schtasks.exe**會顯示下列錯誤訊息：  
  ```
  ERROR: The data is invalid.
  ```  
  您無法復原損毀的工作。 若要還原系統的工作排程功能，請使用**schtasks.exe**或**排程**的工作來刪除系統中的工作，然後重新排定它們。

## <a name="schtasks-create"></a><a name=BKMK_create></a>schtasks 建立

排定工作。

**Schtasks**會針對每個排程類型使用不同的參數組合。 若要查看用來建立工作的結合語法，或查看用來建立具有特定排程類型之工作的語法，請按一下下列其中一個選項。
-   [結合的語法和參數描述](#BKMK_syntax)
-   [若要排程每隔 N 分鐘執行一次的工作](#BKMK_minutes)
-   [若要排程每隔 N 小時執行一次的工作](#BKMK_hours)
-   [若要排程每隔 N 天執行一次的工作](#BKMK_days)
-   [若要排程每隔 N 周執行一次的工作](#BKMK_weeks)
-   [若要排程每隔 N 個月執行一次的工作](#BKMK_months)
-   [若要排程在一周的特定一天執行的工作](#BKMK_spec_day)
-   [若要排程在當月特定周執行的工作](#BKMK_spec_week)
-   [若要排程每個月在特定日期執行的工作](#BKMK_spec_date)
-   [若要排程在當月最後一天執行的工作](#BKMK_last_day)
-   [排程執行一次的工作](#BKMK_once)
-   [若要排程每次系統啟動時執行的工作](#BKMK_startup)
-   [若要排程在使用者登入時執行的工作](#BKMK_logon)
-   [若要排程在系統閒置時執行的工作](#BKMK_idle)
-   [排定立即執行的工作](#BKMK_now)
-   [若要排程以不同許可權執行的工作](#BKMK_diff_perms)
-   [若要排程以系統許可權執行的工作](#BKMK_sys_perms)
-   [若要排程執行多個程式的工作](#BKMK_multi_progs)
-   [若要排程在遠端電腦上執行的工作](#BKMK_remote)

### <a name="combined-syntax-and-parameter-descriptions"></a><a name=BKMK_syntax></a>結合的語法和參數描述

#### <a name="syntax"></a>語法

```
schtasks /create /sc <ScheduleType> /tn <TaskName> /tr <TaskRun> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]] [/ru {[<Domain>\]<User> | System}] [/rp <Password>] [/mo <Modifier>] [/d <Day>[,<Day>...] | *] [/m <Month>[,<Month>...]] [/i <IdleTime>] [/st <StartTime>] [/ri <Interval>] [{/et <EndTime> | /du <Duration>} [/k]] [/sd <StartDate>] [/ed <EndDate>] [/it] [/z] [/f]
```

##### <a name="parameters"></a>參數

##### <a name="sc-scheduletype"></a>/sc \<ScheduleType >

指定排程類型。 有效值為 MINUTE、每小時、每天、每週、每月、一次、ONSTART、整理、ONIDLE。

|排程類型|描述|
|-------------|-----------|
|分鐘、每小時、每天、每週、每月|指定排程的時間單位。|
|即可|工作會在指定的日期和時間執行一次。|
|ONSTART|此工作會在每次系統啟動時執行。 您可以指定開始日期，或在下一次系統啟動時執行工作。|
|整理|每當使用者（任何使用者）登入時，就會執行此工作。 您可以指定日期，或在使用者下一次登入時執行工作。|
|ONIDLE|此工作會在系統閒置一段指定的時間時執行。 您可以指定日期，或在下一次系統閒置時執行工作。|

##### <a name="tn-taskname"></a>\<TaskName > 的/tn

指定工作的名稱。 系統上的每個工作都必須有唯一的名稱。 名稱必須符合檔案名的規則，而且不得超過238個字元。 使用引號括住包含空格的名稱。

##### <a name="tr-taskrun"></a>\</Tr u n > 的/tr

指定工作執行的程式或命令。 輸入可執行檔、指令檔或批次檔的完整路徑和檔案名。 路徑名稱不能超過262個字元。 如果您省略路徑，則**schtasks**會假設檔案位於*SystemRoot*\System32 目錄中。

##### <a name="s-computer"></a>/s \<電腦 >

在指定的遠端電腦上排定工作。 輸入遠端電腦的名稱或 IP 位址（包含或不含反斜線）。 預設是本機電腦。 只有當您使用 **/s**時， **/u**和 **/p**參數才有效。

##### <a name="u-domainuser"></a>/u [\<Domain >\]<User>

以指定之使用者帳戶的許可權執行此命令。 預設值是本機電腦目前使用者的許可權。 **/U**和 **/p**參數僅適用于在遠端電腦上排程工作（ **/s**）。

指定之帳戶的許可權是用來排程工作和執行工作。 若要以不同使用者的許可權執行工作，請使用 **/ru**參數。

使用者帳戶必須是遠端電腦上 Administrators 群組的成員。 此外，本機電腦必須位於與遠端電腦相同的網域中，或必須位於遠端電腦網域所信任的網域中。

##### <a name="p-password"></a>/p \<密碼 >

提供 **/u**參數中指定之使用者帳戶的密碼。 如果您使用 **/u**參數，但省略 **/p**參數或 password 引數，則**schtasks**會提示您輸入密碼，並遮蔽您輸入的文字。

**/U**和 **/p**參數僅適用于在遠端電腦上排程工作（ **/s**）。

##### <a name="ru-domainuser--system"></a>/ru {[\<Domain >\]<User> |筆記本電腦

以指定之使用者帳戶的許可權執行工作。 根據預設，工作會以本機電腦目前使用者的許可權執行，或使用 **/u**參數所指定之使用者的許可權（如果包含的話）。 在本機或遠端電腦上排程工作時， **/ru**參數是有效的。


|       值        |                                                    描述                                                    |
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| [\<Domain >\]<User> |                                       指定替代的使用者帳戶。                                        |
|    系統或     | 指定本機系統帳戶，這是作業系統和系統服務所使用的高許可權帳戶。 |

##### <a name="rp-password"></a>/rp \<密碼 >

提供在 **/ru**參數中指定之使用者帳戶的密碼。 如果您在指定使用者帳戶時省略此參數，則**schtasks.exe**會提示您輸入密碼，並遮蔽您鍵入的文字。

請勿針對以系統帳號憑證（ **/Ru 系統**）執行的工作使用 **/rp**參數。 系統帳戶沒有密碼，而且**schtasks.exe**不會提示您輸入密碼。

##### <a name="mo-modifier"></a>/mo \<修飾詞 >

指定工作在其排程類型內執行的頻率。 此參數有效，但為選擇性，一分鐘、每小時、每天、每週和每月排程。 預設值為 1。

|排程類型|修飾詞值|描述|
|-------------|---------------|-----------|
|分鐘|1 - 1439|工作會每 \<N > 分鐘執行一次。|
|次|1 - 23|工作會每 \<N > 小時執行一次。|
|DAILY|1 - 365|工作會每 \<N > 天執行一次。|
|提交|1 - 52|工作會每 \<N > 周執行一次。|
|即可|沒有修飾詞。|此工作會執行一次。|
|ONSTART|沒有修飾詞。|工作會在啟動時執行。|
|整理|沒有修飾詞。|當 **/u**參數所指定的使用者登入時，就會執行此工作。|
|ONIDLE|沒有修飾詞。|此工作會在系統閒置了 **/i**參數所指定的分鐘數之後執行，這是與 ONIDLE 搭配使用的必要條件。|
|每月|1 - 12|工作會每 \<N > 個月執行一次。|
|每月|LASTDAY|工作會在當月的最後一天執行。|
|每月|第一、第二、第三、第四、最後|使用搭配 **/d**\<Day > 參數，在特定一周和每天執行工作。 例如，在當月的第三個星期三。|

##### <a name="d-dayday--"></a>/d Day [，Day ...] |*

指定一周中的一天（或幾天），或一個月的一天（或幾天）。 僅適用于每週或每月排程。


| 排程類型 |              Modifier              |     日期值（/d）      |                                                                                                 描述                                                                                                 |
|---------------|------------------------------------|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    提交     |               1 - 52               | 週一-SUN [，週一至周日 ...] |                                                                                                     \*                                                                                                      |
|    每月    | 第一、第二、第三、第四、最後 |        週一-周日         |                                                                                   特定周排程所需。                                                                                    |
|    每月    |          無或 {1-12}          |          1 - 31          | 選擇性且僅適用于沒有修飾詞（ **/mo**）參數（特定日期排程），或當 **/mo**為 1-12 時（每個 \<N > 個月排程）。 預設值為第1天（當月的第一天）。 |

##### <a name="m-monthmonth"></a>/m Month [，Month ...]

指定排程工作應在一年中執行的月份或月份。 有效值為 JAN-DEC 和 * （每月）。 **/M**參數只適用于每月排程。 使用 LASTDAY 修飾詞時，這是必要的。 否則，它是選擇性的，而且預設值是 * （每月）。

##### <a name="i-idletime"></a>/i \<IdleTime >

指定在工作開始之前，電腦閒置的分鐘數。 有效的值是從1到999的整數。 此參數只有在使用 ONIDLE 排程時才有效，而且是必要的。

##### <a name="st-starttime"></a>/st \<StartTime >

以 \<HH： MM > 24 小時格式指定工作一開始的時間（每次啟動時）。 預設值是本機電腦上的目前時間。 **/St**參數的有效時間為 MINUTE、每小時、每天、每週、每月和一次排程。 需要一次排程。

##### <a name="ri-interval"></a>/ri \<間隔 >

指定以分鐘為單位的重複間隔。 這不適用於排程類型： MINUTE、每小時、ONSTART、整理和 ONIDLE。 有效範圍為1到599940分鐘（599940分鐘 = 9999 小時）。 如果指定了/ET 或/DU，則重複間隔會預設為10分鐘。

##### <a name="et-endtime"></a>/et \<EndTime >

以 \<HH： MM > 24 小時格式，指定分鐘或每小時工作排程結束的時間。 在指定的結束時間之後， **schtasks**就不會再次啟動工作，直到開始時間重複為止。 根據預設，工作排程沒有結束時間。 這個參數是選擇性的，而且只有分鐘或每小時的排程才有效。

如需範例，請參閱：
-   若要排程在非上班時間內每隔100分鐘執行一次的工作，**以排程每**\<N >**分鐘** 區段執行的工作。

##### <a name="du-duration"></a>/du \<持續時間 >

以 \<HHHH HHHH： MM > 24 小時格式，指定分鐘或每小時排程的最大時間長度。 經過指定的時間之後， **schtasks**就不會再次啟動工作，直到開始時間重複為止。 根據預設，工作排程沒有最長的持續時間。 這個參數是選擇性的，而且只有分鐘或每小時的排程才有效。

如需範例，請參閱：
-   若要將每3小時執行一次的工作排程為，以**排程每隔**\<N >**小時**一節執行的工作。

##### <a name="k"></a>/k

停止在 **/et**或 **/du**指定的時間執行工作的程式。 如果沒有 **/k**，則**schtasks**在到達 **/et**或 **/du**所指定的時間之後，不會再啟動程式，但如果程式仍在執行中，則不會將它停止。 這個參數是選擇性的，而且只有分鐘或每小時的排程才有效。

如需範例，請參閱：
-   若要排程在非上班時間內每隔100分鐘執行一次的工作，**以排程每**\<N >**分鐘** 區段執行的工作。

##### <a name="sd-startdate"></a>/sd \<起始 >

指定工作排程開始的日期。 預設值是本機電腦上的目前日期。 **/Sd**參數對所有排程類型而言都是有效的，而且是選擇性的。

[開始*日期*] 的格式會因在 [**控制台**] 的 [**地區及語言選項**] 中為本機電腦選取的地區設定而異。 每個地區設定只能有一個有效的格式。

有效的日期格式會列在下表中。 使用與本機電腦 [**控制台**] 的 [**地區及語言選項**] 中為 [**簡短日期**] 選取的格式最相似的格式。


|       值       |                                        描述                                         |
|-------------------|--------------------------------------------------------------------------------------------|
| \<MM >/<DD>/<YYYY> | 用於月優先的格式，例如**英文（美國）** 和**西班牙文（巴拿馬）** 。 |
| \<DD >/<MM>/<YYYY> |       用於第一天的格式，例如**保加利亞**文和**荷蘭文（荷蘭）** 。        |
| \<YYYY >/<MM>/<DD> |          用於年份優先的格式，例如**瑞典**文和**法文（加拿大）** 。          |

/ed \<結束日期 >

指定排程結束的日期。 這個參數是選擇性的。 它在一次、ONSTART、整理或 ONIDLE 排程中無效。 根據預設，排程沒有結束日期。

*結束*日期的格式會因在 [**控制台**] 的 [**地區及語言選項**] 中為本機電腦選取的地區設定而異。 每個地區設定只能有一個有效的格式。

有效的日期格式會列在下表中。 使用與本機電腦 [**控制台**] 的 [**地區及語言選項**] 中為 [**簡短日期**] 選取的格式最相似的格式。


|       值       |                                        描述                                         |
|-------------------|--------------------------------------------------------------------------------------------|
| \<MM >/<DD>/<YYYY> | 用於月優先的格式，例如**英文（美國）** 和**西班牙文（巴拿馬）** 。 |
| \<DD >/<MM>/<YYYY> |       用於第一天的格式，例如**保加利亞**文和**荷蘭文（荷蘭）** 。        |
| \<YYYY >/<MM>/<DD> |          用於年份優先的格式，例如**瑞典**文和**法文（加拿大）** 。          |

##### <a name="it"></a>/it

指定只有在執行身分使用者（執行工作的使用者帳戶）登入電腦時，才會執行工作。 這個參數不會影響以系統許可權執行的工作。

根據預設，當工作已排程時，[執行身分] 使用者是本機電腦的目前使用者，如果使用的是 **/u**參數所指定的帳號，則為該使用者。 不過，如果命令包含 **/ru**參數，則「執行身分」使用者就是 **/ru**參數所指定的帳號。

如需範例，請參閱：
-   若要排定每隔70天執行一次的工作，如果我登入**以排程每** *N* **天**執行一次的工作。
-   只有在特定使用者登入時才執行工作，才能**排程以不同許可權執行的工作**一節。

##### <a name="z"></a>/z

指定在完成排程時刪除工作。

##### <a name="f"></a>/f

指定要建立工作，並在指定的工作已經存在時隱藏警告。

##### <a name=""></a>/?

在命令提示字元顯示說明。

### <a name="to-schedule-a-task-that-runs-every-n-minutes"></a><a name=BKMK_minutes></a>若要排程每隔 N 分鐘執行一次的工作

#### <a name="minute-schedule-syntax"></a>分鐘排程語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc minute [/mo {1 - 1439}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [{/et <HH:MM> | /du <HHHH:MM>} [/k]] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在一分鐘的時間內，需要 **/sc minute**參數。 **/Mo** （修飾詞）參數是選擇性的，而且會指定每次執行工作之間的分鐘數。 [ **/Mo** ] 的預設值為1（每分鐘）。 **/Et** （結束時間）和 **/du** （duration）參數是選擇性的，而且可以搭配或不搭配 **/k** （end task）參數使用。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-every-20-minutes"></a>若要排程每隔20分鐘執行一次的工作

下列命令會將安全性腳本（每秒）排程為每隔20分鐘執行一次。 此命令會使用 **/sc**參數來指定分鐘的排程，並使用 **/mo**參數來指定20分鐘的間隔。

因為此命令不包含開始日期或時間，所以工作會在命令完成後的20分鐘開始，每隔20分鐘執行一次系統。 請注意，安全性腳本來源檔案位於遠端電腦上，但是工作是在本機電腦上排程和執行。
```
schtasks /create /sc minute /mo 20 /tn Security Script /tr \\central\data\scripts\sec.vbs
```

#### <a name="to-schedule-a-task-that-runs-every-100-minutes-during-non-business-hours"></a>排定在非上班時間每100分鐘執行一次的工作

下列命令會排程每隔100分鐘在 5:00 P.M. 的本機電腦上執行的安全性腳本（每秒）。 和 7:59 A.M。 每天。 此命令會使用 **/sc**參數來指定分鐘排程，並使用 **/mo**參數來指定100分鐘的間隔。 它會使用 **/st**和 **/et**參數來指定每日排程的開始時間和結束時間。 如果腳本仍在上午7:59 執行，它也會使用 **/k**參數來停止它。 如果沒有 **/k**，則**schtasks**不會在上午7:59 之後啟動腳本，但如果實例在上午6:20 開始， 仍在執行中，不會將它停止。
```
schtasks /create /tn Security Script /tr sec.vbs /sc minute /mo 100 /st 17:00 /et 08:00 /k
```

### <a name="to-schedule-a-task-that-runs-every-n-hours"></a><a name=BKMK_hours></a>若要排程每隔 N 小時執行一次的工作

#### <a name="hourly-schedule-syntax"></a>每小時排程語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc hourly [/mo {1 - 23}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [{/et <HH:MM> | /du <HHHH:MM>} [/k]] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

依每小時的排程，需要 **/sc 每小時**參數。 **/Mo** （修飾詞）參數是選擇性的，而且會指定每次執行工作之間的時數。 [ **/Mo** ] 的預設值為1（每小時）。 **/K** （結束工作）參數是選擇性的，而且可以搭配任何一個 **/et** （在指定時間結束）或 **/du** （在指定的間隔之後結束）使用。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-every-five-hours"></a>若要排程每五個小時執行一次的工作

下列命令會排定 MyApp 程式每隔五個小時執行一次（2002年3月的第一天）。 它會使用 **/mo**參數來指定 interval 和 **/sd**參數，以指定開始日期。 因為此命令不會指定開始時間，所以會使用目前的時間作為開始時間。

因為本機電腦在 [**控制台**] 的 [**地區及語言選項**] 中設定為使用 [**英文（辛巴威）** ] 選項，所以開始日期的格式為 MM/DD/YYYY （03/01/2002）。
```
schtasks /create /sc hourly /mo 5 /sd 03/01/2002 /tn My App /tr c:\apps\myapp.exe
```

#### <a name="to-schedule-a-task-that-runs-every-hour-at-five-minutes-past-the-hour"></a>排定每小時在過去一小時內執行的工作

下列命令會將 MyApp 程式排程成每小時從午夜開始的五分鐘執行一次。 由於會省略 **/mo**參數，因此此命令會使用每小時排程的預設值，也就是每隔（1）小時。 如果此命令在早上12:05 之後執行，程式就不會執行到隔天的時間。
```
schtasks /create /sc hourly /st 00:05 /tn My App /tr c:\apps\myapp.exe
```

#### <a name="to-schedule-a-task-that-runs-every-3-hours-for-10-hours"></a>將每3小時執行一次的工作排程為10小時

下列命令會排定 MyApp 程式每隔3小時執行10小時。

此命令會使用 **/sc**參數來指定每小時排程，並使用 **/mo**參數來指定3小時的間隔。 它會使用 **/st**參數來啟動午夜的排程，而 **/du**參數則會在10小時後結束週期。 由於程式只會執行幾分鐘的時間，因此，如果程式在持續時間到期時仍在執行中，則不需要使用 **/k**參數來停止程式。
```
schtasks /create /tn My App /tr myapp.exe /sc hourly /mo 3 /st 00:00 /du 0010:00
```
在此範例中，工作會在上午12:00、3:00 A.M.、6:00 A.M. 和上午9:00 執行。 因為持續時間為10小時，所以工作不會在下午12:00 重新執行。 相反地，它會在上午12:00 重新開機。 下一天。

### <a name="to-schedule-a-task-that-runs-every-n-days"></a><a name=BKMK_days></a>若要排程每隔 N 天執行一次的工作

#### <a name="daily-schedule-syntax"></a>每日排程語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc daily [/mo {1 - 365}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在每日排程中，需要 **/sc 每日**參數。 **/Mo** （修飾詞）參數是選擇性的，而且會指定每次執行工作之間的天數。 [ **/Mo** ] 的預設值為1（每天）。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-every-day"></a>排程每天執行的工作

下列範例會排定 MyApp 程式每天執行一次，每日上午8:00 直到2002年12月31日為止。 因為它省略了 **/mo**參數，所以會使用預設間隔1來每天執行命令。

在此範例中，因為本機電腦系統是在 [**控制台**] 的 [**地區及語言選項**] 中設定為 [**英文（英國）** ] 選項，所以結束日期的格式為 DD/MM/YYYY （31/12/2002）
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc daily /st 08:00 /ed 31/12/2002
```

#### <a name="to-schedule-a-task-that-runs-every-12-days"></a>若要排程每12天執行一次的工作

下列範例會將 MyApp 程式排程在下午1:00 的每12天執行一次。 （13:00）自2002年12月31日起。 此命令會使用 **/mo**參數來指定兩個（2）天的間隔和 **/sd**和 **/st**參數，以指定日期和時間。

在此範例中，由於系統是在 [**控制台**] 的 [**地區及語言選項**] 中設定為 [**英文（辛巴威）** ] 選項，因此結束日期的格式為 MM/DD/YYYY （12/31/2002）
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc daily /mo 12 /sd 12/31/2002 /st 13:00
```

#### <a name="to-schedule-a-task-that-runs-every-70-days-if-i-am-logged-on"></a>排定每隔70天執行一次的工作（如果我登入的話）

下列命令會將安全性腳本（每秒）排程為每70天執行一次。 此命令會使用 **/mo**參數來指定70天的間隔。 此外，它也會使用 **/it**參數來指定工作執行時，只有在其帳戶執行工作的使用者登入電腦時才會執行。 因為工作將會以 [我的使用者帳戶] 的許可權執行，所以只有在我登入時才會執行工作。
```
schtasks /create /tn Security Script /tr sec.vbs /sc daily /mo 70 /it
```

> [!NOTE]
> 若要使用僅限互動式（ **/it**）屬性來識別工作，請使用 verbose 查詢 **（/query/v**）。 在具有 **/it**之工作的詳細查詢顯示中，[**登入模式]** 欄位的值為 [**僅限互動**]。

### <a name="to-schedule-a-task-that-runs-every-n-weeks"></a><a name=BKMK_weeks></a>若要排程每隔 N 周執行一次的工作

#### <a name="weekly-schedule-syntax"></a>每週排程語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc weekly [/mo {1 - 52}] [/d {<MON - SUN>[,MON - SUN...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在每週排程中，需要 **/sc 每週**參數。 **/Mo** （修飾詞）參數是選擇性的，而且會指定每次執行工作之間的周數。 [ **/Mo** ] 的預設值為1（每週）。

每週排程也有選擇性的 **/d**參數，可將工作排定在一周指定的日子執行，或在所有的天數（ *）。預設值為 [週一（星期一）]。[* 每天] （）選項相當於排程每日工作。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-every-six-weeks"></a>若要排程每隔六周執行一次的工作

下列命令會排定 MyApp 程式在遠端電腦上每隔六周執行一次。 此命令會使用 **/mo**參數來指定間隔。 因為命令會省略 **/d**參數，所以工作會在星期一執行。

此命令也會使用 **/s**參數來指定遠端電腦和 **/u**參數，以使用使用者的系統管理員帳戶許可權來執行命令。 因為省略了 **/p**參數，所以**schtasks.exe**會提示使用者輸入系統管理員帳戶密碼。

此外，因為命令是從遠端執行，所以命令中的所有路徑（包括 MyApp 的路徑）都是指遠端電腦上的路徑。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /mo 6 /s Server16 /u Admin01
```

#### <a name="to-schedule-a-task-that-runs-every-other-week-on-friday"></a>若要排程在星期五每隔一周執行的工作

下列命令會將工作排程為每隔一個星期五執行。 它會使用 **/mo**參數來指定兩周間隔和 **/d**參數，以指定一周中的哪一天。 若要排程每個星期五執行的工作，請省略 **/mo**參數，或將它設定為1。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /mo 2 /d FRI
```

### <a name="to-schedule-a-task-that-runs-every-n-months"></a><a name=BKMK_months></a>若要排程每隔 N 個月執行一次的工作

#### <a name="syntax"></a>語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly [/mo {1 - 12}] [/d {1 - 31}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在此排程類型中，需要 **/sc 每月**參數。 **/Mo** （修飾詞）參數，指定每次執行工作之間的月數，是選擇性的，預設值是1（每個月）。 此排程類型也有選擇性的 **/d**參數，可將工作排程在當月指定的日期執行。 預設值為1（當月的第一天）。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-on-the-first-day-of-every-month"></a>若要排程在每個月的第一天執行的工作

下列命令會排定 MyApp 程式在每個月的第一天執行。 因為1的值是 **/mo** （修飾詞）參數和 **/d** （day）參數的預設值，所以命令會省略這些參數。
```
schtasks /create /tn My App /tr myapp.exe /sc monthly
```

#### <a name="to-schedule-a-task-that-runs-every-three-months"></a>若要排程每隔三個月執行一次的工作

下列命令會排定 MyApp 程式每隔三個月執行一次。 它會使用 **/mo**參數來指定間隔。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo 3
```

#### <a name="to-schedule-a-task-that-runs-at-midnight-on-the-21st-day-of-every-other-month"></a>若要排程在每個月21日午夜執行的工作

下列命令會將 MyApp 程式排程為在每個月第21天的午夜執行。 此命令會指定此工作應執行一年，從2002年7月2日到2003年6月30日。

此命令會使用 **/mo**參數來指定每月間隔（每兩個月）、指定日期的 **/d**參數，以及用來指定時間的 **/st** 。 它也會使用 **/sd**和 **/ed**參數來分別指定開始日期和結束日期。 因為本機電腦是在 [**控制台**] 的 [**地區及語言選項**] 中設定為 [**英文（南非）** ] 選項，所以會以本機格式（YYYY/MM/DD）來指定日期。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo 2 /d 21 /st 00:00 /sd 2002/07/01 /ed 2003/06/30 
```

### <a name="to-schedule-a-task-that-runs-on-a-specific-day-of-the-week"></a><a name=BKMK_spec_day></a>若要排程在一周的特定一天執行的工作

#### <a name="weekly-schedule-syntax"></a>每週排程語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc weekly [/d {<MON - SUN>[,MON - SUN...] | *}] [/mo {1 - 52}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

每週排程的日期是每週排程的變化。 在每週排程中，需要 **/sc 每週**參數。 **/Mo** （修飾詞）參數是選擇性的，而且會指定每次執行工作之間的周數。 [ **/Mo** ] 的預設值為1（每週）。 **/D**參數是選擇性的，會將工作排程在一周指定的日子執行，或在所有的日子（\*）。 預設值為 [週一（星期一）]。 [每日] 選項（ **/d \*** ）相當於排程每日工作。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-every-wednesday"></a>若要排程每星期三執行的工作

下列命令會排定 MyApp 程式在星期三每週執行一次。 此命令會使用 **/d**參數來指定一周中的哪一天。 因為命令省略了 **/mo**參數，所以工作會每週執行一次。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /d WED
```

#### <a name="to-schedule-a-task-that-runs-every-eight-weeks-on-monday-and-friday"></a>若要排程在星期一和星期五每八周執行一次的工作

下列命令會將工作排程在每八周的星期一和星期五執行。 它會使用 **/d**參數來指定 days 和 **/mo**參數，以指定八周間隔。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /mo 8 /d MON,FRI
```

### <a name="to-schedule-a-task-that-runs-on-a-specific-week-of-the-month"></a><a name=BKMK_spec_week></a>若要排程在當月特定周執行的工作

#### <a name="specific-week-syntax"></a>特定周語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /mo {FIRST | SECOND | THIRD | FOURTH | LAST} /d MON - SUN [/m {JAN - DEC[,JAN - DEC...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在此排程類型中，需要 **/sc 每月**參數、 **/mo** （修飾詞）參數和 **/d** （day）參數。 **/Mo** （修飾詞）參數會指定工作執行的周。 **/D**參數會指定一周中的哪幾天。 （您只能針對此排程類型指定一周中的一天）。此排程也有選擇性的 **/m** （month）參數，可讓您針對特定月份或每月（\*）排定工作。 **/M**參數的預設值是每個月（\*）。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-for-the-second-sunday-of-every-month"></a>針對每個月的第二個星期日排程工作

下列命令會將 MyApp 程式排程在每個月的第二個星期日執行。 它會使用 **/mo**參數來指定當月的第二周和 **/d**參數來指定日期。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo SECOND /d SUN
```

#### <a name="to-schedule-a-task-for-the-first-monday-in-march-and-september"></a>若要在3月和9月的第一個星期一排程工作

下列命令會排定 MyApp 程式在3月和9月的第一個星期一執行。 它會使用 **/mo**參數來指定當月的第一周和 **/d**參數來指定日期。 它會使用 **/m**參數來指定月份，並以逗號分隔月份引數。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo FIRST /d MON /m MAR,SEP
```

### <a name="to-schedule-a-task-that-runs-on-a-specific-date-each-month"></a><a name=BKMK_spec_date></a>若要排程每個月在特定日期執行的工作

#### <a name="specific-date-syntax"></a>特定日期語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /d {1 - 31} [/m {JAN - DEC[,JAN - DEC...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在 [特定日期] 排程類型中，需要 **/sc [每月**] 參數和 [ **/d** （天）] 參數。 **/D**參數指定月份的日期（1-31），而不是一周中的某一天。 您只能在排程中指定一天。 這個排程類型的 **/mo** （修飾詞）參數無效。

這個排程類型的 **/m** （month）參數是選擇性的，預設值是每個月（<em>）。 **Schtasks</em>* 不會讓您針對 **/m**參數所指定的月份不會發生的日期排定工作。 不過，如果省略 **/m**參數，並針對不會出現在每個月（例如第31天）的日期排程工作，則工作不會在較短的月份執行。 若要排程當月最後一天的工作，請使用 [最後一天] 排程類型。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-for-the-first-day-of-every-month"></a>若要在每個月的第一天排程工作

下列命令會排定 MyApp 程式在每個月的第一天執行。 因為預設的修飾詞為 none （無修飾詞），所以預設的日期是第1天，而預設月份是每個月，此命令不需要任何額外的參數。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly
```

#### <a name="to-schedule-a-task-for-the-15th-days-of-may-and-june"></a>若要將工作排定在5月15日至6月

下列命令會將 MyApp 程式排程于下午3:00 年5月15日到6月15日前執行。 （15:00）。 它會使用 **/m**參數來指定日期和 **/m**參數，以指定月份。 它也會使用 **/st**參數來指定開始時間。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /d 15 /m MAY,JUN /st 15:00
```

### <a name="to-schedule-a-task-that-runs-on-the-last-day-of-a-month"></a><a name=BKMK_last_day></a>若要排程在當月最後一天執行的工作

#### <a name="last-day-syntax"></a>最後一天的語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /mo LASTDAY /m {JAN - DEC[,JAN - DEC...] | *} [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在 [最後一天] 排程類型中，需要 **/sc [每月**] 參數、[ **/mo LASTDAY** ] （修飾詞）參數和 **/m** （month）參數。 **/D** （day）參數無效。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-for-the-last-day-of-every-month"></a>若要排程每個月最後一天的工作

下列命令會將 MyApp 程式排程在每個月的最後一天執行。 它會使用 **/mo**參數來指定最後一天，以及使用萬用字元（*）的 **/m**參數來指出程式每個月執行一次。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo lastday /m *
```

#### <a name="to-schedule-a-task-at-600-pm-on-the-last-days-of-february-and-march"></a>若要在下午6:00 排程工作 在2月和3月的最後一天

下列命令會將 MyApp 程式排程在2月的最後一天和下午6:00 的最後一天執行。 它會使用 **/mo**參數來指定最後一天、用來指定月份的 **/m**參數，以及用來指定開始時間的 **/st**參數。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo lastday /m FEB,MAR /st 18:00
```

### <a name="to-schedule-a-task-that-runs-once"></a><a name=BKMK_once></a>排程執行一次的工作

#### <a name="syntax"></a>語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc once /st <HH:MM> [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在 [執行一次] 排程類型中，需要 **/sc 一次**參數。 指定工作執行時間的 **/st**參數是必要的。 **/Sd**參數會指定工作執行的日期，這是選擇性的。 這個排程類型的 **/mo** （修飾詞）和 **/ed** （結束日期）參數無效。

如果指定的日期和時間是在過去，根據本機電腦的時間，則**Schtasks**不允許您排定工作執行一次。 若要排程在不同時區的遠端電腦上執行一次的工作，您必須在本機電腦上進行該日期和時間之前進行排程。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-one-time"></a>若要排程執行一次的工作

下列命令會排定 MyApp 程式在2003年1月1日午夜執行。 它會使用 **/sc**參數來指定排程類型，以及用來指定日期和時間的 **/sd**和**st** 。

因為本機電腦會在 [**控制台**] 的 [**地區及語言選項**] 中使用 [**英文（美國）** ] 選項，所以開始日期的格式為 MM/DD/YYYY。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc once /sd 01/01/2003 /st 00:00
```

### <a name="to-schedule-a-task-that-runs-every-time-the-system-starts"></a><a name=BKMK_startup></a>若要排程每次系統啟動時執行的工作

#### <a name="syntax"></a>語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onstart [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在 [啟動時排程] 類型中，需要 **/sc onstart**參數。 **/Sd** （開始日期）參數是選擇性的，而且預設值是目前的日期。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-when-the-system-starts"></a>若要排程在系統啟動時執行的工作

下列命令會排程 MyApp 程式在系統每次啟動時執行，從2001年3月15日開始：

因為本機電腦在 [**控制台**] 的 [**地區及語言選項**] 中使用 [**英文（美國）** ] 選項，所以開始日期的格式為 MM/DD/YYYY。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc onstart /sd 03/15/2001
```

### <a name="to-schedule-a-task-that-runs-when-a-user-logs-on"></a><a name=BKMK_logon></a>若要排程在使用者登入時執行的工作

#### <a name="syntax"></a>語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onlogon [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

[登入排程類型] 會排定在任何使用者登入電腦時執行的工作。 在 [登入排程] 類型中，需要 **/sc 整理**參數。 **/Sd** （開始日期）參數是選擇性的，而且預設值是目前的日期。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-when-a-user-logs-on-to-a-remote-computer"></a>若要排程在使用者登入遠端電腦時執行的工作

下列命令會排程每次使用者（任何使用者）登入遠端電腦時執行批次檔。 它會使用 **/s**參數來指定遠端電腦。 因為命令是遠端的，所以命令中的所有路徑（包括批次檔的路徑）都是指遠端電腦上的路徑。
```
schtasks /create /tn Start Web Site /tr c:\myiis\webstart.bat /sc onlogon /s Server23
```

### <a name="to-schedule-a-task-that-runs-when-the-system-is-idle"></a><a name=BKMK_idle></a>若要排程在系統閒置時執行的工作

#### <a name="syntax"></a>語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onidle /i {1 - 999} [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

[閒置時間] 排程類型會在 [ **/i** ] 參數指定的時間內沒有使用者活動時，排定執行的工作。 在 [閒置時的排程類型] 中，需要 **/sc 的 onidle**參數和 **/i**參數。 **/Sd** （開始日期）是選擇性的，而且預設值是目前的日期。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-whenever-the-computer-is-idle"></a>若要排程在電腦閒置時執行的工作

下列命令會排定 MyApp 程式在電腦閒置時執行。 它會使用必要的 **/i**參數來指定電腦必須在工作啟動前十分鐘維持閒置狀態。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc onidle /i 10
```

### <a name="to-schedule-a-task-that-runs-now"></a><a name=BKMK_now></a>排定立即執行的工作

**Schtasks**沒有 [立即執行] 選項，但您可以藉由建立執行一次的工作，並在幾分鐘內啟動，以模擬該選項。

#### <a name="syntax"></a>語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc once [/st <HH:MM>] /sd <MM/DD/YYYY> [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-a-few-minutes-from-now"></a>排定從現在開始執行幾分鐘的工作。

下列命令會在2002年11月13日下午2:18，將工作排程為執行一次。 當地時間。

因為本機電腦在 [**控制台**] 的 [**地區及語言選項**] 中使用 [**英文（美國）** ] 選項，所以開始日期的格式為 MM/DD/YYYY。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc once /st 14:18 /sd 11/13/2002
```

### <a name="to-schedule-a-task-that-runs-with-different-permissions"></a><a name=BKMK_diff_perms></a>若要排程以不同許可權執行的工作

您可以排程所有類型的工作，以在本機和遠端電腦上以替代帳戶的許可權執行。 除了特定排程類型所需的參數之外， **/ru**參數是必要的，而 **/rp**參數則是選擇性的。

#### <a name="examples"></a>範例

#### <a name="to-run-a-task-with-administrator-permissions-on-the-local-computer"></a>若要在本機電腦上以系統管理員許可權執行工作

下列命令會排定 MyApp 程式在本機電腦上執行。 它會使用 **/ru**來指定工作應該以使用者的系統管理員帳戶（Admin06）的許可權來執行。 在此範例中，工作已排程為每個星期二執行，但是您可以使用任何排程類型來執行具有替代許可權的工作。
```
schtasks /create /tn My App /tr myapp.exe /sc weekly /d TUE /ru Admin06
```
在回應中， **schtasks.exe**會提示您輸入 Admin06 帳戶的 [執行身分] 密碼，然後顯示成功訊息。
```
Please enter the run as password for Admin06: ********
SUCCESS: The scheduled task My App has successfully been created.
```

#### <a name="to-run-a-task-with-alternate-permissions-on-a-remote-computer"></a>若要在遠端電腦上執行具有替代許可權的工作

下列命令會將 MyApp 程式排程在行銷電腦上每四天執行一次。

此命令會使用 **/sc**參數來指定每日排程和 **/mo**參數，以指定四天的間隔。

此命令會使用 **/s**參數來提供遠端電腦的名稱，並使用 **/u**參數來指定有權在遠端電腦上排程工作的帳戶（在行銷電腦上為 Admin01）。 它也會使用 **/ru**參數來指定工作應以使用者的非系統管理員帳戶（在 Reskits 網域中為 User01）的許可權執行。 如果沒有 **/ru**參數，工作就會以 **/u**所指定之帳戶的許可權執行。
```
schtasks /create /tn My App /tr myapp.exe /sc daily /mo 4 /s Marketing /u Marketing\Admin01 /ru Reskits\User01
```
**Schtasks**會先要求 **/u**參數（用來執行命令）所命名的使用者密碼，然後再要求由 **/ru**參數命名的使用者密碼（以執行工作）。 驗證密碼之後， **schtasks**會顯示一則訊息，指出已排程工作。
```
Type the password for Marketing\Admin01:********

Please enter the run as password for Reskits\User01: ********

SUCCESS: The scheduled task My App has successfully been created.
```

#### <a name="to-run-a-task-only-when-a-particular-user-is-logged-on"></a>只有在特定使用者登入時才執行工作

下列命令會將 AdminCheck 程式排程為在每星期五 4:00 A.M. 的公用電腦上執行，但僅限於電腦的系統管理員登入。

此命令會使用 **/sc**參數來指定每週排程、指定日期的 **/d**參數，以及用來指定開始時間的 **/st**參數。

此命令會使用 **/s**參數來提供遠端電腦的名稱，並使用 **/u**參數來指定有權在遠端電腦上排程工作的帳戶。 它也會使用 **/ru**參數，將工作設定為以公用電腦系統管理員的許可權執行（Public\Admin01）和 **/it**參數，以指出只有在 Public\Admin01 帳戶登入時才會執行工作。
```
schtasks /create /tn Check Admin /tr AdminCheck.exe /sc weekly /d FRI /st 04:00 /s Public /u Domain3\Admin06 /ru Public\Admin01 /it
```
**注意**
-   若要使用僅限互動式（ **/it**）屬性來識別工作，請使用 verbose 查詢 **（/query/v**）。 在具有 **/it**之工作的詳細查詢顯示中，[**登入模式]** 欄位的值為 [**僅限互動**]。

### <a name="to-schedule-a-task-that-runs-with-system-permissions"></a><a name=BKMK_sys_perms></a>若要排程以系統許可權執行的工作

所有類型的工作都可以使用本機和遠端電腦上的系統帳戶許可權來執行。 除了特定排程類型所需的參數之外，還需要 **/ru 系統**（或 * */ru * *）參數，而且 **/rp**參數無效。

**重要事項**
-   系統帳戶沒有互動式登入許可權。 使用者無法看到或與以系統許可權執行的程式或工作互動。
-   **/Ru**參數會決定工作執行時所使用的許可權，而不是用來排程工作的許可權。 只有系統管理員可以排程工作，而不論 **/ru**參數的值為何。

**注意**

若要識別以系統許可權執行的工作，請使用 verbose 查詢（ **/query** **/v**）。 在系統執行工作的詳細查詢顯示中，[執行身分**使用者**] 欄位的值為 [ **NT AUTHORITY\SYSTEM** ]，[**登入模式]** 欄位的值為 [**僅限背景**]。

#### <a name="examples"></a>範例

#### <a name="to-run-a-task-with-system-permissions"></a>若要以系統許可權執行工作

下列命令會使用系統帳戶的許可權，將 MyApp 程式排程在本機電腦上執行。 在此範例中，工作已排程在每個月的第15天執行，但是您可以使用任何排程類型來執行具有系統許可權的工作。

此命令會使用 **/Ru system**參數來指定系統安全性內容。 由於系統工作不會使用密碼，因此會省略 **/rp**參數。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /d 15 /ru System
```
在回應中， **schtasks.exe**會顯示參考用訊息和成功訊息。 它不會提示輸入密碼。
```
INFO: The task will be created under user name (NT AUTHORITY\SYSTEM).
SUCCESS: The Scheduled task My App has successfully been created.
```

#### <a name="to-run-a-task-with-system-permissions-on-a-remote-computer"></a>若要在遠端電腦上執行具有系統許可權的工作

下列命令會將 MyApp 程式排程在每天上午4:00 的 Finance01 電腦上執行。 具有系統許可權。

此命令會使用 **/tn**參數來命名工作和 **/tr**參數，以指定 MyApp 程式的遠端副本。 它會使用 **/sc**參數來指定每日排程，但省略 **/mo**參數，因為1（每天）是預設值。 它會使用 **/st**參數來指定開始時間，也就是工作每天執行的時間。

此命令會使用 **/s**參數來提供遠端電腦的名稱，並使用 **/u**參數來指定有權在遠端電腦上排程工作的帳戶。 它也會使用 **/ru**參數來指定工作應該在系統帳戶下執行。 如果沒有 **/ru**參數，工作就會以 **/u**所指定之帳戶的許可權執行。
```
schtasks /create /tn My App /tr myapp.exe /sc daily /st 04:00 /s Finance01 /u Admin01 /ru System
```
**Schtasks**會要求 **/u**參數所命名的使用者密碼，並在驗證密碼之後，顯示一則訊息，指出工作已建立，而且會以系統帳戶的許可權執行。
```
Type the password for Admin01:**********

INFO: The Schedule Task My App will be created under user name (NT AUTHORITY\
SYSTEM).
SUCCESS: The scheduled task My App has successfully been created.
```

### <a name="to-schedule-a-task-that-runs-more-than-one-program"></a><a name=BKMK_multi_progs></a>若要排程執行多個程式的工作

每個工作只會執行一個程式。 不過，您可以建立執行多個程式的批次檔，然後排程執行批次檔的工作。 下列程式示範此方法：
1. 建立可啟動您要執行之程式的批次檔。

   在此範例中，您會建立一個啟動事件檢視器（Eventvwr.msc）和「系統監視器」（Perfmon）的批次檔。  
   - 開啟文字編輯器，例如記事本。
   - 輸入每個程式之可執行檔的名稱和完整路徑。 在此情況下，檔案包含下列語句。  
     ```
     C:\Windows\System32\Eventvwr.exe 
     C:\Windows\System32\Perfmon.exe
     ```  
   - 將檔案儲存為 MyApps。
2. 使用**schtasks.exe**建立執行 MyApps 的工作。

   下列命令會建立監視工作，每當有人登入時就會執行。 它會使用 **/tn**參數來命名工作，並使用 **/tr**參數來執行 MyApps。 它會使用 **/sc**參數來指示整理排程類型，以及用來以使用者的系統管理員帳戶許可權執行工作的 **/ru**參數。  
   ```
   schtasks /create /tn Monitor /tr C:\MyApps.bat /sc onlogon /ru Reskit\Administrator
   ```  
   由於此命令的結果，每當使用者登入電腦時，工作都會啟動事件檢視器和系統監視器。

### <a name="to-schedule-a-task-that-runs-on-a-remote-computer"></a><a name=BKMK_remote></a>若要排程在遠端電腦上執行的工作

若要將工作排定在遠端電腦上執行，您必須將工作新增至遠端電腦的排程。 所有類型的工作都可以在遠端電腦上排程，但必須符合下列條件。
-   您必須具有排程工作的許可權。 因此，您必須使用遠端電腦上 Administrators 群組成員的帳戶登入本機電腦，或者您必須使用 **/u**參數來提供遠端電腦之系統管理員的認證。
-   只有當本機和遠端電腦位於相同網域，或本機電腦位於遠端電腦網域所信任的網域時，您才可以使用 **/u**參數。 否則，遠端電腦就無法驗證指定的使用者帳戶，也無法確認帳戶是否為系統管理員群組的成員。
-   工作必須具有足夠的許可權，才能在遠端電腦上執行。 所需的許可權會因工作而異。 根據預設，工作會以本機電腦目前使用者的許可權執行，或者，如果使用 **/u**參數，則會以 **/u**參數所指定之帳戶的許可權執行工作。 不過，您可以使用 **/ru**參數，以不同使用者帳戶或系統許可權的許可權來執行工作。

#### <a name="examples"></a>範例

#### <a name="an-administrator-schedules-a-task-on-a-remote-computer"></a>系統管理員在遠端電腦上排程工作

下列命令會排程 MyApp 程式在 SRV01 遠端電腦上每隔10天立即開始執行。 此命令會使用 **/s**參數來提供遠端電腦的名稱。 因為本機目前的使用者是遠端電腦的系統管理員，所以不需要使用 **/u**參數來提供排程工作的替代許可權。

請注意，在遠端電腦上排程工作時，所有參數都會參考遠端電腦。 因此， **/tr**參數所指定的可執行檔是指遠端電腦上的 MyApp 複本。
```
schtasks /create /s SRV01 /tn My App /tr c:\program files\corpapps\myapp.exe /sc daily /mo 10
```
在 [回應] 中，[ **schtasks** ] 會顯示成功訊息，指出已排程工作。

#### <a name="a-user-schedules-a-command-on-a-remote-computer-case-1"></a>使用者在遠端電腦上排定命令（案例1）

下列命令會將 MyApp 程式排程在 SRV06 遠端電腦上每隔三個小時執行一次。 因為排程工作需要系統管理員許可權，所以命令會使用 **/u**和 **/p**參數來提供使用者的系統管理員帳戶（在 Reskits 網域中為 Admin01）的認證。 根據預設，這些許可權也會用來執行工作。 不過，由於工作不需要系統管理員許可權即可執行，因此命令會包含 **/u**和 **/rp**參數來覆寫預設值，並在遠端電腦上以使用者的非系統管理員帳戶許可權執行工作。
```
schtasks /create /s SRV06 /tn My App /tr c:\program files\corpapps\myapp.exe /sc hourly /mo 3 /u reskits\admin01 /p R43253@4$ /ru SRV06\user03 /rp MyFav!!Pswd
```
在 [回應] 中，[ **schtasks** ] 會顯示成功訊息，指出已排程工作。

#### <a name="a-user-schedules-a-command-on-a-remote-computer-case-2"></a>使用者在遠端電腦上排定命令（案例2）

下列命令會將 MyApp 程式排程在每個月的最後一天執行于 SRV02 遠端電腦上。 由於本機目前使用者（user03）不是遠端電腦的系統管理員，因此此命令會使用 **/u**參數來提供使用者的系統管理員帳戶（Reskits 網域中的 Admin01）的認證。 系統管理員帳戶許可權將用來排程工作和執行工作。
```
schtasks /create /s SRV02 /tn My App /tr c:\program files\corpapps\myapp.exe /sc monthly /mo LASTDAY /m * /u reskits\admin01
```
因為此命令不包含 **/p** （password）參數，所以**schtasks**會提示您輸入密碼。 然後它會顯示成功訊息，在此案例中為警告。
```
Type the password for reskits\admin01:********

SUCCESS: The scheduled task My App has successfully been created.

WARNING: The Scheduled task My App has been created, but may not run because
the account information could not be set.
```
此警告表示遠端網域無法驗證 **/u**參數所指定的帳號。 在此情況下，遠端網域無法驗證使用者帳戶，因為本機電腦不是遠端電腦網域所信任之網域的成員。 發生這種情況時，工作作業會出現在已排程的工作清單中，但工作實際上是空的，而且不會執行。

詳細資訊查詢的下列顯示畫面會公開此工作的問題。 請注意，在顯示畫面中，[**下次執行時間]** 的值**永遠**不會，而且無法**從工作排程器資料庫中**抓取 [**以使用者身分執行**] 的值。

如果這部電腦是相同網域或受信任網域的成員，則工作會成功排程，並依指定執行。
```
HostName: SRV44
TaskName: My App
Next Run Time: Never
Status:
Logon mode: Interactive/Background
Last Run Time: Never
Last Result: 0
Creator: user03
Schedule: At 3:52 PM on day 31 of every month, start
 starting 12/14/2001
Task To Run: c:\program files\corpapps\myapp.exe
Start In: myapp.exe
Comment: N/A
Scheduled Task State: Disabled
Scheduled Type: Monthly
Start Time: 3:52:00 PM
Start Date: 12/14/2001
End Date: N/A
Days: 31
Months: JAN,FEB,MAR,APR,MAY,JUN,JUL,AUG,SEP,OCT,NO
V,DEC
Run As User: Could not be retrieved from the task sched
uler database
Delete Task If Not Rescheduled: Enabled
Stop Task If Runs X Hours and X Mins: 72:0
Repeat: Every: Disabled
Repeat: Until: Time: Disabled
Repeat: Until: Duration: Disabled
Repeat: Stop If Still Running: Disabled
Idle Time: Disabled
Power Management: Disabled
```

#### <a name="remarks"></a>備註

-   若要以不同使用者的許可權執行 **/create**命令，請使用 **/u**參數。 **/U**參數僅適用于遠端電腦上的排程工作。
-   若要查看更多的**schtasks/create**範例，請輸入**schtasks/create/？** 。
-   若要排程以不同使用者的許可權執行的工作，請使用 **/ru**參數。 **/Ru**參數對本機和遠端電腦上的工作而言是有效的。
-   若要使用 **/u**參數，本機電腦必須位於與遠端電腦相同的網域中，或必須位於遠端電腦網域所信任的網域中。 否則，可能是工作未建立，或工作作業是空的，而且工作不會執行。
-   [ **Schtasks** ] 一律會提示輸入密碼，除非您提供，即使您在本機電腦上使用目前的使用者帳戶來排程工作也一樣。 這是**schtasks**的正常行為。
-   **Schtasks**不會驗證程式檔案位置或使用者帳戶密碼。 如果您沒有為使用者帳戶輸入正確的檔案位置或正確的密碼，則會建立工作，但不會執行。 此外，如果帳戶的密碼變更或過期，而且您沒有變更儲存在工作中的密碼，則工作不會執行。
-   系統帳戶沒有互動式登入許可權。 使用者看不到，也無法與以系統許可權執行的程式互動。
-   每個工作只會執行一個程式。 不過，您可以建立一個批次檔來啟動多個工作，然後排程執行批次檔的工作。
-   您可以在建立工作之後立即進行測試。 使用**執行**作業來測試工作，然後檢查 SchedLgU 檔案（*SystemRoot*\SchedLgU.txt）是否有錯誤。

## <a name="schtasks-change"></a><a name=BKMK_change></a>schtasks 變更

變更工作的下列一個或多個屬性。
-   工作執行的程式（ **/tr**）。
-   執行工作所用的使用者帳戶（ **/ru**）。
-   使用者帳戶的密碼（ **/rp**）。
-   將互動式屬性新增至工作（ **/it**）。

### <a name="syntax"></a>語法

```
schtasks /change /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]] [/ru {[<Domain>\]<User> | System}] [/rp <Password>] [/tr <TaskRun>] [/st <StartTime>] [/ri <Interval>] [{/et <EndTime> | /du <Duration>} [/k]] [/sd <StartDate>] [/ed <EndDate>] [/{ENABLE | DISABLE}] [/it] [/z]
```

#### <a name="parameters"></a>參數

|          詞彙           |                                                                                                                                                                                                                                                                                                                                     定義                                                                                                                                                                                                                                                                                                                                      |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     \<TaskName > 的/tn     |                                                                                                                                                                                                                                                                                                               識別要變更的工作。 輸入工作名稱。                                                                                                                                                                                                                                                                                                               |
|     /s \<電腦 >      |                                                                                                                                                                                                                                                                               指定遠端電腦的名稱或 IP 位址（不含反斜線）。 預設是本機電腦。                                                                                                                                                                                                                                                                               |
|  /u [\<Domain >\]<User>  |                                                                                                                                                                 以指定之使用者帳戶的許可權執行此命令。 預設值是本機電腦目前使用者的許可權。 指定的使用者帳戶必須是遠端電腦上 Administrators 群組的成員。 **/U**和 **/p**參數只有在遠端電腦上變更工作（ **/s**）時才有效。                                                                                                                                                                  |
|     /p \<密碼 >      |                                                                                                                                                                                              指定 **/u**參數中指定之使用者帳戶的密碼。 如果您使用 **/u**參數，但省略 **/p**參數或 password 引數，則**schtasks**會提示您輸入密碼。</br>只有當您使用 **/s**時， **/u**和 **/p**參數才有效。                                                                                                                                                                                               |
| /ru {[\<Domain >\]<User> |                                                                                                                                                                                                                                                                                                                                       筆記本電腦                                                                                                                                                                                                                                                                                                                                       |
|     /rp \<密碼 >     |                                                                                                                                                                                                                                                 指定現有使用者帳戶的新密碼，或為 **/ru**參數所指定的使用者帳戶。 使用搭配本機系統帳戶時，會忽略這個參數。                                                                                                                                                                                                                                                  |
|     \</Tr u n > 的/tr      |                                                                                                                                                                                  變更工作執行的程式。 輸入可執行檔、指令檔或批次檔的完整路徑和檔案名。 如果您省略路徑，則**schtasks**會假設檔案位於 \<systemroot > \System32 目錄中。 指定的程式會取代工作所執行的原始程式。                                                                                                                                                                                  |
|    /st \<Starttime >     |                                                                                                                                                                                                                                                              使用24小時制時間格式（HH： mm），指定工作的開始時間。 例如，14:30 的值相當於12小時的 2:30 PM 時間。                                                                                                                                                                                                                                                               |
|     /ri \<間隔 >     |                                                                                                                                                                                                                                                                           指定排程工作的重複間隔（以分鐘為單位）。 有效範圍是 1-599940 （599940分鐘 = 9999 小時）。                                                                                                                                                                                                                                                                            |
|     /et \<EndTime >      |                                                                                                                                                                                                                                                               使用24小時制時間格式（HH： mm），指定工作的結束時間。 例如，14:30 的值相當於12小時的 2:30 PM 時間。                                                                                                                                                                                                                                                                |
|     /du \<持續時間 >     |                                                                                                                                                                                                                                                                                                     指定在 \<EndTime > 或 <Duration>上關閉工作（如果有指定）。                                                                                                                                                                                                                                                                                                      |
|           /k            |                                                                                                                                                                   停止在 **/et**或 **/du**指定的時間執行工作的程式。 如果沒有 **/k**，則**schtasks**在到達 **/et**或 **/du**所指定的時間之後，不會再啟動程式，但如果程式仍在執行中，則不會將它停止。 這個參數是選擇性的，而且只有分鐘或每小時的排程才有效。                                                                                                                                                                   |
|    /sd \<起始 >     |                                                                                                                                                                                                                                                                                              指定應該執行工作的第一個日期。 日期格式為 MM/DD/YYYY。                                                                                                                                                                                                                                                                                               |
|     /ed \<結束日期 >      |                                                                                                                                                                                                                                                                                                 指定工作應該執行的最後一個日期。 格式為 MM/DD/YYYY。                                                                                                                                                                                                                                                                                                  |
|         /ENABLE         |                                                                                                                                                                                                                                                                                                                       指定以啟用排程工作。                                                                                                                                                                                                                                                                                                                       |
|        /DISABLE         |                                                                                                                                                                                                                                                                                                                      指定停用排程工作。                                                                                                                                                                                                                                                                                                                       |
|           /it           | 指定只有在執行身分使用者（執行工作的使用者帳戶）登入電腦時，才執行排程工作。</br>這個參數不會影響以系統許可權執行的工作，或是已經設定了互動式屬性的工作。 您不能使用 change 命令從工作中移除僅限互動式的屬性。</br>根據預設，當工作已排程時，[執行身分] 使用者是本機電腦的目前使用者，如果使用的是 **/u**參數所指定的帳號，則為該使用者。 不過，如果命令包含 **/ru**參數，則「執行身分」使用者就是 **/ru**參數所指定的帳號。 |
|           /z            |                                                                                                                                                                                                                                                                                                          指定在完成排程時刪除工作。                                                                                                                                                                                                                                                                                                          |
|           /?            |                                                                                                                                                                                                                                                                                                                        在命令提示字元顯示說明。                                                                                                                                                                                                                                                                                                                         |

### <a name="remarks"></a>備註

-   **/Tn**和 **/s**參數會識別工作。 **/Tr**、 **/ru**和 **/rp**參數會指定您可以變更之工作的屬性。
-   **/Ru**和 **/rp**參數會指定工作執行時所用的許可權。 **/U**和 **/p**參數會指定用來變更工作的許可權。
-   若要變更遠端電腦上的工作，使用者必須使用屬於遠端電腦上 Administrators 群組成員的帳戶登入本機電腦。
-   若要以不同使用者的許可權執行 **/change**命令（ **/u**， **/p**），本機電腦必須位於與遠端電腦相同的網域中，或必須位於遠端電腦網域所信任的網域中。
-   系統帳戶沒有互動式登入許可權。 使用者看不到，也無法與以系統許可權執行的程式互動。
-   若要識別具有 **/it**屬性的工作，請使用 verbose 查詢（ **/query/v**）。 在具有 **/it**之工作的詳細查詢顯示中，[**登入模式]** 欄位的值為 [**僅限互動**]。

### <a name="examples"></a>範例

### <a name="to-change-the-program-that-a-task-runs"></a>變更工作執行的程式

下列命令會將「病毒檢查」工作執行的程式從 VirusCheck 變更為 VirusCheck2。 此命令會使用 **/tn**參數來識別工作和 **/tr**參數，以指定工作的新程式。 （您無法變更工作名稱）。
```
schtasks /change /tn Virus Check /tr C:\VirusCheck2.exe
```
在回應中， **schtasks.exe**會顯示下列成功訊息：
```
SUCCESS: The parameters of the scheduled task Virus Check have been changed.
```
此命令的結果是，「病毒檢查」工作現在會執行 VirusCheck2。

### <a name="to-change-the-password-for-a-remote-task"></a>若要變更遠端工作的密碼

下列命令會針對遠端電腦 Woodgrove-svr01 上的 RemindMe 工作變更使用者帳戶的密碼。 此命令會使用 **/tn**參數來識別工作，並使用 **/s**參數來指定遠端電腦。 它會使用 **/rp**參數來指定新的密碼，p@ssWord3。

每當使用者帳戶的密碼過期或變更時，就需要此程式。 如果工作中儲存的密碼不再有效，則工作不會執行。
```
schtasks /change /tn RemindMe /s Svr01 /rp p@ssWord3
```
在回應中， **schtasks.exe**會顯示下列成功訊息：
```
SUCCESS: The parameters of the scheduled task RemindMe have been changed.
```
由於此命令的結果，RemindMe 工作現在會在其原始使用者帳戶下執行，但使用新的密碼。

### <a name="to-change-the-program-and-user-account-for-a-task"></a>變更工作的程式和使用者帳戶

下列命令會變更工作執行的程式，並變更執行工作所使用的使用者帳戶。 基本上，它會針對新的工作使用舊的排程。 此命令會變更 ChkNews 工作，在每天早上9:00 啟動 Notepad.exe，改為啟動 Internet Explorer。

此命令會使用 **/tn**參數來識別工作。 它會使用 **/tr**參數來變更工作執行的程式，以及使用 **/ru**參數來變更工作執行時所用的使用者帳戶。

會省略用來提供使用者帳戶密碼的 **/ru**和 **/rp**參數。 您必須提供帳戶的密碼，但您可以使用 **/ru**和 **/rp**參數，並以純文字輸入密碼，或等候**schtasks.exe**提示您輸入密碼，然後在 [遮蔽文字] 中輸入密碼。
```
schtasks /change /tn ChkNews /tr c:\program files\Internet Explorer\iexplore.exe /ru DomainX\Admin01
```
在回應中， **schtasks.exe**會要求使用者帳戶的密碼。 它會遮蔽您輸入的文字，因此不會顯示密碼。
```
Please enter the password for DomainX\Admin01: 
```
請注意， **/tn**參數會識別工作，而且 **/tr**和 **/ru**參數會變更工作的屬性。 您不能使用其他參數來識別工作，也無法變更工作名稱。

在回應中， **schtasks.exe**會顯示下列成功訊息：
```
SUCCESS: The parameters of the scheduled task ChkNews have been changed.
```
由於此命令的結果，ChkNews 工作現在會以系統管理員帳戶的許可權執行 Internet Explorer。

### <a name="to-change-a-program-to-the-system-account"></a>將程式變更為系統帳戶

下列命令會變更 SecurityScript 工作，使其以系統帳戶的許可權執行。 它會使用 * */ru * * 參數來表示系統帳戶。
```
schtasks /change /tn SecurityScript /ru 
```
在回應中， **schtasks.exe**會顯示下列成功訊息：
```
INFO: The run as user name for the scheduled task SecurityScript will be changed to NT AUTHORITY\SYSTEM.
SUCCESS: The parameters of the scheduled task SecurityScript have been changed.
```
因為以系統帳戶許可權執行的工作不需要密碼，所以**schtasks.exe**不會提示您輸入。

### <a name="to-run-a-program-only-when-i-am-logged-on"></a>只有在我登入時才執行程式

下列命令會將僅限互動式屬性新增至 MyApp （現有的工作）。 這個屬性可確保只有在執行身分使用者（也就是執行工作的使用者帳戶）登入電腦時，才會執行工作。

此命令會使用 **/tn**參數來識別工作和 **/it**參數，以將僅限互動式的屬性新增至工作。 因為工作已經以 [我的使用者帳戶] 的許可權執行，所以我不需要變更工作的 [ **/ru** ] 參數。
```
schtasks /change /tn MyApp /it
```
在回應中， **schtasks.exe**會顯示下列成功訊息。
```
SUCCESS: The parameters of the scheduled task MyApp have been changed.
```

## <a name="schtasks-run"></a><a name=BKMK_run></a>schtasks 執行

立即啟動排程工作。 **執行**作業會忽略排程，但會使用工作中儲存的程式檔案位置、使用者帳戶和密碼來立即執行工作。

### <a name="syntax"></a>語法

```
schtasks /run /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="parameters"></a>參數

|         詞彙          |                                                                                                                                                                 定義                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    \<TaskName > 的/tn    |                                                                                                                                                       必要。 識別工作。                                                                                                                                                        |
|    /s \<電腦 >     |                                                                                                           指定遠端電腦的名稱或 IP 位址（不含反斜線）。 預設是本機電腦。                                                                                                           |
| /u [\<Domain >\]<User> | 以指定之使用者帳戶的許可權執行此命令。 根據預設，此命令會以本機電腦目前使用者的許可權執行。</br>指定的使用者帳戶必須是遠端電腦上 Administrators 群組的成員。 只有當您使用 **/s**時， **/u**和 **/p**參數才有效。 |
|    /p \<密碼 >     |                          指定 **/u**參數中指定之使用者帳戶的密碼。 如果您使用 **/u**參數，但省略 **/p**參數或 password 引數，則**schtasks**會提示您輸入密碼。</br>只有當您使用 **/s**時， **/u**和 **/p**參數才有效。                           |
|          /?           |                                                                                                                                                    在命令提示字元顯示說明。                                                                                                                                                     |

### <a name="remarks"></a>備註

-   使用此作業來測試您的工作。 如果工作未執行，請檢查工作排程器服務交易記錄檔，\<Systemroot > \SchedLgU.txt，以找出錯誤。
-   執行工作不會影響工作排程，也不會變更排程工作的下一個執行時間。
-   若要從遠端執行工作，必須在遠端電腦上排程工作。 當您執行它時，此工作只會在遠端電腦上執行。 若要確認工作是否正在遠端電腦上執行，請使用 [工作管理員] 或 [工作排程器交易記錄] \<Systemroot > \SchedLgU.txt。

### <a name="examples"></a>範例

### <a name="to-run-a-task-on-the-local-computer"></a>若要在本機電腦上執行工作

下列命令會啟動「安全性腳本」工作。
```
schtasks /run /tn Security Script
```
在回應中， **schtasks.exe**會啟動與工作相關聯的腳本，並顯示下列訊息：
```
SUCCESS: Attempted to run the scheduled task Security Script.
```
正如訊息所暗示， **schtasks**會嘗試啟動程式，但它並不是真的啟動程式。

### <a name="to-run-a-task-on-a-remote-computer"></a>在遠端電腦上執行工作

下列命令會啟動遠端電腦 Woodgrove-svr01 上的更新工作：
```
schtasks /run /tn Update /s Svr01
```
在此情況下， **schtasks.exe**會顯示下列錯誤訊息：
```
ERROR: Unable to run the scheduled task Update.
```
若要找出錯誤的原因，請查看 Woodgrove-svr01 上的「排程的工作」交易記錄檔 C:\Windows\SchedLgU.txt。 在此情況下，記錄檔中會出現下列專案：
```
Update.job (update.exe) 3/26/2001 1:15:46 PM ** ERROR **
The attempt to log on to the account associated with the task failed, therefore, the task did not run.
The specific error is:
0x8007052e: Logon failure: unknown user name or bad password.
Verify that the task's Run-as name and password are valid and try again.
```
顯然，工作中的使用者名稱或密碼在系統上是不正確。 下列**schtasks/change**命令會更新 Woodgrove-svr01 上更新工作的使用者名稱和密碼：
```
schtasks /change /tn Update /s Svr01 /ru Administrator /rp PassW@rd3
```
在**變更**命令完成之後，就會重複**執行**命令。 這次，update.exe 程式會啟動，而**schtasks.exe**會顯示下列訊息：
```
SUCCESS: Attempted to run the scheduled task Update.
```
正如訊息所暗示， **schtasks**會嘗試啟動程式，但它並不是真的啟動程式。

## <a name="schtasks-end"></a><a name=BKMK_end></a>schtasks 結束

停止工作所啟動的程式。

### <a name="syntax"></a>語法

```
schtasks /end /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="parameters"></a>參數

|         詞彙          |                                                                                                                                                               定義                                                                                                                                                                |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    \<TaskName > 的/tn    |                                                                                                                                         必要。 識別啟動程式的工作。                                                                                                                                         |
|    /s \<電腦 >     |                                                                                                                        指定遠端電腦的名稱或 IP 位址。 預設是本機電腦。                                                                                                                        |
| /u [\<Domain >\]<User> | 以指定之使用者帳戶的許可權執行此命令。 根據預設，此命令會以本機電腦目前使用者的許可權執行。 指定的使用者帳戶必須是遠端電腦上 Administrators 群組的成員。 只有當您使用 **/s**時， **/u**和 **/p**參數才有效。 |
|    /p \<密碼 >     |                        指定 **/u**參數中指定之使用者帳戶的密碼。 如果您使用 **/u**參數，但省略 **/p**參數或 password 引數，則**schtasks**會提示您輸入密碼。</br>只有當您使用 **/s**時， **/u**和 **/p**參數才有效。                         |
|          /?           |                                                                                                                                                             顯示說明。                                                                                                                                                              |

### <a name="remarks"></a>備註

**Schtasks.exe**只會結束已排程工作啟動之程式的實例。 若要停止其他進程，請使用 TaskKill。 如需詳細資訊，請參閱[Taskkill](taskkill.md)。

### <a name="examples"></a>範例

### <a name="to-end-a-task-on-a-local-computer"></a>若要在本機電腦上結束工作

下列命令會停止「我的記事本」工作啟動的 Notepad.exe 實例：
```
schtasks /end /tn My Notepad
```
在回應中， **schtasks.exe**會停止工作已啟動的 notepad.exe 實例，並顯示下列成功訊息：
```
SUCCESS: The scheduled task My Notepad has been terminated successfully.
```

### <a name="to-end-a-task-on-a-remote-computer"></a>結束遠端電腦上的工作

下列命令會停止遠端電腦上 InternetOn 工作所啟動的 Internet Explorer 實例，Woodgrove-svr01：
```
schtasks /end /tn InternetOn /s Svr01
```
在回應中， **schtasks.exe**會停止工作已啟動的 Internet Explorer 實例，並顯示下列成功訊息：
```
SUCCESS: The scheduled task InternetOn has been terminated successfully.
```

## <a name="schtasks-delete"></a><a name=BKMK_delete></a>schtasks 刪除

刪除已排程的工作。

### <a name="syntax"></a>語法

```
schtasks /delete /tn {<TaskName> | *} [/f] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="parameters"></a>參數

|         詞彙          |                                                                                                                                                                 定義                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   /tn {\<TaskName >    |                                                                                                                                                                     \*}                                                                                                                                                                     |
|          /f           |                                                                                                                                  抑制確認訊息。 工作會在不發出警告的情況下刪除。                                                                                                                                  |
|    /s \<電腦 >     |                                                                                                           指定遠端電腦的名稱或 IP 位址（不含反斜線）。 預設是本機電腦。                                                                                                           |
| /u [\<Domain >\]<User> | 以指定之使用者帳戶的許可權執行此命令。 根據預設，此命令會以本機電腦目前使用者的許可權執行。</br>指定的使用者帳戶必須是遠端電腦上 Administrators 群組的成員。 只有當您使用 **/s**時， **/u**和 **/p**參數才有效。 |
|    /p \<密碼 >     |                          指定 **/u**參數中指定之使用者帳戶的密碼。 如果您使用 **/u**參數，但省略 **/p**參數或 password 引數，則**schtasks**會提示您輸入密碼。</br>只有當您使用 **/s**時， **/u**和 **/p**參數才有效。                           |
|          /?           |                                                                                                                                                    在命令提示字元顯示說明。                                                                                                                                                     |

### <a name="remarks"></a>備註

- 「**刪除**」作業會從排程中刪除工作。 它不會刪除工作執行的程式或中斷執行中的程式。
- **Delete \\** * 命令會刪除所有針對電腦排定的工作，而不只是目前使用者排程的工作。

### <a name="examples"></a>範例

### <a name="to-delete-a-task-from-the-schedule-of-a-remote-computer"></a>若要從遠端電腦的排程中刪除工作

下列命令會從遠端電腦的排程刪除 [啟動郵件] 工作。 它會使用 **/s**參數來識別遠端電腦。
```
schtasks /delete /tn Start Mail /s Svr16
```
在回應中， **schtasks.exe**會顯示下列確認訊息。 若要刪除工作，請按下 Y<strong>。</strong>若要取消命令，請輸入**n**：
```
WARNING: Are you sure you want to remove the task Start Mail (Y/N )? 
SUCCESS: The scheduled task Start Mail was successfully deleted.
```

### <a name="to-delete-all-tasks-scheduled-for-the-local-computer"></a>刪除本機電腦上排定的所有工作

下列命令會從本機電腦的排程中刪除所有工作，包括其他使用者所排程的作業。 它會使用 **/tn \\** * 參數來代表電腦上的所有工作，並使用 **/f**參數來抑制確認訊息。
```
schtasks /delete /tn * /f
```
在回應中， **schtasks.exe**會顯示下列成功訊息，指出已排程唯一的工作 SecureScript。

`SUCCESS: The scheduled task SecureScript was successfully deleted.`

## <a name="schtasks-query"></a><a name=BKMK_query></a>schtasks 查詢

顯示排程在電腦上執行的工作。

### <a name="syntax"></a>語法

```
schtasks [/query] [/fo {TABLE | LIST | CSV}] [/nh] [/v] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="parameters"></a>參數

|         詞彙          |                                                                                                                                                                 定義                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /query        |                                                                                                                        作業名稱是選擇性的。 輸入**schtasks**而不使用任何參數，會執行查詢。                                                                                                                         |
|      /fo {資料表       |                                                                                                                                                                    LIST                                                                                                                                                                     |
|          /nh          |                                                                                                            省略資料表顯示中的資料行標題。 此參數適用于**資料表**和**CSV**輸出格式。                                                                                                             |
|          /v           |                                                                                                         將工作的「先進屬性」新增至顯示畫面。</br>使用 **/v**的查詢應格式化為**LIST**或**CSV**。                                                                                                          |
|    /s \<電腦 >     |                                                                                                           指定遠端電腦的名稱或 IP 位址（不含反斜線）。 預設是本機電腦。                                                                                                           |
| /u [\<Domain >\]<User> | 以指定之使用者帳戶的許可權執行此命令。 根據預設，此命令會以本機電腦目前使用者的許可權執行。</br>指定的使用者帳戶必須是遠端電腦上 Administrators 群組的成員。 只有當您使用 **/s**時， **/u**和 **/p**參數才有效。 |
|    /p \<密碼 >     |                                        指定 **/u**參數中指定之使用者帳戶的密碼。 如果您使用 **/u**，但省略 **/p**或 password 引數，則**schtasks**會提示您輸入密碼。</br>只有當您使用 **/s**時， **/u**和 **/p**參數才有效。                                         |
|          /?           |                                                                                                                                                    在命令提示字元顯示說明。                                                                                                                                                     |

### <a name="remarks"></a>備註

**Schtasks.exe**只會結束已排程工作啟動之程式的實例。 若要停止其他進程，請使用 TaskKill。 如需詳細資訊，請參閱[Taskkill](taskkill.md)。

### <a name="examples"></a>範例

### <a name="to-display-the-scheduled-tasks-on-the-local-computer"></a>顯示本機電腦上的排程工作

下列命令會顯示針對本機電腦排定的所有工作。 這些命令會產生相同的結果，並可交替使用。
```
schtasks
schtasks /query
```
在回應中， **schtasks.exe**會以預設的簡單資料表格式顯示工作，如下表所示：
```
TaskName Next Run Time Status
========================= ======================== ==============
Microsoft Outlook At logon time
SecureScript 14:42:00 PM , 2/4/2001
```

### <a name="to-display-advanced-properties-of-scheduled-tasks"></a>若要顯示已排程工作的 advanced 屬性

下列命令會要求在本機電腦上執行工作的詳細顯示。 它會使用 **/v**參數來要求詳細的（詳細資訊）顯示和 **/fo list**參數，將顯示格式設定為清單以方便閱讀。 您可以使用此命令來確認您建立的工作具有預定的迴圈模式。

**schtasks/query/fo LIST/v**

在回應中， **schtasks.exe**會顯示所有工作的詳細屬性清單。 以下顯示的工作清單會顯示已排定在上午4:00 執行的任務。 在每個月的最後一個星期五：
```
HostName: RESKIT01
TaskName: SecureScript
Next Run Time: 4:00:00 AM , 3/30/2001
Status: Not yet run
Logon mode: Interactive/Background
Last Run Time: Never
Last Result: 0
Creator: user01
Schedule: At 4:00 AM on the last Fri of every month, starting 3/24/2001
Task To Run: C:\WINDOWS\system32\notepad.exe
Start In: notepad.exe
Comment: N/A
Scheduled Task State: Enabled
Scheduled Type: Monthly
Modifier: Last FRIDAY
Start Time: 4:00:00 AM
Start Date: 3/24/2001
End Date: N/A
Days: FRIDAY
Months: JAN,FEB,MAR,APR,MAY,JUN,JUL,AUG,SEP,OCT,NOV,DEC
Run As User: RESKIT\user01
Delete Task If Not Rescheduled: Enabled
Stop Task If Runs X Hours and X Mins: 72:0
Repeat: Until Time: Disabled
Repeat: Duration: Disabled
Repeat: Stop If Still Running: Disabled
Idle: Start Time(For IDLE Scheduled Type): Disabled
Idle: Only Start If Idle for X Minutes: Disabled
Idle: If Not Idle Retry For X Minutes: Disabled
Idle: Stop Task If Idle State End: Disabled
Power Mgmt: No Start On Batteries: Disabled
Power Mgmt: Stop On Battery Mode: Disabled
```

### <a name="to-log-tasks-scheduled-for-a-remote-computer"></a>記錄遠端電腦的排程工作

下列命令會要求針對遠端電腦排程的工作清單，並將工作新增至本機電腦上以逗號分隔的記錄檔。 您可以使用此命令格式來收集和追蹤針對多部電腦排程的工作。

此命令會使用 **/s**參數來識別遠端電腦 Reskit16、指定格式的 **/fo**參數，以及用來隱藏欄位標題的 **/nh**參數。 **>>** 附加符號會將輸出重新導向至本機電腦上的工作記錄檔 P0102，woodgrove-svr01。 因為此命令是在遠端電腦上執行，所以本機電腦路徑必須是完整的。
```
schtasks /query /s Reskit16 /fo csv /nh >> \\svr01\data\tasklogs\p0102.csv
```
在回應中， **schtasks.exe**會將針對 Reskit16 電腦排程的工作新增至本機電腦 woodgrove-svr01 上的 p0102。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
