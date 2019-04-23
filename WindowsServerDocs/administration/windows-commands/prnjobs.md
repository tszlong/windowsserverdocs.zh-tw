---
title: prnjobs
description: 了解如何從命令列管理列印工作。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 03a7f0cf36539272140ea39903bab585b4bc5c72
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889579"
---
# <a name="prnjobs"></a>prnjobs

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

暫停、 繼續、 取消，並列出列印工作。

## <a name="syntax"></a>語法
```
cscript Prnjobs {-z | -m | -x | -l | -?} [-s <ServerName>] 
[-p <printerName>] [-j <JobID>] [-u <UserName>] [-w <Password>]
```

## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|-z|暫停指定的列印工作 **-j**參數。|
|-m|繼續使用指定的列印工作 **-j**參數。|
|-x|取消使用指定的列印工作 **-j**參數。|
|-l|列出所有的列印工作列印佇列中。|
|-s\<伺服器名稱 >|指定裝載的印表機，您想要管理遠端電腦的名稱。 如果您沒有指定的電腦，則會使用本機電腦。|
|-p \<printerName>|指定您想要管理印表機的名稱。 必要。|
|-j \<JobID>|（依 ID 編號） 中指定您想要取消列印工作。|
|-u \<UserName> -w <Password>|指定具有連線至裝載的印表機，您想要管理之電腦的權限的帳戶。 目標電腦的本機 Administrators 群組的所有成員都有這些權限，但可以也對其他使用者授與權限。 如果您未指定帳戶，您必須登入具有可使用該指令的這些權限的帳戶。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
-   **Prnjobs**命令是 Visual Basic 指令碼位於 %WINdir%\System32\printing_Admin_Scripts\\ <language>目錄。 若要使用這個命令中，在命令提示字元中，輸入**cscript**後面 prnjobs 檔案或將目錄變更為適當的資料夾完整路徑。 例如: 
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnjobs
    ```
-   如果您提供的資訊包含空格，請使用引號括住的文字 (例如`"computer Name"`)。

## <a name="BKMK_examples"></a>範例
若要暫停作業識別碼為 27 傳送到遠端電腦名 HRServer 為 colorprinter 印表機上列印的列印工作，請輸入：
```
cscript prnjobs -z -s HRServer -p colorprinter -j 27
```
若要列出所有目前的列印工作，在名為 colorprinter_2 本機印表機佇列中，輸入：
```
cscript prnjobs -l -p colorprinter_2
```

#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[列印命令參考資料](print-command-reference.md)
