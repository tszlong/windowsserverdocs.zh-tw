---
title: prnjobs
description: 瞭解如何從命令列管理列印工作。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5ad34199-7a5a-40c1-8053-bccd5929df43
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: c4fb9be9545274bbbf33926042f7a4deec5ceb05
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372103"
---
# <a name="prnjobs"></a>prnjobs

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

暫停、繼續、取消和列出列印工作。

## <a name="syntax"></a>語法
```
cscript Prnjobs {-z | -m | -x | -l | -?} [-s <ServerName>] 
[-p <printerName>] [-j <JobID>] [-u <UserName>] [-w <Password>]
```

## <a name="parameters"></a>參數

|          參數           |                                                                                                                                                                                        描述                                                                                                                                                                                        |
|------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              -z              |                                                                                                                                                                 暫停使用 **-j**參數指定的列印工作。                                                                                                                                                                 |
|              -m              |                                                                                                                                                                繼續使用 **-j**參數指定的列印工作。                                                                                                                                                                 |
|              -x              |                                                                                                                                                                取消使用 **-j**參數指定的列印工作。                                                                                                                                                                 |
|              -l              |                                                                                                                                                                        列出列印佇列中的所有列印工作。                                                                                                                                                                         |
|       -s \<ServerName >       |                                                                                                                  指定裝載您要管理之印表機的遠端電腦名稱稱。 如果您未指定電腦，則會使用本機電腦。                                                                                                                  |
|      -p \<printerName >       |                                                                                                                                                           指定您想要管理的印表機名稱。 必要。                                                                                                                                                            |
|         -j \<JobID >          |                                                                                                                                                                指定您想要取消的列印工作（依識別碼編號）。                                                                                                                                                                 |
| -u \<UserName >-w <Password> | 指定具有許可權可連接到裝載您要管理之印表機的電腦的帳戶。 目的電腦的本機系統管理員群組的所有成員都具有這些許可權，但也可以將許可權授與給其他使用者。 如果您未指定帳戶，您必須使用具有這些許可權的帳戶登入，命令才能正常執行。 |
|              /?              |                                                                                                                                                                           在命令提示字元顯示說明。                                                                                                                                                                            |

## <a name="remarks"></a>備註
-   **Prnjobs**命令是位於%WINdir%\System32\printing_Admin_Scripts @ no__t-1 @ no__t-2 目錄中的 Visual Basic 腳本。 若要使用此命令，請在命令提示字元中輸入**cscript** ，後面接著 prnjobs 檔案的完整路徑，或將目錄變更為適當的資料夾。 例如:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnjobs.vbs
    ```
-   如果您提供的資訊包含空格，請使用引號括住文字（例如，`"computer Name"`）。

## <a name="BKMK_examples"></a>典型
若要暫停工作識別碼為27的列印工作，並傳送至名為 HRServer 的遠端電腦，以在名為 colorprinter 的印表機上列印，請輸入：
```
cscript prnjobs.vbs -z -s HRServer -p colorprinter -j 27
```
若要在名為 colorprinter_2 的本機印表機的佇列中列出所有目前的列印工作，請輸入：
```
cscript prnjobs.vbs -l -p colorprinter_2
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [列印命令參考資料](print-command-reference.md)
