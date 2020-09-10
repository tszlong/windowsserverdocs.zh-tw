---
title: tskill
description: Tskill 的參考文章，此文章會結束在遠端桌面工作階段主機伺服器上的會話中執行的處理常式。
ms.topic: reference
ms.assetid: 08986e6a-6900-4ece-85a1-8f73b14db1b3 Lizap
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 24785d10cc09d494850bad5442f72111260dd261
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626709"
---
# <a name="tskill"></a>tskill

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

結束在遠端桌面工作階段主機伺服器上的會話中執行的處理常式。


> [!NOTE]
> 若要瞭解最新版本的新功能，請參閱 Windows server TechNet Library 中的 [Windows server 2012 遠端桌面服務的新功能](/previous-versions/orphan-topics/ws.11/hh831527(v=ws.11)) 。

## <a name="syntax"></a>語法
```
tskill {<ProcessID> | <ProcessName>} [/server:<ServerName>] [/id:<SessionID> | /a] [/v]
```

### <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|\<ProcessID>|指定您要結束之進程的識別碼。|
|\<ProcessName>|指定您要結束之進程的名稱。 這個參數可以包含萬用字元。|
|/server:\<ServerName>|指定包含您要結束之進程的終端機伺服器。 如果未指定 **/server** ，則會使用目前的 RD 工作階段主機伺服器。|
|/id\<SessionID>|結束在指定的會話中執行的處理常式。|
|/a|結束正在所有會話中執行的進程。|
|/v|顯示正在執行之動作的相關資訊。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註
- 除非您是系統管理員，否則您只能使用 **tskill** 來結束屬於您的進程。 系統管理員具有所有 **tskill** 功能的完整存取權，而且可以結束在其他使用者會話中執行的處理常式。
- 當工作階段中執行的所有處理程序均結束時，工作階段也會結束。
- 如果您使用 *ProcessName* 和 **/server：**<em>ServerName</em> 參數，您也必須指定 **/id：**<em>SessionID</em> 或 **/a** 參數。

## <a name="examples"></a>範例
- 若要結束進程6543，請輸入：
  ```
  tskill 6543
  ```
- 若要結束在會話5上執行的 process explorer，請輸入：
  ```
  tskill explorer /id:5
  ```
  ## <a name="additional-references"></a>其他參考資料
  - [命令列語法索引鍵](command-line-syntax-key.md) 
  [遠端桌面服務 (終端機服務) 命令參考](remote-desktop-services-terminal-services-command-reference.md)
