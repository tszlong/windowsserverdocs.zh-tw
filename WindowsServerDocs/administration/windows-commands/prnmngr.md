---
title: prnmngr
description: 瞭解如何新增、刪除和列出印表機和連線。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39eee1a8-4b41-4c9f-941e-486495135eb8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 12981519a1d3bfc079a58e5883bc845955b8a8c6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372066"
---
# <a name="prnmngr"></a>prnmngr

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

除了設定和顯示預設印表機以外，新增、刪除及列出印表機或印表機連線。

## <a name="syntax"></a>語法
```
cscript Prnmngr {-a | -d | -x | -g | -t | -l | -?}[c] [-s <ServerName>] 
[-p <printerName>] [-m <printermodel>] [-r <PortName>] [-u <UserName>] 
[-w <Password>]
```

## <a name="parameters"></a>參數

|           參數           |                                                                                                                                                                                        描述                                                                                                                                                                                        |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              -a               |                                                                                                                                                                             新增本機印表機連接。                                                                                                                                                                              |
|              -d.ddd...e               |                                                                                                                                                                               刪除印表機連接。                                                                                                                                                                               |
|              -x               |                                                                                                               從使用 **-s**參數指定的伺服器刪除所有印表機。 如果您未指定伺服器，Windows 會刪除本機電腦上的所有印表機。                                                                                                               |
|              -g               |                                                                                                                                                                               顯示預設印表機。                                                                                                                                                                               |
|              -t               |                                                                                                                                                        將預設印表機設定為 **-p**參數所指定的印表機。                                                                                                                                                         |
|              -l               |                                                                                                         列出安裝在 **-s**參數所指定之伺服器上的所有印表機。 如果您未指定伺服器，Windows 會列出安裝在本機電腦上的印表機。                                                                                                         |
|               c               |                                                                                                                                      指定參數適用于印表機連接。 可以與 **-a**和 **-x**參數搭配使用。                                                                                                                                      |
|        -s <ServerName>        |                                                                                                                  指定裝載您要管理之印表機的遠端電腦名稱稱。 如果您未指定電腦，則會使用本機電腦。                                                                                                                  |
|       -p \<printerName >       |                                                                                                                                                                指定您想要管理的印表機名稱。                                                                                                                                                                 |
|     -m \<DrivermodelName >     |                                                                                                          指定您想要安裝的驅動程式（依名稱）。 驅動程式通常是以其支援的印表機型號來命名。 如需詳細資訊，請參閱印表機檔。                                                                                                           |
|        -r \<PortName >         |                                                                         指定印表機連接的埠。 如果這是平行或序列埠，請使用埠的識別碼（例如，LPT1：或 COM1：）。 如果這是 TCP/IP 通訊埠，請使用新增埠時所指定的埠名稱。                                                                          |
| -u \<UserName >-w \<Password > | 指定具有許可權可連接到裝載您要管理之印表機的電腦的帳戶。 目的電腦的本機系統管理員群組的所有成員都具有這些許可權，但也可以將許可權授與給其他使用者。 如果您未指定帳戶，您必須使用具有這些許可權的帳戶登入，命令才能正常執行。 |
|              /?               |                                                                                                                                                                           在命令提示字元顯示說明。                                                                                                                                                                            |

## <a name="remarks"></a>備註
-   **Prndrvr**命令是位於%WINdir%\System32\printing_Admin_Scripts @ no__t-1 @ no__t-2 目錄中的 Visual Basic 腳本。 若要使用此命令，請在命令提示字元中輸入**cscript** ，後面接著**prnmngr**檔案的完整路徑，或將目錄變更為適當的資料夾。 例如:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnmngr
    ```
-   如果您提供的資訊包含空格，請使用引號括住文字（例如，`"computer Name"`）。

## <a name="BKMK_examples"></a>典型
若要新增名為 colorprinter_2 且連接到本機電腦上之 LPT1 的印表機，而且需要名為 [彩色印表機 Driver1] 的印表機驅動程式，請輸入：
```
cscript prnmngr -a -p colorprinter_2 -m "color printer Driver1" -r lpt1:
```
若要從名為 HRServer 的遠端電腦刪除名為 colorprinter_2 的印表機，請輸入：
```
cscript prnmngr -d -s HRServer -p colorprinter_2 
```

#### <a name="additional-references"></a>其他參考
[命令列語法索引鍵](command-line-syntax-key.md)
[列印命令參考](print-command-reference.md)
