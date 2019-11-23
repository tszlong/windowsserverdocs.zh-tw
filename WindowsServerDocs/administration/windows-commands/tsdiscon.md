---
title: tsdiscon
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 13139674-7dee-4965-8cac-32f4928e8b9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 577ff8ee672583b85c907642bd21256124aa8034
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369873"
---
# <a name="tsdiscon"></a>tsdiscon

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

中斷會話與遠端桌面工作階段主機（rd 工作階段主機）伺服器的連線。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

> [!NOTE]
> 在 Windows Server 2008 R2 中，終端機服務已重新命名為遠端桌面服務。 若要瞭解最新版本的新功能，請參閱 Windows Server TechNet Library 中的[Windows server 2012 遠端桌面服務的新功能](https://technet.microsoft.com/library/hh831527)。

## <a name="syntax"></a>語法
```
tsdiscon [<SessionID> | <SessionName>] [/server:<ServerName>] [/v]
```

## <a name="parameters"></a>Parameters

|參數|描述|
|-------|--------|
|\<SessionId >|指定要中斷連線之會話的識別碼。|
|\<SessionName >|指定要中斷連線之會話的名稱。|
|/server：\<ServerName >|指定包含您要中斷連線之會話的終端機伺服器。 否則，會使用目前的 rd 工作階段主機伺服器。|
|/v|顯示正在執行之動作的相關資訊。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
-   您必須擁有 [完全控制] 許可權或 [中斷連線] 特殊存取權限，才能中斷其他使用者與會話的連線。
-   如果未指定會話識別碼或會話名稱， **tsdiscon**會中斷目前會話的連線。
-   當您在中斷連線會話時執行的任何應用程式，都會在您重新連線到該會話時自動執行，而不會遺失資料。 使用 [**重設會話**] 來結束已中斷連線會話的執行中應用程式，但請注意，這可能會導致會話的資料遺失。
-   只有當您從遠端伺服器使用**tsdiscon**時，才需要 **/server**參數。
-   無法中斷連接主控台會話。

## <a name="BKMK_examples"></a>典型
- 若要中斷目前會話的連線，請輸入：
  ```
  tsdiscon
  ```
- 若要中斷會話10的連線，請輸入：
  ```
  tsdiscon 10
  ```
- 若要中斷名為 TERM04 的會話連線，請輸入：
  ```
  tsdiscon TERM04
  ```
  #### <a name="additional-references"></a>其他參考資料
  
  [ &#40;遠端桌面服務終端機服務&#41;命令參考](remote-desktop-services-terminal-services-command-reference.md)[的命令列語法金鑰](command-line-syntax-key.md)
