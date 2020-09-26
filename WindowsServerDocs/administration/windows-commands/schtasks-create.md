---
title: schtasks 建立
description: Schtasks create 命令的參考文章，其
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 09/16/2020
ms.openlocfilehash: 4a7c3eb621f4c3d640fcb1f057b9d3cd00834e31
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91390724"
---
# <a name="schtasks-create"></a>schtasks 建立

排程工作。

## <a name="syntax"></a>語法

```
schtasks /create /sc <scheduletype> /tn <taskname> /tr <taskrun> [/s <computer> [/u [<domain>\]<user> [/p <password>]]] [/ru {[<domain>\]<user> | system}] [/rp <password>] [/mo <modifier>] [/d <day>[,<day>...] | *] [/m <month>[,<month>...]] [/i <idletime>] [/st <starttime>] [/ri <interval>] [{/et <endtime> | /du <duration>} [/k]] [/sd <startdate>] [/ed <enddate>] [/it] [/z] [/f]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| /sc `<scheduletype>` | 指定排程類型。 有效的值包括：<ul><li>**分鐘** -指定工作應該執行之前的分鐘數。</li><li>**每小時** -指定應該執行工作的時數。</li><li>[**每日**]-指定工作應該執行的天數。</li><li>**每週** 指定工作應該執行之前的周數。</li><li>**每月** -指定應該執行工作的月數。</li><li>**一次** -指定該工作在指定的日期和時間執行一次。</li><li>**ONSTART** -指定在每次系統啟動時執行工作。 您可以指定開始日期，或在下次系統啟動時執行工作。</li><li>**整理** -指定每當使用者 (任何使用者) 登入時，就會執行此工作。 您可以指定日期，或在使用者下一次登入時執行工作。</li><li>**ONIDLE** -指定只要系統閒置一段指定的時間，就會執行該工作。 您可以指定日期，或在下次系統閒置時執行工作。</li></ul> |
| /tn `<taskname>` | 指定工作的名稱。 系統上的每個工作都必須有唯一的名稱，而且必須符合檔案名的規則，但不能超過238個字元。 使用引號來括住包含空格的名稱。 |
| /tr `<Taskrun>` | 指定工作執行的程式或命令。 輸入可執行檔、指令檔或批次檔的完整路徑和檔案名。 路徑名稱不能超過262個字元。 如果您未加入路徑， **schtasks** 會假設檔案位於 `<systemroot>\System32` 目錄中。 |
| /s `<computer>` | 指定遠端電腦的名稱或 IP 位址 (包含或不含反斜線) 。 預設是本機電腦。 |
| u `[<domain>]` | 使用指定之使用者帳戶的許可權來執行此命令。 預設值是本機電腦目前使用者的許可權。 只有當您使用 **/s**時， **/u**和 **/p**參數才有效。 指定之帳戶的許可權是用來排程工作和執行工作。 若要使用不同使用者的許可權來執行工作，請使用 **/ru** 參數。 使用者帳戶必須是遠端電腦上 Administrators 群組的成員。 此外，本機電腦必須位於與遠端電腦相同的網域中，或者必須位於遠端電腦網域信任的網域中。 |
| /p `<password>` | 指定 **/u** 參數中指定之使用者帳戶的密碼。 如果您在沒有 **/p**參數或 password 引數的情況下使用 **/u**參數，則 schtasks.exe 會提示您輸入密碼。 只有當您使用 **/s**時， **/u**和 **/p**參數才有效。 |
| /ru `{[<domain>\]<user> | system}` | 使用指定之使用者帳戶的許可權來執行工作。 根據預設，工作會以本機電腦目前使用者的許可權執行，或以 **/u** 參數所指定之使用者的許可權執行（如果包含的話）。 在本機或遠端電腦上排程工作時， **/ru** 參數是有效的。 有效的選項包括：<ul><li>**網域** -指定替代的使用者帳戶。</li><li>**系統** -指定本機系統帳戶，也就是作業系統和系統服務所使用的高度特殊許可權帳戶。</li></ul> |
| /rp `<password>` | 指定現有使用者帳戶的密碼，或由 **/ru** 參數指定的使用者帳戶。 如果您在指定使用者帳戶時未使用此參數，SchTasks.exe 會在您下次登入時提示您輸入密碼。 請勿針對以系統帳號憑證執行的工作使用 **/rp** 參數 (**/ru 系統**) 。 系統帳戶沒有密碼，且 SchTasks.exe 不會提示您輸入密碼。 |
| /mo `<modifiers>` | 指定工作在其排程類型內執行的頻率。 有效的選項包括：<ul><li>**分鐘** -指定每分鐘執行一次工作 <n> 。 您可以使用介於 1-1439 分鐘之間的任何值。 根據預設，這是1分鐘。</li><li>**每** 小時-指定每個小時執行一次工作 <n> 。 您可以使用介於 1-23 小時之間的任何值。 根據預設，這是1小時。</li><li>**每日** -指定每日執行一次工作 <n> 。 您可以使用 1-365 天之間的任何值。 根據預設，這是1天。</li><li>**每週** -指定每週執行一次工作 <n> 。 您可以使用介於 1-52 周之間的任何值。 根據預設，這是1周。</li><li>**每月** -指定每個月執行一次工作 <n> 。 您可以使用下列的任何值：<ul><li>介於 1-12 個月之間的數位</li><li>**LASTDAY** -在當月的最後一天執行工作</li><li>**第一個、第二個、第三個或第四個，以及 `/d <day>` 參數** -指定執行工作的特定周和日。 例如，在當月的第三個星期三。</li></ul></li><li>**一次** -指定工作執行一次。</li><li>**ONSTART** -指定在啟動時執行工作。</li><li>**整理** -指定當 **/u** 參數所指定的使用者登入時，執行工作。</li><li>**ONIDLE** -指定當系統閒置到 **/i** 參數指定的分鐘數之後，執行工作</li></ul> |
| /d DAY [，DAY ...] | 指定工作在其排程類型內執行的頻率。 有效的選項包括：<ul><li>**每週** -指定在1-52 周之間提供值，每週執行一次工作。 （選擇性）您也可以新增值為 [星期一] 或 [週一至 SUN ...] 的範圍，以新增一周的特定日子) 。</li><li>**每月** -指定每個月執行一次的工作，方法是提供第一個、第二、第三、第四、最後一個值。 （選擇性）您也可以新增值為周日，或在 1-12 個月之間提供數位，以新增一周的特定日子。 如果您使用此選項，您也可以提供介於1-31 之間的數位，以新增月份中的特定日期。<p>**注意：** 只有在沒有 **/mo** 參數的情況下，或如果 **/mo** 參數是每月 (1-12) ，才有效的日期值為 1-31。 預設值為第1天 () 月份的第一天。</li></ul> |
| /m MONTH [，MONTH ...] | 指定排程工作應該在一年中執行的月份或月份。 有效的選項包括 JAN-DEC 和 `*` 每個月的 () 。 **/M**參數只適用于每月排程。 使用 LASTDAY 修飾詞時，這是必要的。 否則，它是選擇性的，而且 `*` 每個月都會 (預設值) 。 |
| /i <Idletime> | 指定工作啟動前的電腦閒置的分鐘數。 有效的值是從1到999的整數。 此參數僅適用于 ONIDLE 排程，因此是必要的。 |
| /st `<Starttime>` | 使用24小時制時間格式（HH： mm），指定工作的開始時間。 預設值是本機電腦上的目前時間。 **/St**參數適用于分鐘、每小時、每日、每週、每月和一次排程。 需要一次排程。 |
| /ri `<interval>` | 指定排程工作的重複間隔（以分鐘為單位）。 這不適用於排程類型： MINUTE、每小時、ONSTART、整理和 ONIDLE。 有效範圍是 1-599940 (599940 分鐘 = 9999 小時) 。 如果指定了 **/et** 或 **/du** 參數，預設值為 **10 分鐘**。 |
| /et `<Endtime>` | 以 <HH： MM> 24 小時的格式，指定以分鐘或小時工作排程結束的當日時間。 在指定的結束時間之後，schtasks.exe 就不會重新開機工作，直到開始時間重複。 依預設，工作排程沒有結束時間。 此參數是選擇性的，且僅適用于分鐘或小時的排程。 |
| /du `<duration>` | 以 <HHHH HHHH： MM> 24 小時格式指定分鐘或小時排程的最長時間長度。 經過指定的時間之後，schtasks.exe 就不會重新開機工作，直到開始時間重複。 依預設，工作排程沒有最大持續時間。 此參數是選擇性的，且僅適用于分鐘或小時的排程。 |
| /k | 停止此工作在 **/et** 或 **/du**指定的時間執行的程式。 如果沒有 **/k**，在它到達 **/et** 或 **/du** 所指定的時間之後，仍不會重新開機程式，也不會在程式仍在執行時停止程式。 此參數是選擇性的，且僅適用于分鐘或小時的排程。 |
| /sd <Startdate> | 指定工作排程的開始日期。 預設值是本機電腦上的目前日期。 [開始使用] 的格式會因 [**地區及語言選項**] 中針對本機電腦所選取的地區設定**而有所不同。** 每個地區設定只有一個有效的格式。 有效的日期格式包括 (請務必選擇在本機電腦上的 [**地區及語言選項** **] 中選取的格式**最類似的格式) ：<ul><li>`<MM>//` -指定使用月份優先的格式，例如英文 (美國) 和西班牙文 (巴拿馬) 。</li><li>`<DD>//` -指定使用日優先格式，例如保加利亞文和荷蘭文 (荷蘭) 。</li><li>`<YYYY>//` -指定要用於年份優先的格式，例如瑞典文和法文 (加拿大) 。</li></ul> |
| /ed `<Enddate>` | 指定排程結束的日期。 這是選擇性參數。 它在一次、ONSTART、整理或 ONIDLE 排程中無效。 依預設，排程沒有結束日期。 預設值是本機電腦上的目前日期。 [ **結束** 日期] 的格式會隨著針對 [ **地區及語言選項**] 中的本機電腦所選取的地區設定而有所不同。 每個地區設定只有一個有效的格式。 有效的日期格式包括 (請務必選擇在本機電腦上的 [**地區及語言選項** **] 中選取的格式**最類似的格式) ：<ul><li>`<MM>//` -指定使用月份優先的格式，例如英文 (美國) 和西班牙文 (巴拿馬) 。</li><li>`<DD>//` -指定使用日優先格式，例如保加利亞文和荷蘭文 (荷蘭) 。</li><li>`<YYYY>//` -指定要用於年份優先的格式，例如瑞典文和法文 (加拿大) 。</li></ul> |
| /it | 指定只有當 [執行身分] 使用者 (執行工作的使用者帳戶) 登入電腦時，才執行排定的工作。 此參數不會影響以系統許可權執行的工作，或是已設定僅限互動式屬性的工作。 您無法使用變更命令從工作中移除僅限互動式的屬性。 依預設，[以使用者身份執行] 是排程工作時的目前本機電腦使用者，或是由 **/u** 參數指定的帳號（如果使用的話）。 但是，如果命令包含 **/ru** 參數，則 run as 使用者就是 **/ru** 參數所指定的帳號。 |
| /z | 指定在完成排程時刪除工作。 |
| /f | 指定要建立工作，並在指定的工作已經存在時隱藏警告。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="to-schedule-a-task-to-run-every-n-minutes"></a>排定每分鐘執行一次工作 `<n>`

