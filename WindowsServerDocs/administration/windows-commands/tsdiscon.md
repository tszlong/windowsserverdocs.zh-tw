---
title: tsdiscon
description: Tsdiscon 的參考文章，會中斷與遠端桌面工作階段主機伺服器的會話連線。
ms.topic: reference
ms.assetid: 13139674-7dee-4965-8cac-32f4928e8b9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9fd0292ab1bd53a424c0acaa4b6a2dc98cb1f0a0
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89026856"
---
# <a name="tsdiscon"></a>tsdiscon

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

中斷會話與遠端桌面工作階段主機伺服器的連線。



> [!NOTE]
> 若要瞭解最新版本的新功能，請參閱 Windows server TechNet Library 中的 [Windows server 2012 遠端桌面服務的新功能](/previous-versions/orphan-topics/ws.11/hh831527(v=ws.11)) 。

## <a name="syntax"></a>語法
```
tsdiscon [<SessionID> | <SessionName>] [/server:<ServerName>] [/v]
```

### <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|\<SessionId>|指定要中斷連接之會話的識別碼。|
|\<SessionName>|指定要中斷連接之會話的名稱。|
|/server:\<ServerName>|指定包含您要中斷連線之會話的終端機伺服器。 否則，就會使用目前的 rd 工作階段主機伺服器。|
|/v|顯示正在執行之動作的相關資訊。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
-   您必須擁有 [完全控制] 許可權或 [中斷連線] 特殊存取權限，才能中斷另一位使用者與會話的連線。
-   如果未指定會話識別碼或會話名稱， **tsdiscon** 會中斷目前會話的連線。
-   當您重新連線到不會遺失資料的會話時，任何在中斷連線會話時執行的應用程式都會自動執行。 使用 **重設會話** 來結束已中斷連線會話的執行中應用程式，但請注意，這可能會導致會話的資料遺失。
-   只有當您從遠端伺服器使用**tsdiscon**時，才需要 **/server**參數。
-   主控台會話無法中斷連線。

## <a name="examples"></a>範例
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
  ## <a name="additional-references"></a>其他參考資料
  - [命令列語法索引鍵](command-line-syntax-key.md) 
  [遠端桌面服務 (終端機服務) 命令參考](remote-desktop-services-terminal-services-command-reference.md)
