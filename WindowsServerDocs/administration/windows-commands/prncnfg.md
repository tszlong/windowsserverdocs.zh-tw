---
title: prncnfg
description: Prncnfg 命令的參考文章，可設定或顯示印表機的設定資訊。
ms.topic: reference
ms.assetid: 38a4e8fa-3122-495b-a125-35b926bc6415
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: ba5d465a46a23261942428761d11ef279b78a62e
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038719"
---
# <a name="prncnfg"></a>prncnfg

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

設定或顯示印表機的設定資訊。 此命令是位於目錄中的 Visual Basic 腳本 `%WINdir%\System32\printing_Admin_Scripts\<language>` 。 若要在命令提示字元中使用此命令，請輸入 **cscript** ，後面接著 prncnfg 檔案的完整路徑，或將目錄變更為適當的資料夾。 例如： `cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prncnfg` 。

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
| -S `<Servername>` | 指定裝載您要管理之印表機的遠端電腦名稱稱。 如果您未指定電腦，則會使用本機電腦。 |
| -P `<Printername>` | 指定您想要管理之印表機的名稱。 必要。 |
| -z `<newprintername>` | 指定新的印表機名稱。 需要 **-x** 和 **-P** 參數。 |
| -u `<Username>` -w `<password>` | 指定有權連線到裝載您要管理之印表機之電腦的帳戶。 目的電腦的本機系統管理員群組的所有成員都具有這些許可權，但也可以將許可權授與其他使用者。 如果您未指定帳戶，則必須以具有這些許可權的帳戶登入，才能使用命令。 |
| -r `<portname>` | 指定印表機連線的埠。 如果這是平行或序列埠，請使用埠的識別碼 (例如，LPT1 或 COM1) 。 如果這是 TCP/IP 通訊埠，請使用新增埠時所指定的埠名稱。 |
| -l `<location>` | 指定印表機位置，例如 **Copyroom**。 如果位置包含空格，請使用引號括住文字，例如「 **複製房間**」。|
| -h `<sharename>` | 指定印表機的共用名稱。 |
| -m `<comment>` | 指定印表機的批註字串。 |
| -f `<separatorfilename>` | 指定檔案，其中包含在分隔符號頁面上顯示的文字。 |
| -y `<datatype>` | 指定印表機可以接受的資料類型。 |
| -st `<starttime>` | 設定印表機的可用性有限。 指定印表機的可用當日時間。 如果您在印表機無法使用時將檔傳送至印表機，則會將檔保留 (的多工緩衝處理) 直到印表機變成可用。 您必須將時間指定為24小時制。 例如，若要指定 11:00 P.M.，請輸入 **2300**。 |
| -ui `<endtime>` | 設定印表機的可用性有限。 指定無法再使用印表機的時間。 如果您在印表機無法使用時將檔傳送至印表機，則會將檔保留 (的多工緩衝處理) 直到印表機變成可用。 您必須將時間指定為24小時制。 例如，若要指定 11:00 P.M.，請輸入 **2300**。 |
| -o `<priority>` | 指定多工緩衝處理器用來將列印工作路由傳送到列印佇列的優先權。 具有較高優先順序的列印佇列會在優先順序較低的任何佇列之前，收到其所有工作。 |
| -i `<defaultpriority>` | 指定指派給每個列印工作的預設優先順序。 |
| `{+|-}`共用 | 指定是否在網路上共用此印表機。 |
| `{+|-}`直接 | 指定是否應該直接將檔傳送至印表機，而不需要進行多工緩衝處理。 |
| `{+|-}`發表 | 指定是否應該在 active directory 中發佈此印表機。 如果您發行印表機，其他使用者可以根據其位置和功能來搜尋該印表機 (例如彩色列印和裝訂) 。 |
| `{+|-}`隱藏 | Reserved 函數。 |
| `{+|-}`rawonly | 指定是否只有原始資料列印工作可以在此佇列中進行多工緩衝處理。 |
| `{+|-}`} 已排入佇列 | 指定在檔的最後一頁進行多工緩衝處理之前，印表機不應該開始列印。 列印程式在檔完成列印之前無法使用。 不過，使用此參數可確保印表機可以使用整份檔。 |
| `{+|-}`keepprintedjobs | 指定多工緩衝處理程式列印之後是否應該保留檔。 啟用此選項可讓使用者從列印佇列，而不是從列印程式將檔重新提交到印表機。 |
| `{+|-}`workoffline | 指定如果電腦未連線到網路，使用者是否能夠將列印工作傳送到列印佇列。 |
| `{+|-}`enabledevq | 指定不符合印表機設定的列印工作 (例如，非 PostScript 印表機的非 postscript 檔案) 應該保留在佇列中，而不是列印。 |
| `{+|-}`docompletefirst | 指定多工緩衝處理器是否應以較低優先順序傳送已完成的列印工作，然後再傳送較高優先順序且尚未完成幕後處理的列印工作。 如果啟用此選項，而且沒有任何檔完成幕後處理，則多工緩衝處理器會在較小的檔之前傳送較大的檔。 如果您想要以工作優先權的成本最大化印表機效率，請啟用此選項。 如果停用此選項，則多工緩衝處理器一律會先將優先順序較高的作業傳送到其各自的佇列。 |
| `{+|-}`enablebidi | 指定印表機是否將狀態資訊傳送至多工緩衝處理器。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要顯示名為 *colorprinter_2* 之印表機的設定資訊，並以名為 *HRServer*的遠端電腦所裝載的列印佇列，請輸入：

```
cscript prncnfg -g -S HRServer -P colorprinter_2
```

若要設定名為 *colorprinter_2* 的印表機，讓名為 *HRServer* 的遠端電腦上的多工緩衝處理器保持列印工作，請輸入：

```
cscript prncnfg -t -S HRServer -P colorprinter_2 +keepprintedjobs
```

若要將名為 *HRServer* 的遠端電腦上的印表機名稱從 *colorprinter_2* 變更為 *colorprinter 3*，請輸入：

```
cscript prncnfg -x -S HRServer -P colorprinter_2 -z "colorprinter 3"
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [列印命令參考資料](print-command-reference.md)
