---
title: tskill
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 08986e6a-6900-4ece-85a1-8f73b14db1b3 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b582334d7b79b2badbb86818be1093b6a5f55080
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440810"
---
# <a name="tskill"></a>tskill

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

結束遠端桌面工作階段主機 (rd 工作階段主機) 伺服器上執行的工作階段中的程序。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要了解最新版本的新功能，請參閱[Windows Server 2012 中的遠端桌面服務在何種新的 s](https://technet.microsoft.com/library/hh831527)在 Windows Server TechNet 文件庫中。

## <a name="syntax"></a>語法
```
tskill {<ProcessID> | <ProcessName>} [/server:<ServerName>] [/id:<SessionID> | /a] [/v]
```

## <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|\<ProcessID>|指定您想要結束處理序的識別碼。|
|\<ProcessName>|指定您想要結束處理序的名稱。 這個參數可以包含萬用字元。|
|/server:\<伺服器名稱 >|指定包含您想要結束處理序的終端機伺服器。 如果 **/server**未指定，會使用目前的 RD 工作階段主機伺服器。|
|/id:\<SessionID >|結束指定的工作階段中執行的程序。|
|/a|結束執行所有工作階段中的處理序。|
|/v|顯示已執行之動作的相關資訊。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
- 您可以使用**tskill**結束屬於您，這些處理序，除非您是系統管理員。 系統管理員具有完整存取權**tskill**函式，並可以結束處理程序在其他使用者工作階段中執行。
- 當所有工作階段中執行的處理序結束時，工作階段也會結束。
- 如果您使用*ProcessName*並 **/server:** <em>ServerName</em>參數，您也必須指定其中一個 **/id:** <em>SessionID</em>或 **/a**參數。

## <a name="BKMK_examples"></a>範例
- 若要結束處理程序 6543，輸入：
  ```
  tskill 6543
  ```
- 若要結束處理程序 「 總管 」 工作階段 5 上執行，請輸入：
  ```
  tskill explorer /id:5
  ```
  #### <a name="additional-references"></a>其他參考資料
  [命令列語法重點](command-line-syntax-key.md)
  [遠端桌面服務&#40;終端機服務&#41;命令參考](remote-desktop-services-terminal-services-command-reference.md)
