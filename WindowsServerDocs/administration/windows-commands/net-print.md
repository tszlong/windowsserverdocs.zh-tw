---
title: Net print
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 241f9d74cb537924cf69c1e0bb5fd73a422c4b23
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373297"
---
# <a name="net-print"></a>Net print

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示指定之印表機佇列或指定之列印工作的相關資訊，或控制指定的列印工作。
如需如何使用此命令的範例，請參閱本檔的[範例](#BKMK_examples)一節。
> [!NOTE]
> 此命令在 Windows 7 和 Windows Server 2008 R2 中已被取代。 不過，您可以使用 prnjobs、Windows Management Instrumentation （WMI）或 Windows PowerShell Cmdlet 來執行許多相同的工作。 如需詳細資訊，請參閱 < [prnjobs](prnjobs.md)， [Windows Management Instrumentation](https://go.microsoft.com/fwlink/?LinkID=29991) (https://go.microsoft.com/fwlink/?LinkID=29991) ， [Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=128426) (https://go.microsoft.com/fwlink/?LinkID=128426) ， 以及 [TechNet Script Center 組件庫](https://go.microsoft.com/fwlink/?LinkId=164635) (https://go.microsoft.com/fwlink/?LinkId=164635) 。
> ## <a name="syntax"></a>語法
> ```
> Net print {\\<computerName>\<Sharename> | 
> \\<computerName> <JobNumber> [/hold | /release | /delete]} [help]
> ```
> ## <a name="parameters"></a>參數
> 
> |               參數               |                                                                                                                                                                                                                     描述                                                                                                                                                                                                                      |
> |----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |    \\\\<computerName>\\<Sharename>     |                                                                                                                                                                            指定您要顯示資訊的電腦和列印佇列（依名稱）。                                                                                                                                                                             |
> |           \\\\<computerName>           |                                                                                                                                 指定主控您想要控制之列印工作的電腦（依名稱）。 如果您未指定電腦，則會假設使用本機電腦。 需要 <JobNumber> 參數。                                                                                                                                  |
> |              <JobNumber>               |                                             指定您想要控制的列印工作編號。 此號碼是由裝載列印工作所傳送之列印佇列的電腦所指派。 電腦將數位指派給列印工作之後，該號碼就不會指派給該電腦所裝載之任何佇列中的任何其他列印工作。 使用 \\ @ no__t-1 @ no__t-2 參數時為必要。                                             |
> | [/hold &#124; /release &#124; /delete] | 指定要對列印工作採取的動作。<br /><br />- **/Hold**參數會延遲作業，讓其他列印工作略過它，直到釋放為止。<br />- **/Release**參數會釋放已延遲的列印工作。<br />- **/Delete**參數會從列印佇列中移除列印工作。<br /><br />如果您指定了作業號碼，但未指定任何動作，則會顯示列印工作的相關資訊。 |
> |                  help                  |                                                                                                                                                                                                     顯示**Net print**命令的說明。                                                                                                                                                                                                     |
> 
> ## <a name="remarks"></a>備註
> - **Net print** \\ @ no__t-2 @ no__t-3 顯示共用印表機佇列中列印工作的相關資訊。 以下是名為「鐳射」的共用印表機佇列中，所有列印工作的報表範例：
>   ```
>   printers at \\PRODUCTION
>   Name              Job #      Size      Status
>   -----------------------------
>   LASER Queue       3 jobs               *printer active*
>      USER1          84        93844      printing
>      USER2          85        12555      Waiting
>      USER3          86        10222      Waiting
>   ```
> - 以下是列印工作的報表範例：
>   ```
>   Job #            35
>   Status           Waiting
>   Size             3096
>   remark
>   Submitting user  USER2
>   Notify           USER2
>   Job data type
>   Job parameters
>   additional info
>   ```
>   ## <a name="BKMK_examples"></a>典型
>   這個範例示範如何列出 \\ \ 生產電腦上的 Dotmatrix 列印佇列內容：
>   ```
>   Net print \\Production\Dotmatrix 
>   ```
>   這個範例示範如何在 \\ \ 生產電腦上顯示作業編號35的相關資訊：
>   ```
>   Net print \\Production 35 
>   ```
>   這個範例示範如何在 \\ \ 生產電腦上延遲作業編號263：
>   ```
>   Net print \\Production 263 /hold 
>   ```
>   這個範例示範如何在 \\ \ 生產電腦上釋放作業編號263：
>   ```
>   Net print \\Production 263 /release 
>   ```
>   #### <a name="additional-references"></a>其他參考
>   [命令列語法索引鍵](command-line-syntax-key.md)
>   [列印命令參考](print-command-reference.md)
