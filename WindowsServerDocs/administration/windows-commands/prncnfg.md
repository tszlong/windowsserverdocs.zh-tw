---
title: prncnfg
description: Prncnfg 命令的參考文章，它會設定或顯示印表機的設定資訊。
ms.topic: article
ms.assetid: 38a4e8fa-3122-495b-a125-35b926bc6415
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 4f7329d4f5c7441232efffbc40dcc1177f083e1e
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884763"
---
# <a name="prncnfg"></a>prncnfg

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

設定或顯示印表機的設定資訊。 此命令是位於目錄中的 Visual Basic 腳本 `%WINdir%\System32\printing_Admin_Scripts\<language>` 。 若要在命令提示字元中使用此命令，請輸入**cscript** ，後面接著 prncnfg 檔案的完整路徑，或將目錄變更為適當的資料夾。 例如：`cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prncnfg`。

## <a name="syntax"></a>語法

```
cscript prncnfg {-g | -t | -x | -?} [-S <Servername>] [-P <Printername>] [-z <newprintername>] [-u <Username>] [-w <password>] [-r <portname>] [-l <location>] [-h <sharename>] [-m <comment>] [-f <separatorfilename>] [-y <datatype>] [-st <starttime>] [-ut <untiltime>] [-i <defaultpriority>] [-o <priority>] [<+|->shared] [<+|->direct] [<+|->hidden] [<+|->published] [<+|->rawonly] [<+|->queued] [<+|->enablebidi] [<+|->keepprintedjobs] [<+|->workoffline] [<+|->enabledevq] [<+|->docompletefirst]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| -g | 顯示印表機的設定資訊。 |
| -t | 設定印表機。 |
| -X | 重新命名印表機。 |
| -S`<Servername>` | 指定裝載您要管理之印表機的遠端電腦名稱稱。 如果您未指定電腦，則會使用本機電腦。 |
| -P`<Printername>` | 指定您想要管理的印表機名稱。 必要。 |
| -z`<newprintername>` | 指定新的印表機名稱。 需要 **-x**和 **-P**參數。 |
| -u `<Username>` -w`<password>` | 指定具有許可權可連接到裝載您要管理之印表機的電腦的帳戶。 目的電腦的本機系統管理員群組的所有成員都具有這些許可權，但也可以將許可權授與給其他使用者。 如果您未指定帳戶，您必須使用具有這些許可權的帳戶登入，命令才能正常執行。 |
| -r`<portname>` | 指定印表機連接的埠。 如果這是平行或序列埠，請使用埠 (的識別碼，例如，LPT1 或 COM1) 。 如果這是 TCP/IP 通訊埠，請使用新增埠時所指定的埠名稱。 |
| -l`<location>` | 指定印表機位置，例如**Copyroom**。 如果位置包含空格，請使用引號括住文字，例如「**複製房間**」。|
| -h`<sharename>` | 指定印表機的共用名稱。 |
| -m`<comment>` | 指定印表機的批註字串。 |
| -f`<separatorfilename>` | 指定包含 [分隔符號] 頁面上所顯示文字的檔案。 |
| -y`<datatype>` | 指定印表機可以接受的資料類型。 |
| -st`<starttime>` | 設定印表機的可用性有限。 指定印表機可供使用的時間。 如果您在印表機無法使用時將檔傳送至印表機，檔會保留 (的多工緩衝處理) ，直到印表機變成可用為止。 您必須將時間指定為24小時制。 例如，若要指定下午11:00，請輸入**2300**。 |
| -未通過`<endtime>` | 設定印表機的可用性有限。 指定無法再使用印表機的當日時間。 如果您在印表機無法使用時將檔傳送至印表機，檔會保留 (的多工緩衝處理) ，直到印表機變成可用為止。 您必須將時間指定為24小時制。 例如，若要指定下午11:00，請輸入**2300**。 |
| -o`<priority>` | 指定多工緩衝處理器用來將列印工作路由到列印佇列的優先權。 優先順序較高的列印佇列會在優先順序較低的任何佇列之前，接收其所有作業。 |
| -i`<defaultpriority>` | 指定指派給每個列印工作的預設優先權。 |
| `{+|-}`共用 | 指定此印表機是否會在網路上共用。 |
| `{+|-}`直銷 | 指定是否要將檔直接傳送至印表機，而不需要進行多工緩衝處理。 |
| `{+|-}`發佈 | 指定此印表機是否應在 active directory 中發佈。 如果您發行印表機，其他使用者可以根據其位置和功能進行搜尋， (例如彩色列印和裝訂) 。 |
| `{+|-}`隱含 | 保留的函式。 |
| `{+|-}`rawonly | 指定是否只有原始資料列印工作可以在此佇列中進行多工緩衝處理。 |
| `{+|-}`} 已排入佇列 | 指定在檔的最後一頁為多工緩衝處理之前，印表機不應該開始列印。 列印程式在檔完成列印之前無法使用。 不過，使用此參數可確保印表機可使用整份檔。 |
| `{+|-}`keepprintedjobs | 指定多工緩衝處理器在列印之後是否應保留檔。 啟用此選項可讓使用者從列印佇列將檔重新提交至印表機，而不是從列印程式。 |
| `{+|-}`workoffline | 指定使用者是否能夠在電腦未連線到網路時，將列印工作傳送到列印佇列。 |
| `{+|-}`enabledevq | 指定不符合印表機設定的列印工作是否 (例如，從多工緩衝處理到非 PostScript 印表機的 PostScript 檔案) 應該保留在佇列中，而不是列印出來。 |
| `{+|-}`docompletefirst | 指定多工緩衝處理器是否應該以較低的優先順序傳送已完成緩衝處理的列印工作，然後再以較高的優先順序（尚未完成多工緩衝）傳送列印工作。 若此選項已啟用，而且沒有任何檔完成緩衝處理，則多工緩衝處理器會在較小的檔之前傳送較大的檔。 如果您想要以作業優先順序的成本將印表機效率最大化，請啟用此選項。 如果停用此選項，則多工緩衝處理器一律會先將優先順序更高的作業傳送到其各自的佇列。 |
| `{+|-}`enablebidi | 指定印表機是否將狀態資訊傳送至多工緩衝處理器。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要使用名為*HRServer*的遠端電腦所裝載的列印佇列來顯示名為*colorprinter_2*之印表機的設定資訊，請輸入：

```
cscript prncnfg -g -S HRServer -P colorprinter_2
```

若要設定名為*colorprinter_2*的印表機，讓名為*HRServer*的遠端電腦中的多工緩衝處理器在列印之後保留列印工作，請輸入：

```
cscript prncnfg -t -S HRServer -P colorprinter_2 +keepprintedjobs
```

若要將名為*HRServer*的遠端電腦上的印表機名稱從*colorprinter_2*變更為*colorprinter 3*，請輸入：

```
cscript prncnfg -x -S HRServer -P colorprinter_2 -z "colorprinter 3"
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [列印命令參考資料](print-command-reference.md)