在分鐘的排程中，需要 **/sc 分鐘** 的參數。 **/Mo** (修飾詞) 參數是選擇性的，並指定每次執行工作之間的分鐘數。 **/Mo**的預設值是每分鐘的*1* () 。 **/Et** (結束時間) 和 **/du** (duration) 參數是選擇性的，而且可以搭配或不使用 **/k** (end 工作) 參數使用。

### <a name="examples"></a>範例

- 若要排程安全性腳本（ *vbs*）每隔20分鐘執行一次，請輸入：

    ```
    schtasks /create /sc minute /mo 20 /tn Security Script /tr \\central\data\scripts\sec.vbs
    ```

    由於此範例不包含開始日期或時間，因此工作會在命令完成後的20分鐘內開始執行，並在每隔20分鐘執行一次系統。 請注意，安全性腳本來源檔案位於遠端電腦上，但工作已排程並在本機電腦上執行。

- 若要排定在本機電腦上每隔100分鐘執行一次安全腳本（ *vbs*），請在 5:00 P.M. 之間執行 和上午7:59 每天輸入：

    ```
    schtasks /create /tn Security Script /tr sec.vbs /sc minute /mo 100 /st 17:00 /et 08:00 /k
    ```

    此範例使用 **/sc** 參數來指定分鐘的排程，並使用 **/mo** 參數指定100分鐘的間隔。 它會使用 **/st** 和 **/et** 參數來指定每天排程的開始時間和結束時間。 如果腳本仍在上午7:59 執行，它也會使用 **/k** 參數來停止腳本。 在沒有 **/k**的情況下，如果實例是在上午6:20 開始，則不會在上午7:59 之後啟動腳本。 仍在執行，不會將它停止。

