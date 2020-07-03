---
title: prnmngr
description: Prnmngr 命令的參考文章，除了設定和顯示預設印表機以外，還會新增、刪除及列出印表機或印表機連線。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39eee1a8-4b41-4c9f-941e-486495135eb8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 562d25a95fa3ccd556b65d0a29b866557c842c55
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85924173"
---
# <a name="prnmngr"></a>prnmngr

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

除了設定和顯示預設印表機以外，新增、刪除及列出印表機或印表機連線。 此命令是位於目錄中的 Visual Basic 腳本 `%WINdir%\System32\printing_Admin_Scripts\<language>` 。 若要在命令提示字元中使用此命令，請輸入**cscript** ，後面接著 prnmngr 檔案的完整路徑，或將目錄變更為適當的資料夾。 例如：`cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnmngr`。

## <a name="syntax"></a>語法

```
cscript prnmngr {-a | -d | -x | -g | -t | -l | -?}[c] [-s <Servername>] [-p <Printername>] [-m <printermodel>] [-r <portname>] [-u <Username>]
[-w <password>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| -a | 新增本機印表機連接。 |
| -d | 刪除印表機連接。 |
| -X | 從 **-s**參數所指定的伺服器刪除所有印表機。 如果您未指定伺服器，Windows 會刪除本機電腦上的所有印表機。 |
| -g | 顯示預設印表機。 |
| -t | 將預設印表機設定為 **-p**參數所指定的印表機。 |
| -l | 列出安裝在 **-s**參數所指定之伺服器上的所有印表機。 如果您未指定伺服器，Windows 會列出安裝在本機電腦上的印表機。 |
| c | 指定參數適用于印表機連接。 可以與 **-a**和 **-x**參數搭配使用。 |
| -s`<Servername>` | 指定裝載您要管理之印表機的遠端電腦名稱稱。 如果您未指定電腦，則會使用本機電腦。 |
| -p`<Printername>` | 指定您想要管理的印表機名稱。 |
| -m`<Modelname>` | 指定您想要安裝的驅動程式（依名稱）。 驅動程式通常是以其支援的印表機型號來命名。 如需詳細資訊，請參閱印表機檔。 |
| -r`<portname>` | 指定印表機連接的埠。 如果這是平行或序列埠，請使用埠的識別碼（例如，LPT1：或 COM1：）。 如果這是 TCP/IP 通訊埠，請使用新增埠時所指定的埠名稱。 |
| -u `<Username>` -w`<password>` | 指定具有許可權可連接到裝載您要管理之印表機的電腦的帳戶。 目的電腦的本機系統管理員群組的所有成員都具有這些許可權，但也可以將許可權授與給其他使用者。 如果您未指定帳戶，您必須使用具有這些許可權的帳戶登入，命令才能正常執行。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 如果您提供的資訊包含空格，請使用引號括住文字（例如「電腦名稱稱」）。

### <a name="examples"></a>範例

若要新增名為 colorprinter_2 且連接到本機電腦上之 LPT1 的印表機，而且需要稱為彩色印表機 Driver1 的印表機驅動程式，請輸入：

```
cscript prnmngr -a -p colorprinter_2 -m "color printer Driver1" -r lpt1:
```

若要從名為 HRServer 的遠端電腦刪除名為 colorprinter_2 的印表機，請輸入：

```
cscript prnmngr -d -s HRServer -p colorprinter_2
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [列印命令參考資料](print-command-reference.md)
