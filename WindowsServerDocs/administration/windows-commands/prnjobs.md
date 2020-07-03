---
title: prnjobs
description: Prnjobs 命令的參考文章，它會暫停、繼續、取消和列出列印工作。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5ad34199-7a5a-40c1-8053-bccd5929df43
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 79ad0631a2d1c871664ecebc11c26f2e005ca772
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85924193"
---
# <a name="prnjobs"></a>prnjobs

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

暫停、繼續、取消和列出列印工作。 此命令是位於目錄中的 Visual Basic 腳本 `%WINdir%\System32\printing_Admin_Scripts\<language>` 。 若要在命令提示字元中使用此命令，請輸入**cscript** ，後面接著 prnjobs 檔案的完整路徑，或將目錄變更為適當的資料夾。 例如：`cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnjobs.vbs`。

## <a name="syntax"></a>語法

```
cscript prnjobs {-z | -m | -x | -l | -?} [-s <Servername>] [-p <Printername>] [-j <JobID>] [-u <Username>] [-w <password>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| -Z | 暫停 **-j**參數所指定的列印工作。 |
| -M | 繼續 **-j**參數所指定的列印工作。 |
| -X | 取消 **-j**參數所指定的列印工作。 |
| -l | 列出列印佇列中的所有列印工作。 |
| -s`<Servername>` | 指定裝載您要管理之印表機的遠端電腦名稱稱。 如果您未指定電腦，則會使用本機電腦。 |
| -p`<Printername>` | 必要。 指定您想要管理的印表機名稱。 |
| -j`<JobID>` | 指定您想要取消的列印工作（依識別碼編號）。 |
| -u `<Username>` -w`<password>` | 指定具有許可權可連接到裝載您要管理之印表機的電腦的帳戶。 目的電腦的本機系統管理員群組的所有成員都具有這些許可權，但也可以將許可權授與給其他使用者。 如果您未指定帳戶，您必須使用具有這些許可權的帳戶登入，命令才能正常執行。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 如果您提供的資訊包含空格，請使用引號括住文字（例如「電腦名稱稱」）。

### <a name="examples"></a>範例

若要暫停工作識別碼為27的列印工作，並傳送至名為 HRServer 的遠端電腦，以在名為 colorprinter 的印表機上列印，請輸入：

```
cscript prnjobs.vbs -z -s HRServer -p colorprinter -j 27
```

若要列出本機印表機的佇列中名為 colorprinter_2 的所有目前列印工作，請輸入：

```
cscript prnjobs.vbs -l -p colorprinter_2
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [列印命令參考資料](print-command-reference.md)
