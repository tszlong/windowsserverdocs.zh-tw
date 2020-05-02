---
title: at
description: At 命令的參考主題，會排定在指定的時間和日期，于電腦上執行命令和程式。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ff18fd16-9437-4c53-8794-bfc67f5256b3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3aafcf4cbc4a6626a3390fe5ad6a305b90dfaec0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718924"
---
# <a name="at"></a>at

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

排定在指定的時間和日期，于電腦上執行的命令和程式。 只有**在排程服務正在執行時**，您才能使用。 在不使用參數的情況下，**于**列出已排程的命令。 您必須是本機 Administrators 群組的成員，才能執行此命令。

## <a name="syntax"></a>語法

```
at [\computername] [[id] [/delete] | /delete [/yes]]
at [\computername] <time> [/interactive] [/every:date[,...] | /next:date[,...]] <command>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `\<computername\>` | 指定遠端電腦。 如果您省略此參數，請**在**排程本機電腦上的命令和程式。 |
| `<id>` | 指定指派給已排程命令的識別碼。 |
| /delete | 取消已排程的命令。 如果您省略*識別碼*，電腦上所有已排程的命令都會被取消。 |
| /yes | 當您刪除已排程的事件時，對系統中的所有查詢回答 [是]。 |
| `<time>` | 指定您要執行命令的時間。 時間以小時：分鐘表示，24小時制標記法（也就是00:00 （午夜）到23:59）。 |
| 互動式 | 允許*命令*在*命令*執行時，與登入之使用者的桌面進行互動。 |
| 每個 | 在每個指定的一天或一周或一個月的某一天（例如，每個星期四或每個月的第三天）執行*命令*。 |
| `<date>` | 指定您要執行命令的日期。 您可以指定一或多個星期（也就是 type **M**、**T**、**W**、**Th**、**F**、**S**、**Su**）或當月的一或多天（也就是輸入1到31）。 以逗號分隔多個日期專案。 如果您省略*date*， **at**會使用當月的目前日期。 |
| 下一個 | 在下一天出現時執行*命令*（例如，下一個星期四）。 |
| `<command>` | 指定您想要執行的 Windows 命令、程式（也就是 .exe 或 .com 檔案）或批次檔（也就是 .bat 或 .cmd 檔案）。 當命令需要路徑做為引數時，請使用絕對路徑（也就是以磁碟機號開頭的完整路徑）。 如果命令位於遠端電腦上，請為伺服器和共用名稱指定通用命名慣例（UNC）標記法，而不是遠端磁碟機號。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="remarks"></a>備註

- 此命令不會在執行命令之前自動載入 cmd.exe。 如果您不是執行可執行檔（.exe），您必須在命令開頭明確載入 cmd.exe，如下所示：

    ```
    cmd /c dir > c:\test.out
    ```

- 如果在沒有命令列選項的情況下使用此命令，排程的工作會出現在與下列格式類似的資料表中：

    ```
    Status  ID   Day        time        Command Line
    OK      1    Each F     4:30 PM     net send group leads status due
    OK      2    Each M     12:00 AM    chkstor > check.file
    OK      3    Each F     11:59 PM    backup2.bat
    ```

- 如果使用此命令包含識別碼（*ID*），則只會顯示單一專案的資訊，其格式如下所示：  

    ```
    Task ID: 1
    Status: OK
    Schedule: Each  F
    Time of Day: 4:30 PM
    Command: net send group leads status due
    ```

- 在您排程命令之後（尤其是具有命令列選項的命令），請在不使用任何命令列選項的情況下輸入，**以**檢查命令語法是否正確。 如果 [**命令列**] 欄位中的資訊錯誤，請刪除命令並重新輸入。 如果仍然不正確，請使用較少的命令列選項來重新輸入命令。

- **在**執行時，以背景進程來排程的命令。 [輸出] 不會顯示在 [電腦] 畫面上。 若要將輸出重新導向至檔案，請使用`>`重新導向符號。 如果您將輸出重新導向至檔案，不論您是在命令列`^`或批次檔**中使用，** 都必須在重新導向符號之前使用 escape 符號。 例如，若要將輸出重新導向至*輸出 .txt*，請輸入：

    ```
    at 14:45 c:\test.bat ^>c:\output.txt
    ```

    執行中命令的目前目錄是 systemroot 資料夾。

- 如果您在排程要執行的命令之後變更系統時間，請**在**不使用命令列選項的情況下輸入 **，以修改**過的系統時間同步處理排程器。

- 已排程的命令會儲存在登錄中。 因此，如果您重新開機排程服務，就不會遺失已排程的工作。

- 請不要將重新導向的磁片磁碟機用於存取網路的排程工作。 排程服務可能無法存取重新導向的磁片磁碟機，或者，如果在排程工作執行時有不同的使用者登入，則可能不會出現重新導向的磁片磁碟機。 相反地，請使用排程工作的 UNC 路徑。 例如：  

    ```
    at 1:00pm my_backup \\server\share
    ```

    請勿使用下列語法，其中**x：** 是使用者所建立的連接：  

    ```
    at 1:00pm my_backup x:
    ```

    如果您排定的**at**命令使用磁碟機號連線到共用目錄，請在使用磁片磁碟機時，包含**at**命令以中斷連接磁片磁碟機。 如果磁片磁碟機未中斷連線，指定的磁碟機號將無法在命令提示字元中使用。

- 根據預設，使用此命令排定的工作將會在72小時後停止。 您可以修改登錄來變更此預設值。

    **修改登錄**

    > [!Caution]
    > 不正確地編輯登錄可能會對系統造成嚴重的損害。 變更登錄之前，您應該先備份電腦所有的重要資料。

    1. 啟動登錄編輯程式（regedit.exe）。

    2. 在登錄中找出並按一下下列機碼：`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Schedule`

    3. 在 [**編輯**] 功能表上，按一下 [**新增值**]，然後新增下列登錄值：

        - **值名稱。** atTaskMaxHours

        - **資料類型。** reg_DWOrd 

        - **基.** Decimal

        - **數值資料：** 0. [**數值資料**] 欄位中的值為**0**表示沒有限制，而且不會停止。 從1到99的值表示小時數。

- 您可以使用 [排定的工作] 資料夾來查看或修改使用此命令所建立之工作的設定。 當您使用此命令來排程工作時，工作會列在 [排程工作] 資料夾中，其名稱如下：**at3478**。 不過，如果您透過 [排定的工作] 資料夾修改工作，它會升級為一般排定的工作。 **At**命令不會再看到此工作，而且 [at 帳戶] 設定不再適用。 您必須明確輸入工作的使用者帳戶和密碼。

## <a name="examples"></a>範例

若要顯示在行銷伺服器上排程的命令清單，請輸入：

```
at \\marketing
```

若要深入瞭解 Corp 伺服器上識別碼為3的命令，請輸入：

```
at \\corp 3
```

若要將 net share 命令排定在 Corp 伺服器上的上午8:00 執行 然後將清單重新導向至維護伺服器、在 [報告] 共用目錄中，以及 Corp 檔案中輸入：

```
at \\corp 08:00 cmd /c net share reports=d:\marketing\reports >> \\maintenance\reports\corp.txt
```

若要在每隔五天的午夜將行銷伺服器的硬碟備份到磁帶機，請建立名為 Archive 的 batch 程式，其中包含備份命令，然後排程要執行的批次程式，輸入：

```
at \\marketing 00:00 /every:5,10,15,20,25,30 archive
```

若要取消目前伺服器上排程的所有命令，請清除 **[排程的**資訊]，如下所示：

```
at /delete
```

若要執行不是可執行檔（.exe）的命令，請在命令前面加上**cmd/c** ，以載入 cmd.exe，如下所示：

```
cmd /c dir > c:\test.out
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [schtasks](schtasks.md)。 另一個命令列排程工具。
