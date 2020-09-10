---
title: schtasks
description: '* * * * 的參考文章'
ms.topic: reference
ms.assetid: 2e713203-3dd8-491b-b9e1-9423618dc7e8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9710e0b8156ae6a09302c3ffdeaea3e48e7953d1
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637032"
---
# <a name="schtasks"></a>schtasks



排程要定期或特定時間執行的命令和程式。 新增和移除排程中的工作、啟動和停止隨選工作，以及顯示和變更排程工作。

若要查看命令語法，請按一下下列其中一個命令：
-   [schtasks 建立](#BKMK_create)
-   [schtasks 變更](#BKMK_change)
-   [schtasks 執行](#BKMK_run)
-   [schtasks end](#BKMK_end)
-   [schtasks 刪除](#BKMK_delete)
-   [schtasks 查詢](#BKMK_query)

## <a name="remarks"></a>備註

- **SchTasks.exe**會在**主控台**中執行與**排程**工作相同的作業。 您可以搭配使用這些工具，也可以交替使用。
- **Schtasks** 會取代 **At.exe**，這是舊版 Windows 中包含的工具。 雖然 Windows Server 2003 系列中仍包含 **At.exe** ，但仍建議使用 **schtasks.exe** 命令列工作排程工具。
- **Schtasks**命令中的參數可以依任何順序出現。 在不使用任何參數的情況下輸入 **schtasks.exe** 會執行查詢。
- **Schtasks**的許可權
  -   您必須擁有執行命令的許可權。 任何使用者都可以在本機電腦上排程工作，並且可以查看和變更其排定的工作。 Administrators 群組的成員可以排程、查看及變更本機電腦上的所有工作。
  -   若要在遠端電腦上排程、查看或變更工作，您必須是遠端電腦上 Administrators 群組的成員，或者您必須使用 **/u** 參數提供遠端電腦的系統管理員認證。
  -   只有當本機和遠端電腦位於相同網域，或本機電腦位於遠端電腦網域信任的網域中時，您才可以在 **/create**或 **/change**操作中使用 **/u**參數。 否則，遠端電腦無法驗證指定的使用者帳戶，也無法確認該帳戶是否為系統管理員群組的成員。
  -   工作必須具有執行的許可權。 所需的許可權會因工作而異。 根據預設，工作會以本機電腦目前使用者的許可權執行，或以 **/u** 參數所指定之使用者的許可權執行（如果包含的話）。 若要以不同使用者帳戶的許可權或系統許可權來執行工作，請使用 **/ru** 參數。
- 若要確認已排程的工作已執行，或找出排程工作未執行的原因，請參閱工作排程器 service 交易記錄檔、 *SystemRoot*\SchedLgU.txt。 此記錄會記錄所有使用此服務的工具所起始的執行，包括 **排程** 的工作和 **SchTasks.exe**。
- 在罕見的情況下，工作檔案會損毀。 損毀的工作無法執行。 當您嘗試在損毀的工作上執行作業時， **SchTasks.exe** 會顯示下列錯誤訊息：
  ```
  ERROR: The data is invalid.
  ```
  您無法復原損毀的工作。 若要還原系統的工作排程功能，請使用 **SchTasks.exe** 或 **排程** 工作來刪除系統中的工作並重新排程。

## <a name="schtasks-create"></a><a name=BKMK_create></a>schtasks 建立

排程工作。

**Schtasks** 針對每個排程類型使用不同的參數組合。 若要查看建立工作的組合語法，或查看建立具有特定排程類型之工作的語法，請按一下下列其中一個選項。
-   [結合的語法和參數描述](#BKMK_syntax)
-   [排程每隔 N 分鐘執行一次的工作](#BKMK_minutes)
-   [排程每隔 N 小時執行一次的工作](#BKMK_hours)
-   [排程每隔 N 天執行一次的工作](#BKMK_days)
-   [排定每隔 N 周執行一次的工作](#BKMK_weeks)
-   [排程每隔 N 個月執行一次的工作](#BKMK_months)
-   [排定在一周的特定一天執行的工作](#BKMK_spec_day)
-   [排定在每月特定周執行的工作](#BKMK_spec_week)
-   [排定每個月于特定日期執行的工作](#BKMK_spec_date)
-   [排程在一個月的最後一天執行的工作](#BKMK_last_day)
-   [排程執行一次的工作](#BKMK_once)
-   [排定在每次系統啟動時執行的工作](#BKMK_startup)
-   [排程使用者登入時執行的工作](#BKMK_logon)
-   [排定在系統閒置時執行的工作](#BKMK_idle)
-   [排定立即執行的工作](#BKMK_now)
-   [若要排程以不同許可權執行的工作](#BKMK_diff_perms)
-   [若要排程以系統許可權執行的工作](#BKMK_sys_perms)
-   [若要排程執行多個程式的工作](#BKMK_multi_progs)
-   [排程在遠端電腦上執行的工作](#BKMK_remote)

### <a name="combined-syntax-and-parameter-descriptions"></a><a name=BKMK_syntax></a>結合的語法和參數描述

#### <a name="syntax"></a>語法

```
schtasks /create /sc <ScheduleType> /tn <TaskName> /tr <TaskRun> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]] [/ru {[<Domain>\]<User> | System}] [/rp <Password>] [/mo <Modifier>] [/d <Day>[,<Day>...] | *] [/m <Month>[,<Month>...]] [/i <IdleTime>] [/st <StartTime>] [/ri <Interval>] [{/et <EndTime> | /du <Duration>} [/k]] [/sd <StartDate>] [/ed <EndDate>] [/it] [/z] [/f]
```

##### <a name="parameters"></a>參數

##### <a name="sc-scheduletype"></a>/sc \<ScheduleType>

指定排程類型。 有效的值為 MINUTE、每小時、每天、每週、每月、一次、ONSTART、整理、ONIDLE。

|排程類型|描述|
|-------------|-----------|
|分鐘、每小時、每日、每週、每月|指定排程的時間單位。|
|一旦|工作會在指定的日期和時間執行一次。|
|ONSTART|每次系統啟動時，就會執行此工作。 您可以指定開始日期，或在下次系統啟動時執行工作。|
|整理|每當使用者 (任何使用者) 登入時，就會執行此工作。 您可以指定日期，或在使用者下一次登入時執行工作。|
|ONIDLE|只要系統閒置一段指定的時間，就會執行此工作。 您可以指定日期，或在下次系統閒置時執行工作。|

##### <a name="tn-taskname"></a>/tn \<TaskName>

指定工作的名稱。 系統上的每個工作都必須有唯一的名稱。 名稱必須符合檔案名的規則，而且不得超過238個字元。 使用引號來括住包含空格的名稱。

##### <a name="tr-taskrun"></a>/tr \<TaskRun>

指定工作執行的程式或命令。 輸入可執行檔、指令檔或批次檔的完整路徑和檔案名。 路徑名稱不能超過262個字元。 如果您省略路徑， **schtasks** 會假設檔案位於 *SystemRoot*\System32 目錄中。

##### <a name="s-computer"></a>/s \<Computer>

在指定的遠端電腦上排定工作。 輸入遠端電腦的名稱或 IP 位址 (包含或不含反斜線) 。 預設是本機電腦。 只有當您使用 **/s**時， **/u**和 **/p**參數才有效。

##### <a name="u-domainuser"></a>u-sql\<Domain>\]<User>

使用指定之使用者帳戶的許可權來執行此命令。 預設值是本機電腦目前使用者的許可權。 **/U**和 **/p**參數只適用于在遠端電腦上排程工作 (**/s**) 。

指定之帳戶的許可權是用來排程工作和執行工作。 若要使用不同使用者的許可權來執行工作，請使用 **/ru** 參數。

使用者帳戶必須是遠端電腦上 Administrators 群組的成員。 此外，本機電腦必須位於與遠端電腦相同的網域中，或者必須位於遠端電腦網域信任的網域中。

##### <a name="p-password"></a>/p \<Password>

提供 **/u** 參數中所指定之使用者帳戶的密碼。 如果您使用 **/u** 參數，但省略 **/p** 參數或 password 引數，則 **schtasks.exe** 會提示您輸入密碼，並遮蔽您輸入的文字。

**/U**和 **/p**參數只適用于在遠端電腦上排程工作 (**/s**) 。

##### <a name="ru-domainuser--system"></a>/ru {[ \<Domain> \] <User> |系統

使用指定之使用者帳戶的許可權來執行工作。 根據預設，工作會以本機電腦目前使用者的許可權執行，或以 **/u** 參數所指定之使用者的許可權執行（如果包含的話）。 在本機或遠端電腦上排程工作時， **/ru** 參數是有效的。


|       值        |                                                    描述                                                    |
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| [\<Domain>\]<User> |                                       指定替代的使用者帳戶。                                        |
|    系統或     | 指定本機系統帳戶，這是作業系統和系統服務所使用的高度特殊許可權帳戶。 |

##### <a name="rp-password"></a>/rp \<Password>

提供在 **/ru** 參數中指定之使用者帳戶的密碼。 如果您在指定使用者帳戶時省略此參數， **SchTasks.exe** 會提示您輸入密碼，並遮蔽您輸入的文字。

請勿針對以系統帳號憑證執行的工作使用 **/rp** 參數 (**/ru 系統**) 。 系統帳戶沒有密碼，且 **SchTasks.exe** 不會提示您輸入密碼。

##### <a name="mo-modifier"></a>/mo \<Modifier>

指定工作在其排程類型內執行的頻率。 此參數有效，但可以是選擇性、每小時、每日、每週和每月排程。 預設值為 1。

|排程類型|修飾元值|描述|
|-------------|---------------|-----------|
|MINUTE|1 - 1439|此工作會每分鐘執行一次 \<N> 。|
|小時|1 - 23|每小時執行一次此工作 \<N> 。|
|每天|1 - 365|此工作會每隔一 \<N> 天執行。|
|每週|1 - 52|此工作會每週執行一次 \<N> 。|
|一旦|無修飾詞。|此工作會執行一次。|
|ONSTART|無修飾詞。|工作會在啟動時執行。|
|整理|無修飾詞。|當 **/u** 參數指定的使用者登入時，就會執行此工作。|
|ONIDLE|無修飾詞。|當系統閒置了 **/i** 參數所指定的分鐘數之後，此工作就會執行，這是搭配 ONIDLE 使用的必要分鐘數。|
|每月|1 - 12|此工作會每個月執行一次 \<N> 。|
|每月|LASTDAY|工作會在當月的最後一天執行。|
|每月|第一、第二、第三、第四、最後|使用與 **/d** \<Day> 參數，在特定周和日執行工作。 例如，在當月的第三個星期三。|

##### <a name="d-dayday--"></a>/d Day [，Day ...] |*

指定一天 (或一天的) ，或一天 (或一天) 的月份。 只適用于每週或每月排程。


| 排程類型 |              修飾詞              |      (/d) 的日值      |                                                                                                 描述                                                                                                 |
|---------------|------------------------------------|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    每週     |               1 - 52               | 星期一-周日 [，週一至周日 ...] |                                                                                                     \*                                                                                                      |
|    每月    | 第一、第二、第三、第四、最後 |        星期一-周日         |                                                                                   特定周排程所需。                                                                                    |
|    每月    |          無或 {1-12}          |          1 - 31          | 選擇性且僅適用于 no 修飾詞 (**/mo**) 參數 (特定日期排程) 或當 **/mo** 為 1-12 (每 \<N> 月排程) 。 預設值為第1天 () 月份的第一天。 |

##### <a name="m-monthmonth"></a>/m Month [，Month ...]

指定排程工作應該在一年中執行的月份或月份。 有效值為 JAN-DEC 和 * (每個月) 。 **/M**參數只適用于每月排程。 使用 LASTDAY 修飾詞時，這是必要的。 否則，它是選擇性的，預設值為 * (每個月) 。

##### <a name="i-idletime"></a>/i \<IdleTime>

指定工作啟動前的電腦閒置的分鐘數。 有效的值是從1到999的整數。 此參數僅適用于 ONIDLE 排程，然後是必要的。

##### <a name="st-starttime"></a>/st \<StartTime>

指定工作開始 (每次啟動時的時間（以24小時制的) ） \<HH:MM> 。 預設值是本機電腦上的目前時間。 **/St**參數適用于分鐘、每小時、每日、每週、每月和一次排程。 這是一次排程的必要項。

##### <a name="ri-interval"></a>/ri \<Interval>

指定重複間隔（以分鐘為單位）。 這不適用於排程類型： MINUTE、每小時、ONSTART、整理和 ONIDLE。 有效範圍是1到599940分鐘 (599940 分鐘 = 9999 小時) 。 如果指定了/ET 或/DU，則重複間隔預設為10分鐘。

##### <a name="et-endtime"></a>/et \<EndTime>

指定以24小時制的時間，以分鐘或小時工作排程結束的時間 \<HH:MM> 。 在指定的結束時間之後， **schtasks.exe** 就不會重新開機工作，直到開始時間重複。 依預設，工作排程沒有結束時間。 此參數是選擇性的，且僅適用于分鐘或小時的排程。

如需範例，請參閱：
-   若要排程每隔100分鐘執行一次的工作，**以排程每分鐘執行一次的工作** \<N> **minutes** 。

##### <a name="du-duration"></a>/du \<Duration>

以24小時的格式指定分鐘或每小時排程的最大時間長度 \<HHHH:MM> 。 經過指定的時間之後， **schtasks.exe** 就不會重新開機工作，直到開始時間重複。 依預設，工作排程沒有最大持續時間。 此參數是選擇性的，且僅適用于分鐘或小時的排程。

如需範例，請參閱：
-   若要排程每隔3小時執行一次的工作，**以排定每個小時執行一次的工作** \<N> **hours** 。

##### <a name="k"></a>/k

停止此工作在 **/et** 或 **/du**指定的時間執行的程式。 如果 **/k**沒有/k **，在**它到達 **/et**或 **/du**所指定的時間之後，就不會重新開機程式，但如果程式仍在執行中，則不會停止程式。 此參數是選擇性的，且僅適用于分鐘或小時的排程。

如需範例，請參閱：
-   若要排程每隔100分鐘執行一次的工作，**以排程每分鐘執行一次的工作** \<N> **minutes** 。

##### <a name="sd-startdate"></a>/sd \<StartDate>

指定工作排程的開始日期。 預設值是本機電腦上的目前日期。 **/Sd**參數是有效的，而且所有排程類型都是選擇性的。

[開始使用] 的格式會隨著**主控台**的 [**地區及語言選項**] 中為本機電腦所選取的地區設定*而有所不同。* 每個地區設定只有一個有效的格式。

有效的日期格式如下表所示。 在本機電腦上**主控台**的 [地區及語言選項] 中，使用 [**地區及語言選項** **] 中選取**的格式最類似的格式。


|       值       |                                        描述                                         |
|-------------------|--------------------------------------------------------------------------------------------|
| \<MM>/<DD>/<YYYY> | 使用的月份優先格式，例如 **英文 (美國) ** 和 **西班牙文 (巴拿馬) **。 |
| \<DD>/<MM>/<YYYY> |       使用於第一天格式，例如 **保加利亞** 文和 **荷蘭文 (荷蘭) **。        |
| \<YYYY>/<MM>/<DD> |          用於年份優先的格式，例如 **瑞典** 文和 **法文 (加拿大) **。          |

/ed \<EndDate>

指定排程結束的日期。 這是選擇性參數。 它在一次、ONSTART、整理或 ONIDLE 排程中是不正確。 依預設，排程沒有結束日期。

[*結束*日期] 的格式會隨著**主控台**的 [**地區及語言選項**] 中為本機電腦所選取的地區設定而有所不同。 每個地區設定只有一個有效的格式。

有效的日期格式如下表所示。 在本機電腦上**主控台**的 [地區及語言選項] 中，使用 [**地區及語言選項** **] 中選取**的格式最類似的格式。


|       值       |                                        描述                                         |
|-------------------|--------------------------------------------------------------------------------------------|
| \<MM>/<DD>/<YYYY> | 使用的月份優先格式，例如 **英文 (美國) ** 和 **西班牙文 (巴拿馬) **。 |
| \<DD>/<MM>/<YYYY> |       使用於第一天格式，例如 **保加利亞** 文和 **荷蘭文 (荷蘭) **。        |
| \<YYYY>/<MM>/<DD> |          用於年份優先的格式，例如 **瑞典** 文和 **法文 (加拿大) **。          |

##### <a name="it"></a>/it

指定只有當 [執行身分] 使用者 (執行工作的使用者帳戶) 登入電腦時才執行工作。 此參數不會影響以系統許可權執行的工作。

依預設，「執行身份」使用者是排定工作時的本機電腦目前使用者，或是由 **/u** 參數指定的帳號（如果使用的話）。 但是，如果命令包含 **/ru** 參數，則 run as 使用者就是 **/ru** 參數所指定的帳號。

如需範例，請參閱：
-   若要排程每隔70天執行一次的工作，如果我在中登入， **以排程每隔** *N* **天** 執行一次的工作。
-   只有當特定使用者登入時才執行工作， **以排程使用不同許可權區段執行的工作** 。

##### <a name="z"></a>/z

指定在完成排程時刪除工作。

##### <a name="f"></a>/f

指定要建立工作，並在指定的工作已經存在時隱藏警告。

##### <a name=""></a>/?

在命令提示字元顯示說明。

### <a name="to-schedule-a-task-that-runs-every-n-minutes"></a><a name=BKMK_minutes></a>排程每隔 N 分鐘執行一次的工作

#### <a name="minute-schedule-syntax"></a>分鐘排程語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc minute [/mo {1 - 1439}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [{/et <HH:MM> | /du <HHHH:MM>} [/k]] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在分鐘的排程中，需要 **/sc 分鐘** 的參數。 **/Mo** (修飾詞) 參數是選擇性的，並指定每次執行工作之間的分鐘數。 **/Mo**的預設值是每分鐘的 1 () 。 **/Et** (結束時間) 和 **/du** (duration) 參數是選擇性的，而且可以搭配或不使用 **/k** (end 工作) 參數使用。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-every-20-minutes"></a>排程每隔20分鐘執行一次的工作

下列命令會排定每隔20分鐘執行一次安全性腳本（vbs）。 此命令會使用 **/sc** 參數來指定分鐘的排程，並使用 **/mo** 參數指定20分鐘的間隔。

由於此命令不包含開始日期或時間，因此工作會在命令完成後的20分鐘內開始執行，並在每隔20分鐘執行一次系統。 請注意，安全性腳本來源檔案位於遠端電腦上，但工作已排程並在本機電腦上執行。
```
schtasks /create /sc minute /mo 20 /tn Security Script /tr \\central\data\scripts\sec.vbs
```

#### <a name="to-schedule-a-task-that-runs-every-100-minutes-during-non-business-hours"></a>排定在非上班時間每隔100分鐘執行一次的工作

下列命令會排定在本機電腦上每隔100分鐘執行一次安全腳本（vbs），以在 5:00 P.M. 之間執行 和上午7:59 每天。 此命令會使用 **/sc** 參數來指定分鐘的排程，並使用 **/mo** 參數指定100分鐘的間隔。 它會使用 **/st** 和 **/et** 參數來指定每天排程的開始時間和結束時間。 如果腳本仍在上午7:59 執行，它也會使用 **/k** 參數來停止腳本。 在沒有 **/k**的情況下，如果實例是在上午 6:20 **開始，則不會在** 上午7:59 之後啟動腳本。 仍在執行，不會將它停止。
```
schtasks /create /tn Security Script /tr sec.vbs /sc minute /mo 100 /st 17:00 /et 08:00 /k
```

### <a name="to-schedule-a-task-that-runs-every-n-hours"></a><a name=BKMK_hours></a>排程每隔 N 小時執行一次的工作

#### <a name="hourly-schedule-syntax"></a>每小時排程語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc hourly [/mo {1 - 23}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [{/et <HH:MM> | /du <HHHH:MM>} [/k]] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

每小時的排程都需要 **/sc 每小時** 參數。 **/Mo** (修飾詞) 參數是選擇性的，並指定每次執行工作之間的時數。 **/Mo**的預設值是每小時 (1) 。 **/K** (end task) 參數是選擇性的，而且可以在指定的時間使用 **/et** (end) 或 **/du** (指定的間隔) 之後結束。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-every-five-hours"></a>排定每隔五個小時執行一次的工作

下列命令會排程在2002年3月的第一天開始每隔五個小時執行一次 MyApp 程式。 它會使用 **/mo** 參數指定間隔，並使用 **/sd** 參數指定開始日期。 因為此命令不會指定開始時間，所以會使用目前的時間作為開始時間。

因為本機電腦是設定為使用英文版中的 [**地區及語言****主控台**選項] 中的**英文 (辛巴威) **選項，所以開始日期的格式為 MM/DD/YYYY (03/01/2002) 。
```
schtasks /create /sc hourly /mo 5 /sd 03/01/2002 /tn My App /tr c:\apps\myapp.exe
```

#### <a name="to-schedule-a-task-that-runs-every-hour-at-five-minutes-past-the-hour"></a>排定在一小時內的五分鐘執行一次的工作

下列命令會將 MyApp 程式排程為每小時執行一次，從午夜之前的五分鐘開始。 因為省略了 **/mo** 參數，所以命令會使用每小時排程的預設值，也就是每 (1) 小時。 如果這個命令在上午12:05 之後執行，程式就不會執行到下一天。
```
schtasks /create /sc hourly /st 00:05 /tn My App /tr c:\apps\myapp.exe
```

#### <a name="to-schedule-a-task-that-runs-every-3-hours-for-10-hours"></a>排程每3小時執行一次的工作10小時

下列命令會將 MyApp 程式排程為每3小時執行10小時。

此命令會使用 **/sc** 參數指定每小時排程，並使用 **/mo** 參數指定3小時的間隔。 它會使用 **/st** 參數在午夜開始排程，而 **/du** 參數則會在10小時之後結束週期。 由於程式只會執行幾分鐘的時間，因此不需要使用 **/k** 參數，它會在持續時間過期時停止程式（如果它仍在執行中）。
```
schtasks /create /tn My App /tr myapp.exe /sc hourly /mo 3 /st 00:00 /du 0010:00
```
在此範例中，工作會在上午12:00 點、3:00 A.M.、上午6:00 和上午9:00 執行。 因為持續時間為10小時，所以不會在下午12:00 執行該工作。 相反地，它會在上午12:00 開始。 下一天。

### <a name="to-schedule-a-task-that-runs-every-n-days"></a><a name=BKMK_days></a>排程每隔 N 天執行一次的工作

#### <a name="daily-schedule-syntax"></a>每日排程語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc daily [/mo {1 - 365}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在每日排程中，需要 **/sc 每日** 參數。 **/Mo** (修飾詞) 參數是選擇性的，並指定每次執行工作之間的天數。 **/Mo**的預設值是每一天)  (1。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-every-day"></a>排定每天執行的工作

若要排程 MyApp 程式每天執行一次，每天上午8:00 執行一次。 到2002年12月31日為止。 因為它省略了 **/mo** 參數，所以預設間隔1會用來執行每日的命令。

在此範例中，因為本機電腦系統在**主控台**的 [**地區及語言選項**] 中設定為**英文 (英國) **選項，結束日期的格式為 DD/MM/YYYY (31/12/2002) 
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc daily /st 08:00 /ed 31/12/2002
```

#### <a name="to-schedule-a-task-that-runs-every-12-days"></a>排定每隔12天執行一次的工作

將 MyApp 程式排程在下午1:00 的每十二天執行 自2002年12月31日起， (13:00) 。 命令使用 **/mo** 參數指定兩 (2) 天的間隔，以及 **/sd** 和 **/st** 參數指定日期和時間。

在此範例中，由於系統設定為 [**主控台**中**地區和語言選項**的**英文 (辛巴威) **選項，因此結束日期的格式為 MM/DD/YYYY (12/31/2002) 
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc daily /mo 12 /sd 12/31/2002 /st 13:00
```

#### <a name="to-schedule-a-task-that-runs-every-70-days-if-i-am-logged-on"></a>排定每隔70天執行一次的工作（如果我已登入）

下列命令會排定每隔70天執行一次的安全性腳本（vbs）。 此命令會使用 **/mo** 參數指定70天的間隔。 它也會使用 **/it** 參數，指定只有當工作執行所在帳戶的使用者登入電腦時，才會執行此工作。 由於工作會以我的使用者帳戶許可權執行，因此工作只會在我登入時執行。
```
schtasks /create /tn Security Script /tr sec.vbs /sc daily /mo 70 /it
```

> [!NOTE]
> 若要使用僅限互動式的 (**/it**) 屬性來識別工作，請使用詳細資訊查詢 ** (/query/v**) 。 在使用 **/it**工作的詳細資訊查詢顯示中，[ **登入模式]** 欄位的值為 [ **僅限互動**]。

### <a name="to-schedule-a-task-that-runs-every-n-weeks"></a><a name=BKMK_weeks></a>排定每隔 N 周執行一次的工作

#### <a name="weekly-schedule-syntax"></a>每週排程語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc weekly [/mo {1 - 52}] [/d {<MON - SUN>[,MON - SUN...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在每週排程中，需要 **/sc 每週** 的參數。 **/Mo** (修飾詞) 參數是選擇性的，並指定每次執行工作之間的周數。 **/Mo**的預設值是每週的 1 () 。

每週排程也有選擇性的 **/d** 參數，可將工作排程在一周指定的日子執行，或在所有天 (*) 。預設值為 Monday (星期一) 。[ * 每天] () 選項相當於排程每日工作。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-every-six-weeks"></a>排定每隔六周執行一次的工作

下列命令會將 MyApp 程式排定在每隔六周的遠端電腦上執行。 此命令會使用 **/mo** 參數指定間隔。 因為此命令會省略 **/d** 參數，所以工作會在星期一執行。

此命令也會使用 **/s** 參數指定遠端電腦，並使用 **/u** 參數以使用者的系統管理員帳戶許可權執行命令。 因為省略了 **/p** 參數， **SchTasks.exe** 會提示使用者提供系統管理員帳戶密碼。

此外，因為命令是從遠端執行，所以命令中的所有路徑（包括 MyApp.exe 的路徑）都會參考遠端電腦上的路徑。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /mo 6 /s Server16 /u Admin01
```

#### <a name="to-schedule-a-task-that-runs-every-other-week-on-friday"></a>排程在星期五每隔一周執行的工作

下列命令會將工作排程在每隔一個星期五執行。 它會使用 **/mo** 參數來指定兩周的間隔，並使用 **/d** 參數來指定一周中的哪一天。 若要排程每週五執行的工作，請省略 **/mo** 參數或將它設定為1。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /mo 2 /d FRI
```

### <a name="to-schedule-a-task-that-runs-every-n-months"></a><a name=BKMK_months></a>排程每隔 N 個月執行一次的工作

#### <a name="syntax"></a>語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly [/mo {1 - 12}] [/d {1 - 31}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在此排程類型中，需要 **/sc 每月** 參數。 **/Mo** (修飾詞) 參數，指定每次執行工作之間的月數，是選擇性的，而每個月的預設值是 1 () 。 此排程類型也有選擇性的 **/d** 參數，可將工作排程在該月的指定日期執行。 預設值為1， (月份的第一天) 。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-on-the-first-day-of-every-month"></a>排定在每個月的第一天執行的工作

下列命令會將 MyApp 程式排定在每個月的第一天執行。 因為值1是 **/mo** (修飾詞的預設值) 參數和 **/d** (day) 參數，所以這些參數會從命令中省略。
```
schtasks /create /tn My App /tr myapp.exe /sc monthly
```

#### <a name="to-schedule-a-task-that-runs-every-three-months"></a>排定每隔三個月執行一次的工作

下列命令會排定每隔三個月執行一次 MyApp 程式。 它會使用 **/mo** 參數指定間隔。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo 3
```

#### <a name="to-schedule-a-task-that-runs-at-midnight-on-the-21st-day-of-every-other-month"></a>排定在每個月的21天午夜執行的工作

下列命令會將 MyApp 程式排程在每個月21日的午夜執行。 此命令會指定這項工作應該執行一年，從2002年7月2日到2003年6月30日。

此命令會使用 **/mo** 參數指定每兩個月的每月間隔 () 、指定日期的 **/d** 參數，以及用來指定時間的 **/st** 。 它也會使用 **/sd** 和 **/ed** 參數分別指定開始日期和結束日期。 由於本機電腦是在**主控台**的 [**地區及語言選項**] 中設定為 [**英文 (南非]) **選項，因此會以當地格式（YYYY/MM/DD）來指定日期。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo 2 /d 21 /st 00:00 /sd 2002/07/01 /ed 2003/06/30
```

### <a name="to-schedule-a-task-that-runs-on-a-specific-day-of-the-week"></a><a name=BKMK_spec_day></a>排定在一周的特定一天執行的工作

#### <a name="weekly-schedule-syntax"></a>每週排程語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc weekly [/d {<MON - SUN>[,MON - SUN...] | *}] [/mo {1 - 52}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

每週排程的日期是每週排程的變化。 在每週排程中，需要 **/sc 每週** 的參數。 **/Mo** (修飾詞) 參數是選擇性的，並指定每次執行工作之間的周數。 **/Mo**的預設值是每週的 1 () 。 **/D**參數是選擇性的，它會將工作排程在一周指定的日子執行，或在所有天 (\*) 。 預設值為 Monday (星期一) 。 [每日] 選項** (\* /d**) 相當於排程每日工作。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-every-wednesday"></a>排定每個星期三執行的工作

下列命令會將 MyApp 程式排定在每週的星期三執行。 此命令會使用 **/d** 參數來指定一周的哪一天。 因為命令省略了 **/mo** 參數，所以工作會每週執行一次。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /d WED
```

#### <a name="to-schedule-a-task-that-runs-every-eight-weeks-on-monday-and-friday"></a>排定在星期一和星期五每隔八周執行一次的工作

下列命令會將工作排定在每隔八周的星期一和星期五執行。 它使用 **/d** 參數指定 days 和 **/mo** 參數，以指定八周間隔。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /mo 8 /d MON,FRI
```

### <a name="to-schedule-a-task-that-runs-on-a-specific-week-of-the-month"></a><a name=BKMK_spec_week></a>排定在每月特定周執行的工作

#### <a name="specific-week-syntax"></a>特定周語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /mo {FIRST | SECOND | THIRD | FOURTH | LAST} /d MON - SUN [/m {JAN - DEC[,JAN - DEC...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在此排程類型中， **/sc 的每月** 參數、 **/mo** (修飾詞) 參數，以及 **/d** (day) 參數是必要參數。 **/Mo** (修飾詞) 參數指定工作執行的周。 **/D**參數指定一周的哪一天。  (您只能針對此排程類型指定一周中的一天。 ) 此排程也有選擇性的 **/m** (month) 參數，可讓您針對特定月份或每月 () 排程工作 \* 。 **/M**參數的預設值是每個月 (\*) 。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-for-the-second-sunday-of-every-month"></a>為每個月的第二個星期日排程工作

下列命令會將 MyApp 程式排定在每個月的第二個星期日執行。 它會使用 **/mo** 參數來指定月份的第二周，並使用 **/d** 參數指定日期。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo SECOND /d SUN
```

#### <a name="to-schedule-a-task-for-the-first-monday-in-march-and-september"></a>在3月和9月的前星期一排程工作

下列命令會將 MyApp 程式排程在3月和9月的前星期一執行。 它會使用 **/mo** 參數來指定月份的第一周，並使用 **/d** 參數指定日期。 它會使用 **/m** 參數來指定月份，並以逗號分隔 month 引數。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo FIRST /d MON /m MAR,SEP
```

### <a name="to-schedule-a-task-that-runs-on-a-specific-date-each-month"></a><a name=BKMK_spec_date></a>排定每個月于特定日期執行的工作

#### <a name="specific-date-syntax"></a>特定日期語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /d {1 - 31} [/m {JAN - DEC[,JAN - DEC...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在特定日期排程類型中，需要 **/sc 的每月** 參數和 **/d** (day) 參數。 **/D**參數指定月份的日期 (1-31) ，而不是一周的日期。 您只能在排程中指定一天。 **/Mo** (修飾詞) 參數對此排程類型無效。

**/M** (month) 參數是此排程類型的選擇性參數，預設值為每個月 (<em>) 。 **Schtasks</em> *不讓您針對在 **/m**參數指定的月份中不會發生的日期排程工作。 但是，如果省略 **/m** 參數，並針對每個月未出現的日期排程工作（例如31天），則工作不會在較短的月份執行。 若要在當月的最後一天排程工作，請使用 [最後一天] 排程類型。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-for-the-first-day-of-every-month"></a>在每個月的第一天排程工作

下列命令會將 MyApp 程式排定在每個月的第一天執行。 由於預設修飾詞為 none (沒有任何修飾詞) ，預設 day 是 day 1，而預設月份是每個月，此命令不需要任何額外的參數。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly
```

#### <a name="to-schedule-a-task-for-the-15th-days-of-may-and-june"></a>在5月15日和6月排程工作

下列命令會將 MyApp 程式排定在5月15日和6月15日下午3:00 執行。  (15:00) 。 它會使用 **/m** 參數指定日期和 **/m** 參數來指定月份。 它也會使用 **/st** 參數來指定開始時間。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /d 15 /m MAY,JUN /st 15:00
```

### <a name="to-schedule-a-task-that-runs-on-the-last-day-of-a-month"></a><a name=BKMK_last_day></a>排程在一個月的最後一天執行的工作

#### <a name="last-day-syntax"></a>Last day 語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /mo LASTDAY /m {JAN - DEC[,JAN - DEC...] | *} [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在最後一天的排程類型中， **/sc 的每月** 參數、 **/mo LASTDAY** (修飾詞) 參數，以及 **/m** (month) 參數是必要參數。 **/D** (day) 參數無效。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-for-the-last-day-of-every-month"></a>在每個月的最後一天排程工作

下列命令會將 MyApp 程式排定在每個月的最後一天執行。 它使用 **/mo** **參數指定** 最後一天，並使用萬用字元 ( * ) 指出程式每個月執行一次。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo lastday /m *
```

#### <a name="to-schedule-a-task-at-600-pm-on-the-last-days-of-february-and-march"></a>若要在下午6:00 排程工作 在二月和三月的最後一天

下列命令會將 MyApp 程式排程在二月和三月的最後一天（下午6:00）執行。 它會使用 **/mo** 參數指定最後一天、指定月份的 **/m** 參數，以及指定開始時間的 **/st** 參數。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo lastday /m FEB,MAR /st 18:00
```

### <a name="to-schedule-a-task-that-runs-once"></a><a name=BKMK_once></a>排程執行一次的工作

#### <a name="syntax"></a>語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc once /st <HH:MM> [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在 [執行一次] 排程類型中，需要 [ **/sc 一次** ] 參數。 需要指定工作執行時間的 **/st** 參數。 **/Sd**參數指定工作執行的日期，是選擇性的。 **/Mo** (修飾詞) 和 **/ed** (結束日期) 參數對此排程類型無效。

如果指定的日期和時間在過去，根據本機電腦的時間，則 [ **Schtasks** ] 不允許您排定工作執行一次。 若要排程在不同時區的遠端電腦上執行一次的工作，您必須在本機電腦上進行該日期和時間之前進行排程。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-one-time"></a>排程執行一次的工作

下列命令會將 MyApp 程式排程在2003年1月1日午夜執行。 它會使用 **/sc** 參數來指定排程類型，並使用 **/sd** 和 **st** 來指定日期和時間。

由於本機電腦會在**主控台**的 [**地區及語言選項**] 中使用**英文 (美國) **選項，因此開始日期的格式為 MM/DD/YYYY。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc once /sd 01/01/2003 /st 00:00
```

### <a name="to-schedule-a-task-that-runs-every-time-the-system-starts"></a><a name=BKMK_startup></a>排定在每次系統啟動時執行的工作

#### <a name="syntax"></a>語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onstart [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在啟動排程類型中，需要 **/sc onstart** 參數。 **/Sd** (開始日期) 參數是選擇性的，預設值是目前的日期。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-when-the-system-starts"></a>排定在系統啟動時執行的工作

下列命令會排程在每次系統啟動時執行 MyApp 程式，從2001年3月15日開始：

由於本機電腦是使用**主控台**中 [**地區及語言選項**] 的 [**英文 (美國) **選項，因此開始日期的格式為 MM/DD/YYYY。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc onstart /sd 03/15/2001
```

### <a name="to-schedule-a-task-that-runs-when-a-user-logs-on"></a><a name=BKMK_logon></a>排程使用者登入時執行的工作

#### <a name="syntax"></a>語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onlogon [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

[On logon schedule] 類型會排定每次使用者登入電腦時執行的工作。 在 [登入排程類型] 中，需要 **/sc 整理** 參數。 **/Sd** (開始日期) 參數是選擇性的，預設值是目前的日期。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-when-a-user-logs-on-to-a-remote-computer"></a>排定當使用者登入遠端電腦時執行的工作

下列命令會排程每次使用者 (任何使用者) 登入遠端電腦時，要執行的批次檔。 它會使用 **/s** 參數指定遠端電腦。 因為此命令為 remote，所以命令中的所有路徑（包括批次檔的路徑）都會參考遠端電腦上的路徑。
```
schtasks /create /tn Start Web Site /tr c:\myiis\webstart.bat /sc onlogon /s Server23
```

### <a name="to-schedule-a-task-that-runs-when-the-system-is-idle"></a><a name=BKMK_idle></a>排定在系統閒置時執行的工作

#### <a name="syntax"></a>語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onidle /i {1 - 999} [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

「閒置時間排程」類型會排定在 **/i** 參數指定的時間內沒有使用者活動時執行的工作。 在 [依閒置時間排程類型] 中，需要 **/sc onidle** 參數和 **/i** 參數。 **/Sd** (開始日期) 是選擇性的，預設值是目前的日期。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-whenever-the-computer-is-idle"></a>排定每次電腦閒置時執行的工作

下列命令會排定 MyApp 程式在電腦閒置時執行。 它使用必要的 **/i** 參數來指定電腦必須在工作開始之前的10分鐘內保持閒置。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc onidle /i 10
```

### <a name="to-schedule-a-task-that-runs-now"></a><a name=BKMK_now></a>排定立即執行的工作

**Schtasks** 沒有立即執行的選項，但是您可以建立一次執行一次並在幾分鐘內啟動的工作來模擬該選項。

#### <a name="syntax"></a>語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc once [/st <HH:MM>] /sd <MM/DD/YYYY> [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-a-few-minutes-from-now"></a>排定從現在開始執行幾分鐘的工作。

下列命令會將工作排程在2002年11月13日（下午2:18）執行一次 給電腦。

由於本機電腦是使用**主控台**中 [**地區及語言選項**] 的 [**英文 (美國) **選項，因此開始日期的格式為 MM/DD/YYYY。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc once /st 14:18 /sd 11/13/2002
```

### <a name="to-schedule-a-task-that-runs-with-different-permissions"></a><a name=BKMK_diff_perms></a>若要排程以不同許可權執行的工作

您可以排程在本機和遠端電腦上使用其他帳戶的許可權來執行所有類型的工作。 除了特定排程類型所需的參數之外，還需要 **/ru** 參數，而 **/rp** 參數是選擇性的。

#### <a name="examples"></a>範例

#### <a name="to-run-a-task-with-administrator-permissions-on-the-local-computer"></a>若要在本機電腦上以系統管理員許可權執行工作

下列命令會排定在本機電腦上執行 MyApp 程式。 它會使用 **/ru** 來指定工作應以使用者的系統管理員帳戶許可權執行 (Admin06) 。 在此範例中，工作已排程為每個星期二執行，但您可以使用任何排程類型來執行具有替代許可權的工作。
```
schtasks /create /tn My App /tr myapp.exe /sc weekly /d TUE /ru Admin06
```
在 [回應] 中， **SchTasks.exe** 會提示您輸入 Admin06 帳戶的 run as 密碼，然後顯示成功訊息。
```
Please enter the run as password for Admin06: ********
SUCCESS: The scheduled task My App has successfully been created.
```

#### <a name="to-run-a-task-with-alternate-permissions-on-a-remote-computer"></a>在遠端電腦上執行具有替代許可權的工作

下列命令會將 MyApp 程式排定在每四天的行銷電腦上執行。

此命令會使用 **/sc** 參數指定每日排程和 **/mo** 參數，以指定四天的間隔。

此命令會使用 **/s** 參數提供遠端電腦的名稱，並使用 **/u** 參數來指定具有在)  (Admin01 的遠端電腦上排程工作之許可權的帳戶。 它也會使用 **/ru** 參數來指定工作應以使用者的非系統管理員帳戶許可權執行， (在 Reskits 網域) 中 User01。 如果沒有 **/ru** 參數，工作會以 **/u**所指定帳戶的許可權來執行。
```
schtasks /create /tn My App /tr myapp.exe /sc daily /mo 4 /s Marketing /u Marketing\Admin01 /ru Reskits\User01
```
**Schtasks** 會先向 **/u** 參數所命名的使用者要求密碼 (來執行命令) 然後要求由 **/ru** 參數命名的使用者密碼 (，以執行工作) 。 驗證密碼之後， **schtasks.exe** 會顯示一則訊息，指出已排程工作。
```
Type the password for Marketing\Admin01:********

Please enter the run as password for Reskits\User01: ********

SUCCESS: The scheduled task My App has successfully been created.
```

#### <a name="to-run-a-task-only-when-a-particular-user-is-logged-on"></a>只有在特定使用者登入時才執行工作

下列命令會將 AdminCheck.exe 程式排程在每星期五上午4:00 執行的公用電腦上執行，但只有在電腦的系統管理員登入時才會執行。

此命令會使用 **/sc** 參數來指定每週排程、指定日期的 **/d** 參數，以及用來指定開始時間的 **/st** 參數。

此命令會使用 **/s** 參數提供遠端電腦的名稱，並使用 **/u** 參數來指定有許可權可在遠端電腦上排程工作的帳戶。 它也會使用 **/ru** 參數，將工作設定為以公用電腦系統管理員的許可權執行 (Public\Admin01) 和 **/it** 參數，指出只有在 Public\Admin01 帳戶登入時才會執行此工作。
```
schtasks /create /tn Check Admin /tr AdminCheck.exe /sc weekly /d FRI /st 04:00 /s Public /u Domain3\Admin06 /ru Public\Admin01 /it
```
**注意**
-   若要使用僅限互動式的 (**/it**) 屬性來識別工作，請使用詳細資訊查詢 ** (/query/v**) 。 在使用 **/it**工作的詳細資訊查詢顯示中，[ **登入模式]** 欄位的值為 [ **僅限互動**]。

### <a name="to-schedule-a-task-that-runs-with-system-permissions"></a><a name=BKMK_sys_perms></a>若要排程以系統許可權執行的工作

所有類型的工作都可以在本機和遠端電腦上以系統帳戶的許可權來執行。 除了特定排程類型所需的參數之外，還需要 **/ru 系統** (或 * */ru * * ) 參數，而 **/rp** 參數則無效。

**重要**
-   系統帳戶沒有互動式登入許可權。 使用者無法看到以系統許可權執行的程式或工作，或與其互動。
-   **/Ru**參數會決定工作執行時所使用的許可權，而不是用來排程工作的許可權。 只有系統管理員可以排程工作，不論是否有 **/ru** 參數的值。

**注意**

若要識別以系統許可權執行的工作，請使用詳細資訊查詢 (**/query** **/v**) 。 在顯示系統執行工作的詳細資訊查詢中，[ **執行為使用者** ] 欄位的值為 **NT AUTHORITY\SYSTEM** ，而 [ **登入模式]** 欄位的值 **只**是 [背景]。

#### <a name="examples"></a>範例

#### <a name="to-run-a-task-with-system-permissions"></a>以系統許可權執行工作

下列命令會將 MyApp 程式排程在本機電腦上，以系統帳戶的許可權執行。 在此範例中，工作會排程在每個月的第15天執行，但是您可以使用任何排程類型來執行具有系統許可權的工作。

此命令會使用 **/Ru 系統** 參數來指定系統安全性內容。 由於系統工作不使用密碼，因此會省略 **/rp** 參數。
```
schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /d 15 /ru System
```
在 [回應] 中， **SchTasks.exe** 會顯示參考用訊息和成功訊息。 它不會提示您輸入密碼。
```
INFO: The task will be created under user name (NT AUTHORITY\SYSTEM).
SUCCESS: The Scheduled task My App has successfully been created.
```

#### <a name="to-run-a-task-with-system-permissions-on-a-remote-computer"></a>在遠端電腦上執行具有系統許可權的工作

下列命令會將 MyApp 程式排程在每早上上午4:00 的 Finance01 電腦上執行。 具有系統許可權。

此命令會使用 **/tn** 參數來命名工作，並使用 **/tr** 參數來指定 MyApp 程式的遠端副本。 它會使用 **/sc** 參數來指定每日排程，但會省略 **/mo** 參數，因為每一天) 預設為 1 (。 它會使用 **/st** 參數來指定開始時間，也就是每天執行工作的時間。

此命令會使用 **/s** 參數提供遠端電腦的名稱，並使用 **/u** 參數來指定有許可權可在遠端電腦上排程工作的帳戶。 它也會使用 **/ru** 參數指定工作應該在系統帳戶下執行。 如果沒有 **/ru** 參數，工作會以 **/u**所指定帳戶的許可權來執行。
```
schtasks /create /tn My App /tr myapp.exe /sc daily /st 04:00 /s Finance01 /u Admin01 /ru System
```
**Schtasks** 要求由 **/u** 參數命名的使用者密碼，而在驗證密碼之後，會顯示一則訊息，指出工作已建立，且將以系統帳戶的許可權執行。
```
Type the password for Admin01:**********

INFO: The Schedule Task My App will be created under user name (NT AUTHORITY\
SYSTEM).
SUCCESS: The scheduled task My App has successfully been created.
```

### <a name="to-schedule-a-task-that-runs-more-than-one-program"></a><a name=BKMK_multi_progs></a>若要排程執行多個程式的工作

每項工作只會執行一個程式。 不過，您可以建立執行多個程式的批次檔，然後排程執行批次檔的工作。 下列程式示範此方法：
1. 建立可啟動您要執行之程式的批次檔。

   在此範例中，您會建立一個 ( # A0) 和系統監視器 ( # A1) 開始事件檢視器的批次檔。
   - 開啟文字編輯器，例如 [記事本]。
   - 輸入每個程式之可執行檔的名稱和完整路徑。 在此情況下，檔案包含下列語句。
     ```
     C:\Windows\System32\Eventvwr.exe
     C:\Windows\System32\Perfmon.exe
     ```
   - 將檔案儲存為 MyApps.bat。
2. 使用 **Schtasks.exe** 建立 MyApps.bat 執行的工作。

   下列命令會建立監視工作，每當有人登入時就會執行此工作。 它會使用 **/tn** 參數來命名工作，並使用 **/tr** 參數來執行 MyApps.bat。 它會使用 **/sc** 參數來指出整理排程類型和 **/ru** 參數，以使用使用者的系統管理員帳戶許可權來執行工作。
   ```
   schtasks /create /tn Monitor /tr C:\MyApps.bat /sc onlogon /ru Reskit\Administrator
   ```
   此命令的結果是，每當使用者登入電腦時，工作都會啟動事件檢視器和系統監視器。

### <a name="to-schedule-a-task-that-runs-on-a-remote-computer"></a><a name=BKMK_remote></a>排程在遠端電腦上執行的工作

若要將工作排程在遠端電腦上執行，您必須將工作新增至遠端電腦的排程。 您可以在遠端電腦上排程所有類型的工作，但必須符合下列條件。
-   您必須具有排程工作的許可權。 因此，您必須使用遠端電腦上 Administrators 群組成員的帳戶登入本機電腦，或者您必須使用 **/u** 參數來提供遠端電腦的系統管理員的認證。
-   只有當本機和遠端電腦位於相同網域，或本機電腦位於遠端電腦網域信任的網域中時，您才能使用 **/u** 參數。 否則，遠端電腦無法驗證指定的使用者帳戶，也無法確認該帳戶是否為系統管理員群組的成員。
-   工作必須具有足夠的許可權，才能在遠端電腦上執行。 所需的許可權會因工作而異。 根據預設，工作會以本機電腦目前使用者的許可權執行，或者，如果使用 **/u** 參數，工作會以 **/u** 參數所指定之帳戶的許可權執行。 不過，您可以使用 **/ru** 參數執行具有不同使用者帳戶許可權或系統許可權的工作。

#### <a name="examples"></a>範例

#### <a name="an-administrator-schedules-a-task-on-a-remote-computer"></a>系統管理員在遠端電腦上排定工作

下列命令會將 MyApp 程式排程在每隔10天立即啟動的 SRV01 遠端電腦上執行。 命令使用 **/s** 參數提供遠端電腦的名稱。 因為本機目前的使用者是遠端電腦的系統管理員，所以不需要提供替代許可權來排程工作的 **/u** 參數。

請注意，在遠端電腦上排程工作時，所有參數都會參考遠端電腦。 因此， **/tr** 參數所指定的可執行檔是指遠端電腦上 MyApp.exe 的複本。
```
schtasks /create /s SRV01 /tn My App /tr c:\program files\corpapps\myapp.exe /sc daily /mo 10
```
為了回應， **schtasks.exe** 會顯示成功訊息，指出已排程工作。

#### <a name="a-user-schedules-a-command-on-a-remote-computer-case-1"></a>使用者會在遠端電腦上排定命令 (案例 1) 

下列命令會將 MyApp 程式排程在每三小時的 SRV06 遠端電腦上執行。 因為排程工作需要系統管理員許可權，所以命令會使用 **/u** 和 **/p** 參數來提供使用者系統管理員帳戶的認證， (在 Reskits 網域) 中 Admin01。 根據預設，這些許可權也會用來執行工作。 不過，由於工作不需要系統管理員許可權即可執行，因此命令會包含 **/u** 和 **/rp** 參數以覆寫預設值，並在遠端電腦上以使用者的非系統管理員帳戶的許可權執行工作。
```
schtasks /create /s SRV06 /tn My App /tr c:\program files\corpapps\myapp.exe /sc hourly /mo 3 /u reskits\admin01 /p R43253@4$ /ru SRV06\user03 /rp MyFav!!Pswd
```
為了回應， **schtasks.exe** 會顯示成功訊息，指出已排程工作。

#### <a name="a-user-schedules-a-command-on-a-remote-computer-case-2"></a>使用者會在遠端電腦上排定命令 (案例 2) 

下列命令會排程 MyApp 程式在每個月的最後一天于 SRV02 遠端電腦上執行。 因為本機目前的使用者 (user03) 不是遠端電腦的系統管理員，所以命令會使用 **/u** 參數提供 Reskits 網域中 (Admin01 的使用者系統管理員帳號憑證) 。 系統管理員帳戶許可權將用來排程工作和執行工作。
```
schtasks /create /s SRV02 /tn My App /tr c:\program files\corpapps\myapp.exe /sc monthly /mo LASTDAY /m * /u reskits\admin01
```
因為命令未包含 **/p** (password) 參數，所以 **schtasks** 會提示您輸入密碼。 然後，它會顯示成功訊息，在此案例中會顯示警告。
```
Type the password for reskits\admin01:********

SUCCESS: The scheduled task My App has successfully been created.

WARNING: The Scheduled task My App has been created, but may not run because
the account information could not be set.
```
此警告表示遠端網域無法驗證 **/u** 參數所指定的帳號。 在此情況下，遠端網域無法驗證使用者帳戶，因為本機電腦不是遠端電腦網域所信任之網域的成員。 發生這種情況時，工作作業會出現在已排程工作的清單中，但工作實際上是空的，而且不會執行。

從詳細資訊查詢顯示的下列顯示會顯示該工作的問題。 請注意，在顯示畫面中，[**下一次執行時間]** 的值**永遠**不會，而且無法**從工作**排程器資料庫抓取 [以**使用者身份執行**] 的值。

如果此電腦是相同網域或受信任網域的成員，則工作會成功排程，並依指定執行。
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

-   若要使用不同使用者的許可權來執行 **/create** 命令，請使用 **/u** 參數。 **/U**參數只對遠端電腦上的排程工作有效。
-   若要查看更多的 **schtasks.exe/create** 範例，請輸入 **schtasks.exe/create/？** 。
-   若要排程以不同使用者的許可權執行的工作，請使用 **/ru** 參數。 **/Ru**參數對本機和遠端電腦上的工作有效。
-   若要使用 **/u** 參數，本機電腦必須位於與遠端電腦相同的網域中，或者必須位於遠端電腦網域信任的網域中。 否則，可能是未建立工作，或者工作作業是空的，而且工作不會執行。
-   除非您提供密碼，否則**schtasks.exe**一律會提示輸入密碼，即使您在本機電腦上使用目前的使用者帳戶排程工作也是如此。 這是適用于 **schtasks.exe**的正常行為。
-   **Schtasks** 不會驗證程式檔案位置或使用者帳戶密碼。 如果您未針對使用者帳戶輸入正確的檔案位置或正確的密碼，則會建立工作，但不會執行該工作。 此外，如果帳戶的密碼變更或過期，而您未變更工作中儲存的密碼，則工作不會執行。
-   系統帳戶沒有互動式登入許可權。 使用者看不到，而且無法與系統許可權執行的程式互動。
-   每項工作只會執行一個程式。 不過，您可以建立一個批次檔來啟動多個工作，然後排程執行批次檔的工作。
-   您可以在建立工作時立即進行測試。 使用「 **執行** 」作業來測試工作，然後檢查 SchedLgU.txt 檔 (*SystemRoot*\SchedLgU.txt) 是否有錯誤。

## <a name="schtasks-change"></a><a name=BKMK_change></a>schtasks 變更

變更工作的下列一或多個屬性。
-   工作執行的程式 (**/tr**) 。
-   工作執行時所使用的使用者帳戶 (**/ru**) 。
-   使用者帳戶的密碼 (**/rp**) 。
-   將僅限互動式屬性新增至工作 (**/it**) 。

### <a name="syntax"></a>語法

```
schtasks /change /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]] [/ru {[<Domain>\]<User> | System}] [/rp <Password>] [/tr <TaskRun>] [/st <StartTime>] [/ri <Interval>] [{/et <EndTime> | /du <Duration>} [/k]] [/sd <StartDate>] [/ed <EndDate>] [/{ENABLE | DISABLE}] [/it] [/z]
```

#### <a name="parameters"></a>參數

|          詞彙           |                                                                                                                                                                                                                                                                                                                                     定義                                                                                                                                                                                                                                                                                                                                      |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /tn \<TaskName>     |                                                                                                                                                                                                                                                                                                               識別要變更的工作。 輸入工作名稱。                                                                                                                                                                                                                                                                                                               |
|     /s \<Computer>      |                                                                                                                                                                                                                                                                               指定遠端電腦的名稱或 IP 位址 (包含或不含反斜線) 。 預設是本機電腦。                                                                                                                                                                                                                                                                               |
|  u-sql\<Domain>\]<User>  |                                                                                                                                                                 使用指定之使用者帳戶的許可權來執行此命令。 預設值是本機電腦目前使用者的許可權。 指定的使用者帳戶必須是遠端電腦上 Administrators 群組的成員。 **/U**和 **/p**參數只適用于在遠端電腦上變更工作 (**/s**) 。                                                                                                                                                                  |
|     /p \<Password>      |                                                                                                                                                                                              指定 **/u** 參數中指定之使用者帳戶的密碼。 如果您使用 **/u** 參數，但省略 **/p** 參數或 password 引數，則 **schtasks.exe** 會提示您輸入密碼。</br>只有當您使用 **/s**時， **/u**和 **/p**參數才有效。                                                                                                                                                                                               |
| /ru {[\<Domain>\]<User> |                                                                                                                                                                                                                                                                                                                                       系統                                                                                                                                                                                                                                                                                                                                       |
|     /rp \<Password>     |                                                                                                                                                                                                                                                 為現有的使用者帳戶或由 **/ru** 參數指定的使用者帳戶指定新密碼。 使用本機系統帳戶時，會忽略這個參數。                                                                                                                                                                                                                                                  |
|     /tr \<TaskRun>      |                                                                                                                                                                                  變更工作執行的程式。 輸入可執行檔、指令檔或批次檔的完整路徑和檔案名。 如果您省略路徑， **schtasks** 會假設檔案位於 \<systemroot> \System32 目錄中。 指定的程式會取代工作所執行的原始程式。                                                                                                                                                                                  |
|    /st \<Starttime>     |                                                                                                                                                                                                                                                              使用24小時制時間格式（HH： mm），指定工作的開始時間。 例如，14:30 的值相當於下午2:30 的12小時時間。                                                                                                                                                                                                                                                               |
|     /ri \<Interval>     |                                                                                                                                                                                                                                                                           指定排程工作的重複間隔（以分鐘為單位）。 有效範圍是 1-599940 (599940 分鐘 = 9999 小時) 。                                                                                                                                                                                                                                                                            |
|     /et \<EndTime>      |                                                                                                                                                                                                                                                               使用24小時制時間格式（HH： mm）來指定工作的結束時間。 例如，14:30 的值相當於下午2:30 的12小時時間。                                                                                                                                                                                                                                                                |
|     /du \<Duration>     |                                                                                                                                                                                                                                                                                                     指定在或上關閉工作 \<EndTime> <Duration> （如果有指定的話）。                                                                                                                                                                                                                                                                                                      |
|           /k            |                                                                                                                                                                   停止此工作在 **/et** 或 **/du**指定的時間執行的程式。 如果 **/k**沒有/k **，在**它到達 **/et**或 **/du**所指定的時間之後，就不會重新開機程式，但如果程式仍在執行中，則不會停止程式。 此參數是選擇性的，且僅適用于分鐘或小時的排程。                                                                                                                                                                   |
|    /sd \<StartDate>     |                                                                                                                                                                                                                                                                                              指定應執行工作的第一個日期。 日期格式為 MM/DD/YYYY。                                                                                                                                                                                                                                                                                               |
|     /ed \<EndDate>      |                                                                                                                                                                                                                                                                                                 指定執行工作的最後日期。 格式為 MM/DD/YYYY。                                                                                                                                                                                                                                                                                                  |
|         /ENABLE         |                                                                                                                                                                                                                                                                                                                       指定要啟用排定的工作。                                                                                                                                                                                                                                                                                                                       |
|        /DISABLE         |                                                                                                                                                                                                                                                                                                                      指定要停用排程工作。                                                                                                                                                                                                                                                                                                                       |
|           /it           | 指定只有當 [執行身分] 使用者 (執行工作的使用者帳戶) 登入電腦時，才執行排定的工作。</br>此參數不會影響以系統許可權執行的工作，或是已設定僅限互動式屬性的工作。 您無法使用變更命令從工作中移除僅限互動式的屬性。</br>依預設，「執行身份」使用者是排定工作時的本機電腦目前使用者，或是由 **/u** 參數指定的帳號（如果使用的話）。 但是，如果命令包含 **/ru** 參數，則 run as 使用者就是 **/ru** 參數所指定的帳號。 |
|           /z            |                                                                                                                                                                                                                                                                                                          指定在完成排程時刪除工作。                                                                                                                                                                                                                                                                                                          |
|           /?            |                                                                                                                                                                                                                                                                                                                        在命令提示字元顯示說明。                                                                                                                                                                                                                                                                                                                         |

### <a name="remarks"></a>備註

-   **/Tn**和 **/s**參數會識別工作。 **/Tr**、 **/ru**和 **/rp**參數指定您可以變更的工作屬性。
-   **/Ru**和 **/rp**參數指定工作執行時所用的許可權。 **/U**和 **/p**參數會指定用來變更工作的許可權。
-   若要變更遠端電腦上的工作，使用者必須使用遠端電腦上 Administrators 群組成員的帳戶登入本機電腦。
-   若要使用不同使用者的許可權來執行 **/change** 命令 (**/u**， **/p**) ，本機電腦必須位於與遠端電腦相同的網域中，或者必須位於遠端電腦網域信任的網域中。
-   系統帳戶沒有互動式登入許可權。 使用者看不到，而且無法與系統許可權執行的程式互動。
-   若要使用 **/it** 屬性來識別工作，請使用詳細資訊查詢 (**/query/v**) 。 在使用 **/it**工作的詳細資訊查詢顯示中，[ **登入模式]** 欄位的值為 [ **僅限互動**]。

### <a name="examples"></a>範例

### <a name="to-change-the-program-that-a-task-runs"></a>若要變更工作執行的程式

下列命令會將病毒檢查工作執行的程式從 VirusCheck.exe 變更為 VirusCheck2.exe。 此命令會使用 **/tn** 參數來識別工作，並使用 **/tr** 參數來指定工作的新程式。  (您無法變更工作名稱。 ) 
```
schtasks /change /tn Virus Check /tr C:\VirusCheck2.exe
```
在 [回應] 中， **SchTasks.exe** 會顯示下列成功訊息：
```
SUCCESS: The parameters of the scheduled task Virus Check have been changed.
```
因為此命令的結果，[病毒檢查] 工作現在會 VirusCheck2.exe 執行。

### <a name="to-change-the-password-for-a-remote-task"></a>變更遠端工作的密碼

下列命令會變更遠端電腦（>woodgrove-svr01）上 RemindMe 工作的使用者帳戶密碼。 此命令會使用 **/tn** 參數來識別工作，並使用 **/s** 參數來指定遠端電腦。 它會使用 **/rp** 參數指定新的密碼 p@ssWord3 。

每當使用者帳戶的密碼過期或變更時，就需要此程式。 如果儲存在工作中的密碼不再有效，則工作不會執行。
```
schtasks /change /tn RemindMe /s Svr01 /rp p@ssWord3
```
在 [回應] 中， **SchTasks.exe** 會顯示下列成功訊息：
```
SUCCESS: The parameters of the scheduled task RemindMe have been changed.
```
這項命令的結果是，RemindMe 工作現在會在其原始使用者帳戶下執行，但使用新的密碼。

### <a name="to-change-the-program-and-user-account-for-a-task"></a>變更工作的程式和使用者帳戶

下列命令會變更工作執行的程式，並變更工作執行時所使用的使用者帳戶。 基本上，它會針對新的工作使用舊的排程。 此命令會變更 ChkNews 工作，此工作會在每早上上午9:00 開始 Notepad.exe 啟動，以改為開始 Internet Explorer。

此命令會使用 **/tn** 參數來識別工作。 它會使用 **/tr** 參數來變更工作執行的程式，以及使用 **/ru** 參數來變更工作執行時所使用的使用者帳戶。

會省略 **/ru**和 **/rp** 參數（提供使用者帳戶的密碼）。 您必須提供帳戶的密碼，但您可以使用 **/ru**和 **/rp** 參數並以純文字輸入密碼，或等候 **SchTasks.exe** 提示您輸入密碼，然後在 [遮蔽文字] 中輸入密碼。
```
schtasks /change /tn ChkNews /tr c:\program files\Internet Explorer\iexplore.exe /ru DomainX\Admin01
```
在回應中， **SchTasks.exe** 要求使用者帳戶的密碼。 它會遮蔽您輸入的文字，因此不會顯示密碼。
```
Please enter the password for DomainX\Admin01:
```
請注意， **/tn** 參數會識別工作，而 **/tr** 和 **/ru** 參數會變更工作的屬性。 您無法使用其他參數來識別工作，也無法變更工作名稱。

在 [回應] 中， **SchTasks.exe** 會顯示下列成功訊息：
```
SUCCESS: The parameters of the scheduled task ChkNews have been changed.
```
這項命令的結果是，ChkNews 工作現在會以系統管理員帳戶的許可權來執行 Internet Explorer。

### <a name="to-change-a-program-to-the-system-account"></a>將程式變更為系統帳戶

下列命令會將 SecurityScript 工作變更為以系統帳戶的許可權執行。 它會使用 * */ru * * 參數來表示系統帳戶。
```
schtasks /change /tn SecurityScript /ru
```
在 [回應] 中， **SchTasks.exe** 會顯示下列成功訊息：
```
INFO: The run as user name for the scheduled task SecurityScript will be changed to NT AUTHORITY\SYSTEM.
SUCCESS: The parameters of the scheduled task SecurityScript have been changed.
```
由於以系統帳戶許可權執行的工作不需要密碼，因此 **SchTasks.exe** 不會提示您輸入密碼。

### <a name="to-run-a-program-only-when-i-am-logged-on"></a>只有當我登入時才執行程式

下列命令會將僅限互動式屬性新增至 MyApp （現有的工作）。 這個屬性可確保只有當執行身分使用者（也就是工作執行所在的使用者帳戶）登入電腦時，才會執行此工作。

此命令會使用 **/tn** 參數來識別工作，並使用 **/it** 參數將僅限互動式屬性新增至工作。 因為工作已以我的使用者帳戶的許可權執行，所以不需要變更工作的 **/ru** 參數。
```
schtasks /change /tn MyApp /it
```
在 [回應] 中， **SchTasks.exe** 會顯示下列成功訊息。
```
SUCCESS: The parameters of the scheduled task MyApp have been changed.
```

## <a name="schtasks-run"></a><a name=BKMK_run></a>schtasks 執行

立即啟動排程工作。 **執行**作業會忽略排程，但會使用在工作中儲存的程式檔位置、使用者帳戶和密碼來立即執行工作。

### <a name="syntax"></a>語法

```
schtasks /run /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="parameters"></a>參數

|         詞彙          |                                                                                                                                                                 定義                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /tn \<TaskName>    |                                                                                                                                                       必要。 識別工作。                                                                                                                                                        |
|    /s \<Computer>     |                                                                                                           指定遠端電腦的名稱或 IP 位址 (包含或不含反斜線) 。 預設是本機電腦。                                                                                                           |
| u-sql\<Domain>\]<User> | 使用指定之使用者帳戶的許可權來執行此命令。 根據預設，命令會以本機電腦目前使用者的許可權執行。</br>指定的使用者帳戶必須是遠端電腦上 Administrators 群組的成員。 只有當您使用 **/s**時， **/u**和 **/p**參數才有效。 |
|    /p \<Password>     |                          指定 **/u** 參數中指定之使用者帳戶的密碼。 如果您使用 **/u** 參數，但省略 **/p** 參數或 password 引數，則 **schtasks.exe** 會提示您輸入密碼。</br>只有當您使用 **/s**時， **/u**和 **/p**參數才有效。                           |
|          /?           |                                                                                                                                                    在命令提示字元顯示說明。                                                                                                                                                     |

### <a name="remarks"></a>備註

-   使用此作業來測試您的工作。 如果工作未執行，請檢查工作排程器服務交易記錄檔， \<Systemroot>\SchedLgU.txt，找出錯誤。
-   執行工作並不會影響工作排程，也不會變更下次排程工作的執行時間。
-   若要從遠端執行工作，必須在遠端電腦上排程工作。 當您執行此工作時，工作只會在遠端電腦上執行。 若要確認工作正在遠端電腦上執行，請使用工作管理員或工作排程器交易記錄 \<Systemroot>\SchedLgU.txt。

### <a name="examples"></a>範例

### <a name="to-run-a-task-on-the-local-computer"></a>在本機電腦上執行工作

下列命令會啟動安全性腳本工作。
```
schtasks /run /tn Security Script
```
在 [回應] 中， **SchTasks.exe** 會啟動與工作相關聯的腳本，並顯示下列訊息：
```
SUCCESS: Attempted to run the scheduled task Security Script.
```
這則訊息表示， **schtasks.exe** 會嘗試啟動程式，但無法正常啟動程式。

### <a name="to-run-a-task-on-a-remote-computer"></a>在遠端電腦上執行工作

下列命令會啟動遠端電腦上的更新工作： >woodgrove-svr01：
```
schtasks /run /tn Update /s Svr01
```
在此情況下， **SchTasks.exe** 會顯示下列錯誤訊息：
```
ERROR: Unable to run the scheduled task Update.
```
若要找出錯誤的原因，請在 [排程工作] 交易記錄檔中，查看 >woodgrove-svr01 上的 C:\Windows\SchedLgU.txt。 在此情況下，記錄檔中會出現下列專案：
```
Update.job (update.exe) 3/26/2001 1:15:46 PM ** ERROR **
The attempt to log on to the account associated with the task failed, therefore, the task did not run.
The specific error is:
0x8007052e: Logon failure: unknown user name or bad password.
Verify that the task's Run-as name and password are valid and try again.
```
顯然，工作中的使用者名稱或密碼在系統上無效。 下列的 **schtasks.exe/change** 命令會更新 >woodgrove-svr01 上更新工作的使用者名稱和密碼：
```
schtasks /change /tn Update /s Svr01 /ru Administrator /rp PassW@rd3
```
在 **變更** 命令完成之後，就會重複 **執行** 命令。 這次，Update.exe 程式隨即啟動， **SchTasks.exe** 顯示下列訊息：
```
SUCCESS: Attempted to run the scheduled task Update.
```
這則訊息表示， **schtasks.exe** 會嘗試啟動程式，但無法正常啟動程式。

## <a name="schtasks-end"></a><a name=BKMK_end></a>schtasks end

停止工作所啟動的程式。

### <a name="syntax"></a>語法

```
schtasks /end /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="parameters"></a>參數

|         詞彙          |                                                                                                                                                               定義                                                                                                                                                                |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /tn \<TaskName>    |                                                                                                                                         必要。 識別啟動程式的工作。                                                                                                                                         |
|    /s \<Computer>     |                                                                                                                        指定遠端電腦的名稱或 IP 位址。 預設是本機電腦。                                                                                                                        |
| u-sql\<Domain>\]<User> | 使用指定之使用者帳戶的許可權來執行此命令。 根據預設，命令會以本機電腦目前使用者的許可權執行。 指定的使用者帳戶必須是遠端電腦上 Administrators 群組的成員。 只有當您使用 **/s**時， **/u**和 **/p**參數才有效。 |
|    /p \<Password>     |                        指定 **/u** 參數中指定之使用者帳戶的密碼。 如果您使用 **/u** 參數，但省略 **/p** 參數或 password 引數，則 **schtasks.exe** 會提示您輸入密碼。</br>只有當您使用 **/s**時， **/u**和 **/p**參數才有效。                         |
|          /?           |                                                                                                                                                             顯示說明。                                                                                                                                                              |

### <a name="remarks"></a>備註

**SchTasks.exe** 只會結束排程工作所啟動之程式的實例。 若要停止其他處理常式，請使用 TaskKill。 如需詳細資訊，請參閱 [Taskkill](taskkill.md)。

### <a name="examples"></a>範例

### <a name="to-end-a-task-on-a-local-computer"></a>若要在本機電腦上結束工作

下列命令會停止「我的記事本」工作所啟動的 Notepad.exe 實例：
```
schtasks /end /tn My Notepad
```
在 [回應] 中， **SchTasks.exe** 會停止工作啟動 Notepad.exe 的實例，並顯示下列成功訊息：
```
SUCCESS: The scheduled task My Notepad has been terminated successfully.
```

### <a name="to-end-a-task-on-a-remote-computer"></a>若要在遠端電腦上結束工作

下列命令會停止遠端電腦上 InternetOn 工作所啟動的 Internet Explorer 實例，>woodgrove-svr01：
```
schtasks /end /tn InternetOn /s Svr01
```
在 [回應] 中， **SchTasks.exe** 會停止工作啟動 Internet Explorer 的實例，並顯示下列成功訊息：
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
|   tn\<TaskName>    |                                                                                                                                                                     \*}                                                                                                                                                                     |
|          /f           |                                                                                                                                  抑制確認訊息。 這項工作會在沒有警告的情況下刪除。                                                                                                                                  |
|    /s \<Computer>     |                                                                                                           指定遠端電腦的名稱或 IP 位址 (包含或不含反斜線) 。 預設是本機電腦。                                                                                                           |
| u-sql\<Domain>\]<User> | 使用指定之使用者帳戶的許可權來執行此命令。 根據預設，命令會以本機電腦目前使用者的許可權執行。</br>指定的使用者帳戶必須是遠端電腦上 Administrators 群組的成員。 只有當您使用 **/s**時， **/u**和 **/p**參數才有效。 |
|    /p \<Password>     |                          指定 **/u** 參數中指定之使用者帳戶的密碼。 如果您使用 **/u** 參數，但省略 **/p** 參數或 password 引數，則 **schtasks.exe** 會提示您輸入密碼。</br>只有當您使用 **/s**時， **/u**和 **/p**參數才有效。                           |
|          /?           |                                                                                                                                                    在命令提示字元顯示說明。                                                                                                                                                     |

### <a name="remarks"></a>備註

- **刪除**作業會從排程中刪除工作。 它不會刪除工作執行的程式，也不會中斷正在執行的程式。
- **Delete \\ *** 命令會刪除所有已排程的電腦工作，而不只是目前使用者排定的工作。

### <a name="examples"></a>範例

### <a name="to-delete-a-task-from-the-schedule-of-a-remote-computer"></a>從遠端電腦的排程刪除工作

下列命令會從遠端電腦的排程刪除開始郵件工作。 它會使用 **/s** 參數來識別遠端電腦。
```
schtasks /delete /tn Start Mail /s Svr16
```
在 [回應] 中， **SchTasks.exe** 會顯示下列確認訊息。 若要刪除工作，請按 Y 鍵<strong>。</strong> 若要取消命令，請輸入 **n**：
```
WARNING: Are you sure you want to remove the task Start Mail (Y/N )?
SUCCESS: The scheduled task Start Mail was successfully deleted.
```

### <a name="to-delete-all-tasks-scheduled-for-the-local-computer"></a>刪除本機電腦上排定的所有工作

下列命令會從本機電腦的排程中刪除所有工作，包括其他使用者排定的工作。 它會使用 **/tn \\ *** 參數來代表電腦上的所有工作，並使用 **/f**參數來抑制確認訊息。
```
schtasks /delete /tn * /f
```
在 [回應] 中， **SchTasks.exe** 會顯示下列成功訊息，指出已刪除唯一排定的工作 SecureScript。

`SUCCESS: The scheduled task SecureScript was successfully deleted.`

## <a name="schtasks-query"></a><a name=BKMK_query></a>schtasks 查詢

顯示排程要在電腦上執行的工作。

### <a name="syntax"></a>語法

```
schtasks [/query] [/fo {TABLE | LIST | CSV}] [/nh] [/v] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="parameters"></a>參數

|         詞彙          |                                                                                                                                                                 定義                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /query        |                                                                                                                        作業名稱是選擇性的。 在不使用任何參數的情況下輸入 **schtasks.exe** 會執行查詢。                                                                                                                         |
|      /fo \<format>    |  指定輸出格式。 有效的值為 TABLE、LIST 和 CSV                                                                                                                                 |
|          /nh          |                                                                                                            省略資料表顯示中的資料行標題。 此參數適用于 **資料表** 和 **CSV** 輸出格式。                                                                                                             |
|          /v           |                                                                                                         將工作的 advanced 屬性加入至顯示器。</br>使用 **/v** 的查詢應該格式化為 **清單** 或 **CSV**。                                                                                                          |
|    /s \<Computer>     |                                                                                                           指定遠端電腦的名稱或 IP 位址 (包含或不含反斜線) 。 預設是本機電腦。                                                                                                           |
| u-sql\<Domain>\]<User> | 使用指定之使用者帳戶的許可權來執行此命令。 根據預設，命令會以本機電腦目前使用者的許可權執行。</br>指定的使用者帳戶必須是遠端電腦上 Administrators 群組的成員。 只有當您使用 **/s**時， **/u**和 **/p**參數才有效。 |
|    /p \<Password>     |                                        指定 **/u** 參數中指定之使用者帳戶的密碼。 如果您使用 **/u**，但省略 **/p** 或 password 引數，則 **schtasks.exe** 會提示您輸入密碼。</br>只有當您使用 **/s**時， **/u**和 **/p**參數才有效。                                         |
|          /?           |                                                                                                                                                    在命令提示字元顯示說明。                                                                                                                                                     |

### <a name="remarks"></a>備註

**SchTasks.exe** 只會結束排程工作所啟動之程式的實例。 若要停止其他處理常式，請使用 TaskKill。 如需詳細資訊，請參閱 [Taskkill](taskkill.md)。

### <a name="examples"></a>範例

### <a name="to-display-the-scheduled-tasks-on-the-local-computer"></a>顯示本機電腦上的排程工作

下列命令會顯示針對本機電腦排程的所有工作。 這些命令會產生相同的結果，而且可以交換使用。
```
schtasks
schtasks /query
```
在 [回應] 中， **SchTasks.exe** 顯示預設的簡單資料表格式的工作，如下表所示：
```
TaskName Next Run Time Status
========================= ======================== ==============
Microsoft Outlook At logon time
SecureScript 14:42:00 PM , 2/4/2001
```

### <a name="to-display-advanced-properties-of-scheduled-tasks"></a>顯示已排程工作的 advanced 屬性

下列命令會要求詳細顯示本機電腦上的工作。 它使用 **/v** 參數來要求詳細的 (詳細資訊) 顯示，並使用 **/fo list** 參數將顯示格式化為清單，方便您閱讀。 您可以使用此命令來確認您所建立的工作具有預期的迴圈模式。

**schtasks/query/fo 清單/v**

在 [回應] 中， **SchTasks.exe** 會顯示所有工作的詳細屬性清單。 下列顯示顯示排定在上午4:00 執行之工作的工作清單。 在每個月的最後一個星期五：
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

下列命令會要求針對遠端電腦排程的工作清單，並將工作新增至本機電腦上的逗點分隔記錄檔。 您可以使用此命令格式來收集和追蹤針對多部電腦排程的工作。

此命令會使用 **/s** 參數來識別遠端電腦 Reskit16、指定格式的 **/fo** 參數，並使用 **/nh** 參數來隱藏資料行標題。 **>>** 附加符號會將輸出重新導向至本機電腦 >woodgrove-svr01 上的工作記錄檔 p0102.csv。 由於命令會在遠端電腦上執行，因此本機電腦路徑必須是完整的。
```
schtasks /query /s Reskit16 /fo csv /nh >> \\svr01\data\tasklogs\p0102.csv
```
在 [回應] 中， **SchTasks.exe** 會將 Reskit16 電腦排程的工作新增至本機電腦 >woodgrove-svr01 上的 p0102.csv 檔案。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
