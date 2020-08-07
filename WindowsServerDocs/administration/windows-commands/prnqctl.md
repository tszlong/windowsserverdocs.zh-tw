---
title: prnqctl
description: Prnqctl 命令的參考文章，它會列印測試頁，並暫停或繼續印表機。
ms.topic: article
ms.assetid: 8df9dfa7-984c-4276-bb7d-e7675e7c399e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 9ab7c6e8302ebd2c94daee98d8bbef87ecfd4854
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884695"
---
# <a name="prnqctl"></a>prnqctl

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

列印測試頁、暫停或繼續印表機，以及清除印表機佇列。 此命令是位於目錄中的 Visual Basic 腳本 `%WINdir%\System32\printing_Admin_Scripts\<language>` 。 若要在命令提示字元中使用此命令，請輸入**cscript** ，後面接著 prnqctl 檔案的完整路徑，或將目錄變更為適當的資料夾。 例如：`cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnqctl`。

## <a name="syntax"></a>語法

```
cscript Prnqctl {-z | -m | -e | -x | -?} [-s <Servername>] [-p <Printername>] [-u <Username>] [-w <password>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| -Z | 在 **-p**參數所指定的印表機上暫停列印。 |
| -M | 繼續在 **-p**參數所指定的印表機上列印。 |
| -E | 在 **-p**參數所指定的印表機上列印測試頁。 |
| -X | 取消 **-p**參數所指定之印表機上的所有列印工作。 |
| -s`<Servername>` | 指定裝載您要管理之印表機的遠端電腦名稱稱。 如果您未指定電腦，則會使用本機電腦。 |
| -p`<Printername>` | 必要。 指定您想要管理的印表機名稱。 |
| -u `<Username>` -w`<password>` | 指定具有許可權可連接到裝載您要管理之印表機的電腦的帳戶。 目的電腦的本機系統管理員群組的所有成員都具有這些許可權，但也可以將許可權授與給其他使用者。 如果您未指定帳戶，您必須使用具有這些許可權的帳戶登入，命令才能正常執行。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 如果您提供的資訊包含空格，請使用引號括住文字 (例如「電腦名稱稱」 ) 。

### <a name="examples"></a>範例

若要在 Server1 電腦共用的 Laserprinter1 印表機上列印測試頁 \\ ，請輸入：

```
cscript prnqctl -e -s Server1 -p Laserprinter1
```

若要在本機電腦的 Laserprinter1 印表機上暫停列印，請輸入：

```
cscript prnqctl -z -p Laserprinter1
```

若要取消本機電腦上 Laserprinter1 印表機上的所有列印工作，請輸入：

```
cscript prnqctl -x -p Laserprinter1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [列印命令參考資料](print-command-reference.md)
