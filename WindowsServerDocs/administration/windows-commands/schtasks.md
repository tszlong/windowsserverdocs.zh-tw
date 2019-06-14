---
title: schtasks
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2e713203-3dd8-491b-b9e1-9423618dc7e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 76ae264ded3cd0611c06b56c1074a2d916513da4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441610"
---
# <a name="schtasks"></a>schtasks



排程定期或在特定時間執行的命令和程式。 新增及從排程中移除工作，啟動和停止工作，視情況下，會顯示和變更排程的工作。

若要檢視命令語法，請按一下其中一個下列的命令：
-   [schtasks 建立](#BKMK_create)
-   [schtasks 變更](#BKMK_change)
-   [schtasks 執行](#BKMK_run)
-   [schtasks 結束](#BKMK_end)
-   [schtasks delete](#BKMK_delete)
-   [schtasks 查詢](#BKMK_query)

## <a name="remarks"></a>備註

- **SchTasks.exe**執行相同的作業，作為**排定的工作**中**控制台**。 同時，交替，您可以使用這些工具。
- **Schtasks**會取代**At.exe**，舊版 Windows 中隨附的工具。 雖然**At.exe**仍然包含在 Windows Server 2003 系列**schtasks**是建議的命令列工作，排程工具。
- 中的參數**schtasks**命令可以按照任何順序出現。 鍵入**schtasks**沒有任何參數執行查詢。
- 權限**schtasks**  
  -   您必須執行命令的權限。 任何使用者可以排程工作，以在本機電腦上的，也可以檢視和變更其排程的工作。 系統管理員群組的成員可以排程、 檢視及變更在本機電腦上的所有工作。
  -   若要排程、 檢視或變更遠端電腦上的工作，您必須在遠端電腦上 Administrators 群組的成員，或者您必須使用 **/u**參數，以提供的遠端電腦的系統管理員認證。
  -   您可以使用 **/u**中的參數 **/ 建立**或 **/變更**僅在本機和遠端電腦位於相同的網域或本機電腦是網域中的作業，遠端電腦的網域信任。 否則，在遠端電腦無法驗證指定的使用者帳戶，而且無法確認帳戶是 Administrators 群組的成員。
  -   工作必須具有執行權限。 所需的權限會隨著工作。 根據預設，工作會執行與本機電腦中，目前使用者的權限或所指定之使用者的權限 **/u**參數，如果有包含。 若要使用不同的使用者帳戶的權限或系統權限，請執行工作，請使用 **/ru**參數。
- 若要確認已排程的工作執行，或若要了解為什麼排定的工作未執行，請參閱工作排程器服務的交易記錄檔中， *SystemRoot*\SchedLgU.txt。 此記錄檔記錄嘗試執行由所有的工具，可使用服務，包括**排定的工作**並**SchTasks.exe**。
- 在少數情況下，工作檔案損毀。 損毀的工作不會執行。 當您嘗試對損毀的工作，執行作業時**SchTasks.exe**會顯示下列錯誤訊息：  
  ```
  ERROR: The data is invalid.
  ```  
  您無法復原損毀的工作。 若要還原對工作排定系統功能，請使用**SchTasks.exe**或是**排定的工作**從系統刪除工作，並重新排定它們。

## <a name="BKMK_create"></a>schtasks 建立

排程工作。

**Schtasks**每種排程類型會使用不同的參數組合。 若要查看結合的語法來建立工作，或查看特定的排程類型與建立工作的語法，請按一下下列選項之一。
-   [合併的語法和參數描述](#BKMK_syntax)
-   [若要排程的工作，每個 N 分鐘執行一次](#BKMK_minutes)
-   [若要排程的工作，每個 N 小時執行一次](#BKMK_hours)
-   [若要排程每 N 天執行的工作](#BKMK_days)
-   [若要排程的工作執行每隔 N 週](#BKMK_weeks)
-   [若要排程執行每 N 個月的工作](#BKMK_months)
-   [排程會在一週的特定日期執行的工作](#BKMK_spec_day)
-   [若要排程每月特定週數執行的工作](#BKMK_spec_week)
-   [排程會在每個月後的特定日期執行的工作](#BKMK_spec_date)
-   [若要排程在當月的最後一天執行的工作](#BKMK_last_day)
-   [排程執行一次的工作](#BKMK_once)
-   [若要排程執行每次系統啟動時啟動的工作](#BKMK_startup)
-   [若要排程工作執行時的使用者登入](#BKMK_logon)
-   [排程會在系統閒置時執行的工作](#BKMK_idle)
-   [若要排程會立即執行的工作](#BKMK_now)
-   [若要排程執行具有不同的權限的工作](#BKMK_diff_perms)
-   [若要排程執行具有系統權限的工作](#BKMK_sys_perms)
-   [若要排程的工作執行一個以上的程式](#BKMK_multi_progs)
-   [排程會在遠端電腦執行的工作](#BKMK_remote)

### <a name="BKMK_syntax"></a>合併的語法和參數描述

#### <a name="syntax"></a>語法

```
schtasks /create /sc <ScheduleType> /tn <TaskName> /tr <TaskRun> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]] [/ru {[<Domain>\]<User> | System}] [/rp <Password>] [/mo <Modifier>] [/d <Day>[,<Day>...] | *] [/m <Month>[,<Month>...]] [/i <IdleTime>] [/st <StartTime>] [/ri <Interval>] [{/et <EndTime> | /du <Duration>} [/k]] [/sd <StartDate>] [/ed <EndDate>] [/it] [/z] [/f]
```

#### <a name="parameters"></a>參數

##### <a name="sc-scheduletype"></a>/sc \<ScheduleType>

指定的排程類型。 有效值為分鐘，每小時、 每天、 每週、 每月一次，ONSTART、 登入時，ONIDLE。

|排程類型|描述|
|-------------|-----------|
|分鐘，每小時、 每天、 每週、 每月|指定排程的時間單位。|
|一次|工作執行一次在指定的日期和時間。|
|ONSTART|每次系統啟動時，就會執行工作。 您可以指定開始日期，或在下次系統啟動的時執行工作。|
|登入時|每當使用者 （任何使用者） 登入，就會執行工作。 您可以指定日期，或執行工作的下次使用者登入。|
|ONIDLE|每當系統閒置一段指定時間時，就會執行工作。 您可以指定日期，或系統處於閒置狀態的下一次執行工作。|

##### <a name="tn-taskname"></a>/tn\<工作名稱 >

指定工作的名稱。 每個工作在系統上的必須有唯一的名稱。 名稱必須符合檔案名稱的規則，而且不能超過 238 個英數字元。 您可以使用引號來括住包含空格的名稱。

##### <a name="tr-taskrun"></a>t \<TaskRun >

指定程式或工作所執行的命令。 輸入可執行檔、 指令碼檔案或批次檔的完整的路徑和檔案名稱。 路徑名稱不得超過 262 個字元。 如果您省略路徑， **schtasks**會假設該檔案位於*SystemRoot*\System32 目錄。

##### <a name="s-computer"></a>/s\<電腦 >

排程指定的遠端電腦上的工作。 輸入名稱或 IP 位址的遠端電腦 （或不含反斜線）。 預設是本機電腦。 **/U**並 **/p**參數都有效只有當您使用 **/s**。

##### <a name="u-domainuser"></a>/u [\<網域 >\]<User>

使用指定的使用者帳戶的權限執行此命令。 預設為本機電腦的目前使用者的權限。 **/U**並 **/p**參數是僅適用於遠端電腦上排程工作 ( **/s**)。

排程工作，以及執行工作，會使用指定的帳戶的權限。 若要執行工作的不同的使用者權限時，使用 **/ru**參數。

使用者帳戶必須是遠端電腦上 Administrators 群組的成員。 此外，在本機電腦必須在遠端電腦，與相同的網域，或必須是遠端電腦的網域信任的網域中。

##### <a name="p-password"></a>/p \<Password>

中所指定的使用者帳戶提供密碼 **/u**參數。 如果您使用 **/u**參數，但省略 **/p**參數或在 password 引數， **schtasks**會提示您輸入密碼，與遮蔽了您所輸入的文字。

**/U**並 **/p**參數是僅適用於遠端電腦上排程工作 ( **/s**)。

##### <a name="ru-domainuser--system"></a>/ru {[\<網域 >\] <User> |系統}

使用指定的使用者帳戶的權限執行工作。 根據預設，工作會執行與本機電腦中，目前使用者的權限或所指定之使用者的權限 **/u**參數，如果有包含。 **/Ru**排定在本機或遠端電腦上的工作時，參數才有效。


|       值        |                                                    描述                                                    |
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| [\<網域 >\]<User> |                                       指定替代的使用者帳戶。                                        |
|    系統或 「 」    | 指定本機系統帳戶，使用的作業系統和系統服務的高特殊權限帳戶。 |

##### <a name="rp-password"></a>/rp\<密碼 >

中指定的使用者帳戶提供密碼 **/ru**參數。 如果您省略這個參數，指定使用者帳戶時**SchTasks.exe**會提示您輸入密碼，與遮蔽了您所輸入的文字。

請勿使用 **/rp**使用系統帳戶認證來執行工作的參數 ( **/ru 系統**)。 系統帳戶沒有密碼並**SchTasks.exe**不會提示的其中一個。

##### <a name="mo-modifier"></a>每月\<修飾詞 >

指定其排程類型內的工作執行頻率。 此參數才有效，但選擇性的如需一分鐘，每小時、 每天、 每週和每月排程。 預設值為 1。

|排程類型|修飾詞值|描述|
|-------------|---------------|-----------|
|分鐘|1 - 1439|工作執行每個\<N > 分鐘。|
|每小時|1 - 23|工作執行每個\<N > 時數。|
|每日|1 - 365|工作執行每個\<N > 天。|
|每週|1 - 52|工作執行每個\<N > 週。|
|一次|沒有修飾詞。|工作執行一次。|
|ONSTART|沒有修飾詞。|在啟動時執行工作。|
|登入時|沒有修飾詞。|工作執行時所指定的使用者 **/u**參數登入。|
|ONIDLE|沒有修飾詞。|工作執行之後，系統所指定的分鐘數會閒置 **/i**參數，所需的 ONIDLE 搭配使用。|
|每月|1 - 12|工作執行每個\<N > 個月。|
|每月|LASTDAY|在當月的最後一天執行工作。|
|每月|首先，第二，第三，第四，最後一個|搭配使用 **/d**\<天 > 上的特定週次和日子執行工作的參數。 例如，在該月的第三個星期三。|

##### <a name="d-dayday--"></a>/d Day[,Day...] | *

指定一週或一天 （或天） 的每個月的一天 （或天）。 只與每週或每月排程的生效。


| 排程類型 |              修飾詞              |     日期值 (/d)      |                                                                                                 描述                                                                                                 |
|---------------|------------------------------------|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    每週     |               1 - 52               | MON - SUN[,MON - SUN...] |                                                                                                     \*                                                                                                      |
|    每月    | 首先，第二，第三，第四，最後一個 |        平日 24 SUN         |                                                                                   所需的特定週排程。                                                                                    |
|    每月    |          無或 {1-12}          |          1 - 31          | 選擇性且有效只能搭配任何修飾詞 ( **/月**) 參數 （特定日期的排程） 或當 **/月**是 1 到 12 個 (」 每個\<N > 個月 」 排程)。 預設值為一天 1 （每月的第一天）。 |

##### <a name="m-monthmonth"></a>/m 月 [，...月]

指定每個月或排程的工作的執行期間的年度月份。 有效值為 1 月-12 月和 * （每個月）。 **/M**只能搭配每月排程的參數無效。 使用 LASTDAY 修飾詞時，它是必要的。 否則，它是選擇性，預設值 * （每個月）。

##### <a name="i-idletime"></a>/i \<IdleTime>

指定分鐘數台電腦閒置工作啟動之前。 有效的值是 1 到 999 的整數數字。 此參數才有效只能搭配 ONIDLE 排程，則需要。

##### <a name="st-starttime"></a>/st \<StartTime >

指定在工作開始 （每次啟動時） 中的當日時間\<hh: mm > 24 小時格式。 預設值是在本機電腦上目前的時間。 **/St**參數是有效分鐘、 每小時、 每天、 每週、 每月，並一次排程。 需要一次排程。

##### <a name="ri-interval"></a>/ri\<間隔 >

指定的重複間隔，以分鐘為單位。 這並不適用於排程類型：分鐘、 小時、 ONSTART、 登入時和 ONIDLE。 有效範圍是 1 到 599940 分鐘 （599940 分鐘 = 9999 小時）。 如果指定了 /ET 或 /DU，重複間隔預設為 10 分鐘。

##### <a name="et-endtime"></a>/et \<EndTime >

指定結尾為分鐘或每小時的工作排程的當日時間\<hh: mm > 24 小時格式。 指定的結束時間之後, **schtasks**才會開始工作一次的開始時間重複執行。 根據預設，工作排程已結束時間。 這個參數是選擇性的只以分鐘或每小時排程才有效。

如需範例，請參閱：
-   「 排程的工作分鐘執行一次每 100 在非上班時間 」**排程工作執行每個** \<N >**分鐘**一節。

##### <a name="du-duration"></a>/du\<持續時間 >

指定的分鐘的時間中的每小時排程的時間長度的最大長度\<HHHH:MM > 24 小時格式。 在指定的時間過了之後**schtasks**才會開始工作一次的開始時間重複執行。 根據預設，工作排程會具有沒有最大持續時間。 這個參數是選擇性的只以分鐘或每小時排程才有效。

如需範例，請參閱：
-   「 排程的工作執行 10 小時每隔 3 小時 」**排程工作執行每個** \<N >**小時**區段。

##### <a name="k"></a>/k

工作在所指定的時間執行的程式就會停止 **/et**或是 **/du**。 不含 **/k**， **schtasks**後未啟動程式再次達到所指定的時間 **/et**或是 **/du**，但它不會停止如果仍在執行程式。 這個參數是選擇性的只以分鐘或每小時排程才有效。

如需範例，請參閱：
-   「 排程的工作分鐘執行一次每 100 在非上班時間 」**排程工作執行每個** \<N >**分鐘**一節。

##### <a name="sd-startdate"></a>/sd \<StartDate>

指定工作排程開始的日期。 預設值是在本機電腦上目前的日期。 **/Sd**參數是有效而且是選擇性的所有排程類型。

格式*StartDate*選取 在本機電腦的地區設定會隨著**地區及語言選項**中**控制台**。 只有一種格式僅適用於每個地區設定的。

有效的日期格式會列在下表中。 使用選取的格式最類似的格式**簡短日期**中**地區及語言選項**中**控制台**本機電腦上。


|       值       |                                        描述                                         |
|-------------------|--------------------------------------------------------------------------------------------|
| \<MM>/<DD>/<YYYY> | 使用每月第一個格式，例如**英文 （美國）** 並**西班牙文 （巴拿馬）** 。 |
| \<DD>/<MM>/<YYYY> |       使用日第一個格式，例如**保加利亞文**並**荷蘭文 （荷蘭）** 。        |
| \<YYYY>/<MM>/<DD> |          使用以年起始格式，例如**瑞典文**並**法文 （加拿大）** 。          |

/ed\<結束日期 >

指定的排程結束的日期。 這個參數是選擇性的。 您不在一次，ONSTART、 登入時或 ONIDLE 排程中有效。 根據預設，排程擁有無結束日期。

格式*EndDate*選取 在本機電腦的地區設定會隨著**地區及語言選項**中**控制台**。 只有一種格式僅適用於每個地區設定的。

有效的日期格式會列在下表中。 使用選取的格式最類似的格式**簡短日期**中**地區及語言選項**中**控制台**本機電腦上。


|       值       |                                        描述                                         |
|-------------------|--------------------------------------------------------------------------------------------|
| \<MM>/<DD>/<YYYY> | 使用每月第一個格式，例如**英文 （美國）** 並**西班牙文 （巴拿馬）** 。 |
| \<DD>/<MM>/<YYYY> |       使用日第一個格式，例如**保加利亞文**並**荷蘭文 （荷蘭）** 。        |
| \<YYYY>/<MM>/<DD> |          使用以年起始格式，例如**瑞典文**並**法文 （加拿大）** 。          |

##### <a name="it"></a>/it

指定要執行工作時，才 「 執行身分 」 使用者 （工作所執行的使用者帳戶） 登入電腦。 此參數會有任何作用，擁有系統管理權限執行的工作。

根據預設，「 執行身分 」 使用者是本機電腦時排定工作的目前的使用者或所指定的帳戶 **/u**參數，如果使用的話。 不過，如果此命令包含 **/ru**參數，然後 「 執行身分 」 使用者是所指定的帳戶 **/ru**參數。

如需範例，請參閱：
-   「 若要排程執行每隔 70 天，如果我已經登的工作 」 中**排程工作執行每個** *N* **天**一節。
-   「 執行只有當特定使用者的工作在登入 」**來排程執行具有不同的權限的工作**一節。

##### <a name="z"></a>/z

指定要刪除時完成它的排程工作。

##### <a name="f"></a>/f

指定要建立工作，並隱藏警告，如果已經存在指定的工作。

##### <a name=""></a>/?

在命令提示字元顯示說明。

### <a name="BKMK_minutes"></a>若要排程的工作，每個 N 分鐘執行一次

#### <a name="minute-schedule-syntax"></a>每分鐘的排程語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc minute [/mo {1 - 1439}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [{/et <HH:MM> | /du <HHHH:MM>} [/k]] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在每分鐘的排程中， **/sc 分鐘**是必要參數。 **/月**（修飾詞） 參數是選擇性的並指定每個執行的工作之間的分鐘數。 預設值 **/月**為 1 （每分鐘）。 **/Et** （結束時間） 和 **/du** （持續時間） 參數是選擇性的可以使用包含或不含 **/k** （結束工作） 參數。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-every-20-minutes"></a>若要排程的工作執行每隔 20 分鐘

下列命令會排定安全性執行的指令碼，Sec.vbs 每隔 20 分鐘。 此命令會使用 **/sc**參數來指定以分鐘排程並 **/月**參數來指定 20 分鐘的間隔。

此命令不會包含開始日期或時間，因為工作可啟動命令完成時，並執行系統正在執行時，之後每隔 20 分鐘之後的 20 分鐘。 請注意安全性的指令碼來源檔案位於遠端電腦，但工作排程，而在本機電腦上執行。
```
schtasks /create /sc minute /mo 20 /tn "Security Script" /tr \\central\data\scripts\sec.vbs
```

#### <a name="to-schedule-a-task-that-runs-every-100-minutes-during-non-business-hours"></a>若要排程每隔 100 分鐘在非上班時間執行的工作

下列命令會排定安全性執行的指令碼，Sec.vbs 本機電腦上下午 5:00 之間每隔 100 分鐘 和上午 7 點 59 每一天。 此命令會使用 **/sc**參數來指定以分鐘排程並 **/月**參數來指定 100 分鐘的間隔。 它會使用 **/st**並 **/et**參數來指定開始時間和結束時間的每一天的排程。 它也會使用 **/k**參數可停止指令碼，如果它仍在執行上午 7 點 59 不含 **/k**， **schtasks**上午 7:59，但如果執行個體就會啟動在早上 6 點 20 之後無法啟動指令碼 是仍在執行，它會停止它。
```
schtasks /create /tn "Security Script" /tr sec.vbs /sc minute /mo 100 /st 17:00 /et 08:00 /k
```

### <a name="BKMK_hours"></a>若要排程的工作，每個 N 小時執行一次

#### <a name="hourly-schedule-syntax"></a>每小時排程語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc hourly [/mo {1 - 23}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [{/et <HH:MM> | /du <HHHH:MM>} [/k]] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在每小時的排程中，**每小時 /sc**是必要參數。 **/月**（修飾詞） 參數是選擇性的並指定每個執行的工作之間的小時數。 預設值 **/月**為 1 （每小時）。 **/K** （結束工作） 參數是選擇性的而且可用以 **/et** （在指定的時間結束） 或 **/du** （結束指定的間隔之後）。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-every-five-hours"></a>若要排程每隔五小時執行的工作

下列命令會排定執行自 2002 年 3 月的第一天起每隔五小時，MyApp 程式。 它會使用 **/月**參數來指定間隔並 **/sd**參數來指定開始日期。 此命令不會指定開始時間，因為目前的時間做為開始時間。

因為本機電腦設定為使用**英文 （辛巴威）** 選項**地區及語言選項**中**控制台**，開始日期的格式為 MM/DD/YYYY(03/01/2002)。
```
schtasks /create /sc hourly /mo 5 /sd 03/01/2002 /tn "My App" /tr c:\apps\myapp.exe
```

#### <a name="to-schedule-a-task-that-runs-every-hour-at-five-minutes-past-the-hour"></a>若要排程每小時的整點過後五分鐘後執行的工作

下列命令會排程執行每小時開始於五分鐘數，MyApp 程式。 因為 **/月**省略參數時，此命令會使用每小時排程，也就是 (1) 的每個小時的預設值。 如果此命令會在上午 12:05 之後執行，程式不會不會執行直到隔天。
```
schtasks /create /sc hourly /st 00:05 /tn "My App" /tr c:\apps\myapp.exe
```

#### <a name="to-schedule-a-task-that-runs-every-3-hours-for-10-hours"></a>若要排程每隔 3 小時執行 10 小時的工作

下列命令會排定為每 3 小時執行 10 小時，MyApp 程式。

此命令會使用 **/sc**參數來指定每小時排程並 **/月**參數來指定 3 小時的間隔。 它會使用 **/st**參數來啟動在午夜的排程並 **/du**結束之後 10 小時的週期參數。 程式執行幾分鐘的時間，因為 **/k**參數，如果它仍在執行持續時間到期時，請停止程式，不需要。
```
schtasks /create /tn "My App" /tr myapp.exe /sc hourly /mo 3 /st 00:00 /du 0010:00
```
在此範例中，工作會執行在上午 12:00、 上午 3:00、 上午 6:00 和上午 9:00 因為持續時間為 10 小時，工作不會執行一次在下午 12:00 相反地，它會啟動一次在上午 12:00 隔天。

### <a name="BKMK_days"></a>若要排程每 N 天執行的工作

#### <a name="daily-schedule-syntax"></a>每日排程語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc daily [/mo {1 - 365}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在每日排程中， **/sc 每天**是必要參數。 **/月**（修飾詞） 參數是選擇性的並指定每個執行的工作之間的天數。 預設值 **/月**為 1 （每一天）。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-every-day"></a>若要排程每日執行的工作

下列範例會排定執行一次一天，每天上午 8:00，MyApp 程式 直到 2002 年 12 月 31日日。 因為後者省略 **/月**參數，預設間隔為 1 用來執行命令的每一天。

在此範例中，因為本機電腦系統設定為**English (United Kingdom)** 選項**地區及語言選項**中**控制台**，格式為結束日期為 DD/MM/YYYY (31/12/2002)
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc daily /st 08:00 /ed 31/12/2002
```

#### <a name="to-schedule-a-task-that-runs-every-12-days"></a>若要排程執行每個 12 天的工作

下列範例會排程在下午 1:00 執行每隔 12 天，MyApp 程式 (13:00) 自 2002 年 12 月 31 日。 此命令會使用 **/月**參數來指定間隔天數兩 （2） 和 **/sd**並 **/st**參數來指定的日期和時間。

在此範例中，因為系統設定為**英文 （辛巴威）** 選項**地區及語言選項**中**控制台**，結束日期的格式為 MM/DD /YYYY (12/31/2002)
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc daily /mo 12 /sd 12/31/2002 /st 13:00
```

#### <a name="to-schedule-a-task-that-runs-every-70-days-if-i-am-logged-on"></a>若要排程執行每隔 70 天，如果我已經登的工作

下列命令會排定安全性執行的指令碼，Sec.vbs 每隔 70 天。 此命令會使用 **/月**參數來指定為 70 天的間隔。 它也會使用 **/it**參數來指定其帳戶下執行工作的使用者登入電腦時，才執行工作。 因為此工作將會執行我的使用者帳戶的權限，則會執行工作，只有當我登入。
```
schtasks /create /tn "Security Script" /tr sec.vbs /sc daily /mo 70 /it
```

> [!NOTE]
> 使用互動式專用識別工作 ( **/it**) 屬性，使用詳細資訊的查詢 **(/ 查詢 /v**)。 中的工作的詳細資訊的查詢顯示 **/it**，則**登入模式**欄位有值為**僅互動**。

### <a name="BKMK_weeks"></a>若要排程的工作執行每隔 N 週

#### <a name="weekly-schedule-syntax"></a>每週排程語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc weekly [/mo {1 - 52}] [/d {<MON - SUN>[,MON - SUN...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在每週排程中， **/sc 每週**是必要參數。 **/月**（修飾詞） 參數是選擇性的並指定每個執行的工作之間的週數。 預設值 **/月**為 1 （每週）。

每週排程也有選用 **/d**參數來排定一週當中的指定天數或所有天執行工作 ( *)。預設為 MON （星期一）。每一天 (* ) 選項相當於排程每日工作。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-every-six-weeks"></a>若要排程的工作執行每隔六週

下列命令會排定在遠端電腦上執行每隔六週，MyApp 程式。 此命令會使用 **/月**參數來指定間隔。 因為此命令會省略 **/d**參數，在星期一執行的工作。

此命令也會使用 **/s**參數來指定遠端電腦並 **/u**以使用者的系統管理員帳戶的權限執行命令的參數。 因為 **/p**省略參數，則**SchTasks.exe**會提示使用者輸入系統管理員帳戶密碼。

此外，因為命令執行遠端命令，包括 MyApp.exe 的路徑中的所有路徑參考到遠端電腦上的路徑。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /mo 6 /s Server16 /u Admin01
```

#### <a name="to-schedule-a-task-that-runs-every-other-week-on-friday"></a>若要排程的工作執行每隔一週星期五

下列命令會排程要執行其他每個星期五的工作。 它會使用 **/月**參數來指定兩週間隔並 **/d**參數來指定一週的星期幾。 若要排程執行的工作每個星期五，省略 **/月**參數或將它設定為 1。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /mo 2 /d FRI
```

### <a name="BKMK_months"></a>若要排程執行每 N 個月的工作

#### <a name="syntax"></a>語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly [/mo {1 - 12}] [/d {1 - 31}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在此排程類型**每月 /sc**是必要參數。 **/月**（修飾詞） 參數，指定每個執行的工作之間的月數，這是選擇性的預設值為 1 （每個月）。 此排程類型也具有選擇性 **/d**排程在指定日期的月份執行工作的參數。 預設值為 1 （每月的第一天）。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-on-the-first-day-of-every-month"></a>若要排程執行每個月的第一天的工作

下列命令會排程在每個月的第一天執行，MyApp 程式。 因為值為 1 是兩者的預設值 **/月**（修飾詞） 參數和 **/d** （天） 參數，此命令會省略這些參數。
```
schtasks /create /tn "My App" /tr myapp.exe /sc monthly
```

#### <a name="to-schedule-a-task-that-runs-every-three-months"></a>若要排程執行每三個月的工作

下列命令會排程執行每三個月，MyApp 程式。 它會使用 **/月**參數來指定間隔。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo 3
```

#### <a name="to-schedule-a-task-that-runs-at-midnight-on-the-21st-day-of-every-other-month"></a>若要排程會在其他每個月的 21 天的午夜所執行的工作

下列命令會排定執行其他每個月的月份，在午夜的 21 天，MyApp 程式。 此命令會指定這項工作，應該執行的一年，從 2002 年 7 月 2 日到 2003 年 6 月 30 日。

此命令會使用 **/月**參數來指定每月的時間間隔 （每隔兩個月）， **/d**參數來指定日期，而 **/st**指定的時間。 它也會使用 **/sd**並 **/ed**參數來指定開始日期和結束日期，分別。 因為本機電腦設定為**英文 （南非）** 選項**地區及語言選項**中**控制台**，當地格式中指定的日期YYYY/MM/DD。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo 2 /d 21 /st 00:00 /sd 2002/07/01 /ed 2003/06/30 
```

### <a name="BKMK_spec_day"></a>排程會在一週的特定日期執行的工作

#### <a name="weekly-schedule-syntax"></a>每週排程語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc weekly [/d {<MON - SUN>[,MON - SUN...] | *}] [/mo {1 - 52}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

「 一週的 day"的排程是每週排程的變化。 在每週排程中， **/sc 每週**是必要參數。 **/月**（修飾詞） 參數是選擇性的並指定每個執行的工作之間的週數。 預設值 **/月**為 1 （每週）。 **/D**參數，這是選擇性，一週當中的指定天數或所有天執行工作的排程 (<em>)。預設為 MON （星期一）。每個 [天] 選項 (</em>* /d \** *) 就相當於排程每日工作。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-every-wednesday"></a>若要排程執行的工作每週三

下列命令會排定為每週執行在星期三，MyApp 程式。 此命令會使用 **/d**參數來指定一週的星期幾。 因為此命令會省略 **/月**參數，工作每週執行。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /d WED
```

#### <a name="to-schedule-a-task-that-runs-every-eight-weeks-on-monday-and-friday"></a>若要排程的工作執行每個八週在星期一和星期五

下列命令會排定在星期一和每個第八個週的星期五執行工作。 它會使用 **/d**參數來指定日子並 **/月**參數來指定八週的間隔。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /mo 8 /d MON,FRI
```

### <a name="BKMK_spec_week"></a>若要排程每月特定週數執行的工作

#### <a name="specific-week-syntax"></a>特定週數的語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /mo {FIRST | SECOND | THIRD | FOURTH | LAST} /d MON - SUN [/m {JAN - DEC[,JAN - DEC...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在此排程類型 **/sc 每月**參數， **/月**（修飾詞） 參數，而 **/d** （天） 參數所需。 **/月**（修飾詞） 參數會指定一週執行工作。 **/D**參數會指定一週的星期幾。 （您可以指定此排程類型之當週的某一天）。此排程也有選擇性 **/m** （月） 參數，可讓您針對特定的幾個月或每月排程的工作 (<em>)。預設值 * */m</em>* 參數為每個月 (* )。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-for-the-second-sunday-of-every-month"></a>每個月的第二個星期日的排程工作

下列命令會排程在每個月的第二個星期日上執行，MyApp 程式。 它會使用 **/月**參數來指定第二個月份的週和 **/d**參數來指定一天。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo SECOND /d SUN
```

#### <a name="to-schedule-a-task-for-the-first-monday-in-march-and-september"></a>若要將工作排定在 3 月和 9 月的第一個星期一

下列命令會排定在 3 月和 9 月的第一個星期一執行，MyApp 程式。 它會使用 **/月**參數來指定月份的第一週和 **/d**參數來指定一天。 它會使用 **/m**參數來指定月份中，以逗號區隔月引數。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo FIRST /d MON /m MAR,SEP
```

### <a name="BKMK_spec_date"></a>排程會在每個月後的特定日期執行的工作

#### <a name="specific-date-syntax"></a>特定日期的語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /d {1 - 31} [/m {JAN - DEC[,JAN - DEC...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在特定日期的排程類型中，**每月 /sc**參數與 **/d** （天） 參數所需。 **/D**參數指定的當月日期 (1-31)，不一週的星期幾。 您可以在排程中指定某一天。 **/月**（修飾詞） 參數無效，無法使用此排程類型。

**/M** （月） 參數是選用此排程類型和預設值是每個月 (<em>)。 **Schtasks</em>* 不會讓您排程的工作不會發生的日期中所指定月份 **/m**參數。 不過，如果省略 **/m**參數，而不會執行工作的工作不會出現在每個月，例如第 31 天，則工作的日期，這是在較短的幾個月的排程。 若要排程的工作在當月最後一天，使用最後一天的排程類型。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-for-the-first-day-of-every-month"></a>將工作排定為每個月第一天

下列命令會排程在每個月的第一天執行，MyApp 程式。 因為預設修飾詞是 none （無修飾詞） 時，預設日期是 1 天，預設每月為每個月，命令不需要任何額外的參數。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly
```

#### <a name="to-schedule-a-task-for-the-15th-days-of-may-and-june"></a>5 月和年 6 月 15 日排程的工作

下列命令會排定在 5 月 15 日和年 6 月 15 上執行下午 3:00，MyApp 程式 (15:00). 它會使用 **/m**參數來指定日期和 **/m**參數來指定月數。 它也會使用 **/st**參數來指定開始時間。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /d 15 /m MAY,JUN /st 15:00
```

### <a name="BKMK_last_day"></a>若要排程在當月的最後一天執行的工作

#### <a name="last-day-syntax"></a>最後一天語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /mo LASTDAY /m {JAN - DEC[,JAN - DEC...] | *} [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在最後一天的排程類型中，**每月 /sc**參數， **/月 LASTDAY** （修飾詞） 參數，而 **/m** （月） 參數所需。 **/D** （天） 參數無效。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-for-the-last-day-of-every-month"></a>將工作排定為每個月最後一天

下列命令會排程在每個月的最後一天執行，MyApp 程式。 它會使用 **/月**參數來指定的最後一天並 **/m**參數搭配萬用字元 （*） 表示每個月執行的程式。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo lastday /m *
```

#### <a name="to-schedule-a-task-at-600-pm-on-the-last-days-of-february-and-march"></a>將工作排定在下午 6:00 在過去的二月和三月的 3 天

下列命令會排程在下午 6:00 執行 2 月的最後一天和 3 月的最後一天，MyApp 程式 它會使用 **/月**參數來指定的最後一天， **/m**參數來指定幾個月，而 **/st**參數來指定開始時間。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo lastday /m FEB,MAR /st 18:00
```

### <a name="BKMK_once"></a>排程執行一次的工作

#### <a name="syntax"></a>語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc once /st <HH:MM> [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在 執行一次排程類型 **/sc 一次**是必要參數。 **/St**是必要參數，指定工作執行的時間。 **/Sd**是選擇性參數，指定工作執行的日期。 **/月**（修飾詞） 及 **/ed** （結束日期） 參數不適用於此排程類型。

**Schtasks**不允許您將工作排定為執行一次，如果指定的時間與日期在過去，根據本機電腦的時間。 若要排程執行一次在不同的時區中的遠端電腦的工作，您必須排程在該日期之前它和時間，就會發生在本機電腦上。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-one-time"></a>若要排程的工作執行一次

下列命令可排程於 2003 年 1 月 1 日午夜執行，MyApp 程式。 它會使用 **/sc**參數來指定排程類型與 **/sd**並**st**指定的日期和時間。

因為本機電腦使用，所以**英文 （美國）** 選項**地區及語言選項**中**控制台**，開始日期的格式為 MM/DD/YYYY。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc once /sd 01/01/2003 /st 00:00
```

### <a name="BKMK_startup"></a>若要排程執行每次系統啟動時啟動的工作

#### <a name="syntax"></a>語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onstart [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

在啟動時的排程類型中 **/sc onstart**是必要參數。 **/Sd** （開始日期） 參數是選擇性的預設值是目前的日期。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-when-the-system-starts"></a>排程系統啟動時執行的工作

下列命令可排程執行每次系統啟動時，自 2001 年 3 月 15 日起，MyApp 程式：

因為本機電腦是使用**英文 （美國）** 選項**地區及語言選項**中**控制台**，開始日期的格式為 MM/DD/YYYY.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc onstart /sd 03/15/2001
```

### <a name="BKMK_logon"></a>若要排程工作執行時的使用者登入

#### <a name="syntax"></a>語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onlogon [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

「 開啟登入 」 的排程類型排程工作的任何使用者登入電腦時，就會執行。 在 「 開啟登入 」 的排程類型中， **/sc 登入時**是必要參數。 **/Sd** （開始日期） 參數是選擇性的預設值是目前的日期。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-when-a-user-logs-on-to-a-remote-computer"></a>若要排程工作執行時的使用者登入遠端電腦

下列命令會排定每次使用者 （任何使用者） 登入遠端電腦執行的批次檔。 它會使用 **/s**參數來指定遠端電腦。 因為命令是遠端，命令，包括批次檔的路徑中的所有路徑會都指向遠端電腦上的路徑。
```
schtasks /create /tn "Start Web Site" /tr c:\myiis\webstart.bat /sc onlogon /s Server23
```

### <a name="BKMK_idle"></a>排程會在系統閒置時執行的工作

#### <a name="syntax"></a>語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onidle /i {1 - 999} [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>備註

「 在閒置 」 的排程類型排程工作時沒有任何使用者活動期間所指定的時間，就會執行 **/i**參數。 中的 「 在閒置 」 的排程類型 **/sc onidle**參數和 **/i**參數所需。 **/Sd** （開始日期） 是選擇性的預設值是目前的日期。

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-whenever-the-computer-is-idle"></a>排程執行時電腦處於閒置狀態的工作

下列命令會排定執行的電腦閒置時，MyApp 程式。 它會使用所需 **/i**參數來指定工作啟動之前的十分鐘必須維持閒置的電腦。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc onidle /i 10
```

### <a name="BKMK_now"></a>若要排程會立即執行的工作

**Schtasks**不具有 「 執行現在 」 選項，但您可以模擬該選項所建立的工作，會執行一次，並在幾分鐘內啟動。

#### <a name="syntax"></a>語法

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc once [/st <HH:MM>] /sd <MM/DD/YYYY> [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="examples"></a>範例

#### <a name="to-schedule-a-task-that-runs-a-few-minutes-from-now"></a>若要排程的工作執行幾分鐘的時間從現在開始。

下列命令排程工作將執行一次，在 2002 年 11 月 13 日的下午 2:18 當地時間。

因為本機電腦是使用**英文 （美國）** 選項**地區及語言選項**中**控制台**，開始日期的格式為 MM/DD/YYYY.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc once /st 14:18 /sd 11/13/2002
```

### <a name="BKMK_diff_perms"></a>若要排程執行具有不同的權限的工作

您可以排定以本機和遠端電腦上的其他帳戶的權限執行的所有類型的工作。 除了針對特定的排程類型，所需的參數 **/ru**是必要參數和 **/rp**參數是選擇性的。

#### <a name="examples"></a>範例

#### <a name="to-run-a-task-with-administrator-permissions-on-the-local-computer"></a>若要在本機電腦上執行系統管理員權限的工作

下列命令會排定在本機電腦上執行，MyApp 程式。 它會使用 **/ru** ，指定工作應該執行的使用者的系統管理員帳戶 (Admin06) 權限。 在此範例中，排程工作執行每個星期二，但您可以使用任何的排程類型的工作，以替代的權限執行。
```
schtasks /create /tn "My App" /tr myapp.exe /sc weekly /d TUE /ru Admin06
```
在回應中， **SchTasks.exe** Admin06 帳戶的 「 執行身分 」 密碼會提示您輸入，然後顯示 成功的訊息。
```
Please enter the run as password for Admin06: ********
SUCCESS: The scheduled task "My App" has successfully been created.
```

#### <a name="to-run-a-task-with-alternate-permissions-on-a-remote-computer"></a>在遠端電腦上，使用替代的權限執行工作

下列命令會排定行銷電腦上執行每隔四天，MyApp 程式。

此命令會使用 **/sc**參數來指定每日排程並 **/月**參數來指定四天的間隔。

此命令會使用 **/s**參數來提供遠端電腦的名稱， **/u**參數來指定具有將工作排定為在遠端電腦上的權限的帳戶 (Admin01 上行銷電腦）。 它也會使用 **/ru**參數來指定工作應該執行使用者的非系統管理員帳戶的權限 (User01 Reskits 網域中的)。 不含 **/ru**參數，工作會執行所指定之帳戶的權限 **/u**。
```
schtasks /create /tn "My App" /tr myapp.exe /sc daily /mo 4 /s Marketing /u Marketing\Admin01 /ru Reskits\User01
```
**Schtasks**所命名之使用者的密碼會先要求 **/u**參數 （若要執行的命令），並接著要求所命名之使用者的密碼 **/ru** （以執行工作） 的參數。 驗證密碼之後, **schtasks**會顯示一則訊息指出，排程工作。
```
Type the password for Marketing\Admin01:********

Please enter the run as password for Reskits\User01: ********

SUCCESS: The scheduled task "My App" has successfully been created.
```

#### <a name="to-run-a-task-only-when-a-particular-user-is-logged-on"></a>若要執行的工作，只有當特定使用者登入

下列命令會排定 AdminCheck.exe 程式在電腦上執行公用每星期五會在上午 4:00，但僅限於如果電腦的系統管理員身分登入。

此命令會使用 **/sc**參數來指定每週排程 **/d**參數來指定日子，而 **/st**參數來指定開始時間。

此命令會使用 **/s**參數來提供遠端電腦的名稱並 **/u**參數來指定具有將工作排定為在遠端電腦上的權限的帳戶。 它也會使用 **/ru**參數來設定使用的公用電腦 (Public\Admin01) 的系統管理員權限來執行工作並 **/it**參數來指出只執行工作當 Public\Admin01 被登入的帳戶。
```
schtasks /create /tn "Check Admin" /tr AdminCheck.exe /sc weekly /d FRI /st 04:00 /s Public /u Domain3\Admin06 /ru Public\Admin01 /it
```
**注意**
-   使用互動式專用識別工作 ( **/it**) 屬性，使用詳細資訊的查詢 **(/ 查詢 /v**)。 中的工作的詳細資訊的查詢顯示 **/it**，則**登入模式**欄位有值為**僅互動**。

### <a name="BKMK_sys_perms"></a>若要排程執行具有系統權限的工作

使用系統帳戶的權限可以執行的所有類型的工作，在本機和遠端電腦上。 除了針對特定的排程類型，所需的參數 **/ru 系統**(或 **/ru"」** ) 是必要參數和 **/rp**參數無效。

**重要事項**
-   系統帳戶沒有互動登入權限。 使用者將無法檢視或與程式互動，或擁有系統管理權限執行的工作。
-   **/Ru**參數會決定工作執行，無法用來排程工作的權限的權限。 只有系統管理員可以排定工作，不論值為何 **/ru**參數。

**注意**

若要識別擁有系統管理權限執行的工作，請使用 詳細資訊的查詢 ( **/查詢** **/v**)。 在系統執行工作的詳細資訊的查詢顯示**使用者的身分執行**欄位有值為**NT AUTHORITY\SYSTEM**而**登入模式**欄位有值為**只有背景**。

#### <a name="examples"></a>範例

#### <a name="to-run-a-task-with-system-permissions"></a>若要使用系統權限執行工作

下列命令會排程在系統帳戶的權限的本機電腦上執行，MyApp 程式。 在此範例中，排程工作的每個月，第 15 天執行，但您可以使用任何的排程類型擁有系統管理權限執行的工作。

此命令會使用 **/ru 系統**參數來指定系統的安全性內容。 因為系統工作不會使用密碼 **/rp**省略參數。
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /d 15 /ru System
```
在回應中， **SchTasks.exe**顯示告知性訊息和成功訊息。 它不會提示輸入密碼。
```
INFO: The task will be created under user name ("NT AUTHORITY\SYSTEM").
SUCCESS: The Scheduled task "My App" has successfully been created.
```

#### <a name="to-run-a-task-with-system-permissions-on-a-remote-computer"></a>若要在遠端電腦上，使用系統權限執行工作

下列命令排程在上午 4:00 每天早上執行 Finance01 電腦上將 MyApp 程式 使用系統權限。

此命令會使用 **/tn**參數來命名工作並**t**參數指定 MyApp 程式的遠端副本。 它會使用 **/sc**參數指定每日排程，但省略 **/月**參數，因為 1 （每天） 是預設值。 它會使用 **/st**參數來指定開始時間，這也是在工作執行每一天的時間。

此命令會使用 **/s**參數來提供遠端電腦的名稱並 **/u**參數來指定具有將工作排定為在遠端電腦上的權限的帳戶。 它也會使用 **/ru**參數來指定工作應該在系統帳戶下執行。 不含 **/ru**參數，工作會執行所指定之帳戶的權限 **/u**。
```
schtasks /create /tn "My App" /tr myapp.exe /sc daily /st 04:00 /s Finance01 /u Admin01 /ru System
```
**Schtasks**要求所命名之使用者的密碼 **/u**參數和驗證密碼之後, 會顯示訊息，指出工作建立，且它會執行與系統的權限帳戶。
```
Type the password for Admin01:**********

INFO: The Schedule Task "My App" will be created under user name ("NT AUTHORITY\
SYSTEM").
SUCCESS: The scheduled task "My App" has successfully been created.
```

### <a name="BKMK_multi_progs"></a>若要排程的工作執行一個以上的程式

每個工作會執行只有一個程式。 不過，您可以建立批次檔執行多個程式，並接著排程要執行批次檔的工作。 下列程序會示範這個方法：
1. 建立批次檔啟動您想要執行的程式。

   在此範例中，您會建立批次檔啟動事件檢視器 (Eventvwr.exe) 和系統監視器 (Perfmon.exe)。  
   - 開啟文字編輯器，例如記事本。
   - 輸入名稱和每個程式的可執行檔的完整的路徑。 在此情況下，該檔案包含下列陳述式。  
     ```
     C:\Windows\System32\Eventvwr.exe 
     C:\Windows\System32\Perfmon.exe
     ```  
   - 將檔案儲存為 MyApps.bat 中。
2. 使用**Schtasks.exe**建立執行 MyApps.bat 的工作。

   下列命令會建立監視工作中，每當任何人登入時執行。 它會使用 **/tn**參數來命名工作，而**t**執行 MyApps.bat 的參數。 它會使用 **/sc**參數來指出登入時的排程類型並 **/ru**以使用者的系統管理員帳戶的權限執行工作的參數。  
   ```
   schtasks /create /tn Monitor /tr C:\MyApps.bat /sc onlogon /ru Reskit\Administrator
   ```  
   由於此命令，每當使用者登入電腦，工作會啟動事件檢視器和系統監視器。

### <a name="BKMK_remote"></a>排程會在遠端電腦執行的工作

若要排程要在遠端電腦上執行的工作，您必須將工作加入遠端電腦的排程。 可以排程工作的所有類型的遠端電腦上，但必須符合下列條件。
-   您必須排程工作的權限。 因此，您必須以本機電腦的遠端電腦上的 Administrators 群組成員的帳戶登入，或您必須使用 **/u**參數，以提供的遠端電腦的系統管理員認證.
-   您可以使用 **/u**參數在本機和遠端電腦位於相同網域或本機電腦是在遠端電腦的網域信任的網域中時，才。 否則，在遠端電腦無法驗證指定的使用者帳戶，而且無法確認帳戶是 Administrators 群組的成員。
-   工作必須有足夠的權限在遠端電腦上執行。 所需的權限會隨著工作。 根據預設，工作執行時會使用本機電腦的目前使用者的權限或者，如果 **/u**參數，工作執行所指定之帳戶的權限 **/u**參數。 不過，您可以使用 **/ru**參數來執行工作，與不同的使用者帳戶的權限或系統權限。

#### <a name="examples"></a>範例

#### <a name="an-administrator-schedules-a-task-on-a-remote-computer"></a>系統管理員排程遠端電腦上的工作

下列命令會排定執行 SRV01 遠端電腦上立即開始每隔 10 天，MyApp 程式。 此命令會使用 **/s**參數來提供遠端電腦的名稱。 因為本機的目前使用者是遠端電腦的系統管理員 **/u**參數，可提供替代的排程工作的權限，不需要。

請注意，當排程工作，在遠端電腦上的，所有參數都指的遠端電腦。 因此，所指定的可執行檔**t**參數指的是 MyApp.exe 的複本，在遠端電腦上。
```
schtasks /create /s SRV01 /tn "My App" /tr "c:\program files\corpapps\myapp.exe" /sc daily /mo 10
```
在回應中， **schtasks**會顯示成功訊息，指出排定的工作。

#### <a name="a-user-schedules-a-command-on-a-remote-computer-case-1"></a>使用者在安排 (案例 1) 在遠端電腦上的命令

下列命令會排定 SRV06 遠端電腦上執行每隔三個小時，MyApp 程式。 因為系統管理員權限，才能排程的工作，此命令會使用 **/u**並 **/p**參數提供的認證，使用者的系統管理員帳戶 (Admin01 Reskits 中網域）。 根據預設，這些權限也可執行工作。 不過，因為工作不需要執行的系統管理員權限，此命令包含 **/u**並 **/rp**參數覆寫預設值，並執行工作之使用者的權限在遠端電腦上的非系統管理員帳戶。
```
schtasks /create /s SRV06 /tn "My App" /tr "c:\program files\corpapps\myapp.exe" /sc hourly /mo 3 /u reskits\admin01 /p R43253@4$ /ru SRV06\user03 /rp MyFav!!Pswd
```
在回應中， **schtasks**會顯示成功訊息，指出排定的工作。

#### <a name="a-user-schedules-a-command-on-a-remote-computer-case-2"></a>使用者在安排 (案例 2) 在遠端電腦上的命令

下列命令會排定 SRV02 遠端電腦上執行的每個月的最後一天，MyApp 程式。 因為本機的目前使用者 (user03) 不是遠端電腦的系統管理員，此命令會使用 **/u**參數，以提供使用者的系統管理員的認證帳戶 (Admin01 Reskits 網域中的)。 排程工作，以及執行工作，將使用系統管理員帳戶權限。
```
schtasks /create /s SRV02 /tn "My App" /tr "c:\program files\corpapps\myapp.exe" /sc monthly /mo LASTDAY /m * /u reskits\admin01
```
因為命令未包含 **/p** （密碼） 參數**schtasks**會提示輸入密碼。 然後它會顯示成功訊息，而在此情況下，一則警告。
```
Type the password for reskits\admin01:********

SUCCESS: The scheduled task "My App" has successfully been created.

WARNING: The Scheduled task "My App" has been created, but may not run because
the account information could not be set.
```
這則警告表示遠端網域無法驗證所指定的帳戶 **/u**參數。 在此情況下，遠端網域無法驗證的使用者帳戶，因為本機電腦不是遠端電腦的網域信任之網域的成員。 當發生這種情況時，工作作業會出現在清單中的排程工作，但工作是實際上是空的它不會執行。

下列顯示詳細資訊的查詢會顯示工作的問題。 在顯示中，請注意，值**下次執行時間**是**永不**且的值**使用者的身分執行**是**無法擷取來自工作排程器資料庫**。

假設這台電腦相同的網域或受信任的網域的成員時，工作會有已成功排程，而且可能會如同指定。
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

-   若要執行 **/create**命令搭配不同的使用者，使用的權限 **/u**參數。 **/U**參數是僅適用於排程工作，在遠端電腦上的。
-   若要檢視更多**schtasks /create**範例中，輸入**schtasks / 建立 /？** 。
-   若要排程執行具有不同的使用者的權限的工作，請使用 **/ru**參數。 **/Ru**參數都是有效的本機和遠端電腦上的工作。
-   若要使用 **/u**參數與遠端電腦位於相同網域中必須是本機電腦或必須位在遠端電腦的網域信任的網域。 否則，可能是未建立工作時，或工作作業是空的工作不會執行。
-   **Schtasks**永遠提示輸入密碼，除非您提供一個，即使您排程使用目前的使用者帳戶的本機電腦上的工作。 這是正常情形**schtasks**。
-   **Schtasks**不會驗證程式檔案位置] 或 [使用者帳戶密碼。 如果您沒有輸入正確的檔案位置或正確的密碼的使用者帳戶，建立工作時，但它不會執行。 此外，如果帳戶的密碼變更或過期，而且您不要變更儲存在工作中的密碼，則工作不會不執行。
-   系統帳戶沒有互動登入權限。 使用者看不到，並無法與擁有系統管理權限執行的程式進行互動。
-   每個工作會執行只有一個程式。 不過，您可以建立啟動多個工作，批次檔，並再排程執行的批次檔的工作。
-   您建立時，您可以測試工作。 使用**執行**測試工作，然後檢查 SchedLgU.txt 檔案作業 (*SystemRoot*\SchedLgU.txt) 是否有錯誤。

## <a name="BKMK_change"></a>schtasks 變更

變更一或多個工作的下列屬性。
-   工作執行之程式 (**t**)。
-   工作所執行的使用者帳戶 ( **/ru**)。
-   使用者帳戶的密碼 ( **/rp**)。
-   將互動式專用的屬性加入至工作 ( **/it**)。

### <a name="syntax"></a>語法

```
schtasks /change /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]] [/ru {[<Domain>\]<User> | System}] [/rp <Password>] [/tr <TaskRun>] [/st <StartTime>] [/ri <Interval>] [{/et <EndTime> | /du <Duration>} [/k]] [/sd <StartDate>] [/ed <EndDate>] [/{ENABLE | DISABLE}] [/it] [/z]
```

### <a name="parameters"></a>參數

|          詞彙           |                                                                                                                                                                                                                                                                                                                                     定義                                                                                                                                                                                                                                                                                                                                      |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /tn\<工作名稱 >     |                                                                                                                                                                                                                                                                                                               識別變更的工作。 輸入工作名稱。                                                                                                                                                                                                                                                                                                               |
|     /s\<電腦 >      |                                                                                                                                                                                                                                                                               指定的名稱或遠端電腦的 IP 位址 （或不含反斜線）。 預設是本機電腦。                                                                                                                                                                                                                                                                               |
|  /u [\<網域 >\]<User>  |                                                                                                                                                                 使用指定的使用者帳戶的權限執行此命令。 預設為本機電腦的目前使用者的權限。 指定的使用者帳戶必須是遠端電腦上 Administrators 群組的成員。 **/U**並 **/p**參數是僅適用於變更遠端電腦上的工作 ( **/s**)。                                                                                                                                                                  |
|     /p \<Password>      |                                                                                                                                                                                              指定在指定的使用者帳戶的密碼 **/u**參數。 如果您使用 **/u**參數，但省略 **/p**參數或 password 引數**schtasks**會提示您輸入密碼。</br>**/U**並 **/p**參數都有效只有當您使用 **/s**。                                                                                                                                                                                               |
| /ru {[\<Domain>\]<User> |                                                                                                                                                                                                                                                                                                                                       系統}                                                                                                                                                                                                                                                                                                                                       |
|     /rp\<密碼 >     |                                                                                                                                                                                                                                                 指定現有的使用者帳戶，或所指定的使用者帳戶的新密碼 **/ru**參數。 會忽略這個參數使用的本機系統帳戶搭配使用。                                                                                                                                                                                                                                                  |
|     t \<TaskRun >      |                                                                                                                                                                                  變更工作執行的程式。 輸入可執行檔、 指令碼檔案或批次檔的完整的路徑和檔案名稱。 如果您省略路徑， **schtasks**假設檔案是在\<systemroot > \System32 目錄。 指定的程式會取代原始工作所執行的程式。                                                                                                                                                                                  |
|    /st \<Starttime >     |                                                                                                                                                                                                                                                              指定工作中，使用 24 小時的時間格式 hh: mm 的開始時間。 例如，值為 14:30 就相當於 12 小時制的時間的下午 2:30。                                                                                                                                                                                                                                                               |
|     /ri\<間隔 >     |                                                                                                                                                                                                                                                                           指定的重複間隔排程的工作，以分鐘為單位。 有效範圍是 1-599940 （599940 分鐘 = 9999 小時）。                                                                                                                                                                                                                                                                            |
|     /et \<EndTime >      |                                                                                                                                                                                                                                                               指定工作中，使用 24 小時的時間格式 hh: mm 的結束時間。 例如，值為 14:30 就相當於 12 小時制的時間的下午 2:30。                                                                                                                                                                                                                                                                |
|     /du\<持續時間 >     |                                                                                                                                                                                                                                                                                                     指定在關閉工作\<EndTime > 或<Duration>，如果指定了。                                                                                                                                                                                                                                                                                                      |
|           /k            |                                                                                                                                                                   工作在所指定的時間執行的程式就會停止 **/et**或是 **/du**。 不含 **/k**， **schtasks**後未啟動程式再次達到所指定的時間 **/et**或是 **/du**，但它不會停止如果仍在執行程式。 這個參數是選擇性的只以分鐘或每小時排程才有效。                                                                                                                                                                   |
|    /sd \<StartDate>     |                                                                                                                                                                                                                                                                                              指定工作應該執行的第一個日期。 日期格式為 MM/DD/YYYY。                                                                                                                                                                                                                                                                                               |
|     /ed\<結束日期 >      |                                                                                                                                                                                                                                                                                                 指定工作應該執行的最後一個日期。 格式為 MM/DD/YYYY。                                                                                                                                                                                                                                                                                                  |
|         /ENABLE         |                                                                                                                                                                                                                                                                                                                       指定要啟用排定的工作。                                                                                                                                                                                                                                                                                                                       |
|        /DISABLE         |                                                                                                                                                                                                                                                                                                                      指定要停用排定的工作。                                                                                                                                                                                                                                                                                                                       |
|           /it           | 指定要執行排定的工作時，才 「 執行身分 」 使用者 （工作所執行的使用者帳戶） 登入電腦。</br>此參數有上擁有系統管理權限執行的工作或工作已設定的僅限互動式屬性沒有作用。 您無法使用變更命令來移除工作中的互動式專用屬性。</br>根據預設，「 執行身分 」 使用者是本機電腦時排定工作的目前的使用者或所指定的帳戶 **/u**參數，如果使用的話。 不過，如果此命令包含 **/ru**參數，然後 「 執行身分 」 使用者是所指定的帳戶 **/ru**參數。 |
|           /z            |                                                                                                                                                                                                                                                                                                          指定要刪除完成時它的排程工作。                                                                                                                                                                                                                                                                                                          |
|           /?            |                                                                                                                                                                                                                                                                                                                        在命令提示字元顯示說明。                                                                                                                                                                                                                                                                                                                         |

### <a name="remarks"></a>備註

-   **/Tn**並 **/s**參數識別工作。 **/Tr**， **/ru**，並 **/rp**參數指定的工作，您可以變更的屬性。
-   **/Ru**，並 **/rp**參數會指定執行工作的權限。 **/U**並 **/p**參數會指定用來變更工作的權限。
-   若要變更遠端電腦上的工作，使用者必須被登入到本機電腦的遠端電腦上的 Administrators 群組成員的帳戶。
-   若要執行 **/變更**命令搭配不同的使用者權限 ( **/u**， **/p**)，本機電腦必須位於與遠端電腦位於相同網域或網域中必須是遠端電腦的網域信任。
-   系統帳戶沒有互動登入權限。 使用者看不到，並無法與擁有系統管理權限執行的程式進行互動。
-   若要識別與工作 **/it**  屬性中使用的詳細資訊的查詢 ( **/查詢 /v**)。 中的工作的詳細資訊的查詢顯示 **/it**，則**登入模式**欄位有值為**僅互動**。

### <a name="examples"></a>範例

### <a name="to-change-the-program-that-a-task-runs"></a>若要變更的工作會執行程式

下列命令會變更病毒檢查工作執行從 VirusCheck.exe VirusCheck2.exe 的程式。 此命令會使用 **/tn**參數來識別工作並**t**參數來指定工作的新程式。 （您無法變更工作的名稱）。
```
schtasks /change /tn "Virus Check" /tr C:\VirusCheck2.exe
```
在回應中， **SchTasks.exe**會顯示下列成功訊息：
```
SUCCESS: The parameters of the scheduled task "Virus Check" have been changed.
```
此命令的結果病毒檢查工作現在會執行 VirusCheck2.exe。

### <a name="to-change-the-password-for-a-remote-task"></a>若要變更遠端工作的密碼

下列命令會變更的遠端電腦上，Svr01 RemindMe 工作的使用者帳戶的密碼。 此命令會使用 **/tn**參數來識別工作並 **/s**參數來指定遠端電腦。 它會使用 **/rp**參數來指定新的密碼， p@ssWord3。

每當使用者帳戶的密碼過期，或變更時，就需要這個程序。 如果在工作中所儲存的密碼已不再有效，則工作不會不會執行。
```
schtasks /change /tn RemindMe /s Svr01 /rp p@ssWord3
```
在回應中， **SchTasks.exe**會顯示下列成功訊息：
```
SUCCESS: The parameters of the scheduled task "RemindMe" have been changed.
```
此命令的結果 RemindMe 工作現在會執行其原始的使用者帳戶，但以新密碼。

### <a name="to-change-the-program-and-user-account-for-a-task"></a>若要變更工作的程式和使用者帳戶

下列命令會變更的工作執行的程式和變更的使用者帳戶下執行的工作。 基本上，它會使用舊的排程工作。 此命令會變更 ChkNews 工作中，在上午 9:00，每天早上啟動 Notepad.exe，改為啟動 Internet Explorer。

此命令會使用 **/tn**參數來識別工作。 它會使用 **/tr**參數來變更工作執行的程式和 **/ru**參數來變更工作所執行的使用者帳戶。

**/Ru**，並 **/rp**省略參數，可提供的使用者帳戶的密碼。 您必須提供密碼的帳戶，但您可以使用 **/ru**，並 **/rp**參數和類型的密碼在純文字，或等待**SchTasks.exe**提示您輸入密碼，然後以遮蔽文字輸入的密碼。
```
schtasks /change /tn ChkNews /tr "c:\program files\Internet Explorer\iexplore.exe" /ru DomainX\Admin01
```
在回應中， **SchTasks.exe**要求的使用者帳戶的密碼。 會模糊您所鍵入的文字，因此看不到密碼。
```
Please enter the password for DomainX\Admin01: 
```
請注意， **/tn**參數識別工作，並 **/tr**並 **/ru**參數變更工作的屬性。 您無法使用另一個參數來識別工作，而且您無法變更工作名稱。

在回應中， **SchTasks.exe**會顯示下列成功訊息：
```
SUCCESS: The parameters of the scheduled task "ChkNews" have been changed.
```
此命令的結果 ChkNews 工作現在執行 Internet Explorer 的系統管理員帳戶權限。

### <a name="to-change-a-program-to-the-system-account"></a>若要將程式變更為 「 系統 」 帳戶

下列命令會變更 SecurityScript 工作，使其執行的系統帳戶的權限。 它會使用 **/ru""** 參數，以指定系統帳戶。
```
schtasks /change /tn SecurityScript /ru ""
```
在回應中， **SchTasks.exe**會顯示下列成功訊息：
```
INFO: The run as user name for the scheduled task "SecurityScript" will be changed to "NT AUTHORITY\SYSTEM".
SUCCESS: The parameters of the scheduled task "SecurityScript" have been changed.
```
使用系統帳戶權限執行的工作不需要密碼，因為**SchTasks.exe**不會提示的其中一個。

### <a name="to-run-a-program-only-when-i-am-logged-on"></a>只有當我登入時執行程式

下列命令會將僅限互動的屬性加入至 MyApp，現有的工作。 這個屬性可確保只有在 「 執行身分 」 使用者，也就是在其下的工作執行時，使用者帳戶登入電腦時，才執行工作。

此命令會使用 **/tn**參數來識別工作並 **/it**將僅限互動式屬性新增至工作的參數。 因為工作已執行我的使用者帳戶的權限時，我不需要變更 **/ru**工作參數。
```
schtasks /change /tn MyApp /it
```
在回應中， **SchTasks.exe**會顯示下列成功訊息。
```
SUCCESS: The parameters of the scheduled task "MyApp" have been changed.
```

## <a name="BKMK_run"></a>schtasks 執行

立即啟動排程的工作。 **執行**作業會忽略排程，但會使用的程式檔案位置、 使用者帳戶和密碼儲存在工作立即執行的工作。

### <a name="syntax"></a>語法

```
schtasks /run /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>參數

|         詞彙          |                                                                                                                                                                 定義                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /tn\<工作名稱 >    |                                                                                                                                                       必要。 識別工作。                                                                                                                                                        |
|    /s\<電腦 >     |                                                                                                           指定的名稱或遠端電腦的 IP 位址 （或不含反斜線）。 預設是本機電腦。                                                                                                           |
| /u [\<網域 >\]<User> | 使用指定的使用者帳戶的權限執行此命令。 根據預設，本機電腦的目前使用者的權限執行命令。</br>指定的使用者帳戶必須是遠端電腦上 Administrators 群組的成員。 **/U**並 **/p**參數都有效只有當您使用 **/s**。 |
|    /p \<Password>     |                          指定在指定的使用者帳戶的密碼 **/u**參數。 如果您使用 **/u**參數，但省略 **/p**參數或 password 引數**schtasks**會提示您輸入密碼。</br>**/U**並 **/p**參數都有效只有當您使用 **/s**。                           |
|          /?           |                                                                                                                                                    在命令提示字元顯示說明。                                                                                                                                                     |

### <a name="remarks"></a>備註

-   若要測試您的工作中使用這項作業。 如果工作未執行，請檢查工作排程器服務的交易記錄檔， \<Systemroot > \SchedLgU.txt，是否有錯誤。
-   執行工作並不會影響工作排程，並不會變更排程工作的下次執行時間。
-   若要從遠端執行的工作，必須在遠端電腦上排程工作。 當您執行它時，工作會執行只能在遠端電腦上。 若要確認已在遠端電腦上執行一項工作，請使用 工作管理員 或 工作排程器的交易記錄檔， \<Systemroot > \SchedLgU.txt。

### <a name="examples"></a>範例

### <a name="to-run-a-task-on-the-local-computer"></a>若要在本機電腦上執行工作

下列命令會啟動 「 安全性指令碼 」 工作。
```
schtasks /run /tn "Security Script"
```
在回應中， **SchTasks.exe**啟動與工作相關聯的指令碼，並顯示下列訊息：
```
SUCCESS: Attempted to run the scheduled task "Security Script".
```
正如其訊息， **schtasks**會嘗試啟動程式，但非常不實際啟動程式。

### <a name="to-run-a-task-on-a-remote-computer"></a>若要在遠端電腦上執行工作

下列命令會啟動 「 更新 」 工作的遠端電腦上，Svr01:
```
schtasks /run /tn Update /s Svr01
```
在此情況下， **SchTasks.exe**會顯示下列錯誤訊息：
```
ERROR: Unable to run the scheduled task "Update".
```
若要找出錯誤的原因，查詢排定的工作交易記錄檔，C:\Windows\SchedLgU.txt Svr01 上。 在此情況下，下列項目會出現在記錄檔：
```
"Update.job" (update.exe) 3/26/2001 1:15:46 PM ** ERROR **
The attempt to log on to the account associated with the task failed, therefore, the task did not run.
The specific error is:
0x8007052e: Logon failure: unknown user name or bad password.
Verify that the task's Run-as name and password are valid and try again.
```
很顯然的使用者名稱或工作中的密碼不正確的系統上。 下列**schtasks /change**命令可更新的使用者名稱和密碼 Svr01 的 「 更新 」 工作：
```
schtasks /change /tn Update /s Svr01 /ru Administrator /rp PassW@rd3
```
在後**變更**命令完成時，**執行**重複命令。 此時，Update.exe 程式啟動並**SchTasks.exe**顯示下列訊息：
```
SUCCESS: Attempted to run the scheduled task "Update".
```
正如其訊息， **schtasks**會嘗試啟動程式，但非常不實際啟動程式。

## <a name="BKMK_end"></a>schtasks 結束

停止啟動工作的程式。

### <a name="syntax"></a>語法

```
schtasks /end /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>參數

|         詞彙          |                                                                                                                                                               定義                                                                                                                                                                |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /tn\<工作名稱 >    |                                                                                                                                         必要。 識別啟動程式的工作。                                                                                                                                         |
|    /s\<電腦 >     |                                                                                                                        指定的名稱或遠端電腦的 IP 位址。 預設是本機電腦。                                                                                                                        |
| /u [\<網域 >\]<User> | 使用指定的使用者帳戶的權限執行此命令。 根據預設，本機電腦的目前使用者的權限執行命令。 指定的使用者帳戶必須是遠端電腦上 Administrators 群組的成員。 **/U**並 **/p**參數都有效只有當您使用 **/s**。 |
|    /p \<Password>     |                        指定在指定的使用者帳戶的密碼 **/u**參數。 如果您使用 **/u**參數，但省略 **/p**參數或 password 引數**schtasks**會提示您輸入密碼。</br>**/U**並 **/p**參數都有效只有當您使用 **/s**。                         |
|          /?           |                                                                                                                                                             顯示說明。                                                                                                                                                              |

### <a name="remarks"></a>備註

**SchTasks.exe**結束的程式，啟動排程工作的執行個體。 若要停止其他處理序，使用 TaskKill。 如需詳細資訊，請參閱 < [Taskkill](taskkill.md)。

### <a name="examples"></a>範例

### <a name="to-end-a-task-on-a-local-computer"></a>若要結束 本機電腦上的工作

下列命令會停止已啟動我的 「 記事本 」 工作的 Notepad.exe 的執行個體：
```
schtasks /end /tn "My Notepad"
```
在回應中， **SchTasks.exe**停止 Notepad.exe，啟動工作，而且它會顯示下列成功訊息的執行個體：
```
SUCCESS: The scheduled task "My Notepad" has been terminated successfully.
```

### <a name="to-end-a-task-on-a-remote-computer"></a>若要結束遠端電腦上的工作

下列命令會停止已啟動的遠端電腦上，Svr01 InternetOn 工作的 Internet Explorer 的執行個體：
```
schtasks /end /tn InternetOn /s Svr01
```
在回應中， **SchTasks.exe**停止，啟動工作，而且它會顯示下列成功訊息的 Internet Explorer 的執行個體：
```
SUCCESS: The scheduled task "InternetOn" has been terminated successfully.
```

## <a name="BKMK_delete"></a>schtasks delete

刪除排程的工作。

### <a name="syntax"></a>語法

```
schtasks /delete /tn {<TaskName> | *} [/f] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>參數

|         詞彙          |                                                                                                                                                                 定義                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   /tn {\<工作名稱 >    |                                                                                                                                                                     \*}                                                                                                                                                                     |
|          /f           |                                                                                                                                  抑制確認訊息。 此工作是已刪除，而不發出警告。                                                                                                                                  |
|    /s\<電腦 >     |                                                                                                           指定的名稱或遠端電腦的 IP 位址 （或不含反斜線）。 預設是本機電腦。                                                                                                           |
| /u [\<網域 >\]<User> | 使用指定的使用者帳戶的權限執行此命令。 根據預設，本機電腦的目前使用者的權限執行命令。</br>指定的使用者帳戶必須是遠端電腦上 Administrators 群組的成員。 **/U**並 **/p**參數都有效只有當您使用 **/s**。 |
|    /p \<Password>     |                          指定在指定的使用者帳戶的密碼 **/u**參數。 如果您使用 **/u**參數，但省略 **/p**參數或 password 引數**schtasks**會提示您輸入密碼。</br>**/U**並 **/p**參數都有效只有當您使用 **/s**。                           |
|          /?           |                                                                                                                                                    在命令提示字元顯示說明。                                                                                                                                                     |

### <a name="remarks"></a>備註

- **刪除**作業會刪除排程工作。 它不會刪除該工作執行程式或中斷執行中的程式。
- **刪除\\** * 命令會刪除所有排程工作的電腦，不只是目前的使用者已排程的工作。

### <a name="examples"></a>範例

### <a name="to-delete-a-task-from-the-schedule-of-a-remote-computer"></a>若要刪除的排程中的遠端電腦的工作

下列命令會刪除 「 啟動郵件 」 工作，從遠端電腦的排程。 它會使用 **/s**參數來識別遠端電腦。
```
schtasks /delete /tn "Start Mail" /s Svr16
```
在回應中， **SchTasks.exe**會顯示下列確認訊息。 若要刪除的工作，請按下 Y<strong>。</strong>若要取消命令，請輸入**n**:
```
WARNING: Are you sure you want to remove the task "Start Mail" (Y/N )? 
SUCCESS: The scheduled task "Start Mail" was successfully deleted.
```

### <a name="to-delete-all-tasks-scheduled-for-the-local-computer"></a>若要刪除已排程的本機電腦的所有工作

下列命令會刪除所有工作，從本機電腦，包括其他使用者已排程工作的排程。 它會使用 **/tn \\** * 來代表電腦上的所有工作的參數和 **/f**參數來抑制確認訊息。
```
schtasks /delete /tn * /f
```
在回應中， **SchTasks.exe**顯示指出排程，唯一的工作 SecureScript，會刪除下列成功訊息。

`SUCCESS: The scheduled task "SecureScript" was successfully deleted.`

## <a name="BKMK_query"></a>schtasks 查詢

顯示排程要在電腦上執行的工作。

### <a name="syntax"></a>語法

```
schtasks [/query] [/fo {TABLE | LIST | CSV}] [/nh] [/v] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>參數

|         詞彙          |                                                                                                                                                                 定義                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       [/query]        |                                                                                                                        作業名稱是選擇性的。 鍵入**schtasks**沒有任何參數執行查詢。                                                                                                                         |
|      /fo {TABLE       |                                                                                                                                                                    清單                                                                                                                                                                     |
|          /nh          |                                                                                                            省略資料行從資料表顯示的標題。 此參數才有效使用**表格**並**CSV**輸出格式。                                                                                                             |
|          /v           |                                                                                                         將顯示進階的屬性的工作。</br>使用查詢 **/v**的格式應為**清單**或是**CSV**。                                                                                                          |
|    /s\<電腦 >     |                                                                                                           指定的名稱或遠端電腦的 IP 位址 （或不含反斜線）。 預設是本機電腦。                                                                                                           |
| /u [\<網域 >\]<User> | 使用指定的使用者帳戶的權限執行此命令。 根據預設，本機電腦的目前使用者的權限執行命令。</br>指定的使用者帳戶必須是遠端電腦上 Administrators 群組的成員。 **/U**並 **/p**參數都有效只有當您使用 **/s**。 |
|    /p \<Password>     |                                        指定在指定的使用者帳戶的密碼 **/u**參數。 如果您使用 **/u**，但省略 **/p**或 password 引數**schtasks**會提示您輸入密碼。</br>**/U**並 **/p**參數都有效只有當您使用 **/s**。                                         |
|          /?           |                                                                                                                                                    在命令提示字元顯示說明。                                                                                                                                                     |

### <a name="remarks"></a>備註

**SchTasks.exe**結束的程式，啟動排程工作的執行個體。 若要停止其他處理序，使用 TaskKill。 如需詳細資訊，請參閱 < [Taskkill](taskkill.md)。

### <a name="examples"></a>範例

### <a name="to-display-the-scheduled-tasks-on-the-local-computer"></a>若要顯示在本機電腦上的排程的工作

下列命令會顯示已排程的本機電腦的所有工作。 這些命令會產生相同的結果，並可交換使用。
```
schtasks
schtasks /query
```
在回應中， **SchTasks.exe**在預設的簡單的表格格式顯示的工作下, 表所示：
```
TaskName Next Run Time Status
========================= ======================== ==============
Microsoft Outlook At logon time
SecureScript 14:42:00 PM , 2/4/2001
```

### <a name="to-display-advanced-properties-of-scheduled-tasks"></a>若要顯示進階的屬性的排程工作

下列命令會要求在本機電腦上工作的詳細的顯示。 它會使用 **/v**參數，要求的詳細 (verbose) 顯示並 **/fo 清單**參數，以方便閱讀清單格式顯示。 若要確認您所建立的工作有預定的循環模式，您可以使用此命令。

**schtasks /query /fo 清單 /v**

在回應中， **SchTasks.exe**會顯示所有工作的詳細的屬性清單。 下列畫面顯示工作清單中的排程在上午 4:00 執行的工作 每個月的最後一個星期五：
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

下列命令會要求的遠端電腦時，排程工作的清單，並將工作新增至以逗號分隔記錄檔在本機電腦上。 您可以使用此命令格式來收集並追蹤對多部電腦排程工作。

此命令會使用 **/s**參數來識別遠端電腦，也就是 Reskit16， **/fo**參數指定的格式和 **/nh**參數，以隱藏資料行標題。 **>>** 將符號重新導向的輸出附加至工作記錄檔，p0102.csv，本機電腦，也就是 Svr01 上。 因為命令是在遠端電腦上執行，本機電腦路徑必須是完整名稱。
```
schtasks /query /s Reskit16 /fo csv /nh >> \\svr01\data\tasklogs\p0102.csv
```
在回應中， **SchTasks.exe**加入 p0102.csv 檔案的本機電腦上，Svr01 Reskit16 電腦的排程工作。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