## <a name="to-schedule-a-task-to-run-every-n-hours"></a>排定每個小時執行一次工作 `<n>`

每小時的排程都需要 **/sc 每小時** 參數。 **/Mo** (修飾詞) 參數是選擇性的，並指定每次執行工作之間的時數。 **/Mo**的預設值是每小時 (*1*) 。 **/K** (end task) 參數是選擇性的，而且可以在指定的時間使用 **/et** (end) 或 **/du** (指定的間隔) 之後結束。

### <a name="examples"></a>範例

- 若要排程 MyApp 程式每隔五個小時執行一次，從2002年3月的第一天開始，請輸入：

    ```
    schtasks /create /sc hourly /mo 5 /sd 03/01/2002 /tn My App /tr c:\apps\myapp.exe
    ```

    在此範例中，本機電腦會在 [**地區及語言選項**] 中使用**英文 (辛巴威) **選項，因此開始日期的格式為 MM/DD/YYYY (03/01/2002) 。

- 若要將 MyApp 程式排程為每小時執行一次，從午夜之前的五分鐘開始，請輸入：

    ```
    schtasks /create /sc hourly /st 00:05 /tn My App /tr c:\apps\myapp.exe
    ```

- 若要將 MyApp 程式排程為每3小時執行一次，請為10小時的總計輸入：

    ```
    schtasks /create /tn My App /tr myapp.exe /sc hourly /mo 3 /st 00:00 /du 0010:00
    ```

    在此範例中，工作會在上午12:00 點、3:00 A.M.、上午6:00 和上午9:00 執行。 因為持續時間為10小時，所以不會在下午12:00 執行工作。 相反地，它會在上午12:00 開始。 下一天。 此外，因為程式只會執行幾分鐘的時間，所以如果程式在持續時間過期時仍在執行，則不需要使用 **/k** 參數來停止程式。

## <a name="to-schedule-a-task-to-run-every-n-days"></a>排程每日執行的工作 `<n>`

