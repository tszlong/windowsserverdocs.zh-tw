---
title: prnmngr
description: 了解如何新增、 刪除和列出印表機連線。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f0e3af7b05b77400d3d8a04d048b34b8c553438d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436229"
---
# <a name="prnmngr"></a>prnmngr

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

加入、 刪除和列出印表機或印表機連線，除了設定及顯示的預設印表機。

## <a name="syntax"></a>語法
```
cscript Prnmngr {-a | -d | -x | -g | -t | -l | -?}[c] [-s <ServerName>] 
[-p <printerName>] [-m <printermodel>] [-r <PortName>] [-u <UserName>] 
[-w <Password>]
```

## <a name="parameters"></a>參數

|           參數           |                                                                                                                                                                                        描述                                                                                                                                                                                        |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              -a               |                                                                                                                                                                             新增本機印表機連線。                                                                                                                                                                              |
|              -d               |                                                                                                                                                                               刪除印表機連線。                                                                                                                                                                               |
|              -x               |                                                                                                               從指定的伺服器中刪除所有印表機 **-s**參數。 如果您未指定 server，Windows 就會刪除本機電腦上的所有印表機。                                                                                                               |
|              -g               |                                                                                                                                                                               會顯示預設印表機。                                                                                                                                                                               |
|              -t               |                                                                                                                                                        將預設的印表機設定為所指定的印表機 **-p**參數。                                                                                                                                                         |
|              -l               |                                                                                                         列出所指定的伺服器上已安裝的所有印表機 **-s**參數。 如果您未指定 server，Windows 就會列出本機電腦上安裝的印表機。                                                                                                         |
|               c               |                                                                                                                                      指定此參數會套用到印表機連線。 適用於 **-a**並 **-x**參數。                                                                                                                                      |
|        -s <ServerName>        |                                                                                                                  指定裝載的印表機，您想要管理遠端電腦的名稱。 如果您沒有指定的電腦，則會使用本機電腦。                                                                                                                  |
|       -p \<printerName>       |                                                                                                                                                                指定您想要管理印表機的名稱。                                                                                                                                                                 |
|     -m \<DrivermodelName>     |                                                                                                          指定您想要安裝的驅動程式 （依名稱）。 驅動程式通常會命名其所支援的印表機機型。 請參閱印表機說明文件，如需詳細資訊。                                                                                                           |
|        -r \<PortName >         |                                                                         指定印表機連線的位置的連接埠。 如果這是平行或序列連接埠，使用連接埠的識別碼 (例如，LPT1： 或 COM1:)。 如果這是在 TCP/IP 通訊埠，請使用 新增連接埠時所指定的連接埠名稱。                                                                          |
| -u \<UserName> -w \<Password> | 指定具有連線至裝載的印表機，您想要管理之電腦的權限的帳戶。 目標電腦的本機 Administrators 群組的所有成員都有這些權限，但可以也對其他使用者授與權限。 如果您未指定帳戶，您必須登入具有可使用該指令的這些權限的帳戶。 |
|              /?               |                                                                                                                                                                           在命令提示字元顯示說明。                                                                                                                                                                            |

## <a name="remarks"></a>備註
-   **Prndrvr**命令是 Visual Basic 指令碼位於 %WINdir%\System32\printing_Admin_Scripts\\ <language>目錄。 若要使用這個命令中，在命令提示字元中，輸入**cscript**後面的完整路徑**prnmngr**檔案，或將目錄變更為適當的資料夾。 例如:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnmngr
    ```
-   如果您提供的資訊包含空格，請使用引號括住的文字 (例如`"computer Name"`)。

## <a name="BKMK_examples"></a>範例
若要新增印表機，名為 colorprinter_2 連線 LPT1 到本機電腦上，而又不需要呼叫彩色印表機 Driver1 的印表機驅動程式，請輸入：
```
cscript prnmngr -a -p colorprinter_2 -m "color printer Driver1" -r lpt1:
```
若要刪除名為 colorprinter_2 從名為 HRServer 遠端電腦的印表機，請輸入：
```
cscript prnmngr -d -s HRServer -p colorprinter_2 
```

#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[列印命令參考資料](print-command-reference.md)
