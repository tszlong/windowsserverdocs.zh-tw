---
title: prncnfg
description: 了解如何設定印表機使用 prncfg 命令。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 38a4e8fa-3122-495b-a125-35b926bc6415
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 6426b053a5c56918768f82cbd7631631abcf0a6f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859149"
---
# <a name="prncnfg"></a>prncnfg

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

設定或顯示印表機的設定資訊。

## <a name="syntax"></a>語法
```
cscript Prncnfg {-g | -t | -x | -?} [-S <ServerName>] [-P <printerName>] [-z <NewprinterName>] [-u <UserName>] [-w <Password>] [-r <PortName>] [-l <Location>] [-h <Sharename>] [-m <Comment>] [-f <SeparatorFileName>] [-y <Datatype>] [-st <starttime>] [-ut <Untiltime>] [-i <DefaultPriority>] [-o <Priority>] [<+|->shared] [<+|->direct] [<+|->hidden] [<+|->published] [<+|->rawonly] [<+|->queued] [<+|->enablebidi] [<+|->keepprintedjobs] [<+|->workoffline] [<+|->enabledevq] [<+|->docompletefirst]
```

## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|-g|顯示印表機的設定資訊。|
|-t|設定印表機。|
|-x|重新命名印表機。|
|-S \<ServerName\>|指定裝載的印表機，您想要管理遠端電腦的名稱。 如果您沒有指定的電腦，則會使用本機電腦。|
|-P \<printerName\>|指定您想要管理印表機的名稱。 必要。|
|-z \<NewprinterName\>|指定新的印表機名稱。 需要 **-x**並 **-P**參數。|
|-u \<UserName\> -w \<Password\>|指定具有連線至裝載的印表機，您想要管理之電腦的權限的帳戶。 目標電腦的本機 Administrators 群組的所有成員都有這些權限，但可以也對其他使用者授與權限。 如果您未指定帳戶，您必須登入具有可使用該指令的這些權限的帳戶。|
|-r \<PortName\>|指定印表機連線的位置的連接埠。 如果這是平行或序列連接埠，然後使用該連接埠 （例如，LPT1 或 COM1） 的識別碼。 如果這是在 TCP/IP 通訊埠，請使用 新增連接埠時所指定的連接埠名稱。|
|-l\<位置\>|指定之印表機位置，例如 「 複製空間 」。|
|-h \<Sharename\>|指定印表機的共用名稱。|
|-m\<註解\>|指定印表機的註解字串。|
|-f \<SeparatorFileName\>|指定的檔案，其中包含出現在 [分隔符號] 頁面的文字。|
|-y\<資料類型\>|指定印表機可以接受的資料類型。|
|-st \<starttime\>|設定印表機為有限的可用性。 指定印表機可用的時間。 如果無法使用時，您可以傳送到印表機的文件，文件會保留 （多工緩衝處理），直到可用的印表機。 您必須指定時間為 24 小時制。 例如，若要指定下午 11:00，輸入**2300年**。|
|-ut \<Endtime\>|設定印表機為有限的可用性。 指定印表機不再可用的時間。 如果無法使用時，您可以傳送到印表機的文件，文件會保留 （多工緩衝處理），直到可用的印表機。 您必須指定時間為 24 小時制。 例如，若要指定下午 11:00，輸入**2300年**。|
|-o\<優先順序\>|指定的多工緩衝處理器用以路由傳送到列印佇列的列印工作的優先順序。 具有較高優先順序的列印佇列會接收任何具有較低優先順序的佇列之前的所有其作業。|
|-i \<DefaultPriority\>|指定指派給每一個列印工作的預設優先權。|
|{+&#124;-}shared|指定是否在網路上共用這個印表機。|
|{+&#124;-}direct|指定是否在文件應直接傳送到印表機沒有正在多工緩衝處理。|
|{+&#124;-}published|指定是否應該在 active directory 中發佈此印表機。 如果您發佈印表機時，其他使用者可以搜尋會根據其位置與功能 （例如列印並裝訂的色彩）。|
|{+&#124;-}hidden|保留的函式。|
|{+&#124;-}rawonly|指定是否只有未經處理的資料列印工作可以多工緩衝處理這個佇列中。|
|{+ &#124; -}queued|指定印表機應該開始後的最後一頁文件的多工緩衝處理，直到列印。 文件列印完成前無法使用列印的程式。 不過，使用此參數可確保整份文件可用於印表機。|
|{+ &#124; -}keepprintedjobs|指定在列印之後，多工緩衝處理器是否應該保留文件。 啟用此選項可讓重新提交至印表機的文件，從列印佇列列印的程式而不是使用者。|
|{+ &#124; -}workoffline|指定使用者是否能夠將列印工作傳送到列印佇列中，如果電腦未連線到網路。|
|{+ &#124; -}enabledevq|指定 不符合印表機設定 （例如 PostScript 檔案多工緩衝處理到非 PostScript 的印表機） 的列印工作是否應該保留在佇列中而不是正在列印。|
|{+ &#124; -}docompletefirst|指定的多工緩衝處理器是否應該傳送優先順序較低的列印工作已完成多工緩衝處理之前傳送具有較高的優先順序，但尚未完成多工緩衝處理列印工作。 如果啟用此選項時，任何文件已完成多工緩衝處理，多工緩衝處理器會將較大的文件傳送較小的之前。 如果您想要最大化印表機的效益，但代價是作業優先順序，應啟用此選項。 如果此選項會停用，多工緩衝處理器一律會傳送較高優先順序的工作到其各自的佇列第一次。|
|{+ &#124; -}enablebidi|指定印表機是否會將狀態資訊傳送至多工緩衝處理器。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
-   **Prncnfg**命令是 Visual Basic 指令碼位於 %WINdir%\System32\printing_Admin_Scripts\\ <language>目錄。 若要使用這個命令中，在命令提示字元中，輸入**cscript**後面 prncnfg 檔案或將目錄變更為適當的資料夾完整路徑。 例如: 
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prncnfg
    ```
-   如果您提供的資訊包含空格，請使用引號括住的文字 (例如`"computer Name"`)。

## <a name="BKMK_examples"></a>範例
若要顯示名為 colorprinter_2 名為 HRServer 遠端電腦所裝載的列印佇列的印表機的組態資訊，請輸入：
```
cscript prncnfg -g -S HRServer -P colorprinter_2 
```

若要設定印表機，名為 colorprinter_2，以便在名為 HRServer 遠端電腦中的多工緩衝處理器已經列印之後，會列印工作，請輸入：
```
cscript prncnfg -t -S HRServer -P colorprinter_2 +keepprintedjobs 
```

若要變更的遠端電腦上的印表機名稱名為 HRServer 從 colorprinter_2 到 colorprinter 3，類型：
```
cscript prncnfg -x -S HRServer -P colorprinter_2 -z "colorprinter 3" 
```

#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[列印命令參考資料](print-command-reference.md)
