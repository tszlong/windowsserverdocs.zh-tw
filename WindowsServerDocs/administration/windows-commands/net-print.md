---
title: Net print
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f59b2015-4698-415d-9a74-09566c466f40
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 441a61756869442fb91d26bfacc64bbeb8b902f4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826009"
---
# <a name="net-print"></a>Net print

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

顯示指定的印表機佇列或指定的列印工作的相關資訊，或控制項指定的列印工作。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)這份文件的區段。
> [!NOTE]
> 此命令已被取代，在 Windows 7 和 Windows Server 2008 R2。 不過，您可以執行許多相同的工作使用 prnjobs、 Windows Management Instrumentation (WMI) 或 Windows PowerShell cmdlet。 如需詳細資訊，請參閱 < [prnjobs](prnjobs.md)， [Windows Management Instrumentation](https://go.microsoft.com/fwlink/?LinkID=29991) (https://go.microsoft.com/fwlink/?LinkID=29991)， [Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=128426) (https://go.microsoft.com/fwlink/?LinkID=128426)，以及[TechNet Script Center 組件庫](https://go.microsoft.com/fwlink/?LinkId=164635) (https://go.microsoft.com/fwlink/?LinkId=164635)。
## <a name="syntax"></a>語法
```
Net print {\\<computerName>\<Sharename> | 
\\<computerName> <JobNumber> [/hold | /release | /delete]} [help]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|\\\\<computerName>\\<Sharename>|指定您想要顯示的資訊了解哪種電腦和列印的佇列 （依名稱）。|
|\\\\<computerName>|指定裝載您想要控制列印工作的電腦 （依名稱）。 如果您沒有指定的電腦，則會假設本機電腦。 需要<JobNumber>參數。|
|<JobNumber>|指定您想要控制列印工作的數目。 這個數字會指定裝載列印佇列的傳送列印工作的電腦。 之後將電腦指派一個數字的列印工作，數字不指派給其他任何列印工作，在該電腦所裝載的佇列中。 使用時所需\\ \\ <computerName>參數。|
|[/hold &#124; /release &#124; /delete]|指定要與列印工作採取的動作。<br /><br />- **/保存**參數延遲的工作，並允許其他列印工作，以略過它，直到釋放它為止。<br />- **/版本**參數釋放已延遲列印工作。<br />- **/刪除**參數從列印佇列中移除列印工作。<br /><br />如果您指定作業的數字，但未指定任何動作時，會顯示列印工作的相關資訊。|
|help|顯示的說明**Net 列印**命令。|
## <a name="remarks"></a>備註
-   **Net 列印** \\ \\ <computerName>共用的印表機佇列中顯示的列印工作的相關資訊。 以下是報告的共用名為雷射印表機佇列中的所有列印工作的範例：
    ```
    printers at \\PRODUCTION
    Name              Job #      Size      Status
    -----------------------------
    LASER Queue       3 jobs               *printer active*
       USER1          84        93844      printing
       USER2          85        12555      Waiting
       USER3          86        10222      Waiting
    ```
-   報表的列印工作的範例如下：
    ```
    Job #            35
    Status           Waiting
    Size             3096
    remark
    Submitting user  USER2
    Notify           USER2
    Job data type
    Job parameters
    additional info
    ```
## <a name="BKMK_examples"></a>範例
此範例示範如何列出 Dotmatrix 列印佇列的內容上\\\Production 電腦：
```
Net print \\Production\Dotmatrix 
```
此範例示範如何顯示工作編號 35 的相關資訊上\\\Production 電腦：
```
Net print \\Production 35 
```
此範例示範如何延遲上的作業數目 263 \\\Production 電腦：
```
Net print \\Production 263 /hold 
```
此範例示範如何在發行作業數目 263 \\\Production 電腦：
```
Net print \\Production 263 /release 
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[列印命令參考資料](print-command-reference.md)
