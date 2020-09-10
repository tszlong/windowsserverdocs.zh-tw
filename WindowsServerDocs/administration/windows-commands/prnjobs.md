---
title: prnjobs
description: Prnjobs 命令的參考文章，此命令會暫停、繼續、取消及列出列印工作。
ms.topic: reference
ms.assetid: 5ad34199-7a5a-40c1-8053-bccd5929df43
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/11/2018
ms.openlocfilehash: 884b8befadf599f1e3e3641dac6bbd384adab16b
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640781"
---
# <a name="prnjobs"></a>prnjobs

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

暫停、繼續、取消和列出列印工作。 此命令是位於目錄中的 Visual Basic 腳本 `%WINdir%\System32\printing_Admin_Scripts\<language>` 。 若要在命令提示字元中使用此命令，請輸入 **cscript** ，後面接著 prnjobs 檔案的完整路徑，或將目錄變更為適當的資料夾。 例如：`cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnjobs.vbs`。

## <a name="syntax"></a>語法

```
cscript prnjobs {-z | -m | -x | -l | -?} [-s <Servername>] [-p <Printername>] [-j <JobID>] [-u <Username>] [-w <password>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| -Z | 暫停 **-j** 參數指定的列印工作。 |
| -M | 繼續 **-j** 參數所指定的列印工作。 |
| -X | 取消 **-j** 參數所指定的列印工作。 |
| -l | 列出列印佇列中的所有列印工作。 |
| -s `<Servername>` | 指定裝載您要管理之印表機的遠端電腦名稱稱。 如果您未指定電腦，則會使用本機電腦。 |
| -p `<Printername>` | 必要。 指定您想要管理之印表機的名稱。 |
| -j `<JobID>` | 依識別碼指定要取消的列印工作)  (。 |
| -u `<Username>` -w `<password>` | 指定有權連線到裝載您要管理之印表機之電腦的帳戶。 目的電腦的本機系統管理員群組的所有成員都具有這些許可權，但也可以將許可權授與其他使用者。 如果您未指定帳戶，則必須以具有這些許可權的帳戶登入，才能使用命令。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 如果您提供的資訊包含空格，請使用引號括住文字 (例如「電腦名稱稱」 ) 。

### <a name="examples"></a>範例

若要暫停工作識別碼為27的列印工作，並將其傳送至名為 HRServer 的遠端電腦，以在名為 colorprinter 的印表機上列印，請輸入：

```
cscript prnjobs.vbs -z -s HRServer -p colorprinter -j 27
```

若要列出本機印表機名稱 colorprinter_2 的佇列中所有目前的列印工作，請輸入：

```
cscript prnjobs.vbs -l -p colorprinter_2
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [列印命令參考資料](print-command-reference.md)
