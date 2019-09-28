---
title: tskill
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 697363c91837ff675a14099fd212f4f0753b739b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392339"
---
# <a name="tskill"></a>tskill

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

結束在遠端桌面工作階段主機（rd 工作階段主機）伺服器上的會話中執行的處理常式。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要瞭解最新版本的新功能，請參閱 Windows Server TechNet Library 中的[Windows server 2012 遠端桌面服務的新功能](https://technet.microsoft.com/library/hh831527)。

## <a name="syntax"></a>語法
```
tskill {<ProcessID> | <ProcessName>} [/server:<ServerName>] [/id:<SessionID> | /a] [/v]
```

## <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|\<ProcessID >|指定您想要結束之進程的識別碼。|
|\<ProcessName >|指定您想要結束之進程的名稱。 這個參數可以包含萬用字元。|
|/server： \<ServerName >|指定包含您要結束之進程的終端機伺服器。 如果未指定 **/server** ，則會使用目前的 RD 工作階段主機伺服器。|
|/id： \<SessionID >|結束在指定會話中執行的進程。|
|/a|結束正在所有會話中執行的進程。|
|/v|顯示正在執行之動作的相關資訊。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
- 除非您是系統管理員，否則您可以使用**tskill**只結束屬於您的進程。 系統管理員具有所有**tskill**功能的完整存取權，而且可以結束在其他使用者會話中執行的處理常式。
- 當會話中執行的所有處理常式結束時，會話也會結束。
- 如果您使用*ProcessName*和 **/server：** <em>ServerName</em>參數，您也必須指定 **/id：** <em>SessionID</em>或 **/a**參數。

## <a name="BKMK_examples"></a>典型
- 若要結束處理常式6543，請輸入：
  ```
  tskill 6543
  ```
- 若要結束在會話5上執行的「explorer」進程，請輸入：
  ```
  tskill explorer /id:5
  ```
  #### <a name="additional-references"></a>其他參考資料
  [命令列語法索引鍵](command-line-syntax-key.md)
  [遠端桌面服務&#40;終端機&#41;服務命令參考](remote-desktop-services-terminal-services-command-reference.md)