在每日排程中，需要 **/sc 每日** 參數。 **/Mo** (修飾詞) 參數是選擇性的，並指定每次執行工作之間的天數。 **/Mo**的預設值是每一天)  (*1* 。

### <a name="examples"></a>範例

- 若要將 MyApp 程式排程為每天上午8:00 執行一次，請在每天上午執行。 在2021年12月31日之前，請輸入：

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc daily /st 08:00 /ed 31/12/2021
    ```

     在此範例中，本機電腦系統會設定為 [**地區及語言選項**] 中的**英文 (英國) **選項，因此結束日期的格式為 DD/MM/YYYY (31/12/2021) 。 此外，因為此範例不包含 **/mo** 參數，所以預設間隔 *1* 會用來每天執行命令。

- 將 MyApp 程式排程在下午1:00 的每十二天執行  (13:00) 從2021年12月31日開始，請輸入：

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc daily /mo 12 /sd 12/31/2002 /st 13:00
    ```

    在此範例中，系統會設定為 [**地區和語言選項**] 中的 [**英文 (辛巴威]) **選項，因此結束日期的格式為 MM/DD/YYYY (12/31/2021) 。

- 若要排程安全性腳本（ *vbs*）每隔70天執行一次，請輸入：

    ```
    schtasks /create /tn Security Script /tr sec.vbs /sc daily /mo 70 /it
    ```

    在此範例中， **/it** 參數是用來指定工作只有在執行工作之帳戶的使用者登入電腦時才執行。 因為工作是以特定使用者帳戶的許可權來執行，所以此工作只會在使用者登入時執行。

    > [!NOTE]
    > 若要使用僅限互動式的 (**/it**) 屬性來識別工作，請使用詳細資訊查詢 (**/query/v**) 。 在使用/it 工作的詳細資訊查詢顯示中，[ **登入模式]** 欄位的值為 [僅限互動]。

## <a name="to-schedule-a-task-to-run-every-n-weeks"></a>排定每週執行一次工作 `<n>`

在每週排程中，需要 **/sc 每週** 的參數。 **/Mo** (修飾詞) 參數是選擇性的，並指定每次執行工作之間的周數。 **/Mo**的預設值是每週的*1* () 。

每週排程也有選擇性的 **/d** 參數，可將工作排程在一周指定的日子執行，或在所有天數 ( # A1。 預設值為 *Monday (星期一) *。 每日 ( # A1 選項相當於排程每日工作。

### <a name="examples"></a>範例

- 若要將 MyApp 程式排程在每隔六周的遠端電腦上執行，請輸入：

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /mo 6 /s Server16 /u Admin01
    ```

    因為這個範例會離開 **/d** 參數，所以工作會在星期一執行。 此範例也會使用 **/s** 參數指定遠端電腦，並使用 **/u** 參數以使用者的系統管理員帳戶許可權執行命令。 此外，因為 **/p** 參數是空的，SchTasks.exe 提示使用者提供系統管理員帳戶密碼，而且因為命令是從遠端執行，所以命令中的所有路徑（包括 MyApp.exe 的路徑）都會參考遠端電腦上的路徑。

- 若要將工作排程在每隔一個星期五執行，請輸入：

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /mo 2 /d FRI
    ```

    這個範例會使用 **/mo** 參數來指定兩周的間隔，並使用 **/d** 參數來指定一周中的哪一天。 若要排程每週五執行的工作，請省略 **/mo** 參數或將它設定為 *1*。

## <a name="to-schedule-a-task-to-run-every-n-months"></a>排定每月執行一項工作 `<n>`

在此排程類型中，需要 **/sc 每月** 參數。 **/Mo** (修飾詞) 參數，指定每次執行工作之間的月數，是選擇性的，而每個月的預設值是*1* () 。 此排程類型也有選擇性的 **/d** 參數，可將工作排程在該月的指定日期執行。 預設值為 *1* ， (月份的第一天) 。

### <a name="examples"></a>範例

- 若要將 MyApp 程式排程在每個月的第一天執行，請輸入：

    ```
    schtasks /create /tn My App /tr myapp.exe /sc monthly
    ```

    **/Mo** (修飾詞的預設值) 參數和 **/d** (day) 參數為*1*，因此在此範例中，您不需要使用這些參數的其中一個。

- 若要排程 MyApp 程式每隔三個月執行一次，請輸入：

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo 3
    ```

    此範例使用 **/mo** 參數指定3個月的間隔。

- 若要在每個月的21天午夜（從2002年7月2日到2003年6月30日），將 MyApp 程式排程在每個月執行，請輸入：

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo 2 /d 21 /st 00:00 /sd 2002/07/01 /ed 2003/06/30
    ```

    此範例會使用 **/mo** 參數來指定每隔兩個月 (的每月間隔) 、指定日期的 **/d** 參數、指定時間的 **/st** 參數，以及指定開始日期和結束日期的 **/sd** 和 **/ed** 參數。 此外，在此範例中，本機電腦會設定為 [**地區和語言選項**] 中的 [**英文 (南非]) **選項，因此會以當地格式（YYYY/MM/DD）來指定日期。

## <a name="to-schedule-a-task-to-run-on-a-specific-day-of-the-week"></a>排定工作在一周的特定日子執行

每週排程的日期是每週排程的變化。 在每週排程中，需要 **/sc 每週** 的參數。 **/Mo** (修飾詞) 參數是選擇性的，並指定每次執行工作之間的周數。 **/Mo**的預設值是每週的*1* () 。 **/D**參數是選擇性的，會將工作排程在一周指定的日子執行，或在所有天數 (**&#42;**) 。 預設值為 *Monday (星期一) *。 「每日」選項 `(/d *)` 相當於排程每日工作。

### <a name="examples"></a>範例

- 若要將 MyApp 程式排定在每週的星期三執行，請輸入：

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /d WED
    ```

    這個範例會使用 **/d** 參數來指定一周的哪一天。 由於命令會離開 **/mo** 參數，因此工作會每週執行一次。

- 若要將工作排程在每隔八周的星期一和星期五執行，請輸入：

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc weekly /mo 8 /d MON,FRI
    ```

    此範例使用 **/d** 參數指定 days 和 **/mo** 參數，以指定八周間隔。

## <a name="to-schedule-a-task-to-run-on-a-specific-week-of-the-month"></a>若要將工作排程在每月特定周執行

在此排程類型中， **/sc 的每月** 參數、 **/mo** (修飾詞) 參數，以及 **/d** (day) 參數是必要參數。 **/Mo** (修飾詞) 參數指定工作執行的周。 **/D**參數指定一周的哪一天。 您可以針對此排程類型指定一周中的一天。 此排程也有選擇性的 **/m** (month) 參數，可讓您針對特定月份或每月排程工作， (**&#42;**) 。 **/M**參數的預設值是每個月 (**&#42;**) 。

### <a name="examples"></a>範例

- 若要將 MyApp 程式排程在每個月的第二個星期日執行，請輸入：

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo SECOND /d SUN
    ```

    此範例會使用 **/mo** 參數來指定月份的第二周，並使用 **/d** 參數指定日期。

- 若要將 MyApp 程式排程在3月和9月的第一個星期一執行，請輸入：

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo FIRST /d MON /m MAR,SEP
    ```

    此範例會使用 **/mo** 參數來指定月份的第一周，並使用 **/d** 參數指定日期。 它會使用 **/m** 參數來指定月份，並以逗號分隔 month 引數。

## <a name="to-schedule-a-task-to-run-on-a-specific-day-each-month"></a>將工作排程在每個月的特定一天執行

在此排程類型中，需要 **/sc 的每月** 參數和 **/d** (day) 參數。 **/D**參數指定月份的日期 (1-31) ，而不是一周的某一天，而且您只能在排程中指定一天。 **/M** (month) 參數是選擇性的，預設值為每個月* ( # B3 *，而 **/mo** (修飾詞) 參數對此排程類型而言是不正確。

Schtasks.exe 不會讓您針對不在 **/m** 參數所指定之月份的日期排程工作。 例如，嘗試排程二月的31天。 但是，如果您未使用 **/m** 參數，並針對每個月未出現的日期排程工作，則工作不會在較短的月份執行。 若要在當月的最後一天排程工作，請使用 [最後一天] 排程類型。

### <a name="examples"></a>範例

- 若要將 MyApp 程式排程在每個月的第一天執行，請輸入：

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly
    ```

    由於預設修飾詞為 *none* (沒有任何修飾詞) ，此命令會使用預設的日 *1*和 *每個月*的預設月份，而不需要任何其他參數。

- 排定 MyApp 程式在5月15日和6月15日下午3:00 執行  (15:00) ，請輸入：

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /d 15 /m MAY,JUN /st 15:00
    ```

    這個範例會使用 **/d** 參數指定日期，並使用 **/m** 參數來指定月份。 它也會使用 **/st** 參數來指定開始時間。

## <a name="to-schedule-a-task-to-run-on-the-last-day-of-a-month"></a>若要將工作排程在一個月的最後一天執行

在最後一天的排程類型中， **/sc 的每月** 參數、 **/mo LASTDAY** (修飾詞) 參數，以及 **/m** (month) 參數是必要參數。 **/D** (day) 參數無效。

### <a name="examples"></a>範例

- 若要將 MyApp 程式排程在每個月的最後一天執行，請輸入：

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo lastday /m *
    ```

    這個範例會使用 **/mo** **參數指定** 最後一天，並使用萬用字元 (**&#42;**) 來表示程式每個月執行一次。

- 若要將 MyApp 程式排程于二月的最後一天執行，並將2006年3月的最後一天在下午6:00 執行，請輸入：

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /mo lastday /m FEB,MAR /st 18:00
    ```

    此範例會使用 **/mo** 參數指定最後一天、指定月份的 **/m** 參數，以及指定開始時間的 **/st** 參數。

## <a name="to-schedule-to-run-once"></a>排程執行一次

在 [執行一次] 排程類型中，需要 [ **/sc 一次** ] 參數。 需要指定工作執行時間的 **/st** 參數。 **/Sd**參數指定工作執行的日期，是選擇性的，而 **/mo** (修飾詞) 和 **/ed** (結束日期) 參數無效。

如果指定的日期和時間在過去，根據本機電腦的時間，「Schtasks」就不會讓您排程工作執行一次。 若要排程在不同時區的遠端電腦上執行一次的工作，您必須在本機電腦上進行該日期和時間之前進行排程。

### <a name="example"></a>範例

- 若要將 MyApp 程式排程在2003年1月1日午夜執行，請輸入：

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc once /sd 01/01/2003 /st 00:00
    ```

    此範例使用 **/sc** 參數來指定排程類型以及 **/sd** 和 **/st** 參數，以指定日期和時間。 此外，在此範例中，本機電腦使用 [**地區及語言選項**] 中的 [**英文 (美國) ** ] 選項，[開始日期] 的格式為 MM/DD/YYYY。

## <a name="to-schedule-a-task-to-run-every-time-the-system-starts"></a>排定每次系統啟動時要執行的工作

在啟動排程類型中，需要 **/sc onstart** 參數。 **/Sd** (開始日期) 參數是選擇性的，預設值是目前的日期。

### <a name="example"></a>範例

- 若要將 MyApp 程式排程在每次系統啟動時執行，從2001年3月15日開始，請輸入：

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc onstart /sd 03/15/2001
    ```

    在此範例中，本機電腦使用 [**地區及語言選項**] 中的 [**英文 (美國) ** ] 選項，[開始日期] 的格式為 MM/DD/YYYY。

## <a name="to-schedule-a-task-to-run-when-a-user-logs-on"></a>排程使用者登入時要執行的工作

[On logon schedule] 類型會排定每次使用者登入電腦時執行的工作。 在 [登入排程類型] 中，需要 **/sc 整理** 參數。 **/Sd** (開始日期) 參數是選擇性的，預設值是目前的日期。

### <a name="example"></a>範例

- 若要排程使用者登入遠端電腦時所執行的工作，請輸入：

    ```
    schtasks /create /tn Start Web Site /tr c:\myiis\webstart.bat /sc onlogon /s Server23
    ```

    此範例會排程每次使用者 (任何使用者) 登入遠端電腦時執行的批次檔。 它會使用 **/s** 參數指定遠端電腦。 因為此命令為 remote，所以命令中的所有路徑（包括批次檔的路徑）都會參考遠端電腦上的路徑。

## <a name="to-schedule-a-task-to-run-when-the-system-is-idle"></a>排程在系統閒置時執行的工作

「閒置時間排程」類型會排定在 **/i** 參數指定的時間內沒有使用者活動時執行的工作。 在 [依閒置時間排程類型] 中，需要 **/sc onidle** 參數和 **/i** 參數。 **/Sd** (開始日期) 是選擇性的，預設值是目前的日期。

### <a name="example"></a>範例

- 若要排程 MyApp 程式在電腦閒置時執行，請輸入：

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc onidle /i 10
    ```

    這個範例會使用必要的 **/i** 參數來指定電腦必須保持閒置10分鐘，工作才會開始。

## <a name="to-schedule-a-task-to-run-now"></a>排定立即執行的工作

Schtasks 沒有立即執行的選項，但是您可以藉由建立執行一次的工作並在幾分鐘內啟動，來模擬該選項。

### <a name="example"></a>範例

- 若要將工作排定為執行一次，請在2020年11月13日下午2:18。 本地時間，輸入：

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc once /st 14:18 /sd 11/13/2002
    ```

    在此範例中，本機電腦會在 [**地區及語言選項**] 中使用**英文 (美國) **選項，因此開始日期的格式為 MM/DD/YYYY。

## <a name="to-schedule-a-task-that-runs-with-different-permissions"></a>若要排程以不同許可權執行的工作

您可以排程在本機和遠端電腦上使用其他帳戶的許可權來執行所有類型的工作。 除了特定排程類型所需的參數之外，還需要 **/ru** 參數，而 **/rp** 參數是選擇性的。

### <a name="examples"></a>範例

- 若要在本機電腦上執行 MyApp 程式，請輸入：

    ```
    schtasks /create /tn My App /tr myapp.exe /sc weekly /d TUE /ru Admin06
    ```

    此範例使用 **/ru** 參數指定工作應以使用者的系統管理員帳戶許可權執行 (*Admin06*) 。 此外，在此範例中，工作已排程為每個星期二執行，但您可以使用任何排程類型來執行具有替代許可權的工作。

    在 [回應] 中，SchTasks.exe 會提示您輸入 *Admin06* 帳戶的「執行密碼」密碼，然後顯示成功訊息：

    ```
    Please enter the run as password for Admin06: ********
    SUCCESS: The scheduled task My App has successfully been created.
    ```

- 若要每隔四天在 *行銷* 電腦上執行 MyApp 程式，請輸入：

    ```
    schtasks /create /tn My App /tr myapp.exe /sc daily /mo 4 /s Marketing /u Marketing\Admin01 /ru Reskits\User01
    ```

    此範例使用 **/sc** 參數指定每日排程，並使用 **/mo** 參數指定四天的間隔。 此外，此範例會使用 **/s** 參數來提供遠端電腦的名稱，以及 **/u** 參數來指定具有在)  (*Admin01* 的遠端電腦上排程工作之許可權的帳戶。 最後，此範例會使用 **/ru**參數來指定工作應以使用者的非系統管理員帳戶許可權執行， (在*Reskits*網域) 中*User01* 。 如果沒有 **/ru** 參數，工作會以 **/u**所指定帳戶的許可權來執行。

    執行此範例時，Schtasks.exe 會先向 **/u** 參數所命名的使用者要求密碼 (來執行命令) 然後要求由 **/ru** 參數命名的使用者密碼 (，以執行工作) 。 驗證密碼之後，schtasks.exe 會顯示一則訊息，指出已排程工作：

    ```
    Type the password for Marketing\Admin01:********
    Please enter the run as password for Reskits\User01: ********
    SUCCESS: The scheduled task My App has successfully been created.
    ```

- 若要執行 [排程] *AdminCheck.exe* 程式在每星期五上午4:00 執行于 *公用* 電腦上，但只有當電腦的系統管理員登入時，請輸入：

    ```
    schtasks /create /tn Check Admin /tr AdminCheck.exe /sc weekly /d FRI /st 04:00 /s Public /u Domain3\Admin06 /ru Public\Admin01 /it
    ```

    此範例使用 **/sc** 參數來指定每週排程、指定日期的 **/d** 參數，以及指定開始時間的 **/st** 參數。 它也會使用 **/s**參數來提供遠端電腦的名稱、使用 **/u**參數來指定具有在遠端電腦上排程工作的帳戶、使用 (*Public\Admin01*) 的許可權，將工作設定為以公用電腦系統管理員的許可權執行的 **/ru**參數，以及使用 **/it**參數來表示工作只會在*Public\Admin01*帳戶登入時執行。

    > [!NOTE]
    > 若要使用僅限互動式的 (**/it**) 屬性來識別工作，請使用 () 的詳細資訊查詢 `/query /v` 。 在使用 **/it**工作的詳細資訊查詢顯示中，[ **登入模式]** 欄位的值為 [ **僅限互動**]。

## <a name="to-schedule-a-task-that-runs-with-system-permissions"></a>若要排程以系統許可權執行的工作

所有類型的工作都可以在本機和遠端電腦上以 **系統** 帳戶的許可權來執行。 除了特定排程類型所需的參數之外，還需要 **/ru 系統** (或 **/ru**) 參數，而 **/rp** 參數則無效。

> [!IMPORTANT]
> **系統**帳戶沒有互動式登入許可權。 使用者無法查看以系統許可權執行的程式或工作，或與其互動。 **/Ru**參數會決定工作執行時所使用的許可權，而不是用來排程工作的許可權。 只有系統管理員可以排程工作，不論是否有 **/ru** 參數的值。
>
> 若要識別以系統許可權執行的工作，請使用詳細資訊查詢 (`/query /v`) 。 在顯示系統執行工作的詳細資訊查詢中，[ **執行為使用者** ] 欄位的值為 **NT AUTHORITY\SYSTEM** ，而 [ **登入模式]** 欄位的值 **只**是 [背景]。

### <a name="examples"></a>範例

- 若要使用 **系統** 帳戶的許可權，將 MyApp 程式排程在本機電腦上執行，請輸入：

    ```
    schtasks /create /tn My App /tr c:\apps\myapp.exe /sc monthly /d 15 /ru System
    ```

    在此範例中，工作會排程在每個月的第15天執行，但是您可以使用任何排程類型來執行具有系統許可權的工作。 此外，此範例會使用 **/Ru 系統** 參數來指定系統安全性內容。 由於系統工作不會使用密碼，因此會省略 **/rp** 參數。

    在 [回應] 中，SchTasks.exe 會顯示參考用訊息和成功訊息，而不提示輸入密碼：

    ```
    INFO: The task will be created under user name (NT AUTHORITY\SYSTEM).
    SUCCESS: The Scheduled task My App has successfully been created.
    ```

- 若要將 MyApp 程式排程在 *Finance01* 電腦上每早上上午4:00 執行，使用系統許可權，請輸入：

    ```
    schtasks /create /tn My App /tr myapp.exe /sc daily /st 04:00 /s Finance01 /u Admin01 /ru System
    ```

    此範例使用 **/tn** 參數來命名工作和 **/tr** 參數，以指定 MyApp 程式的遠端副本、使用 **/sc** 參數指定每日排程，但是會保留 **/mo** 參數，因為每一天) 預設為 *1* (。 此範例也會使用 **/st**參數來指定開始時間，也就是工作每天執行的時間、 **/s**參數提供遠端電腦的名稱、 **/u**參數指定有權在遠端電腦上排程工作的帳戶，以及指定工作應在系統帳戶下**執行的許可權參數。** 如果沒有 **/ru** 參數，工作會使用 **/u** 參數所指定之帳戶的許可權來執行。

    Schtasks.exe 要求由 **/u** 參數命名的使用者密碼，且在驗證密碼之後，會顯示一則訊息，指出該工作已建立，且將以 **系統** 帳戶的許可權執行：

    ```
    Type the password for Admin01:**********

    INFO: The Schedule Task My App will be created under user name (NT AUTHORITY\
    SYSTEM).
    SUCCESS: The scheduled task My App has successfully been created.
    ```

## <a name="to-schedule-a-task-that-runs-more-than-one-program"></a>若要排程執行多個程式的工作

每項工作只會執行一個程式。 不過，您可以建立執行多個程式的批次檔，然後排程執行批次檔的工作。

1. 使用文字編輯器（例如 [記事本]）建立一個批次檔，其中包含啟動事件檢視器 ( # A0) 和系統監視器 ( # A1) 程式所需的 .exe 檔案的名稱和完整路徑。

    ```
    C:\Windows\System32\Eventvwr.exe
    C:\Windows\System32\Perfmon.exe
    ```

2. 將檔案儲存為 *MyApps.bat*、開啟 schtasks.exe，然後輸入下列命令來建立要執行 *MyApps.bat* 的工作：

    ```
    schtasks /create /tn Monitor /tr C:\MyApps.bat /sc onlogon /ru Reskit\Administrator
    ```

    此命令會建立監視工作，每當有人登入時就會執行此工作。 它會使用 **/tn** 參數來命名工作、執行 MyApps.bat 的 **/tr** 參數、使用 **/sc** 參數來指示整理排程類型，並使用 **/ru** 參數以使用者的系統管理員帳戶許可權執行工作。

    此命令的結果是，每當使用者登入電腦時，工作都會啟動事件檢視器和系統監視器。

## <a name="to-schedule-a-task-that-runs-on-a-remote-computer"></a>排程在遠端電腦上執行的工作

若要將工作排程在遠端電腦上執行，您必須將工作新增至遠端電腦的排程。 您可以在遠端電腦上排程所有類型的工作，但必須符合下列條件：

- 您必須具有排程工作的許可權。 因此，您必須使用遠端電腦上 Administrators 群組成員的帳戶登入本機電腦，或者您必須使用 **/u** 參數來提供遠端電腦的系統管理員的認證。

- 只有當本機和遠端電腦位於相同網域，或本機電腦位於遠端電腦網域信任的網域中時，您才能使用 **/u** 參數。 否則，遠端電腦無法驗證指定的使用者帳戶，也無法確認該帳戶是否為系統管理員群組的成員。

- 工作必須具有足夠的許可權，才能在遠端電腦上執行。 所需的許可權會因工作而異。 根據預設，工作會以本機電腦目前使用者的許可權執行，或者，如果使用 **/u** 參數，工作會以 **/u** 參數所指定之帳戶的許可權執行。 不過，您可以使用 **/ru** 參數執行具有不同使用者帳戶許可權或系統許可權的工作。

### <a name="examples"></a>範例

- 若要將 MyApp 程式排程 (為系統管理員) 在 *SRV01* 遠端電腦上每隔十天開始執行，請輸入：

    ```
    schtasks /create /s SRV01 /tn My App /tr c:\program files\corpapps\myapp.exe /sc daily /mo 10
    ```

    此範例使用 **/s** 參數提供遠端電腦的名稱。 因為本機目前的使用者是遠端電腦的系統管理員，所以不需要提供替代許可權來排程工作的 **/u** 參數。

    > [!NOTE]
    > 在遠端電腦上排程工作時，所有參數都會參考遠端電腦。 因此， **/tr** 參數指定的檔案會參考遠端電腦上 MyApp.exe 的複本。

- 若要將 MyApp 程式排程 (為使用者) 每隔三小時在 *SRV06* 遠端電腦上執行，請輸入：

    ```
    schtasks /create /s SRV06 /tn My App /tr c:\program files\corpapps\myapp.exe /sc hourly /mo 3 /u reskits\admin01 /p R43253@4$ /ru SRV06\user03 /rp MyFav!!Pswd
    ```

    因為排程工作需要系統管理員許可權，所以命令會使用 **/u**和 **/p**參數來提供使用者系統管理員帳戶的認證， (在*Reskits*網域) 中*Admin01* 。 根據預設，這些許可權也會用來執行工作。 不過，由於工作不需要系統管理員許可權即可執行，因此命令會包含 **/u** 和 **/rp** 參數以覆寫預設值，並在遠端電腦上以使用者的非系統管理員帳戶的許可權執行工作。

- 將 MyApp 程式排程 (為使用者) 在每個月的最後一天于 *SRV02* 遠端電腦上執行。

    ```
    schtasks /create /s SRV02 /tn My App /tr c:\program files\corpapps\myapp.exe /sc monthly /mo LASTDAY /m * /u reskits\admin01
    ```

    因為本機目前的使用者 (*user03*) 不是遠端電腦的系統管理員，所以命令會使用 **/u**參數來提供使用者的系統管理員帳號憑證， (*Reskits*網域) 中的*Admin01* 。 系統管理員帳戶許可權將用來排程工作和執行工作。

    因為命令未包含 **/p** (password) 參數，所以 schtasks 會提示您輸入密碼。 然後，它會顯示成功訊息，在此案例中為警告：

    ```
    Type the password for reskits\admin01:********

    SUCCESS: The scheduled task My App has successfully been created.
    WARNING: The scheduled task My App has been created, but may not run because the account information could not be set.
    ```

    此警告表示遠端網域無法驗證 **/u** 參數所指定的帳號。 在此情況下，遠端網域無法驗證使用者帳戶，因為本機電腦不是遠端電腦網域信任之網域的成員。 發生這種情況時，工作作業會出現在已排程工作的清單中，但工作實際上是空的，而且不會執行。

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

- 若要使用不同使用者的許可權來執行 **/create** 命令，請使用 **/u** 參數。 **/U**參數只對遠端電腦上的排程工作有效。

- 若要查看更多 `schtasks /create` 範例，請 `schtasks /create /?` 在命令提示字元中輸入。

- 若要排程以不同使用者的許可權執行的工作，請使用 **/ru** 參數。 **/Ru**參數對本機和遠端電腦上的工作有效。

- 若要使用 **/u** 參數，本機電腦必須位於與遠端電腦相同的網域中，或者必須位於遠端電腦網域信任的網域中。 否則，可能是未建立工作，或工作作業是空的，而且工作不會執行。

- 除非您提供密碼，否則 schtasks.exe 一律會提示輸入密碼，即使您在本機電腦上使用目前的使用者帳戶排程工作也是如此。 這是適用于 schtasks.exe 的正常行為。

- Schtasks 不會驗證程式檔案位置或使用者帳戶密碼。 如果您未針對使用者帳戶輸入正確的檔案位置或正確的密碼，則會建立工作，但不會執行該工作。 此外，如果帳戶的密碼變更或過期，而您未變更工作中儲存的密碼，則工作不會執行。

- **系統**帳戶沒有互動式登入許可權。 使用者不會看到並無法與以系統許可權執行的程式互動。

- 每項工作只會執行一個程式。 不過，您可以建立一個批次檔來啟動多個工作，然後排程執行批次檔的工作。

- 您可以在建立工作時立即進行測試。 使用「執行」作業來測試工作，然後檢查 SchedLgU.txt 檔 ( # A1) 是否有錯誤。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [schtasks 變更命令](schtasks-change.md)

- [schtasks delete 命令](schtasks-delete.md)

- [schtasks end 命令](schtasks-end.md)

- [schtasks 查詢命令](schtasks-query.md)

- [schtasks run 命令](schtasks-run.md)